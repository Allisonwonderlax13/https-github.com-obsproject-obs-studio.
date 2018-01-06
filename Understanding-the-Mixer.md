_**Note: This documentation is for a feature that has not yet been released.**_

_It's only available to gather feedback on formatting, structure and wording._

# General Overview

_**Clipping** refers to the actual **distortion** that is heard when an audio signal cannot be accurately reproduced by the gear through which it is passing._

Each entry in the mixer is made up of 5 parts

![Mixer with number indicators](https://i.imgur.com/JYNCE01.png)

1. **The (Volume) Meter** - the coloured bars that light up as audio goes through a source
2. **The Fader** - the volume slider for that source
3. **Mute button** - a speaker icon to mute the source without adjusting the fader
4. **Options button** - provides extra settings for the source
5. **Volume level** - The exact value adjusted by the fader or volume % number, measured in decibels

Most of the time, you want to keep a close eye on the meters. The rest should be 'set once run forever'.

***

# Reading the Volume Meter

## Zones
The volume meter is made up of 3 primary sections signified by the green, yellow and red zones.

![Mixer zones](https://i.imgur.com/eCYfThu.png)

* **Red Zone** - this area should be avoided as it can cause clipping which sounds unpleasant
* **Yellow Zone** - Speech (your own & friends') should always stay in here, occasionally touching the red zone
* **Green Zone** - Everything else including music, game audio and alert sound effects should remain here
   1) Even if other sounds look to be the same volume as your voice, they may in reality sound louder to viewers

## Channels

Each audio source will have at least one volume meter assigned to it.

![Mono, Stereo and Surround example devices](https://i.imgur.com/y1OfYOl.png)

* **(1) Mono source** - viewers will automatically hear this in both left and right channels (headphones/speakers)
* **(2) Stereo source** - left is shown first, right second. Viewers will only hear these as they're assigned.
  1) If only the first meter lights up, enable "Mixdown to Mono" in the Advanced Audio Properties, otherwise your viewers will only hear that source in their left channel
* **(3 or more) Surround source** - when Settings->Audio->Channels is set to Stereo (default), you won't see surround channels
   1) OBS automatically mixes down surround sources to Stereo unless otherwise specified
   2) Ordered as Front Left, Front Right, Front Center, LFE/Sub, Rear Left (5.1), Rear Right (5.1), Side Left (7.1), Side Right (7.1)

## Indicators

![Animated Indicators Example](https://i.imgur.com/UYBtRDG.gif)

1. Left Dot (static): __Input level__ - Live indicator of the volume meter's status (green/yellow/red)
2. Black Dot (always moving): __Vu-meter__ - Shows 'sound pressure', a more accurate indicator of 'loudness'
3. Main Line (always moving): __Peak Programme Meter__ - Has a 'fall-off decay'. After sound stops the bar will slowly go down rather than displaying completely live data, until it reaches -60 dB (empty) or receives new, louder data
4. Right Dot (sometimes static): __Peak__ - Displays the loudest the meter has been in 20 seconds, a great way to see if you're peaking

## Setting Audio Levels
There are a number of ways to configure an audio source's volume.

As you adjust the volume at each step of the process, listen to the device both as early as possible (some devices will have a 'headphone' output or a 'monitor' jack) and again when it hits OBS using Audio Monitoring via Edit->Advanced Audio Properties.

* **Always start at the device in question.**
   1) **Microphones:** check if it has a physical knob (usually labeled "Gain")
   2) **Games/consoles:** most game applications and gaming consoles have their own volume sliders usually hidden in a Settings/Options screen
   3) **Physical mixers:** these have individual gain knobs for their own sources, and a 'Master' before it's sent to your computer & OBS
   4) **For other devices** including audio interfaces, check their user manuals to see how to configure them, as they may require third party software by the manufacturer.
* **Your Operating System** (Windows, macOS, Linux, etc) also has its own volume sliders and mixer
   1) Note that some devices may have a 'safe zone' that is well below the 100% mark. The Samson C01U for example can start clipping around 60% if the input (your voice) is loud enough. Be sure to experiment heavily before attempting to record audio
   2) Your system's primary volume slider **does not** affect the volume of the sound that OBS hears
   3) Individual application volume sliders **do** affect the volume of the sound that OBS hears

Finally, be sure to record a session as you normally would and then listen back to the file before going live.

