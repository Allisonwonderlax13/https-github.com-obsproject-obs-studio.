
# The Peak Programme Meter (PPM)
The PPM is the main visual feature on the OBS meters, it shows as a bar filling the background of the meter.
The length of the bar indicates the peak volume level of what the viewer will hear when playing your video or stream. You can change the volume by moving the fader which is directly below.

The meter is split into three different coloured sections. At -20 dBFS we have the Alignment Level (AL) and at -9 dBFS we have the Permitted Maximum Level (PML).

Traditionally a sinus tone is played through the whole system and gain is configured for each piece of equipment to show the tone to be at exactly the -20 dBFS Alignment Level (AL). That way the level on your mixing desk will be the same as in OBS, etc. This is of course important if you have a lot of audio equipment.

More importantly the Alignment Level (AL) is used as the level where the peaks should fall for non-excited speech. This leaves a lot of head-room for excited speech and loud noises. When using a compressor we need less headroom and can broadcast/record louder than this.

The maximum Permitted Maximum Level (PML) is the maximum level that should be used.




# Input Level
The input level are the small square indicator at the far left of the meter. This is the best place to see if the audio is too loud for the audio interface that captures your microphone.

The colours have the following meters:

* dark green: the input level is less than -50 dBFS
* light green: the input level is  between -50 and -20 dBFS
* yellow: the input level is  between -20 and -9 dBFS
* red: the input level is  between -9 dBFS and -0.5 dBFS
* white: the input level is  larger than -0.5 dBFS

If the indicator is missing it means there is no audio streaming toward OBS. This may be simply due to no audio being available yet, waiting for the user to start playback of an audio file. Or it may indicate a problem like the audio interface having been disconnected.

