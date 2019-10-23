Several 3rd party applications can interfere with the correct operation of OBS Studio. These applications usually hook graphics functions that OBS Studio also wants to hook, which can result in crashes or other strange behavior. If you have any of these applications installed, consider disabling or removing them if you're experiencing issues.

### Dell Backup & Recovery / Alienware Alien Respawn

This backup and recovery software is often bundled with Dell and Alienware systems. It is known to cause crashes whenever OBS Studio tries to open a "Browse" dialog (eg to select an image / video file). It is possible to disable the buggy component without completely removing the program:

* Download https://obsproject.com/downloads/UnregisterDellBackup.cmd
* Right click the .cmd file and run as administrator.

### On-screen Displays / Overlays

Programs which draw on top of games and other software to show additional details such as FPS monitors, temperature / GPU monitors, etc often cause issues due to hook conflicts. These applications include, but are not limited to: 

- MSI Afterburner
- RivaTuner
- EVGA Precision
- Dolby Axon
- ASUS GPU Tweak II
- TeamSpeak / Mumble / Discord overlays
- Raptr
- Overwolf
- FRAPS

Please close these applications or disable their OSD functionality when using OBS.

### Other Capture / Recording Software

Running multiple capture / recording applications at once is likely to cause trouble due to the conflicting hooks. If you run other programs which have the ability to record games such as RivaTuner, EVGA Precision, Action!, DXTory, etc, please close them before using OBS.

### D3DGear

This application installs global DirectX hooks which causes major compatibility issues with OBS. The software must be uninstalled, closing it will not prevent it from continuing to install hooks.

### Other Apps

If you have any of these apps installed, be sure to close them before using OBS. If they still cause issues, you may need to uninstall them.

* Intel GPA
* Sendori
* Astrill VPN Proxy
* Nahimic Audio (MSI Audio Software)
* Personify
* Adobe Dynamic Link (Adobe CC)
* Warsaw (bank software)