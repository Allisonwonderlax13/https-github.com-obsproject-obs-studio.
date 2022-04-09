# Background

**NOTE: This is still a work-in-progress. Features are subject to change.**

Given increasing HDR display adoption across users and industry, it makes sense for OBS to expand its support for color spaces.

OBS has been upgraded in the following areas:

- Composition
- Sources
- Preview
- Video format conversion

# Composition

OBS operates by combining video from various sources, and it does so in a master texture that we don't have a formal name for. I'll refer to it as the canvas.

Prior to version 28, OBS has only supported sRGB implicitly using 8 bits per channel. OBS now supports four color spaces for canvas composition.

The first three of these color spaces are "relative" in that the values do not have a specific luminance. OBS has a new setting "SDR White Level (nits)" to allow users to specify the absolute value for 1.0. Our default of 300 nits is a common recommendation in game development to composite SDR UI against HDR gameplay.

![image](https://user-images.githubusercontent.com/10396506/162562966-1218bedf-7f0c-4ecd-9bce-2bdf7711ca61.png)

This setting is analogous to "SDR content brightness" in Windows HDR settings. They have a [0, 100] scale that defaults to 40. That translates to [80, 480] nits with a default of 240 nits. It should be noted that OBS does not use the OS setting for any of its computations, so don't be surprised if SDR content looks different in OBS on an HDR monitor.

![image](https://user-images.githubusercontent.com/10396506/162563005-6695dc6b-3adf-48b6-986f-3145710ad837.png)

## sRGB (SDR, 8 bits per channel, unsigned normalized, [0, 1] range used)

This is the color space OBS has always supported, the tried-and-true SDR color space for desktops since the 1990s. Color is stored nonlinearly to give more precision to darks, which humans are better able to perceive.

GPUs typically have support for "SRGB" textures, which can convert color values to and from linear space, where most color math should be performed. This support can be enabled or disabled as desired, which OBS uses extensively.

## sRGB (SDR, 16 bits per channel, floating-point, [0, 1] range used)

The same color space, but with more bits of precision. This can be helpful to reduce banding typically seen by 8-bit sRGB.

Unlike its 8-bit counterpart, GPUs do not have automatic linear/nonlinear conversions, and values are always stored/manipulated "linearly." (Floating-point itself being a nonlinear representation is largely an implementation detail.)

You may notice that between sRGB, Rec. 709, and Rec. 601, OBS only performs YCbCr conversions, but does not convert primaries or transfer functions. We treat Rec. 709 and Rec. 601 as if they are sRGB for various reasons.

## "Extended Dynamic Range" (EDR) (HDR, 16 bits per channel, floating-point, [0, 125] range used)

This is similar to our 16-bit sRGB space, but values above 1.0 are valid, and represent colors that are "whiter" than diffuse white, e.g. the Sun, specular highlights. The EDR term comes from Apple as far we know.

## "Canonical Compositing Color Space" (CCCS) (HDR, 16 bits per channel, floating-point, [0, 125] range used)

Similar to EDR, but 1.0 has an absolute value of 80 nits. You may also see this referred to as scRGB or "floating-point scRGB" since scRGB didn't originally involve a floating-point representation. The CCCS term comes from Microsoft as far we know.

It should be noted that this color space is mainly used by HDR windows on Windows. OBS uses this space only for window preview and not for composition because the math simplifies if diffuse white is consistently 1.0.

Mac preview can use EDR directly; CCCS preview may be useful when Linux eventually receives HDR support.

The canvas color space is chosen implicitly by "Color Format" and "Color Space" settings.

![image](https://user-images.githubusercontent.com/10396506/162562528-85fa13b1-0845-416c-a727-070df53a350e.png)

Video color format/space -> Canvas color space:
- NV12/I420/I444/RGB + sRGB/Rec. 709/Rec. 601 = sRGB (8-bit)
- P010/I010 + sRGB/Rec. 709/Rec. 601 = sRGB (16-bit floating-point)
- ~~NV12/I420/I444/RGB + Rec. 2020~~ These combinations are not valid.
- P010/I010 + Rec. 2020 = EDR

# Sources

There are three public types of sources: input, filter, transition. It is important to us not to break the existing ecosystem of external plugins, so OBS has decided on an important guideline.

**Sources are 8-bit sRGB by default, and need to opt into extended color spaces.**

For an input source, this is not such a big deal, but you have to be careful with filters and transitions. If you have a setup like this:

Input [HDR] -> Scene [HDR] -> Transition [HDR] -> Canvas [HDR]

Adding an legacy filter that lacks knowledge of extended color spaces will to premature tonemapping:

Input [HDR] -> (implicit HDR to SDR conversion) -> Filter [SDR] -> Scene [SDR] -> Transition [SDR] -> (implicit SDR to HDR conversion) -> Canvas [HDR]

Note: By the time you read this, all filter/transition sources included with OBS should have been upgraded to support HDR to avoid this problem.

Input Sources that have been upgraded to support HDR include:

- Media Source
- Display Capture (Windows)
- Game Capture (Windows)
- Window Capture (Windows)

## Automatic color space conversion

libobs will automatically convert colors when it detects color space mismatches along a source chain.

- sRGB8 -> sRGB8, sRGB16F -> sRGB16F, EDR -> EDR, CCCS -> CCCS: No extra draw
- sRGB8 -> sRGB16F, sRGB16F -> sRGB8: Regular draw with GPU SRGB automatic conversions enabled
- sRGB8/sRGB16F -> EDR: Regular draw since the [0, 1] range maps identically to the [0, 1] range of EDR
- sRGB8/sRGB16F/EDR -> CCCS: Draw that multiplies RGB with a factor of SDR white level divided by 80.0 to go from relative to absolute.
- EDR -> sRGB8/sRGB16F: Draw that tonemaps HDR to SDR.
- CCCS -> EDR: Draw that multiplies RGB with a factor of 80.0 divided by SDR white level.
- CCCS -> sRGB8/sRGB16F: Draw that multiplies RGB with a factor of 80.0 divided by SDR white level to go from absolute to relative, then tonemaps from HDR to SDR.

A source opts into extended color space support by supplying a `video_get_color_space` callback. The signature looks like this:
`enum gs_color_space (*video_get_color_space)(void *data, size_t count, const enum gs_color_space *preferred_spaces);`

It is important that a source be able to change its answer from frame to frame. For example, someone might change Display Capture to point from an SDR monitor to an HDR monitor.

Prior to rendering a source, libobs (or perhaps a filter/transition) will "ask" the next source what color space it wants to render in, providing the set of "preferred spaces" that will lead to fewest unnecessary conversions and/or highest quality. A source is free to ignore the preferred spaces to simplify its implementation, but matching a preferred source is generally, well, preferred, and usually leads to better performance.

As an example, let's say a video game streamer is playing an HDR game, and sending an SDR video stream to a popular streaming service. If Game Capture were implemented lazily:

- libobs: "Hey Game Capture: What color space are you going to render to? I prefer SDR."
- Game Capture: "HDR"
- libobs: "Okay, here's an HDR render target."
- Game Capture renders HDR.
- libobs re-renders from HDR to SDR.
- GPU is sad.

If Game Capture is implemented with a little more effort (and it is):

- libobs: "Hey Game Capture: What color space are you going to render to? I prefer SDR."
- Game Capture: "SDR"
- libobs: "Okay, here's an SDR render target."
- Game Capture renders its HDR image, tonemapping to SDR in the process.
- libobs does not need to re-render from HDR to SDR.
- GPU is happy.

# Preview

Preview windows on OBS take many forms. There's the main preview, the scene preview in Studio Mode, source/filter previews, windows/fullscreen projectors, and multiview. These are backed by Direct3D or OpenGL swap chains. There is an important rule to keep in mind.

**Previews do not care what your OBS settings are. If it is on an SDR monitor, the preview will be SDR. If it on an HDR monitor, the preview will be HDR.**

On Windows, a monitor is HDR if this setting is enabled.

![image](https://user-images.githubusercontent.com/10396506/162563197-3a6682b0-d8fd-45a0-b5fd-9ed3534abfe8.png)

For the time being, Mac and Linux are limited to SDR previews. Mac support is a bit challenging at the moment, and Linux needs to step up their HDR game before we attempt to add HDR preview support.

This is how preview windows handle content:
- sRGB content on sRGB window: Normal draw
- sRGB content on EDR (Mac) window: Normal draw
- sRGB/EDR content on CCCS (Windows) window: Normal draw, adjusted by SDR White Level / 80.0
- EDR content on sRGB window: Normal draw, tonemapped, lossy
- EDR content on EDR (Mac) window: Normal draw

Another important note:

**Previews are just previews. They do not play a role in composition. You can still process and make HDR videos with OBS without an HDR monitor although it's obviously a better experience if you can see the real colors instead of tonemapped colors.**

# Video format conversion

There are two new input/output video formats in the settings: P010/I010. You can read more about them elsewhere, but they are 10-bit formats appropriate for high-precision SDR or HDR.

There are also two new video color spaces in the settings, Rec. 2020 (PQ), and Rec. 2020 (HLG).

![image](https://user-images.githubusercontent.com/10396506/162562528-85fa13b1-0845-416c-a727-070df53a350e.png)

- P010 works well with NVENC HEVC to generate both high-precision sRGB, and PQ/HLG video.
- I010 can be used by AOM AV1 to generate PQ/HLG video. SVT-AV1 does not currently support HDR.

(NVENC HEVC support may or may not be available at this time.)

OBS can leverage the new 10-bit formats both as input in the media source for video playback, and as output for streaming/recording.