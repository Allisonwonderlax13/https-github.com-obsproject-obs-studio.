The main meter you will need to worry about are the bars filling in the background. When you are speaking you will see this meter bobbing left and right.

You want to adjust the volume so that during speech the meter will bob with the about the same size to the left and on the inside of the yellow area. And within reason, you want to make sure that the meter does not go into the red area.

With adjusting the volume you want to start with the audio interface and mixer volume knobs, and the Sound Preferences window's volume adjustment of your operating system; before adjusting the volume in OBS.

The rest of the chapter have more technical audio engineering information about the meters.

# The Peak Programme Meter (PPM)
The PPM is the main visual feature on the OBS meters, it shows as a bar filling the background of the meter.
The length of the bar indicates the peak volume level of what the viewer will hear when playing your video or stream. You can change the volume by moving the fader which is directly below.

The meter is split into three different coloured sections. At -20 dBFS we have the Alignment Level (AL) and at -9 dBFS we have the Permitted Maximum Level (PML).

Traditionally a sinus tone is played through the whole system and gain is configured for each piece of equipment to show the tone to be at exactly the -20 dBFS Alignment Level (AL). That way the level on your mixing desk will be the same as in OBS, etc. This is of course important if you have a lot of audio equipment in a chain. The value for the alignment level was chosen to be near the average audio level for speech.

The Permitted Maximum Level (PML) is the level where if you go above this value there is a small but potential chance that the sound will get clipped before it reaches the viewer. -9 dBFS was chosen because:

* With most PPM meters, including the one in OBS, the audio may have a peak level 3 dB higher then what is read from the meter.
* When reading PPM meters it is difficult to see the actual peak, so another 3 dB margin is added, this problem is eliminated in OBS due to the peak-hold feature which makes reading accurate.
* Alignment errors, with multiple pieces of equipment in the chain an extra 3 dB margin was added for difference is levels.

# Input Level
The input level are the small square indicator at the far left of the meter. This is the best place to see if the audio is too loud for the audio interface that captures your microphone.

The colours have the following meters:

* dark green: the input level is less than -50 dBFS
* light green: the input level is  between -50 and -20 dBFS
* yellow: the input level is  between -20 and -9 dBFS
* red: the input level is  between -9 dBFS and -0.5 dBFS
* white: the input level is  larger than -0.5 dBFS

If the indicator is missing it means there is no audio streaming toward OBS. This may be simply due to no audio being available yet, waiting for the user to start playback of an audio file. Or it may indicate a problem like the audio interface having been disconnected.
