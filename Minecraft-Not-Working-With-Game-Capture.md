Minecraft does not default to the high performance GPU. In order to get it working with game capture you will need to do the following:

**This is mainly for laptops and all-in-one systems.**

* Close OBS and Minecraft

* Open the Windows 10 Settings App (Start button → Cogwheel icon)

![StartMenuCogwheelIcon](https://i.imgur.com/6dUeodW.png)
* Navigate to System → Display and select "Graphics settings" near the bottom

![](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/01-graphics-settings.png)
* Choose "classic app" / "Desktop app" and hit "Browse". Navigate to javaw.exe, usually located here:
> C:\Program Files (x86)\Minecraft Launcher\runtime\jre-x64\bin\javaw.exe

![](https://i.imgur.com/aDSvJbl.png)

![](https://i.imgur.com/kphrwU3.png)

* Once Minecraft/javaw.exe is added, click "Options" and choose "High Performance"

![](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/laptop-troubleshooting/win10/05-high-perf.png)


***

**If you're using a custom launcher, you might not find javaw.exe in the default location, but instead in a subfolder under the launcher, here is how to locate that:**
* Launch Minecraft(the game, not just the launcher
* Press Ctrl+Shift+ESC(Escape) to open Task Manager
* Right click Java(TM) Platform SE binary and select "Open file location"
* The folder that opens is the location of your javaw.exe. Scroll up and use that path to find the correct javaw.exe to set to High Performance Graphics.
![](https://i.imgur.com/hF0in2S.png)

Once this is completed, restart both OBS and your game (remember that they should have been closed at the beginning of this guide).