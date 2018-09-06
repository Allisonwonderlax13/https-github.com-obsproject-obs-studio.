# Causes and Solutions Guide

This guide contains an explanation for the common issues that will cause OBS, or a game you may be playing, to suffer performance issues while trying to stream or record.

---

### "My game framerate is fine, but OBS can't keep up!"

This is probably because you have either overloaded your GPU or you have a bottleneck between your GPU and the rest of your system. These are subtly different issues, but both are important.


### "Why does OBS even need to use the GPU?"

OBS needs GPU time and resources because it has to composite and render a scene. If you want OBS to require less resources, ~~you must construct additional pylons~~ you'll have to [build simpler scenes and scene collections](#building-simpler-scenes-and-scene-collections).


### "Can't OBS use the CPU to composite and render instead? My CPU is really strong!"

While this is probably technically possible, GPUs are still way more efficient at this kind of task than CPUs. In the vast majority of cases, this would be a net negative impact on system, rendering, and encoding performance.


### "Okay, okay. I just want this to work, so how do I keep my GPU from being overloaded?"

Preventing GPU overload mostly boils down to preventing your GPU from doing more work than it has to. If your GPU had infinite processing power, you could go ahead and run all of your games with uncapped framerates. Sadly, that's not the case. GPUs, even the really strong ones, have a limited amount of resources to use to do things. That means if we want to make it do many things (for example, play a game, composite and render OBS scenes, do everything else your Operating Systems wants it to do, run hardware encoding if there's no dedicated chip, etc.), we'll have to get smart about how much we ask it to do. 


#### Limit your game framerate
"I love playing PUBG/H1Z1/Overwatch/the-latest-AAA+-PC-game on maximum settings with uncapped framerates! My system is a beast, it can handle it! It runs at 1000 FPS, so it looks great on my 24-inch 240Hz monitor!"

Whoa, hold up. Let's talk about this for a sec. You're asking your GPU to render 1000 frames per second for a device that only outputs 240 frames per second. That's asking your GPU to do unnecessary work, because you'll never see all those extra frames. Once you try to add other tasks on to that, like what OBS needs to do, those tasks start to compete for GPU resources, and they don't always get all the resources that they need _in the time that they need those resources_.

To help ensure that the GPU has enough resources for both the game and OBS (and other tasks that need the GPU), the easiest solution is to cap your game's framerate. Most modern games have a built-in option for this. The simplest thing to do here is set the game's framerate to the refresh rate of your monitor. Most PC monitors run at 60Hz, so for those, you can cap your games at 60 FPS. Some PC monitors run at other refresh rates (70Hz, 75Hz, 100Hz, 144Hz, 240Hz, etc.). That's okay. The key here is to cap your game's framerate to the refresh rate of your monitor, so match those numbers up and that should help.


#### Lower your game's graphics settings

This may seem pretty obvious. If you turn down the game's graphics settings, the game needs to use less GPU resources. Playing on maximum settings is great if your system can handle it. However, if your game FPS or OBS FPS aren't stable together, then your system can't handle maximum settings _and_ OBS _at the same time_. Try turning down the game settings and see how your game/OBS stability changes.


#### Disable OBS game capture multi-adapter compatibility

Maybe you saw a guide that said to enable this, maybe you were trying different things to fix your problem, maybe your PC became sentient and enabled it for you. There aren't many situations where you actually _want_ to have this option enabled. If you're running graphics cards in SLI/CrossFire, or if you have a [laptop with multiple graphics cards](https://obsproject.com/wiki/Laptop-Troubleshooting), you may need to have this enabled. In pretty much all other cases, you should disable this option.

To disable this option, open up the properties of your game capture source in OBS, and un-check "Multi-adapter Compatibility".

#### Windows 10 Game Mode/DVR

Kind of. Even Microsoft has admitted that it's not a holy grail that just makes all games run great. It _can_ make some games run more smoothly, but it does this by prioritizing system resources, like the CPU and GPU, to favor the game. This means that it can negatively impact other processes, like OBS. To ensure that Windows isn't pulling resources away from OBS behind the scenes, you should disable Game Mode. This can be done in the Windows Settings app in the Gaming section. Please note that Windows 10 Game Mode is only available on Windows 10 releases starting from the Anniversary Update (Build 14393).

Game DVR is a related feature that allows Windows to record game footage in the background, pretty much like OBS' Replay Buffer feature. However, since this uses additional system resources, you should disable this Windows feature if you are using OBS. This feature can be disabled in the Windows Settings app in the Gaming section in the "Game DVR" tab. Turn off the "Record in the background while I'm playing a game" option. Please note that Windows 10 Game DVR is only available on Windows 10 releases starting from the Anniversary Update (Build 14393).



## Building simpler scenes and scene collections

OBS allows you to build wildly complex scenes and make scene collections as large as you want. However, more complexity comes at a price. Every source requires some amount of resources to be shown in the scene. Most sources will also require some resources even if they aren't visible. This is to allow smoother transitions between scenes in the same scene collection, among other benefits.

If your scenes get very complex or your scene collections become too large, OBS may require more resources than your system can spare while doing other things, like gaming. Fortunately, this can be remedied by following a few practices, which we'll go over below.


#### Keep scene collections small and focused

OBS uses Scene Collections to organize scenes. You don't need to keep every single scene you'll ever use in the same scene collection. If you find OBS performance is not as snappy as it was when you first started, consider splitting your scene collection up into multiple scene collections. You can create new scene collections or duplicate existing scene collections by using the "Scene Collection" menu at the top of the OBS application window.


#### Easy on the filters!

Filters can let you do all sorts of cool stuff: color adjustments, image masking, image sharpening, render delays, and more! However, filters, like everything else, require some resources to compute and render their effects. In fact, some filters can be pretty resource hungry. If possible, try to simplify your filter usage. If you don't want to remove any filters, consider deactivating sources with lots of filters when they are not needed.


#### Browser sources are performance killers

Web browsers are amazingly complex feats of software engineering. They render text, images, 2D graphics, 3D graphics, and animations, and they can play audio, video, and games! However, they are also pretty heavy resource hogs. The same goes for the Browser Source, which is often used to display stream chats, custom animations, overlays, and many other things. Lots of browser sources in a scene (or scene collection) can end up using a lot of system resources.

Try to reduce the number of browser sources that you use. If you have static assets (non-animated overlays), try using other source types for those things (like Image Source). If you need to play a video or audio file, try the Media Source or VLC Source.