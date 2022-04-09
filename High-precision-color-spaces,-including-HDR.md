# Background

**NOTE: This is still a work-in-progress. Features are subject to change.**

Prior to version 28, OBS has only supported sRGB implicitly using 8 bits per channel. Given increasing HDR display adoption across users and industry, it makes sense for OBS to expand its support for color spaces.

OBS has been upgraded in the following areas:

- Composition
- Sources
- Preview
- Video format conversion

# Composition

OBS operates by combining video from various sources, and it does so in a master texture that we don't have a formal name for. I'll refer to it as the canvas.

OBS now supports four color spaces for canvas composition.

The first three of these color spaces are "relative" in that the values do not have a specific luminance. OBS has a new setting "SDR White Level (nits)" to allow users to specify the absolute value for 1.0. Our default of 300 nits is a common recommendation in game development to composite SDR UI against HDR gameplay.

[TODO: add screenshot of SDR White Level setting]

This setting is analogous to "SDR content brightness" in Windows HDR settings. They have a [0, 100] scale that defaults to 40. That translates to [80, 480] nits with a default of 240 nits. It should be noted that OBS does not use this setting for any of its computations, so don't be surprised if SDR content looks different in OBS on an HDR monitor.

[TODO: add screenshot of SDR content brightness setting]

## sRGB (SDR, 8 bits per channel, unsigned normalized, [0, 1] range used)

This is the color space OBS has always supported, the tried-and-true SDR color space for desktops since the 1990s. Color is stored nonlinearly to give more precision to darks, which humans are better able to perceive.

GPUs typically have support for "SRGB" textures, which can convert color values to and from linear space, where most color math should be performed. This support can be enabled or disabled as desired, which OBS uses extensively.

## sRGB (SDR, 16 bits per channel, floating-point, [0, 1] range used)

The same color space, but with more bits of precision. This can be helpful to reduce banding typically seen by 8-bit sRGB.

Unlike its 8-bit counterpart, GPUs do not have automatic linear/nonlinear conversions, and values are always manipulated "linearly." (Floating-point itself being a nonlinear representation is largely an implementation detail).

## "Extended Dynamic Range" (EDR) (HDR, 16 bits per channel, floating-point, [0, 65504] range used)

This is similar to our 16-bit sRGB space, but values above 1.0 are valid, and represent colors that are "whiter" than diffuse white, e.g. the Sun, specular highlights. The EDR term comes from Apple.

## "Canonical Compositing Color Space" (CCCS) (HDR, 16 bits per channel, floating-point, [0, 65504] range used)

Similar to EDR, but 1.0 has an absolute value of 80 nits. You may also see this referred to as scRGB, but that didn't originally involve floating-point numbers.

It should be noted that this color space is mainly used by HDR windows on Windows. OBS uses this space only for window preview because compositing is simplified if 1.0 is kept relative.

Mac preview can use EDR directly; CCCS preview may be useful when Linux eventually receives HDR support.

The canvas color space is chosen implicitly by "Color Format" and "Color Space" settings.

[TODO: add screenshot of color format/space settings]

[TODO: List format + video space = canvas space. Highlight new settings.]

## Automatic color space conversion

[TODO: Explain how all the permutations work.]

# Sources

There are three public types of sources: input, filter, transition. It is important to us not to break the existing ecosystem of external plugins, so OBS has decided on an important guideline.

**Sources are 8-bit sRGB by default, and need to opt into extended color spaces.**

For an input source, this is not such a big deal, but you have to be careful with filters and transitions. If you have a setup like this:

Input [HDR] -> Scene [HDR] -> Transition [HDR] -> Canvas [HDR]

Adding an legacy filter that lacks knowledge of extended color spaces will to premature tonemapping:

Input [HDR] -> (implicit HDR to SDR conversion) -> Filter [SDR] -> Scene [SDR] -> Transition [SDR] -> (implicit SDR to HDR conversion) -> Canvas [HDR]

Note: By the time you read this, all filters and transitions included with OBS should have been upgraded.

A source opts into extended color space support by supplying a `video_get_color_space` callback. The signature looks like this:
`enum gs_color_space (*video_get_color_space)(void *data, size_t count, const enum gs_color_space *preferred_spaces);`

Prior to rendering a source, libobs will "ask" the source what color space it wants to render in, providing the set of "preferred spaces" that will lead to fewest unnecessary conversions and/or highest quality. A source is free to ignore the preferred space to simplify its implementation, but matching a preferred source is generally, well, preferred, and lead to better performance.

As an example, let's say a video game streamer is playing an HDR game and sending an SDR video stream to a popular streaming service. If Game Capture were implemented lazily:

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

[TODO]

# Video format conversion

[TODO]