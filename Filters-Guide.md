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
  * [Gain](#gain)
  * [Noise Gate](#noise-gate)
  * [Noise Suppression](#noise-suppression)
  * [Compressor](#compressor)
  * [VST Plugin](#vst-plugin)

You can add them by right-click your desired Scene, Source or Device and selecting "Filters" (for Audio Devices, click on the gear icon next to your device). But let me explain what the different filters allow you to do.

## Scene and Source Filters

### Image Mask/Blend
![Image Mask](http://www.helping-squad.com/wp-content/uploads/2015/07/image_mask.png)

The Image Mask/Blend filter gives us the option to use the Color or Alpha Channel of an Image as a Mask or to Blend an Image (multiply, addition, subtraction) over your Scene or Source. This can be used to give your webcam a round border for example:
![Image Mask Example](http://www.helping-squad.com/wp-content/uploads/2015/07/image_mask_example.png)

***

### Crop
![Crop Filter](http://www.helping-squad.com/wp-content/uploads/2015/07/crop_filter.png)

The crop filter should explain itself but in short it lets you cut off the top/left/right/bottom of your source/scene to only show the parts you want.

***

### Color Correction
![Color Correction Filter](http://i.imgur.com/MJwx4Ep.png)

Again, the name says it all. You can change the contrast, brightness and gamma of your source and even provide a color overlay.

***

### Scroll
![Scroll Filter](http://www.helping-squad.com/wp-content/uploads/2015/07/scroll_filter.png)

The scroll filter gives us the ability to give our text for example a scrolling effect, left-to-right and top-to-bottom. Negative and positive values will change the direction in which your source will scroll and you can limit the height and width if necessary.

***

### Color Key and Chroma Key

Color Key:

![Color Key Filter](http://www.helping-squad.com/wp-content/uploads/2015/07/color_key.png)

Chroma Key:

![Chroma Key Filter](http://www.helping-squad.com/wp-content/uploads/2015/07/chroma_key.png)

Both the Color Key and Chroma Key filter can be used to remove a certain color of your source and make it transparent. This can be used for green screens and similar stuff. They behave slightly differently, so you will need to experiment and see which works best for your personal use case.

***

### LUT Filter
This filter allows you to apply a LUT to your video sources.

***

### Sharpen
![Sharpen Filter](http://www.helping-squad.com/wp-content/uploads/2015/07/sharpen_filter.png)

The sharpen filter should explain itself as well, if you feel your webcam input for example is a bit blurred and you want to improve the overall sharpness a bit, add the filter and test with different values.

## Audio Device Filters

### Gain
For very quiet audio sources you can add some gain to increase the volume using this filter.

***

### Noise Gate
The Noise Gate allows you to cut off all background noise while you are not talking. Select a close threshold above the noise volume and an open threshold slightly below your voice input to get good results.

***

### Noise Suppression
The Noise Suppression filter can be used to remove mild background noise or white noise that may be in any of your audio sources. While this is generally not effective at large amounts of background noise (i.e. in a loud room) it can be quite effective at reducing things like PC fan noise or other environmental noises.

***

### Compressor

A compressor is very useful if your input (usually a microphone) is tuned for a normal volume, but can sometimes spike to much louder levels, such as impromptu shouting or getting into a heated discussion. It will automatically turn down the input's volume so that it doesn't peak, which can cause distortion and other audio artifacting, and then turn it back up once the volume is back to normal.

In order, the sliders do the following:

- Ratio: The amount of compression to apply. For example, 2:1 will be a weak compression (this translates to an audio level 6dB above the threshold will be 3dB above after the compression), while 6:1 will be a much stronger compression.
- Threshold: Once the input reaches this volume, the compressor will kick in.
- Attack: How quickly, in milliseconds, you want the compressor to kick in when it detects high volumes.
- Release: How quickly, in milliseconds, you want the compressor volume to return to normal once loud volumes calm down.
- Output gain: This acts similarly to the standalone [Gain Filter](#gain). When you compress a signal, usually it ends up quieter, so you apply make-up gain to compensate and bring the overall volume of the source back up.

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


- http://www.reaper.fm/reaplugs/
- https://www.meldaproduction.com/download/plugins

Untested, but highly reviewed:

- https://varietyofsound.wordpress.com/
- http://www.vst4free.com/index.php?dev=Kjaerhus_Audio
- http://www.lesliesanford.com/vst/plugins/

***

In the future more filters will be added to OBS Studio, so always keep an eye out for the next update of the software.

`Original guide by Jack0r, updates/edits by Fenrir and the #obs-dev team`