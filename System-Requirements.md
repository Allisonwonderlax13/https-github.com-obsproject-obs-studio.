OBS Studio requires the following hardware and operating systems to function. Having a compatible system does not guarantee that it is capable of streaming or recording using OBS. The CPU requirements vary considerably depending on the chosen encoder, resolution, FPS and your scene complexity. Try the Tools -> Auto Configuration Wizard in OBS itself to find appropriate settings for your specs.

## Basic Requirements

Windows:
- DirectX 10.1 compatible GPU
- Windows 7 SP1 or newer

macOS:
- Intel CPU (PPC is not supported)
- OpenGL 3.2 compatible GPU
- macOS 10.12 or newer

Linux/Unix
- OpenGL 3.2 compatible GPU
- X window system

## Hardware Encoders

Hardware encoders are generally recommended for local recordings, but not streaming. They can be a last resort if software encoding is not possible. They offer minimal performance impact in exchange for a reduction in quality at the same bitrates as software (x264) encoding using the default preset of veryfast. Currently, all consumer-level hardware encoders are provided by certain GPUs, as listed below.

Please make sure to use the latest drivers for your GPU:
- [NVIDIA](https://www.geforce.com/drivers)
- [AMD](http://support.amd.com)
- Intel GPU drivers are usually available from your mainboard manufacturer, but can also be [found here](https://downloadcenter.intel.com/)

### NVIDIA NVENC
NVIDIA GeForce GTX-class GPU with Kepler architecture (GKxx) or newer (starting with some cards of GTX 600 series).
Support for mobile GPUs can exist, but most will not support NVENC. If you get an error when trying to use the NVENC encoder and your drivers are already up to date, your GPU very likely does not support NVENC.

### Intel Quick Sync Video (QSV)
Intel HD Graphics GPU on Intel Core-i-CPU 2xxx (Sandy Bridge) or newer.
Due to low quality of early iterations of QSV, Intel Core-i-CPU 4xxx (Haswell) or newer is recommended.

QSV can be unstable on Windows 7 and may require [workarounds to enable it](https://obsproject.com/forum/resources/how-to-use-quicksync.82/).

### AMD Advanced Media Framework (AMF)
Please read the [AMF plugin documentation for compatible GPUs](https://github.com/Xaymar/obs-studio_amf-encoder-plugin/wiki/Hardware-Support) as well as the [troubleshooting guide](https://github.com/Xaymar/obs-studio_amf-encoder-plugin/wiki/Guide%3A-Troubleshooting) for issues.