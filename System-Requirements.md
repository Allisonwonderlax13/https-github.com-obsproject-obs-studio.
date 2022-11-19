# Basic Requirements

OBS Studio requires the following hardware and operating systems to function. Having a compatible system does not guarantee that it is capable of streaming or recording using OBS Studio.

The CPU requirements vary considerably depending on the chosen encoder, resolution, FPS and your scene complexity. Try `Tools menu -> Auto Configuration Wizard` to find appropriate settings for your hardware and settings.

## Windows

- DirectX 10.1 compatible GPU
- Windows 10 release 1809 or later, or Windows 11

## macOS

- Intel or Apple Silicon CPU
- OpenGL 3.3-compatible GPU
- macOS "Catalina" (10.15) or later

## Linux/Unix

- OpenGL 3.3-compatible GPU
- X window system or Wayland

# Hardware Encoders

Hardware encoders are generally recommended for best performance as they take the workload off the CPU and to a specialised component in the GPU that can perform video encoding more efficiently. Modern hardware encoders provide very good quality video with minimal performance impact.

However, earlier generation hardware encoders provide a lower-quality image. They offer minimal performance impact in exchange for a reduction in quality at the same bitrates as software encoding using the default preset of veryfast. As such, they can be a last resort if software encoding is not possible such as due to performance constraints.

Consumer-level hardware encoders are built into the following GPUs:

## NVIDIA NVENC

NVIDIA's NVENC encoder is supported on Windows and Linux.

* You will need a GeForce 600-series (Kepler) or higher.
  * Check the [[list of NVIDIA GPUs with NVENC|NVENC-support-in-OBS]].
* For best results, use a GPU with 6th generation NVENC (Turing).
  * This includes the GTX 1650 rev 2 and higher.
  * Note: GTX 1650 rev 1 contains 5th generation NVENC.

Make sure you have the [latest drivers](https://www.nvidia.com/en-us/geforce/drivers/) for your GPU.

## Intel Quick Sync Video (QSV)

Intel's Quick Sync Video encoder is supported on Windows and Linux.

* You will need an Intel HD Graphics GPU on Intel Core-i-CPU 2xxx (Sandy Bridge) or newer.
  * Due to low quality of early iterations of QSV, Intel Core-i-CPU 4xxx (Haswell) or newer is recommended.
* QSV can be unstable on Windows 7 and may require [workarounds to enable it](https://obsproject.com/forum/resources/how-to-use-quicksync.82/).

Make sure you have the latest drivers. These are usually provided by your manufacturer but may also be found at the [Intel Download Center](https://downloadcenter.intel.com/).

## AMD Advanced Media Framework (AMF)

AMD's Advanced Media Framework encoder is supported on Windows and Linux.

* Please read the [AMF plugin documentation for compatible GPUs](https://github.com/obsproject/obs-amd-encoder/wiki/Hardware-Support) as well as the [troubleshooting guide](https://github.com/obsproject/obs-amd-encoder/wiki/Guide%3A-Troubleshooting) for issues.

Make sure you have the [latest drivers](https://www.amd.com/en/support)

## Apple VideoToolbox (VT)

Apple's VideoToolbox is supported on macOS.

* VideoToolbox uses the system's GPU, regardless of brand.
  * For Macs with multiple GPUs, the choice of GPU is determined by VideoToolbox and cannot be overridden.
* VideoToolbox is not suitable for streaming to services that require a constant bitrate (CBR) stream.