### If you tried everything in this guide and are still having issues, please make a post on the [forums](https://obsproject.com/forum) or stop by the [OBS Discord server](https://obsproject/discord).

***

**Windows 10:** latest Windows 10 builds overwrite the selection you make in the NVIDIA Control Panel. The setting is now accessible in the main Settings app (cog icon in the start menu) > System > Display > Graphics settings (small link at the bottom) > Browse.

***

When using OBS on a laptop or multi-GPU system, you may run into performance issues or issues using a specific capture type (i.e. Game or Window capture). This can be very frustrating. The reason this happens is because most modern laptops will come with two GPUs:

- An Intel GPU for 2D applications/your desktop
- A discrete graphics chip (either NVIDIA or AMD) for 3D apps and games.

OBS can only run on one of these GPUs, but your open applications and games could be running on either. For example, if OBS is running on the Intel GPU, you will not be able to use Game Capture for your games running on the discrete (NVIDIA or AMD) GPU. Additionally, if OBS is not running on the discrete GPU, you might run into performance issues. In rare cases, trying to capture a game running on a different GPU than OBS can cause the game to crash. This is not really an issue with OBS, but rather a design choice by laptop manufacturers in order to save power and there's little that can be done on our side. However, we do have several troubleshooting suggestions to try and assist with any issues.

***

### If you are getting a black screen with your Capture Sources or are otherwise having performance issues with OBS on your laptop or multi-GPU system, read the following as it applies to you:
- [I want to use Display Capture](#configure-your-system-for-display-capture)
- [Game/Window Capture for NVIDIA-based system](#for-nvidia-based-laptops)
- [Game/Window Capture for AMD-based system](#for-amd-based-laptops)
- [I'm having trouble using Browser Sources](#browser-sources)

***

## Configure your system for Display Capture
If Display Capture is not working, chances are you need to set OBS to run on the Integrated GPU.

#### Display Capture for NVIDIA-based Laptops
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on "Manage 3D Settings" if it is not already selected
- Under the Program Settings tab, click the "Add" button under where it says "Select a program to customize:"
- Navigate to the .exe path for OBS and add it to the list
  - Default paths are: `C:\Program Files\obs-studio\bin\64bit\obs64.exe` and `C:\Program Files (x86)\obs-studio\bin\32bit\obs32.exe`)
- Make sure it is selected in the drop down list
- Then, under where it says "Select the preferred graphics processor for this program" open the drop down and select "Integrated graphics" from the list
- Save and apply, then open OBS and check Display Capture again

#### For AMD-based Laptops
If you are using an AMD-based laptop, check the [Official AMD support documentation](http://support.amd.com/en-us/kb-articles/Pages/DH-017.aspx).

***
## Configure your system for Game/Window Capture

#### For NVIDIA-based Laptops
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on "Manage 3D Settings" if it is not already selected
- Under the Program Settings tab, click the "Add" button under where it says "Select a program to customize:"
- Navigate to the .exe path for OBS and add it to the list
  - Default paths are: `C:\Program Files (x86)\obs-studio\bin\32bit\obs32.exe` and `C:\Program Files (x86)\obs-studio\bin\64bit\obs64.exe`)
- Make sure it is selected in the drop down list
- Then, under where it says "Select the preferred graphics processor for this program" open the drop down and select "High-performance NVIDIA processor"
  - **NOTE:** If you want to use Display Capture on a Laptop, set this to the Integrated Graphics Processor. This will mean that Window Capture and Game Capture will not work, and the performance of OBS Studio may be degraded.
- Re-open OBS and test again

Alternative directions - this sets the global default so all applications will run on the NVIDIA GPU (higher power consumption):
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on Manage 3D Settings on the left, and then the Global tab on the right
- Click the drop down box below that, select "High-performance NVIDIA processor" and click Apply, then OK
- Re-open OBS and test again

***

#### For AMD-based Laptops
If you are using an AMD-based laptop, check the [Official AMD support documentation](http://support.amd.com/en-us/kb-articles/Pages/DH-017.aspx).

***

## Window and Game Capture and "SLi/Crossfire Capture Mode (Slow)" mode
If you cannot set the preferred GPU (AMD laptops typically), or wish to cross-capture an image from the other GPU after that (for example, the League of Legends lobby window), use Window or Game Capture with the "SLi/Crossfire Capture Mode (Slow)" option enabled to force a capture. "SLi/Crossfire Capture Mode (Slow)" requires a bit more CPU usage, however. Compatibility mode is not recommended for capturing games, but it basically guarantees a capture.


***

## Browser Sources

In OBS Studio v22 and upwards, there is an updated version of the browser source that comes with hardware acceleration on by default. This means that browser sources will be rendered on the GPU. However, on laptops or multi-GPU systems, it may not always run on the same GPU that OBS is running on, and tends to favor the high performance GPU.

You can manually select the GPU that the browser source is run on by following the appropriate instructions for [AMD](#for-amd-based-laptops) or [NVIDIA](#for-nvidia-based-laptops), and adjusting the settings for `obs-browser-page.exe`. By default, OBS Studio's installation of obs-browser-page will be at either `C:\Program Files\obs-studio\obs-plugins\64bit\obs-browser-page.exe` or `C:\Program Files (x86)\obs-studio\obs-plugins\64bit\obs-browser-page.exe`.

If you're still having difficulty getting browser sources to render on a laptop or multi-GPU system, you can disable the new hardware acceleration feature. In OBS Studio, go to **File > Settings > Advanced**, and uncheck the "Enable Browser Source Hardware Acceleration" option.

***

## Special note from Jim
I know it's annoying. I'm not happy that this is the case either. Unfortunately, there's nothing anyone can really do about it. This is just the way laptops are designed in order to save power and battery life.