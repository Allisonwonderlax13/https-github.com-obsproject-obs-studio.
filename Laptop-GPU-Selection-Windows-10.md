### If you tried everything in this guide and are still having issues, please make a post on the [forums](https://obsproject.com/forum) or stop by the [OBS Discord server](https://obsproject/discord).

***

## GPU selection for Windows 10 laptops

* This guide will show you how to select the graphics card OBS is running on using built-in Windows settings
* It is applicable if you are running Windows 10 version 1903 or newer
* If you run Windows 7 or 8, use our separate guides for [NVIDIA](Laptop-GPU-Selection-Nvidia) or [AMD](Laptop-GPU-Selection-Amd) laptops.

***

## Check Windows 10 Version

* This guide works only on Windows 10 1903 or newer.
* To check your version, open the Windows 10 Settings App (Start button > Cogwheel icon) and go to System → About and scroll down to "Windows specification".

![Windows 10 About](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/06-find-version.png)


## Setup

* Close OBS if it is currently open
* Open the Windows 10 Settings App (Start button > Cogwheel icon)
* Navigate to System → Display and select "Graphics settings" near the bottom

![Graphics settings](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/01-graphics-settings.png)

* Select "Classic app" and click "Browse" under Graphics performance preference

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


