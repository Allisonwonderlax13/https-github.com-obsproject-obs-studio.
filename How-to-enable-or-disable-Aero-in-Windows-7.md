Windows Aero is a design setting that enables enhanced graphical effects and allows more sophisticated Window Capturing in OBS. On the other hand, disabling Aero can improve Display Capture performance.

### Change Aero setting by selecting an appropriate design theme

1. Start > Control Panel
1. In the Appearance and Personalization section, click "Change the theme"
1. Chose the desired theme:
    * To *disable* Aero, select "Windows Classic" or "Windows 7 Basic" found under "Basic and High Contrast Themes"
    * To *enable* Aero, select any theme under "Aero Themes"

### Change Aero setting using its Windows service

For advanced users, another way is to start or stop the Desktop Window Manager Session Manager service. To do this, use the Services manager snap-in console (`services.msc`) or an administrative command prompt. For the command prompt, use `net stop uxsms` to disable and `net start uxsms` to enable Aero again.