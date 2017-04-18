Encoding video is a very CPU-intensive operation, and OBS is no exception. OBS uses the best open source video encoding library available, x264, to encode video. However, some people might experience high CPU utilization, and other programs running on your computer might experience degraded performance while OBS is active if your settings are too high for your computer's hardware. In some cases, OBS will say "Encoding overloaded!" on its status bar, meaning that your computer can't encode your video fast enough to maintain the settings you have it set to, which will cause video to freeze after a few seconds, or periodic stuttering.

![Encoding overload](https://pub.rachni.com/img/obs64_2017-03-28_16-19-21.png)

Here are some ways you can reduce resource utilization and, hopefully, make both OBS and your programs run faster while encoding:

## Downscale your output resolution
The resolution that you are encoding at has the biggest impact on CPU usage. For example, 1080p has more than twice the number of pixels in each frame versus 720p, and your CPU usage increases accordingly. The most common way to reduce CPU usage is to downscale your resolution. When you downscale, OBS takes your scene and shrinks it as much as you tell it to before giving it to the encoder. You may want your base resolution at 1080p, since that's the resolution your content is in, but your CPU may not be able to encode an un-downscaled 1080p video. So you can downscale your resolution to 720p (or lower) to keep your image the same, but using a smaller resolution to reduce CPU load.

You can change how much you want to downscale in Settings > Video > Output (Scaled) Resolution. You can keep your Base (Canvas) Resolution the same, so that your layout doesn't change, and then downscale the resolution to whatever gives you good performance.

The different downscale filters (bilinear, bicubic, and Lanczos) simply change the algorithm used to shrink the picture: bilinear is fastest and takes the fewest resources, but doesn't look as good, while Lanczos takes more resources but looks better. Performance-wise, though they aren't that much different. Feel free to experiment with which filter works best for you.

## Lower your frame rate
If you're streaming above 30 FPS, another option is to consider is lowering your frame rate to 30 FPS. It will reduce the number of frames your CPU has to process in a give time span, which will reduce CPU usage. You may even feel the need to lower your frame rate to something below 30 FPS, in the case that your CPU is really weak and struggling.

## Change your x264 preset
The video encoder, x264, has a number of "presets" that will change your video quality and CPU usage accordingly. The OBS default is veryfast, which for the majority of cases is the best balance between CPU usage and video quality. This setting can be changed in Settings > Output (check the Enable Advanced Encoder Settings if you're in Simple mode) > Encoder Preset.

The name of the preset is intended to indicate how "fast" the encoder should run. Faster presets will use less CPU at the cost of quality computations. Slower presets will use use more computations for quality, but will use much more CPU.

For example, if you would like to try to reduce CPU usage without modifying your resolution or FPS, you can reduce your CPU usage by changing your x264 preset to superfast or ultrafast, and x264 will spend less time trying to make the image look good, and will spare you some CPU cycles. The image may look a bit blockier or pixelated, but you will be able to retain your resolution/fps.

Be very careful with this setting, because even one step faster or slower can have a huge impact on CPU usage. For example, the preset named "faster" can use twice the amount of CPU as "veryfast", the one right above it. Always set it back to veryfast if you're not sure what to set this to.

## Try Quicksync, AMF, or NVENC
Quicksync, AMF, and NVENC are hardware encoders that come on recent Intel Integrated GPUs, newer AMD GPUs, and recent nVidia GPUs, respectively. You can offload encoding load to those hardware encoders at the cost of a somewhat noticeable decrease in quality at the same bit rate. Generally speaking, GPU-based encoders don’t quite have as high of quality as x264 for a given bit rate, but the benefit is a greatly reduced load on your CPU.

If you have one of the mentioned hardware encoders, you can see if those options are available to you in Encoding settings. 

- Quicksync is a bit trickier to set up, but here is a guide: [https://obsproject.com/forum/resources/how-to-use-quicksync.82/](https://obsproject.com/forum/resources/how-to-use-quicksync.82/)
- Likewise, here is the troubleshooting guide for the AMF encoder: [https://github.com/Xaymar/obs-studio_amf-encoder-plugin/wiki/Troubleshooting-Guide](https://github.com/Xaymar/obs-studio_amf-encoder-plugin/wiki/Troubleshooting-Guide)

## Check your sources
Some sources such as webcams and capture cards can use a lot of CPU just by being on your scene as they have to decode the video data. If you're using a webcam, check it isn't running at too high of a resolution (more than 480p is rarely needed if it isn't full screen). The Logitech C920 in particular has issues on many systems when running at the full 1080p resolution. Browser sources can also consume CPU if there are complex animations or scripts active.

Check this video for a more detailed explanation (while the video is a bit old and using OBS Classic, this information is still very accurate!): [https://www.youtube.com/watch?v=a274YynXRwI](https://www.youtube.com/watch?v=a274YynXRwI)

## Upgrade your hardware
Some CPUs are so weak that they are near-hopeless for getting anything decent working. Dual-core CPUs and AMD APUs are particularly notorious for this. They might be able to get away with a 360p stream at 25 FPS using the ultrafast preset, but it certainly won't look good. That's up to you to decide. If you have a Sandy Bridge i5 or i7 or later, or an AMD 6-core or 8-core or later, then you should be able to come up with a decent-looking stream at reasonable resolutions and frame rates.

OBS is different from many other streaming/recording programs in that it makes use of your GPU for better performance. Unfortunately, on some older or budget model GPUs this can be a bottleneck in your stream's performance. This is generally due to low memory bandwidth and/or low processor core count. GPUs such as the nVidia GTX 200-series (250, 260, 280) and 9800GT and earlier were once very powerful in their day, but are now very old cards that will make OBS performance suffer greatly.

## Other programs/games use CPU too
Certain programs (particularly games) can use a lot of CPU. This includes some obvious ones, such as Battlefield 4, and some non-obvious ones, such as games played via emulators. If a game uses a lot of CPU, it can interfere with OBS just as OBS can interfere with the game, so you will need to consider turning down these settings to compensate for the game you're playing. You can also use the "Process priority" setting in Settings > Advanced to increase or reduce processor priority of the program. It's common to give OBS "Above normal" process priority to ensure that OBS is prioritized by the system and running smoothly, though use it with caution.

## Ubuntu Dropped Frames with Full Screen Preview
Certain optimized versions of Ubuntu have done tweaks to the Xserver configuration.  This can cause CPU usage to exponentially increase when you enable full screen projector on a second screen.  The problem is most noticeable about 5 to 10 minutes and recording/streaming.   In order to make sure you avoid this issue, make sure you use and install the xserver-xorg-core and appropriate video support (i.e. xserver-xorg-video-intel) packages for your distribution. If you use items like xserver-xorg-hwe-core or other related items, you may run into the cpu usage increase, and this can affect overall system stability and FPS count.

For Ubuntu 16.04 make sure you are using at least xserver-xorg 1.18.4 which is what is distributed with the base packages.
