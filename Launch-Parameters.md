## List of Launch Parameters

OBS Studio supports the following launch parameters for automation and portable use

| Parameter                | Description |
| ----------------------- | ----------- |
| `--help`, `-h`          | Get list of available parameters. |
| `--version`, `-v`       | Get OBS version.* |
| `--startstreaming`      | Automatically start streaming. |
| `--startrecording`      | Automatically start recording. |
| `--startvirtualcam`      | Automatically start virtual camera. |
| `--startreplaybuffer`   | Automatically start Replay Buffer. |
| `--collection "name"` | Start with given scene collection. |
| `--profile "name"`    | Start with given profile. |
| `--scene "name"`      | Start with given scene. |
| `--studio-mode`         | Start with Studio Mode active. |
| `--minimize-to-tray`    | Start minimized to system tray. |
| `--portable`, `-p`      | Use portable mode. |
| `--multi`, `-m`         | Don't warn when launching multiple instances. |
| `--always-on-top`       | Start in 'always on top' mode. |
| `--verbose`             | Make log more verbose. |
| `--unfiltered_log`      | Disable log filter (do not suppress repeated lines). |
| `--disable-updater`     | Disable built-in updater (Windows/macOS only). |
| `--allow-opengl`        | Allow OpenGL renderer on Windows. |

\* = Not available on Windows

## Windows-specific Instructions
To launch OBS via scheduled tasks or other automated means, make sure to also set a working directory ("Start in...") which must point to the folder where `obs64.exe` is located.

This is required, as OBS loads its files and plugins via relative paths starting from the working directory.