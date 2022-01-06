* [FAQs](#faqs)
* Solutions
  * [Quick Fix: Run OBS as Administrator (Windows-only)](#quick-fix-run-obs-as-administrator-windows-only)
  * [Check for Other Programs Using the GPU](#check-for-other-programs-using-the-gpu)
  * [Limit the Game Framerate](#limit-the-game-framerate)
  * [Reduce the Game's Graphical Settings](#reduce-the-games-graphical-settings)
  * [Disable Game Capture Multi-Adapter Compatibility (Windows-only)](#disable-game-capture-multi-adapter-compatibility-windows-only)
  * [Disable Windows Gaming Features (Windows-only)](#disable-windows-gaming-features-windows-only)
  * [Build Simpler Scenes](#build-simpler-scenes)
    * [Reduce Filters](#reduce-filters)
    * [Reduce Browser Sources](#reduce-browser-sources)
    * [Keeps Scene Collections Small and Focused](#keep-scene-collections-small-and-focused)
* [If you still cannot solve the issue](#if-you-still-cannot-solve-the-issue)

****

# FAQs

This guide contains an explanation for the common issues that will cause OBS, or a game you may be playing, to suffer performance issues while trying to stream or record.

### *My game framerate is fine, but OBS can't keep up!*

This is probably because you have either overloaded your GPU or you have a bottleneck between your GPU and the rest of your system. These are subtly different issues, but both are important.

### *Why does OBS even need to use the GPU?*

OBS needs GPU time and resources because it has to composite and render a scene. If you want OBS Studio to require less resources, you must ~~construct additional pylons~~ [build simpler scenes and scene collections](#build-simpler-scenes).

### *Can't OBS use the CPU to composite and render instead? My CPU is really strong!*

While this is probably technically possible, GPUs are still way more efficient at this kind of task than CPUs. In the vast majority of cases, this would be a net negative impact on system, rendering, and encoding performance.

### *Okay, okay. I just want this to work, so how do I keep my GPU from being overloaded?*

Preventing GPU overload mostly boils down to preventing your GPU from doing more work than it has to. If your GPU had infinite processing power, you could go ahead and run all of your games with uncapped framerates. Sadly, that's not the case. GPUs, even the really strong ones, have a limited amount of resources to use to do things. That means if we want to make it do many things (for example, play a game, composite and render OBS scenes, do everything else your operating system wants it to do, run hardware encoding if there's no dedicated chip, etc.), we have to be smart about how much we ask it to do. 

****

# Solutions

## Quick Fix: Run OBS as Administrator (Windows only)

In OBS Studio version 24.0.2 and newer, OBS Studio can ask Windows to reserve some GPU capacity for its use. In many cases, GPU overload issues can be resolved simply by running OBS Studio as administrator; try that before continuing with this guide.

* To run OBS Studio as administrator, close OBS Studio.
  * Right click the OBS Studio shortcut
  * Select `Run as administrator`

## Check for Other Programs Using the GPU

If another program is heavily using the GPU, such as another game in the background or a program that uses the GPU to perform some calculations, consider closing it to save resources. This also applies if you have two instances of GPU-intensive games.

* Check Task Manager (Windows)/Activity Monitor (macOS) for programs using a lot of GPU
  * Note: don't close programs unless you are sure what they are for (i.e. only ones that you opened).
* If you are trying to capture two games simultaneously:
  * Try [reducing one or both of the games' graphical settings](#reduce-the-games-graphical-settings), or
  * Consider capturing them separately rather than at the same time.

## Limit the Game Framerate

We know you love playing your favourite AAA+ games on maximum settings with uncapped framerates. We understand that your system can handle it running at several thousand frames per second on your 240 Hz monitor.

However, if you're asking your GPU to work at full throttle to make your game work, you are leaving very little GPU processing power for OBS Studio. The best option is to limit the game's framerate; this will free up processing power for OBS Studio to composite.

* Limit (cap) the game's framerate to match your monitor's refresh rate.
  * Most PC monitors run at 60 Hz.
  * If you have a monitor with a higher refresh rate, for example 144 Hz, set the framerate to match that number.
* If you still have performance issues, try limiting the game to match your framerate in OBS Studio.

> Note: running the game with an uncapped framerate that exceeds your refresh rate brings diminishing returns, as neither you nor OBS Studio will be able to see them.

## Reduce the Game's Graphical Settings

Playing on maximum settings is great if your system can handle it. However, if your game FPS or OBS FPS aren't stable together, then your system can't handle maximum settings _and_ running OBS Studio _at the same time_. If you turn down the game's graphics settings, the game needs to use less GPU resources.

* Reduce the game's graphical quality settings and see how your game/OBS Studio stability changes.

## Disable Game Capture Multi-Adapter Compatibility (Windows-only)

There aren't many situations where you actually _want_ to have this option enabled. If you're running graphics cards in SLI/CrossFire, or if you have a [laptop with multiple graphics cards](https://obsproject.com/wiki/Laptop-Troubleshooting), you *might* need to have this enabled. In pretty much all other cases, you should disable this option.

* In OBS Studio, open the your game capture source's properties.
  * Un-check "Multi-adapter Compatibility".

## Disable Windows Gaming Features (Windows-only)

While Game Mode _can_ make some games run more smoothly, it does this by prioritizing system resources, like the CPU and GPU, to favor the game. This means that it can negatively impact other processes like OBS Studio.

On recent versions of Windows 10, Game Mode can be left on. However, on Windows 10 versions older than 1809, you should disable Game Mode to ensure that Windows isn't pulling resources away from OBS Studio behind the scenes. This can be done in the Windows Settings app in the Gaming section.

Game DVR is a related feature that allows Windows to record game footage in the background very much like OBS Studio's Replay Buffer feature. However, since this uses additional system resources, you should disable this Windows feature if you are using OBS Studio. 

* To disable Game Mode and/or Game DVR, follow the [How to Disable Windows 10 Gaming Features](https://obsproject.com/wiki/How-to-disable-Windows-10-Gaming-Features) guide.

## Build Simpler Scenes

OBS Studio allows you to build wildly complex scenes and make scene collections as large as you want. However, more complexity comes at a price. Every source requires some amount of resources to be shown in the scene. Most sources will also require some resources even if they aren't visible. This is to allow smoother transitions between scenes in the same scene collection, among other benefits.

If your scenes get very complex or your scene collections become too large, OBS Studio may require more resources than your system can spare while doing other things, like gaming. Fortunately, this can be remedied by following a few practices, which we'll go over below:

### Reduce Filters

Filters can let you do all sorts of cool stuff: color adjustments, image masking, image sharpening, render delays, and more! However, filters, like everything else, require some resources to compute and render their effects. In fact, some filters can be pretty resource hungry.

* Try disabling some filters.
  * If this improves performance, consider removing that filter.
* Apply filters to the smallest possible sources rather than to sources (such as entire scenes).
* If you don't want to remove any filters, consider deactivating sources with lots of filters when they are not needed.

### Reduce Browser Sources

Web browsers are amazingly complex feats of software engineering, able to render text, images, 2D graphics, 3D graphics, and animations, and they can play audio, video, and even games. However, they can be very resource intensive.

This also applies for the Browser Source, which is often used to display stream chats, custom animations, overlays, and many other things. Using a lot of browser sources in a scene (or scene collection) can use a lot of system resources which can have a significant performance impact.

* Try to reduce the number of Browser Sources that you use.
* Reduce Browser Sources' width and height to the minimum required size.
* If you have static assets (non-animated overlays), try other source types.
  * For images, use an Image Source.
  * For video or audio files, use Media Source or VLC Source.
* Some stream overlays, such as tip jars, often employ complex JavaScript to simulate physics. These can have a large performance impact. Try disabling these overlays to see if performance improves.

### Keep Scene Collections Small and Focused

OBS Studio uses Scene Collections to organize scenes. You don't need to keep every single scene you'll ever use in the same scene collection. If you find OBS performance is not as snappy as it was when you first started, consider splitting your scene collection up into multiple scene collections. 

* Create new Scene Collections from the `Scene Collection` menu at the top of the OBS Studio window.
* You can also duplicate your current Scene Collection, then remove the scenes you don't need on a per-collection basis.

# If you still cannot solve the issue

***If you tried everything in this guide and are still having issues, please make a post on the [forums or stop by the OBS Discord server](https://obsproject.com/help).***