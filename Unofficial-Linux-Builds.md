## Table of Contents

* [Arch Linux/Manjaro](#arch-linuxmanjaro)
* [Debian](#debian)
* [Fedora](#fedora)
* [Gentoo](#gentoo)
* [NixOS](#nixos)
* [OpenMandriva](#openmandriva)
* [OpenSUSE](#opensuse)
* [snappy](#snappy)
* [Solus](#solus)
* [UOS/Deepin](#uosdeepin)
* [Void](#void)

***

# About unofficial builds

Please note that the instructions for the following Linux distributions means that they are community provided, and any support for those packages should be directed at the appropriate distro/package maintainers.

For official builds of OBS Studio for Flatpak (all distributions) and Ubuntu, see the [Install Instructions](install-instructions#linux).

# Unofficial builds

## Arch Linux/Manjaro

### Install from AUR

There is OBS packages present in the [AUR](https://wiki.archlinux.org/title/Arch_User_Repository).

* Use your favorite AUR helper or Pamac (Add/Remove Software) with AUR enabled in the settings.
  * `obs-studio-tytan652` provides OBS Studio with VST and browser modules with the same CEF as official builds with some quality of life patches and fixes.
  * `obs-studio-browser` provides OBS Studio with VST and browser modules with the same CEF as official builds.
  * `obs-studio-rc` provides OBS Studio latest version (including Release Candidates) with VST and browser modules with the same CEF as official builds.

Most of those packages also replace `vlc` with `vlc-luajit` to provide VLC sources.

* More [AUR](https://aur.archlinux.org/packages/?O=0&K=obs-studio-) packages are also available.

### Install from Arch Linux/Manjaro official repository

* Graphical: search obs-studio package on Pamac (Add/Remove Software)
* Command Line:
  ```bash
  sudo pacman -S obs-studio
  ```

## Debian

Debian 9.0 "Stretch" or newer is required.  

**Also note that as of 2021-06-13, this package is the outdated 0.0.1 version. It should still work, but won't have all new improvements. Build from source to get the newest version.**

* First, make sure you have everything up-to-date.

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

## Gentoo

Command-line: can be installed using portage by the following command:

```bash
sudo emerge media-video/obs-studio
```

See https://packages.gentoo.org/packages/media-video/obs-studio for available versions and more information.

## NixOS

Command-line: can be installed by the following command:

    ```bash
    nix-env -i obs-studio
    ```

## OpenMandriva

* OBS Studio is included in OpenMandriva Lx3 non-free repository and in restricted repository for upcoming Lx4 release - available now as Cooker.

### OpenMandriva Lx3

* Graphical: search and install "obs-studio" on "OpenMandriva Install and Remove Software" (Rpmdrake)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   urpmi obs-studio
   ```

### OpenMandriva Lx4
* Graphical: search and install "obs-studio" on "OpenMandriva Software Management" (dnfdragora)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   dnf install obs-studio
   ```

## openSUSE
* The Packman repository contains the obs-studio package since it requires
  the fuller version of FFmpeg which is in Packman for legal reasons. If you
  do not already have the Packman repository add it as shown below.
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

## snappy

* If you haven't already, [install snapd](https://docs.snapcraft.io/core/install) (ignore the Support Overview which is outdated).

* Install OBS Studio.

   ```bash
   sudo snap install obs-studio
   ```

## Solus

* Graphical: search `obs-studio` in Software Center
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:
   ```bash
   eopkg install obs-studio
   ```

See https://nixos.org/wiki/OBS for further instructions

## UOS/Deepin

UOS/Deepin 20 or newer is required.

* First, make sure you have everything up-to-date.

   ```bash
   sudo apt-get update
   ```

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following command (or compile it yourself):

   ```bash
   sudo apt-get install ffmpeg
   ```

* Finally, install OBS Studio.

   ```bash
   sudo apt-get install obs-studio
   ```

* or with [Spark Store](https://www.spark-app.store/download.html):

   ```bash
   sudo apt install com.obsproject.studio
   ```

## Void

* First make sure your repositories are up-to-date. OBS is available on the `multilib` repos if you need the 32-bit build.

   ```bash
   sudo xbps-install -S
   ```

* Then install OBS Studio. Any missing dependencies will be installed automatically.
  * If it refuses to install, try running `sudo xbps-install -Su` to update everything first.

   ```bash
   sudo xbps-install obs
   ```