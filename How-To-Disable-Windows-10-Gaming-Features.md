The various Windows 10 gaming features can cause rendering and encoding issues as well as other unwanted effects when using OBS.

It is best to disable all gaming related features in Windows 10 to ensure OBS performs at its best.

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
**This feature can cause major performance issues with OBS and games, it is strongly recommended to disable it.**

The Windows 10 Creators Update (version 1703) added Game Mode which tries to allocate more resources to the current game in focus. 

Disabling Game Mode is done differently depending on your Windows 10 version. Not sure which you have? 

* Press Windows + R and type in "winver" to show your current Windows 10 version.

### Windows 10 "Redstone 5" 1809 (October 2018)
* Settings App > Gaming > Game Mode > set Game Mode to "Off"

![Windows Gaming Settings](https://obsproject.com/images/wiki/2018-12-02_17-22-45_002xY.png)


### Windows 10 "Fall Creators Update" 1709 and "Redstone 4" 1803 (April 2018)
Game Mode is enabled automatically by Windows if supported by your hardware and needs to be disabled individually per-application or globally using the Registry. 

**It's recommended to disable it globally.**

### Disable Game Mode globally using the Registry

This requires a change in your Registry.
Run `regedit.exe` or search for `Registry Editor` in the start menu and launch it. This requires administrator access.

![Registry Editor icon](https://obsproject.com/images/wiki/2018-12-02_17-39-45_N5lKy.png)

In the editor, navigate to `HKEY_CURRENT_USER\Software\Microsoft\GameBar`. You can paste this directly into the address bar at the top to go there quickly.

At the folder, look for a key called `AllowAutoGameMode` in the right panel. If the key is not there, create it. Right-click inside the right panel and select New > "DWORD (32-bit) Value". Rename the new value to `AllowAutoGameMode`.

To disable Game Mode, set this value to 0. To enable Game Mode, set it to 1.

![Registry Editor value highlights](https://obsproject.com/images/wiki/2018-12-02_17-42-46_4NMtR.png)

### Disable Game Mode per-game

To disable it per-game, open the properties of the game shortcut or executable, go to Compatibility and check "Disable full-screen optimisations"

![Windows application properties](https://obsproject.com/images/wiki/2018-12-02_17-36-26_ZtKdV.png)