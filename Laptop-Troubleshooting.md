### If you tried everything in this guide and are still having issues, please make a post on the [forums](https://obsproject.com/forum) or stop by the [OBS Discord server](https://obsproject.com/discord).

***

If you have OBS Version 27 or newer and Windows 10 v1903 or newer, here is a [quick setup guide video](https://www.youtube.com/watch?v=Z77oCZ3lojE) which goes over how to set up Display Capture with the new method, should you have problems with it. If your display capture still doesn't work after following that, seek support in the OBS discord or forums.

If you have an older version of OBS or Windows 10 1809 or older, continue on to the appropriate section of the guide.

***

**Windows 10:** latest Windows 10 builds overwrite the selection you make in the NVIDIA Control Panel. The setting is now accessible in the main Settings app (cog icon in the start menu) > System > Display > Graphics settings (small link at the bottom) > Browse.

***

When using OBS on a laptop or multi-GPU system, you may run into performance issues or issues using a specific capture type (i.e. Game or Window capture). This can be very frustrating. The reason this happens is because most modern laptops will come with two GPUs:

- An Intel GPU for 2D applications/your desktop
- A discrete graphics chip (either NVIDIA or AMD) for 3D apps and games.

OBS can only run on one of these GPUs, but your open applications and games could be running on either. For example, if OBS is running on the Intel GPU, you will not be able to use Game Capture for your games running on the discrete (NVIDIA or AMD) GPU. Additionally, if OBS is not running on the discrete GPU, you might run into performance issues. In rare cases, trying to capture a game running on a different GPU than OBS can cause the game to crash. This is not really an issue with OBS, but rather a design choice by laptop manufacturers in order to save power and there's little that can be done on our side. However, we do have several troubleshooting suggestions to try and assist with any issues.

***

## GPU selection for Windows 10 laptops

* This guide will show you how to select the graphics card OBS is running on using built-in Windows settings.
* It is applicable if you are running **Windows 10 version 1903 or newer**.

* [Browser Source Help](#browser-sources)

***

## Check Windows 10 Version

* This guide works only on Windows 10 1903 or newer.
* To check your version, open the Windows 10 Settings App (Start button → Cogwheel icon) and go to System → About and scroll down to "Windows specification".
* If you run Windows 7 or 8, or Windows 10 up to (and including) version 1809, use our separate guides for [NVIDIA](Laptop-GPU-Selection-Nvidia) or [AMD](Laptop-GPU-Selection-Amd) laptops.


![Windows 10 About](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/06-find-version.png)


## Setup

* Close OBS if it is currently open
* Open the Windows 10 Settings App (Start button → Cogwheel icon)
* Navigate to System → Display and select "Graphics settings" near the bottom

![Graphics settings](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/01-graphics-settings.png)

* Select "Classic app" or "Desktop app" and click "Browse" under Graphics performance preference

![Adding application](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/02-add-application.png)

* Navigate and find your OBS Studio executable. By default this is `C:\Program Files\obs-studio\bin\64bit\obs64.exe`
* Once selected, click "Options" under the OBS Studio entry

![GPU preference options](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/03-open-options.png)

Then follow the steps below, depending on which mode you need.

## For Display Capture

* Choose "Power saving" and click Save

![Display Capture setting](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/04-power-saving.png)

## For Window Capture and Game Capture

* Choose "High performance" and click Save

![Game capture setting](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/05-high-perf.png)


## Browser Sources

In OBS Studio v22 and upwards, there is an updated version of the browser source that comes with hardware acceleration on by default. This means that browser sources will be rendered on the GPU. However, on laptops or multi-GPU systems, it may not always run on the same GPU that OBS is running on, and tends to favor the high performance GPU.

You can manually select the GPU that the browser source is run on by following the appropriate instructions for your version of Windows (follow the guide specific to your GPU if you have Windows 10 1809 or older), and adjusting the settings for `obs-browser-page.exe`. By default, OBS Studio's installation of obs-browser-page will be at either `C:\Program Files\obs-studio\obs-plugins\64bit\obs-browser-page.exe` or `C:\Program Files (x86)\obs-studio\obs-plugins\64bit\obs-browser-page.exe`.

If you're still having difficulty getting browser sources to render on a laptop or multi-GPU system, you can disable the new hardware acceleration feature. In OBS Studio, go to **File > Settings > Advanced**, and uncheck the "Enable Browser Source Hardware Acceleration" option.

***

## Special note from Jim
I know it's annoying. I'm not happy that this is the case either. Unfortunately, there's nothing anyone can really do about it. This is just the way laptops are designed in order to save power and battery life.