* If you still feel the audio needs to be tweaked, now is the time to adjust the Fader within OBS.
   1) If you need something higher than the maximum, you can insert custom percentage (%) values that can go well above 100% using Edit->Advanced Audio Properties.

# 

Other things to keep in mind:

* Microphones will naturally be quieter than anything computer generated – be it games, music or general sound effects. Take that into account when balancing your audio.
* Voice communication software like TeamSpeak, Discord and Mumble allow you to 'boost' the voice volume of other members (both overall and individual users) above the 100% mark if needed.
* Most games do not allow per-user volume control so keep this in mind when playing in a matchmaking system.


***


# Technical Details

## decibel relative to Full Scale (dBFS)
Audio is measured in decibel (dB), which is a logarithmic scale which closely resembles how our ears and brain perceive audio volume.

dB is a **relative** measurement. We could put the value 0 dB anywhere on the meter and be correct. In digital audio it is the convention to use 0 **dBFS** (the FS suffix is to indicate this convention) as the maximum volume that the sound card, audio interface, DA/AD-converter can handle. Lower volume levels are shown with negative dBFS values.

OBS internally uses floating point calculations for audio processing, so it does have the ability to process audio that is louder than 0 dBFS. However in the end when OBS records or streams the video, the audio will **need be below 0 dBFS** or the viewer will hear a nasty distortion called clipping (described at this start of this guide).

## The Peak Programme Meter (PPM)
The PPM is the main visual feature on the OBS meter, lighting up as part of the overall volume meter.
The length of the bar indicates the peak volume level of what the viewer will hear when playing your video or stream. You can change the volume by moving the fader which is directly below.

The meter is split into three different coloured sections. At -20 dBFS we have the Alignment Level (AL) and at -9 dBFS we have the Permitted Maximum Level (PML).

Traditionally a sinus tone is played through the whole system and gain is configured for each piece of equipment to show the tone to be at exactly the **-20 dBFS Alignment Level (AL)**. This way the level on your mixing desk will match that of OBS. This is important if you have a lot of audio equipment in a chain. **The value for the alignment level was chosen to be near the average audio level for speech.**

The Permitted Maximum Level (PML) is the level where if you go above this value there is a small but potential chance that the sound will get clipped before it reaches the viewer. -9 dBFS was chosen because:

* With most PPMs, including the one in OBS, the audio may have a peak level 3 dB higher than what is read from the meter.
* When reading PPMs it is difficult to see the actual peak, so another 3 dB margin is added. This problem is eliminated in OBS due to the peak-hold feature which makes reading accurate.
* Alignment errors, with multiple pieces of equipment in the chain an extra 3 dB margin was added for difference is levels.

_OBS currently implements a "Sample peak program meter (SPPM)", in the future it would be preferred to replace it with a proper "4x Over-sampling peak programme meter", which would make it more accurate for measuring maximum peaks._

## Peak and Hold
There is a small line that moves right with the PPM meter, but then stays there when the PPM moves left again. It will stay there for 20 seconds before returning back to where the PPM meter currently is. This allows you to easily check what the maximum level was after you accidentally made a loud noise.

## VU-meter
A second small line on the meter, black and inside the bar of the PPM meter, is a VU-meter. This meter was traditionally used to determine loudness, because it was cheap to manufacture, and in OBS easy to implement.

It measure the root-mean-square, integrated over a period of 300 ms. Due to the calculation it shows more closely the sound pressure levels than does a peak-meter.

_This meter is less useful, but it kept some structure of the code in tacks so we can replace it with a proper loudness meter based on ITU-R BS.1770-2._

## Input Level
The input level are the small square indicator at the far left of the meter. This is the best place to see if the audio is too loud for the audio interface that captures your microphone.

The colours have the following meters:

* dark green: the input level is less than -50 dBFS
* light green: the input level is  between -50 and -20 dBFS
* yellow: the input level is  between -20 and -9 dBFS
* red: the input level is  between -9 dBFS and -0.5 dBFS
* white: the input level is  larger than -0.5 dBFS

If the indicator is missing it means there is no audio streaming toward OBS. This may be simply due to no audio being available yet, waiting for the user to start playback of an audio file. Or it may indicate a problem like the audio interface having been disconnected.

_The input level meter is before the volume fader, but it is behind any filters that are in use by a source. To correctly determine input level you will need to disable the filters for this source._