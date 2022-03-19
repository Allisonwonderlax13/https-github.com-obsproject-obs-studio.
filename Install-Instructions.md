If you want to develop for OBS, please visit our [Discord](https://obsproject.com/discord) and get to know the devs or have questions answered!

Also, if there is something in this guide you want to change/improve on, it is recommended that you talk about it with the devs in Discord or IRC first.

### About unofficial builds

Please note that any install directions/packages for Linux/FreeBSD distributions listed as *Unofficial* means that they are community provided and may not provide all the features and/or be up to date or stable.

And any support for those packages should be directed at the appropriate distro/package maintainers.

#### About missing browser source and docks
The CEF (Chromium Embedded Framework) adds any browser-related feature to OBS as a vendored dependency, but many distributions don't provide it when building OBS for their official repository. Reasons could be that:
- OBS Studio supports a specific version of CEF which is sometimes an issue with fixed releases and even with rolling releases.
- almost all distributions want to build the CEF package from scratch, so using an already compiled binary to save time is a no-go.

***

### Table of Contents:

* [Build from source](#build-from-source)
* [Windows](#windows)
* [macOS](#macos)
* [Linux](#linux)
  * Supported builds
    * [Ubuntu/Mint](#ubuntumint)
    * [Arch Linux/Manjaro (Unofficial)](#arch-linuxmanjaro-unofficial)
  * Unofficial builds
    * [Fedora (Unofficial)](#fedora-unofficial)
    * [OpenMandriva (Unofficial)](#openmandriva-unofficial)
    * [Solus (Unofficial)](#solus-unofficial)
    * [openSUSE (Unofficial)](#opensuse-unofficial)
    * [Gentoo (Unofficial)](#gentoo-unofficial)
    * [NixOS (Unofficial)](#nixos-unofficial)
    * [UOS/Deepin (Unofficial)](#uosdeepin-unofficial)
    * [Debian/LMDE (Unofficial)](#debianlmde-unofficial)
    * [Void (Unofficial)](#void-unofficial)
    * [snappy (Unofficial)](#snappy-unofficial)
* [FreeBSD](#freebsd)

***

# Build from source

Please see the build instructions for:
* [Windows](build-instructions-for-windows)
* [macOS](build-instructions-for-macos)
* [Linux](build-instructions-for-linux)
* [FreeBSD](build-instructions-for-freebsd)

# Windows

Pre-built Windows versions can be found here: https://github.com/obsproject/obs-studio/releases/

The full .exe installer and .zip contains OBS Studio 32bit, 64bit, Browser Source, and Intel® RealSense™ plugin. You will be prompted during install for the Browser Source and RealSense plugin to be installed if using the .exe installer, otherwise the components are included in the .zip.

The small .exe installer contains the base OBS Studio 32bit, 64bit, Intel® RealSense™ plugin, but does not contain the Browser Source plugin.

NOTE: If using the .zip method for either the full or small install and installing to a non-standard program location (i.e. outside Program Files), you will need to add the security group ALL APPLICATION PACKAGES to have full control over the main OBS Studio directory and sub-directories. Certain features may not function properly without these security rights (primarily, the ability to use game capture on UWP apps).

# macOS

Pre-built macOS versions can be found here: https://github.com/obsproject/obs-studio/releases

Simply run the installer and follow the on-screen directions to install OBS Studio.

# Linux

**NOTE:** OpenGL 3.3 or later is required to use OBS Studio on Linux. You can check what version of OpenGL is supported by your system by typing `glxinfo | grep "OpenGL"` on Terminal.

### Ubuntu/Mint

**Please note that OBS Studio does not work on Chrome OS.**  
* xserver-xorg version 1.18.4 or newer is recommended to avoid potential performance issues with certain features in OBS, such as the fullscreen projector.
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following commands:

   ```bash
   sudo apt install ffmpeg
   ```

* If you want virtual camera support you need v4l2loopback-dkms installed. You can install it with the following command : 

   ```bash
   sudo apt install v4l2loopback-dkms
   ```

* Make sure you enabled the multiverse repo in Ubuntu's software center (NOTE: On newer versions of Ubuntu, adding a repository automatically apt updates.) Then you can install OBS with the following commands:

   ```bash
   sudo add-apt-repository ppa:obsproject/obs-studio
   sudo apt update
   sudo apt install obs-studio
   ```

## Arch Linux/Manjaro

### Install from AUR

There are OBS packages present in the [AUR](https://wiki.archlinux.org/title/Arch_User_Repository).

Use your favorite AUR helper or Pamac (Add/Remove Software) with AUR enabled in the settings.

* `obs-studio-tytan652` provide OBS Studio with VST and browser modules with the same CEF as official builds with some quality of life patches and fixes.

* `obs-studio-browser` provide OBS Studio with VST and browser modules with the same CEF as official builds.

* `obs-studio-rc` provide OBS Studio latest version (including Release Candidates) with VST and browser modules with the same CEF as official builds.

Most of those packages also replace `vlc` with `vlc-luajit` to provide VLC sources.

More [AUR](https://aur.archlinux.org/packages/?O=0&K=obs-studio-) packages are also available.

## Install from Arch Linux/Manjaro official repository

* Graphical: search `obs-studio` package on Pamac (Add/Remove Software)
* Command Line:
   ```bash
   sudo pacman -S obs-studio
   ```

## Fedora

* OBS Studio is included in RPM Fusion.  If you do not have it configured (if you're not sure, then you probably don't have it), you can do so with the following command:

   ```bash
   sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
   ```

* Then you can install OBS with the following command (this pulls all dependencies, including NVENC-enabled FFmpeg):

   ```bash
   sudo dnf install obs-studio
   ```

* For NVIDIA Hardware accelerated encoding, make sure you have CUDA installed:

   ```bash
   sudo dnf install xorg-x11-drv-nvidia-cuda
   ```

* If you have an older card, use this command instead:
   ```bash
   sudo dnf install xorg-x11-drv-nvidia-340xx-cuda
   ```

# OpenMandriva ([Unofficial](#about-unofficial-builds))
* OBS Studio is included in OpenMandriva Lx3 non-free repository and in restricted repository for upcoming Lx4 release - available now as Cooker.

## OpenMandriva Lx3
* Graphical: search and install "obs-studio" on "OpenMandriva Install and Remove Software" (Rpmdrake)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   urpmi obs-studio
   ```

## OpenMandriva Lx4
* Graphical: search and install "obs-studio" on "OpenMandriva Software Management" (dnfdragora)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   dnf install obs-studio
   ```

## Solus ([Unofficial](#about-unofficial-builds))
* Graphical: search `obs-studio` in Software Center
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:
   ```bash
   eopkg install obs-studio
    ```

## openSUSE ([Unofficial](#about-unofficial-builds))
* The Packman repository contains the obs-studio package since it requires the fuller version of FFmpeg which is in Packman for legal reasons. If you do not already have the Packman repository add it as shown below.
  * For openSUSE Tumbleweed:

   ```bash
   sudo zypper ar -cfp 90 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman
   ```

  * For openSUSE Leap 15.2 (If you're using a derivative of Leap, replace $releasever by your leap release number) :

   ```bash
   sudo zypper ar -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_$releasever/' packman
   ```

* The Packman version of FFmpeg should be used for full codec support. To
  ensure any existing codec packages are switched to Packman versions
  execute the following before installing obs-studio.

   ```bash
   sudo zypper dup --from packman --allow-vendor-change
   ```

* Install the obs-studio package.

   ```bash
   sudo zypper in obs-studio
   ```

Links:
* 1 click install, direct rpm links, and download counts:
    http://packman.links2linux.org/package/obs-studio
* Build information:
    https://pmbs.links2linux.de/package/show/Multimedia/obs-studio

## Gentoo ([Unofficial](#about-unofficial-builds))
Command-line: can be installed using portage by the following command:

```bash
sudo emerge media-video/obs-studio
```

See https://packages.gentoo.org/packages/media-video/obs-studio for available versions and more information.

## NixOS ([Unofficial](#about-unofficial-builds))
Command-line: can be installed by the following command:

```bash
nix-env -i obs-studio
```

See https://nixos.org/wiki/OBS for further instructions

***

## UOS/Deepin ([Unofficial](#about-unofficial-builds))
UOS/Deepin 20 or newer is required.

* First, make sure apt has access to the latest versions of packages.

   ```bash
   sudo apt update
   ```

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following command (or compile it yourself):

   ```bash
   sudo apt install ffmpeg
   ```

* Finally, install OBS Studio.

   ```bash
   sudo apt-get install com.obsproject
   ```
* if UOS not using this command, using AppStore search OBS Studio
* or with [Spark Store](https://www.spark-app.store/download.html):

   ```bash
   sudo apt install com.obsproject.studio
   ```

## Debian/LMDE ([Unofficial](#about-unofficial-builds))
Debian 9.0 or newer is required.  
**Please note that OBS Studio does not work on Chrome OS.**

**Also note that as of 2021-06-13, this package is the outdated 0.0.1 version. It should still work, but won't have all new improvements. Build from source to get the newest version.**

* First, make sure apt has access to the latest versions of packages.

   ```bash
   sudo apt update
   ```

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following command (or compile it yourself):

   ```bash
   sudo apt install ffmpeg
   ```

* Finally, install OBS Studio.

   ```bash
   sudo apt install obs-studio
   ```

## Void ([Unofficial](#about-unofficial-builds))
* First make sure your repositories are up-to-date. OBS is available on the `multilib` repos if you need the 32-bit build.

   ```bash
   sudo xbps-install -S
   ```

* Then install OBS Studio. Any missing dependencies will be installed automatically.
  * If it refuses to install, try running `sudo xbps-install -Su` to update everything first.

   ```bash
   sudo xbps-install obs
   ```

## snappy ([Unofficial](#about-unofficial-builds))

* If you haven't already, [install snapd](https://docs.snapcraft.io/core/install) (ignore the Support Overview which is outdated).

* Install OBS Studio.

   ```bash
   sudo snap install obs-studio
   ```

# FreeBSD ([Unofficial](#about-unofficial-builds))

* Install OBS Studio:

   ```bash
   pkg install obs-studio
   ```