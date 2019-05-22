
Certain Windows 10 gaming features can cause rendering and encoding issues when using OBS. It is recommended you disable them to ensure OBS performs at its best. As of March 2019, as long as you're running 1809 or higher and have the latest Windows updates installed, it is now recommended to leave Game Mode enabled.

All features can be found in the **Windows Settings App** under **Gaming**. The keyboard shortcut to open Settings is **Windows + i**.

### Windows 10 Gaming Features

* [Game Bar](#game-bar)
* [Game DVR/Captures](#game-dvrcaptures)
* [Game Mode](#game-mode)

##  Game Bar

* Settings App > Gaming > Game bar> set **"Record game clips, screenshots and broadcast using Game bar"** to **"Off"**

## Game DVR/Captures

Game DVR can cause conflicts with OBS when using hardware encoding such as NVENC. 

Consumer NVIDIA GPUs for example are limited to two(2) encoding sessions. Game DVR and Geforce Experience(ShadowPlay) will often consume both of these sessions if enabled, preventing you from recording or streaming(or both simultaneously) when using NVENC.

**Game DVR can also cause performance issues, it is still recommended to disable Game DVR even if you don't plan on using hardware encoding.**

Depending on your version of Windows 10 the name of this feature changes. 

* Settings App > Gaming > Game DVR> set **"Record in the background while I'm playing a game"** to **"Off"**

or

* Settings App > Gaming > Captures> set **"Record in the background while I'm playing a game"** to **"Off"**

## Game Mode

**This feature used to cause major performance issues with OBS and games. On older versions of Windows 10, it is recommended you disable it.**

The Windows 10 Creators Update (version 1703) added Game Mode. Game Mode tries to allocate all GPU resources to the current game in focus, starving OBS of the GPU resources it needs to render.

Game Mode is enabled automatically by Windows and disabling it is performed differently depending on your version of Windows 10. Not sure which you have? 

* Press Windows + R and type in `winver` to show your current version of Windows 10.

### Windows 10 "Redstone 5" 1809 (October 2018) or newer

Thanks to optimizations Microsoft made in March 2019, you should leave it on. You can check whether it's currently on using the settings below.

* Settings App > Gaming > Game Mode > set Game Mode to **"Off"**, reboot your PC.

![Windows Gaming Settings](https://obsproject.com/images/wiki/2018-12-02_17-22-45_002xY.png)


### Windows 10 "Fall Creators Update" 1709 and "Redstone 4" 1803 (April 2018)

For Windows 10 1803 or earlier, Game Mode must be disabled individually per-game or globally using the Registry. 

**It's recommended to disable it globally.**

### Disable Game Mode globally using the Registry

**The following steps require Administrator access to perform.**

* Press Windows + R> type `regedit`> hit OK/Enter. You can also open the Start Menu and type `regedit` or `Registry Editor`and launch the application.

![Registry Editor icon](https://obsproject.com/images/wiki/2018-12-02_17-39-45_N5lKy.png)

In the Registry Editor: 

1. Navigate to `HKEY_CURRENT_USER\Software\Microsoft\GameBar`. You can paste this directly into the address bar at the top to go there quickly.

2. In the GameBar folder look for an key called `AllowAutoGameMode` in the right panel, if the key is not there you can create it. 
   * To create the key, right-click inside the panel and select New > "DWORD (32-bit) Value". Rename the new key to `AllowAutoGameMode`.

3. To disable Game Mode, set the value for `AllowAutoGameMode` to `0`, close the Registry Editor, reboot your PC.

![Registry Editor value highlights](https://obsproject.com/images/wiki/2018-12-02_17-42-46_4NMtR.png)

### Disable Game Mode per-game

To disable Game Mode per-game:

* Open the properties for the game shortcut or executable, go to **Compatibility** and turn on **"Disable full-screen optimizations"**

![Windows application properties](https://obsproject.com/images/wiki/2018-12-02_17-36-26_ZtKdV.png)