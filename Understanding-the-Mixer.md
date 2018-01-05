# General Overview

Each entry in the mixer is made up of 4 parts

1. The (Volume) Meter - the coloured bars that light up as audio goes through a source
2. The Fader - the volume slider for that source
3. Mute icon - usually a speaker icon, but changes when the source is muted (fader at minimum)
4. Options button - provides extra settings for the source

Most of the time, you want to keep a close eye on the meters. The rest should be 'set once run forever'.

_[Insert image with areas numbered as above here]_

## Reading the Volume Meter

### The Basics
The volume meter is made up of 3 primary sections signified by the green, yellow and red zones.

* No sound should ever hit the far right of the red zone - this will cause clipping and sound bad in the stream/recording
* Speech should ideally always fall in the yellow zone, occasionally touching the red zone
* Everything else like music, game audio, and alert sound effects should remain in the green zone, ONLY touching the yellow zone if you want something to be as loud as your voice (spooky alerts, for example)

_[Insert image showing zones here]_

Each audio source will have at least one volume meter assigned to it.

* (1) Mono audio source. Viewers will automatically hear this in both left and right channels (headphones/speakers)
* (2) Stereo audio source. Left first is shown first, right second. Viewers will only hear these as they're assigned.
  1) If you have an audio source that appears to be stereo (two bars), but only one meter is lighting up (the first), it's recommended to enable "Mixdown to Mono" in the Advanced Audio Properties, otherwise your viewers will only hear it in their left ear.
* (3 or more) Surround audio source. OBS will by default always mix extra channels down into stereo for the final stream/recording so viewers hear everything.
   1) _[TODO Insert how they're ordered here, FL, FR, etc etc]_

_[Insert image examples of mono, stereo and surround sources here]_

### Setting Audio Levels
There are a number of ways to configure an audio source's volume. 

_**More coming soon.**_

* With adjusting the volume you want to start with the audio interface and mixer volume knobs, and the Sound Preferences window's volume adjustment of your operating system; before adjusting the volume in OBS.
* Microphones will naturally be quieter than anything computer generated – be it games, music or general sound effects. Take that into account when balancing sound.


## Technical Details


### The Peak Programme Meter (PPM)
The PPM is the main visual feature on the OBS meters, it shows as a bar filling the background of the meter.
The length of the bar indicates the peak volume level of what the viewer will hear when playing your video or stream. You can change the volume by moving the fader which is directly below.

The meter is split into three different coloured sections. At -20 dBFS we have the Alignment Level (AL) and at -9 dBFS we have the Permitted Maximum Level (PML).

Traditionally a sinus tone is played through the whole system and gain is configured for each piece of equipment to show the tone to be at exactly the -20 dBFS Alignment Level (AL). That way the level on your mixing desk will be the same as in OBS, etc. This is of course important if you have a lot of audio equipment in a chain. The value for the alignment level was chosen to be near the average audio level for speech.

The Permitted Maximum Level (PML) is the level where if you go above this value there is a small but potential chance that the sound will get clipped before it reaches the viewer. -9 dBFS was chosen because:

* With most PPM meters, including the one in OBS, the audio may have a peak level 3 dB higher then what is read from the meter.
* When reading PPM meters it is difficult to see the actual peak, so another 3 dB margin is added, this problem is eliminated in OBS due to the peak-hold feature which makes reading accurate.
* Alignment errors, with multiple pieces of equipment in the chain an extra 3 dB margin was added for difference is levels.

### Input Level
The input level are the small square indicator at the far left of the meter. This is the best place to see if the audio is too loud for the audio interface that captures your microphone.

The colours have the following meters:

* dark green: the input level is less than -50 dBFS
* light green: the input level is  between -50 and -20 dBFS
* yellow: the input level is  between -20 and -9 dBFS
* red: the input level is  between -9 dBFS and -0.5 dBFS
* white: the input level is  larger than -0.5 dBFS

If the indicator is missing it means there is no audio streaming toward OBS. This may be simply due to no audio being available yet, waiting for the user to start playback of an audio file. Or it may indicate a problem like the audio interface having been disconnected.
