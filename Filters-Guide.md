In OBS Studio we have the ability to add filters to our Sources, Scenes and even our Audio Devices. The following filters are available in OBS Studio 18.0.1:

* [Scene and Source Filters](#scene-and-source-filters)
  * [Image Mask/Blend](#image-maskblend)
  * [Crop](#crop)
  * [Color Correction](#color-correction)
  * [Scroll](#scroll)
  * [Color Key](#color-key-and-chroma-key)
  * [LUT Filter](#lut-filter)
  * [Sharpen](#sharpen)
  * [Chroma Key](#color-key-and-chroma-key)
* [Audio Device Filters](#audio-device-filters)
  * [Compressor](#compressor)
  * [Expander](#Expander)
  * [Gain](#gain)
  * [Invert Polarity](#invert-polarity)
  * [Limiter](#Limiter)
  * [Noise Gate](#noise-gate)
  * [Noise Suppression](#noise-suppression)
  * [VST Plugin](#vst-plugin)

You can add them by right-click your desired Scene, Source or Device and selecting "Filters" (for Audio Devices, click on the gear icon next to your device). But let me explain what the different filters allow you to do.

## Scene and Source Filters

### Image Mask/Blend
![Image Mask](https://helping-squad.com/wp-content/uploads/2015/07/image_mask.png)

The Image Mask/Blend filter gives us the option to use the Color or Alpha Channel of an Image as a Mask or to Blend an Image (multiply, addition, subtraction) over your Scene or Source. This can be used to give your webcam a round border for example:
![Image Mask Example](https://helping-squad.com/wp-content/uploads/2015/07/image_mask_example.png)

***

### Crop
![Crop Filter](https://helping-squad.com/wp-content/uploads/2015/07/crop_filter.png)

The crop filter should explain itself but in short it lets you cut off the top/left/right/bottom of your source/scene to only show the parts you want.

***

### Color Correction
![Color Correction Filter](https://i.imgur.com/MJwx4Ep.png)

Again, the name says it all. You can change the contrast, brightness and gamma of your source and even provide a color overlay.

***

### Scroll
![Scroll Filter](https://helping-squad.com/wp-content/uploads/2015/07/scroll_filter.png)

The scroll filter gives us the ability to give our text for example a scrolling effect, left-to-right and top-to-bottom. Negative and positive values will change the direction in which your source will scroll and you can limit the height and width if necessary.

***

### Color Key and Chroma Key

Color Key:

![Color Key Filter](https://helping-squad.com/wp-content/uploads/2015/07/color_key.png)

Chroma Key:

![Chroma Key Filter](https://helping-squad.com/wp-content/uploads/2015/07/chroma_key.png)

Both the Color Key and Chroma Key filter can be used to remove a certain color of your source and make it transparent. This can be used for green screens and similar stuff. They behave slightly differently, so you will need to experiment and see which works best for your personal use case.

***

### LUT Filter
This filter allows you to apply a LUT to your video sources.

***

### Sharpen
![Sharpen Filter](https://helping-squad.com/wp-content/uploads/2015/07/sharpen_filter.png)

The sharpen filter should explain itself as well, if you feel your webcam input for example is a bit blurred and you want to improve the overall sharpness a bit, add the filter and test with different values.

## Audio Device Filters

### Compressor

A compressor is very useful if your input (usually a microphone) is tuned for a normal volume, but can sometimes spike to much louder levels, such as impromptu shouting or getting into a heated discussion. It will automatically turn down the input's volume so that it doesn't peak, which can cause distortion and other audio artifacting, and then turn it back up once the volume is back to normal.

In order, the sliders do the following:

- **Ratio**: The amount of compression to apply. For example, 2:1 will be a weak compression (this translates to an audio level 6dB above the threshold will be 3dB above after the compression), while 6:1 will be a much stronger compression.
- **Threshold**: Once the input reaches this volume, the compressor will kick in.
- **Attack**: How quickly, in milliseconds, you want the compressor to kick in when it detects high volumes.
- **Release**: How quickly, in milliseconds, you want the compressor volume to return to normal once loud volumes calm down.
- **Output gain**: This acts similarly to the standalone [Gain Filter](#gain). When you compress a signal, usually it ends up quieter, so you apply make-up gain to compensate and bring the overall volume of the source back up.


- **Sidechain/Ducking Source**: When a compressor is placed on an output audio source such as your Desktop, sidechain can use the input from a microphone source to "duck" or reduce the level of the output. This will cause your desktop audio to become quieter when you speak into your microphone, returning the audio to full volume when you stop speaking. 
		
### Sidechain Compression/Ducking

The following Sidechain Compression settings are recommended as a place to start, you can adjust the threshold to control the strength of the ducking, attack and release control the shape.
	
* 	Ratio: 32:1
* 	Threshold: -32dB
* 	Attack: 60ms
* 	Release: 600ms
* 	Sidechain/Ducking Source: Microphone

***

### Expander

An expander can be used to reduce background noise such as computer fans, mouse/keyboard clicks, even breathing. An expander reduces the level of an input signal similar to a compressor but does so below the threshold instead of above it.

- **Presets**: Defines default values for the Ratio and Release time.  
	- **Expander**: Low ratio and release time, good for light noise reduction.
	- **Gate**: High ratio and higher release time, can completely block out a signal.
- **Ratio**: The amount of Expansion to apply. A lower ratio such as 2:1 is good for light noise reduction, a higher ratio such as 10:1 will completely gain reduce a signal.
- **Threshold**: Once the input reaches this volume, the expander will stop gain reducing the signal.
- **Attack**: How quickly, in milliseconds, you want the expander to stop gain reducing once the threshold is exceeded. **A shorter attack between 5-10ms is recommended.**
- **Release**: How quickly, in milliseconds, you want the expander to re-engaged once the input drops below the threshold. Higher release times are not recommended, they will cause your background noise audibly fade out. **A release time between 50-120ms is recommended.**
- **Output gain**: This acts similarly to the standalone [Gain Filter](#gain). Increases the output level of the expander.
- **Detection**: Changes how the input level is measured, affecting the sensitivity of the threshold detection. **RMS is recommended.**

	- **RMS**: Averages the input over the last 10ms to reduce the sensitivity of the threshold detection, helps prevent the expander from opening with subtle changes in level.
	- **Peak**: Input measurement is not averaged over time, expander is quicker to react to level changes.

***

### Gain
For very quiet audio sources you can add some gain to increase the volume using this filter.

***

### Invert Polarity
Reverses the phase of an audio signal to correct phase cancellation issues.

***

### Limiter
A limiter is a brick-wall compressor, no signal level can exceed the threshold. Limiters are use to prevent a signal from exceeding 0dB and clipping/distorting.

- **Threshold**: The maximum output level an audio signal can hit.
- **Release**: Because a limiter is a compressor it applies gain reduction to brick-wall output. Release time is how quickly the limiter will return a signal to 1:1 gain reduction.

***

### Noise Gate
The Noise Gate allows you to cut off all background noise while you are not talking. Select a close threshold above the noise volume and an open threshold slightly below your voice input to get good results.

***

### Noise Suppression
The Noise Suppression filter can be used to remove mild background noise or white noise that may be in any of your audio sources. While this is generally not effective at large amounts of background noise (i.e. in a loud room) it can be quite effective at reducing things like PC fan noise or other environmental noises.

0 is off. The further you move the slider to the left, the 'stronger' the filter will be, and the more sounds it will filter out. Keep in mind that this can distort other sounds (like your voice).

***


### VST Plugin
OBS Studio supports many VST2.x plugins. Adding a VST plugin is as simple as adding any other audio filter, but there are some limitations. VST1.x, VST3.x, MIDI control/input in VST plugins, and shell VST plugins are not supported at this time. We have not tested all plugins, and some VST plugins may cause crashes. Make sure you save and back up any settings to avoid loss of data when experimenting with VST.

Lastly, always keep an eye on CPU usage, some VST plugins can be very CPU hungry!

OBS Studio will search for plugins in the following locations:

- Windows (*.dll)
  - C:/Program Files/Steinberg/VstPlugins/
  - C:/Program Files/Common Files/Steinberg/Shared Components/
  - C:/Program Files/Common Files/VST2
  - C:/Program Files/Common Files/VSTPlugins/
  - C:/Program Files/VSTPlugins/
- macOS (*.vst)
  - /Library/Audio/Plug-Ins/VST/
  - ~/Library/Audio/Plug-ins/VST/
- Linux - NOT YET IMPLEMENTED (*.so and *.o)
  - /usr/lib/vst/
  - /usr/lib/lxvst/
  - /usr/lib/linux_vst/
  - /usr/lib64/vst/
  - /usr/lib64/lxvst/
  - /usr/lib64/linux_vst/
  - /usr/local/lib/vst/
  - /usr/local/lib/lxvst/
  - /usr/local/lib/linux_vst/
  - /usr/local/lib64/vst/
  - /usr/local/lib64/lxvst/
  - /usr/local/lib64/linux_vst/
  - ~/.vst/
  - ~/.lxvst/
  - NOTE: If the user has set the VST_PATH environmental variable, OBS will ignore the other search locations and just use the locations listed in VST_PATH.

A short list of free plugins that were used to develop and test the VST support in OBS Studio can be found below. Your experiences may differ, but these are the ones we know have been tested to work in our environments:


- https://www.reaper.fm/reaplugs/
- https://www.meldaproduction.com/download/plugins

Untested, but highly reviewed:

- https://varietyofsound.wordpress.com/
- http://www.vst4free.com/index.php?dev=Kjaerhus_Audio
- http://www.lesliesanford.com/vst/plugins/

***

In the future more filters will be added to OBS Studio, so always keep an eye out for the next update of the software.

`Original guide by Jack0r, updates/edits by Fenrir and the #obs-dev team`