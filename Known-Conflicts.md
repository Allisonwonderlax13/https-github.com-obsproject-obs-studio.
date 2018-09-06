Several 3rd party applications can interfere with the correct operation of OBS Studio. These applications usually hook graphics functions that OBS Studio also wants to hook, which can result in crashes or other strange behavior. If you have any of these applications installed, consider disabling or removing them if you're experiencing issues.

### Dell Backup & Recovery / Alienware Alien Respawn

This backup and recovery software is often bundled with Dell and Alienware systems. It is known to cause crashes whenever OBS Studio tries to open a "Browse" dialog (eg to select an image / video file). It is possible to disable the buggy component without completely removing the program:

* Download https://obsproject.com/downloads/UnregisterDellBackup.cmd
* Right click the .cmd file and run as administrator.

### Nahimic Audio (MSI Audio Software):

The Nahimic audio driver injects a DLL in other processes: NahimicMSIDevProps.dll and NahimicMSIOSD.dll

Both might cause dead locks freezing the user interface. 
Solutions:
* Disable Nahimic drivers
* Uninstall Nahimic drivers

### D3DGear

This application installs global DirectX hooks which causes major compatibility issues with OBS. The software must be uninstalled, closing it will not prevent it from continuing to install hooks.