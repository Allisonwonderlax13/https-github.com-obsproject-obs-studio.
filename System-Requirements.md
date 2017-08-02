OBS Studio requires the following hardware and operating systems to function. Having a compatible system does not guarantee that it is capable of streaming or recording using OBS.

## Basic Requirements

Windows:
- DirectX 11 compatible GPU
- Windows 7 or newer

MacOS:
- Intel CPU
- Mac OSX 10.8.5 or newer (MacOS 10.10 or newer recommended)

Linux/Unix
- OpenGL 3.2 compatible GPU
- X window system

## Hardware Encoders

Hardware encoders are generally recommended for local recordings but not streaming, but can be a last resort if software encoding is not possible. They offer minimal performance impact in trade for a reduction in quality at the same bitrates as software (x264) encoding.

Please make sure to use the latest drivers for your GPU:
- [Nvidia](https://www.geforce.com/drivers)
- [AMD](http://support.amd.com)
- Intel GPU drivers are usually available from your mainboard manufacturer

### Nvidia NVenc
Nvidia GeForce GTX-class GPU with Kepler architecture (GKxx) or newer (starting with some cards of GTX 600 series).

### Intel Quick Sync Video (QSV)
Intel HD Graphics GPU on Intel Core-i-CPU 2xxx (Sandy Bridge) or newer.
Due to low quality of early iterations of QSV, Intel Core-i-CPU 4xxx (Haswell) or newer is recommended.

QSV can be unstable on Windows 7 and may require [workarounds to enable it](https://obsproject.com/forum/resources/how-to-use-quicksync.82/).

### AMD Advanced Media Framework (AMF)
Please read the [AMF plugin documentation for compatible GPUs](https://github.com/Xaymar/obs-studio_amf-encoder-plugin/wiki/Hardware-Support).