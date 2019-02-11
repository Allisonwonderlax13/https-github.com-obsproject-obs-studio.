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
A compressor is very useful if your source (typically a microphone) is set for a normal level but can sometimes spike much louder, such as impromptu shouting or getting into a heated discussion. It will automatically lower the source's volume to reduce the likelihood of it peaking above 0dB, which can cause clipping and distortion, and then turn it back up once the volume is back to normal. 

In short, a compressor makes loud sounds quieter and typically would be placed at or near the beginning of your filter chain.

- **Ratio**: The amount of compression or gain reduction to apply to a signal that is above the threshold. For example, 2:1 will be a weak compression (this translates to an audio level 6dB above the threshold will be 3dB above after the compression), while 6:1 will be a much stronger compression.
- **Threshold**: Once the signal reaches this level the compressor will begin to apply compression at the set ratio. When levels are below the threshold the ratio is 1:1 which translates to no gain reduction.
- **Attack**: How quickly, in milliseconds, you want the compressor to reach full gain reduction when levels exceed the threshold.
- **Release**: How quickly, in milliseconds, you want the compressor to return to zero gain reduction after levels drop below the threshold.
- **Output gain**: This acts similarly to the standalone [Gain Filter](#gain). When you compress a signal it typically ends up quieter, reducing your average level. Applying Output or make-up gain brings the average level of the source back up, which can help improve its clarity and presence over-top of other audio sources. 
- **Sidechain/Ducking Source**: When a compressor is placed on an output audio source such as your Desktop, Sidechain can use the input from a microphone/aux source to reduce the volume of the output source.
Sidechain compression, also known as *Ducking*, can be used to make room for your voice when speaking over-top of music and games by lowering your desktop audio when you speak.

*Note. A source can still exceed 0dB with a compressor if your input level is too loud, your ratio/threshold are not set correctly or you apply too much output gain. To ensure you do not exceed 0dB you can use a [Limiter](#limiter) at the end of your filter chain.*
		
### Sidechain Compression/Ducking

The following sidechain compression settings are recommended as a place to start. Adjust the threshold to control the strength of the ducking, attack/release control how quickly the volume changes.
	
- **Ratio**: 32:1
- **Threshold**: -32dB
- **Attack**: 50-100ms (How fast the audio will duck out)
- **Release**: 500ms (How fast the audio will return to full volume)
- **Output Gain**: 0dB (You do not apply output gain when using Sidechain compression)
- **Sidechain/Ducking Source**: Microphone

***

### Expander
An expander can be used to reduce background noise such as computer fans, mouse/keyboard clicks, even breathing in front of the microphone. Like a compressor, an expander reduces the level of an audio signal but it applies the gain reduction below the threshold instead of above it. 

An expander can be used in place of a gate for noise reduction, they are smoother to open and close due to using an adjustable ratio whereas a gate is a fixed ratio.

In short, an expander makes quiet sounds quieter and typically would be placed near the end of your filter chain, after any compression/other effects but before a [Limiter](#limiter).

- **Presets**: Defines some default values to use for the Ratio and Release time.  
	* **Expander**: Low ratio and release time, good for light noise reduction.
	* **Gate**: High ratio and release time, will gain reduce a signal similar to a gate.
- **Ratio**: The amount of expansion or gain reduction to apply to a signal that is below the threshold. A lower ratio such as 2:1 is good for light noise reduction, a higher ratio such as 10:1 will completely gain reduce a signal. A nice balance is 4:1, this should provide an adequate amount of gain reduction without fully gating the signal.
- **Threshold**: Once the input reaches this level the expander will stop gain reducing the signal. Adjust the threshold until the noise you are trying to reduce is gone, but don't go too far or your voice will begin to get cut off.
- **Attack**: How quickly, in milliseconds, you want the expander to stop gain reducing or *open* once the threshold is exceeded. **An attack between 5-10ms is recommended.**
- **Release**: How quickly, in milliseconds, you want the expander to reach full gain reduction or *close* once the input drops below the threshold. **A release between 50-120ms is recommended.**
- **Output gain**: This acts similarly to the standalone [Gain Filter](#gain). Increases the output level of the expander by appling gain, generally not needed but can be used to increase your mic level before outputting it to a limiter.
- **Detection**: Changes how the input level is measured, affecting the sensitivity of the threshold detection. **RMS is recommended.**
	- **RMS**: Averages the input level measurement over the last 10ms to reduce the sensitivity of the threshold detection, helps smooth out and prevent the expander from opening due to quick little peaks of noise.
	- **Peak**: Input level measurement is not averaged over time, expander is quicker to react to peak level changes.

***

### Gain
Gain should generally be applied at the source before it reaches OBS, but if needed the gain filter can help with very quiet audio sources to increase the output volume.

***

### Invert Polarity
Used to correct phase cancellation issues.

***

### Limiter
Limiters are used to prevent an audio signal from peaking above 0dB which can cause clipping and distortion. A limiter is a special type of compressor with a very fast attack and a very high ratio. 

- **Threshold**: The maximum output level an audio signal can hit, no signal can exceed this level.
- **Release**: Because a limiter is a compressor it applies gain reduction to brick-wall the output level. If and when a signal tries to exceed the threshold, the release is how quickly the limiter will stop gain reducing after the level drops below the set threshold.

When using a Limiter it should be the last filter in your chain.

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