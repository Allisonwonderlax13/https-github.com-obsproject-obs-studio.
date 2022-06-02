Minecraft does not default to the high-performance GPU. In order to get it working with game capture you will need to do the following:

**This is mainly for laptops and all-in-one systems.**

A first troubleshooting step is to make sure that the Game Capture source "Mode" is set to "Capture specific window", and select javaw/Minecraft in the "Window" drop-down

![](https://i.imgur.com/Sbsrdmy.png)
* Close OBS and Minecraft

* Open the Windows 10 Settings App (Start button → Cogwheel icon)

![StartMenuCogwheelIcon](https://i.imgur.com/6dUeodW.png)
* Navigate to System → Display and select "Graphics settings" near the bottom

![](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/01-graphics-settings.png)
* Choose "Classic app" or "Desktop app" (they are the same thing, only named differently on different windows versions) and hit "Browse". 

![aDSvJbl](https://user-images.githubusercontent.com/50419942/171719151-816cab5d-ab66-4694-ac26-f76ffdf6eeab.png)


![kphrwU3](https://user-images.githubusercontent.com/50419942/171719879-e88fd089-cb16-4e20-9b34-9c6ed7c2d973.png)


* Once Minecraft/javaw.exe is added, click "Options" and choose "High Performance"

![](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/05-high-perf.png)

***
**The javaw.exe is usually located here**:

For Minecraft Vanilla Java edition 1.16 and older:

`%localappdata%\Packages\Microsoft.4297127D64EC6_8wekyb3d8bbwe\LocalCache\Local\runtime\jre-legacy\windows-x64\jre-legacy\bin\javaw.exe`

***
For Minecraft Vanilla Java edition 1.17:

`%localappdata%\Packages\Microsoft.4297127D64EC6_8wekyb3d8bbwe\LocalCache\Local\runtime\java-runtime-alpha\windows-x64\java-runtime-alpha\bin\javaw.exe`

***
For Minecraft Java edition 1.18:

`%localappdata%\Packages\Microsoft.4297127D64EC6_8wekyb3d8bbwe\LocalCache\Local\runtime\java-runtime-beta\windows-x64\java-runtime-beta\bin\javaw.exe`

***

**If you're using the Lunar Client or a Custom Launcher, you might not find javaw.exe in the default location, but instead in a subfolder under the launcher. Here is how to locate that:**
* Launch Minecraft(the game, not just the launcher)
* Press Ctrl+Shift+ESC(Escape) to open Task Manager
* Right-click `OpenJDK Platform binary` and select "Open file location"
* The folder that opens is the location of your `javaw.exe`. Scroll up and use that path to find the correct javaw.exe to set to High-Performance Graphics.
![hF0in2S](https://user-images.githubusercontent.com/50419942/171719510-2eb5e81b-46c3-4311-b120-053f2586bd87.png)


Once this is completed, restart both OBS and your game (remember that they should have been closed at the beginning of this guide).

