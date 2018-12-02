The Windows 10 Creators Update (version 1703) added a new Game Mode which tries to allocate more resources to the current game in focus. This features can cause performance issues with OBS and it is recommended to disable it.

Disabling Game Mode is done differently, depending on your Windows 10 version.

### Windows 10 "Redstone 5" 1809 (October 2018)
Settings App > Gaming > Game Mode > set Game Mode to "Off"

![Windows Gaming Settings](https://i.imgur.com/RQ4D2qK.png)

### Windows 10 "Fall Creators Update" 1709 and "Redstone 4" 1803 (April 2018)

Game Mode is enabled automatically by Windows if supported by your hardware and needs to be disabled individually per-application or globally using the Registry. It's recommended to disable it globally.

#### Disable Game Mode globally using the Registry

This requires a change in your Registry.
Run `regedit.exe` or search for `Registry Editor` in the start menu and launch it. This requires administrator access.

![Registry Editor icon](https://i.imgur.com/IdguyRn.png)

In the editor, navigate to `HKEY_CURRENT_USER\Software\Microsoft\GameBar`. You can paste this directly into the address bar at the top to go there quickly.

At the folder, look for a key called `AllowAutoGameMode` in the right panel. If the key is not there, create it. Right-click inside the right panel and select New > "DWORD (32-bit) Value". Rename the new value to `AllowAutoGameMode`.

To disable Game Mode, set this value to 0. To enable Game Mode, set it to 1.

![Registry Editor value highlights](https://i.imgur.com/AQTf72x.png)

#### Disable Game Mode per-game

To disable it per-game, open the properties of the game shortcut or executable, go to Compatibility and check "Disable full-screen optimisations"

![Windows application properties](https://i.imgur.com/sM7Gww6.png)