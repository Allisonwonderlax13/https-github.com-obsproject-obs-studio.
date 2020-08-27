
The new Windows 10 Hardware-accelerated GPU Scheduler ("HAGS") is a new system feature added with Windows 10 version 2004.
It is available only for specific graphics cards and requires up-to-date drivers (for example current Nvidia RTX 2000 series and GTX 1000 series graphics cards).

HAGS is currently known to cause performance and capture issues with OBS, games and overlay tools like Overwolf. It's a new and experimental feature and we currently recommend disabling it.

To disable the Hardware-accelerated GPU Scheduler, change the following option:

Settings App → System → Display → scroll down to Graphics Settings → Turn "Hardware-accelerated GPU scheduling" to **Off**

![Settings App](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/windows-10-hags/2020-08-27_23-40-51_5H1c1.png)

![Display Settings](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/windows-10-hags/2020-08-27_23-45-04_ortRo.png)

![Disable HAGS](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/windows-10-hags/2020-08-27_23-45-26_xBFpE.png)