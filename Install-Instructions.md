If you want to develop for OBS, please visit #obs-dev on Quakenet and get to know the devs or have questions answered!

Also, if there is something in this guide you want to change/improve on, it is recommended that you talk about it with the devs in IRC first.

***

### Table of Contents:

* [Windows](#windows)
  * [Install](#windows-install-directions)
  * [Build from source](#windows-build-directions)
* [macOS](#macos)
  * [Install](#macos-install-directions)
  * [Build from source](#macos-build-directions)
* [Linux](#linux)
  * [Install Directions](#linux-install-directions)
  	* [Ubuntu (14.04+)](#ubuntu-installation)
  	* [Arch Linux (Unofficial)](#arch-linux-installation-unofficial)
  	* [Manjaro (Unofficial)](#manjaro-installation-unofficial)
  	* [Fedora (Unofficial)](#fedora-installation-unofficial)
  	* [openSUSE (Unofficial)](#opensuse-installation-unofficial)
  	* [Gentoo (Unofficial)](#gentoo-installation-unofficial)
  	* [NixOS (Unofficial)](#nixos-installation-unofficial)
  	* [Deepin (Unofficial)](#deepin-installation-unofficial)
        * [Debian Installation (Unofficial)](#debian-installation-unofficial)
  	* [snappy (Unofficial)](#snappy-installation-unofficial)
  * [Build Directions](#linux-build-directions)
    * [Red Hat/Fedora-based](#red-hatfedora-based-build-directions)
    * [Debian-based](#debian-based-build-directions)
    * [openSUSE](#opensuse-build-directions)
    * [Linux portable mode (all distros)](#linux-portable-mode-all-distros)

***

# Windows

### Windows Install Directions:
Pre-built windows versions can be found here: https://github.com/obsproject/obs-studio/releases/

The full .exe installer and .zip contains OBS Studio 32bit, 64bit, Browser Source, and Intel® RealSense™ plugin. You will be prompted during install for the Browser Source and RealSense plugin to be installed if using the .exe installer, otherwise the components are included in the .zip.

The small .exe installer contains the base OBS Studio 32bit, 64bit, Intel® RealSense™ plugin, but does not contain the Browser Source plugin.

NOTE: If using the .zip method for either the full or small install and installing to a non-standard program location (i.e. outside Program Files), you will need to add the security group ALL APPLICATION PACKAGES to have full control over the main OBS Studio directory and sub-directories. Certain features may not function properly without these security rights (primarily, the ability to use game capture on UWP apps).

***

### Windows Build Directions:
* **Requirements for building OBS on windows**
  * Development packages of `FFmpeg`, `x264`, and `cURL`.  
    * Pre-built windows dependencies for VS2013 and VS2015 can be found here:
      * VS2013: https://obsproject.com/downloads/dependencies2013.zip
      * VS2015: https://obsproject.com/downloads/dependencies2015.zip
      * VS2017: https://obsproject.com/downloads/dependencies2017.zip
  * [Qt5](http://www.qt.io/) (minimum version of 5.9 if you're using VS2017)
  * Windows version of [cmake](http://www.cmake.org/)
  * [Visual Studio 2013 (Latest update) or Visual Studio 2015 or Visual Studio 2017](https://www.visualstudio.com/free-developer-offers/)

* **Installation Procedure**
  * Clone the repository and **submodules**:

             git clone --recursive https://github.com/obsproject/obs-studio.git

  * If you do not know what submodules are, or you are not using git from the command line, **PLEASE make sure to fetch the submodules too**.

  * Create one or more of the following subdirectories within the cloned repository for building: `release`, `debug`, and `build` (suffixed with or without 32/64 to specify architecture). They are excluded from the repo in .gitignore for the sake of building, so they are safe to create an use within the repository base directory.

  * You can set the following Variables in cmake-gui or set them as Windows Environment Variables:
    * **Required**
      * `DepsPath` (Path to the include for all dependencies, not including Qt.).
        * An example path if you extracted the dependancies .zip to c:\obs-deps would be:
          * `c:\obs-deps\win32\include`
          * `c:\obs-deps\win64\include`
        * If you wish to specify both 32 and 64 bit dependencies (for multi-arch building), you can use `DepsPath32` and `DepsPath64` to their respective include folders.
      * `QTDIR` (Path to Qt build base directory. GUI is built by default. Set the cmake boolean variable DISABLE_UI to TRUE if you don't want the GUI and this is no longer required. Can be optionally suffixed with 32 or 64 to specify target arch).
        * **NOTE**: Make sure to download Qt 5.X.X prebuilt components for your version of MSVC (64 or 32 bit).
        * Example Qt directories you would use here if you installed Qt5 to D:\Qt would usually look something like this:
          * `(32bit) QTDIR=D:\Qt\5.8\msvc2013`
          * `(64bit) QTDIR64=D:\Qt\5.8\msvc2013_64`
    * **Optional** (If these share the same directory as DepsPath, they do not need to be individually specified.)
      * `FFmpegPath` (Path to just FFmpeg include directory.)  
      * `x264Path` (Path to just x264 include directory.)  
      * `curlPath` (Path to just cURL include directory.)

    * **NOTE**: Search paths and search order for base dependency library/binary files, relative to their include directories:

      Library files  
      * ../lib  
      * ../lib32 (if 32bit)  
      * ../lib64 (if 64bit)  
      * ./lib  
      * ./lib32 (if 32bit)  
      * ./lib64 (if 64bit)

      Binary files: 
      * ../bin  
      *  ../bin32 (if 32bit)  
      * ../bin64 (if 64bit)  
      * ./bin  
      * ./bin32 (if 32bit)  
      * ./bin64 (if 64bit)

  * Run cmake-gui.  
    * In "where is the source code", enter in the repo directory (example: D:/obs).  
    * In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: D:/obs/build).

  * Press 'Configure' and select the generator that fits to your installed VS Version:  
Visual Studio 12 2013, Visual Studio 14 2015, **or their 64bit equivalents** if you want to build the 64bit version of OBS
      * NOTE: If you need to change your dependencies from a build already configured, you will need to uncheck COPIED_DEPENDENCIES and run Configure again.
  
  * If you did not set up Environment Variables earlier you can now configure the DepsPath and if necessary the x264, ffmpeg and curl path in the cmake-gui.
  
  * Press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.

  * Open obs-studio.sln from the 'build' subdirectory in Visual Studio (or click the Open Project button from the cmake-gui in 3.7+), and it should run and be good to go.  All required dependencies should be copied on compile and it should be a fully functional build environment.  The output is built in the 'rundir/[build type]' directory of your 'build' subdirectory.

# macOS

### macOS Install Directions

Pre-built macOS versions can be found here: https://github.com/obsproject/obs-studio/releases

Simply run the installer and follow the on-screen directions to install OBS Studio.

Official macOS builds are available again as of 18.0.1.

***

### macOS Build Directions
* Clone the repository and **submodules**:

         git clone --recursive https://github.com/obsproject/obs-studio.git

* If you do not know what submodules are, or you are not using git from the command line, **PLEASE make sure to fetch the submodules too**.

* Use macports or homebrew and install FFmpeg, x264, Qt5, and cmake.

  * NOTE: Qt5 can also be downloaded/installed via the Qt website, though keep in mind that you will have to set the QTDIR environment variable to the Qt5 build base directory.
  * For example: `export QTDIR=/usr/local/opt/qt`

* Make sure to have the OSX 10.9 or newer SDK installed (comes with recent versions of Xcode)

* In a terminal, go to the obs-studio directory create a 'build' sub directory and change to it, then to build, type:

    on OSX 10.9 or newer:

        cmake .. && make

    on OSX 10.8:

        MACOSX_DEPLOYMENT_TARGET=10.8 cmake -DCMAKE_OSX_SYSROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/ .. && make

* It builds in a modular structure by default.  To run it via terminal, go to build/rundir/RelWithDebInfo/bin, then type ./obs to run.

* NOTE: If you are running via command prompt, you *must* be in the 'bin' directory specified above, otherwise it will not be able to find its files relative to the binary.

* To create an app bundle instead, use the command: `make package`. This will create a .dmg file with an app bundle inside.

# Linux

Any installation directions marked Unofficial are not maintained by the OBS Studio author and may not be up to date or stable.

**NOTE:** OpenGL 3.2 or later is required to use OBS Studio on Linux. You can check what version of OpenGL is supported by your system by typing the following into the terminal:
* glxinfo | grep "OpenGL"

## Linux Install Directions

### Ubuntu Installation
* xserver-xorg version 1.18.4 or newer is recommended to avoid potential performance issues with certain features in OBS, such as the fullscreen projector.

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following commands:

    **For Ubuntu 14.04 LTS**, FFmpeg is not officially included so you will need a specific PPA:

        sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
        sudo apt-get update && sudo apt-get install ffmpeg

    **For Ubuntu 15.04 and following versions**, FFmpeg is officially included:

        sudo apt-get install ffmpeg

* Then you can install OBS with the following commands, make sure you enabled the multiverse repo in Ubuntu's software center:

        sudo add-apt-repository ppa:obsproject/obs-studio
        sudo apt-get update
        sudo apt-get install obs-studio

***

### Arch Linux Installation (Unofficial)
* "Release" version is available on community repository:

        sudo pacman -S obs-studio

* "Git" version is available on AUR:  https://aur.archlinux.org/packages/obs-studio-git


***

### Manjaro Installation (Unofficial)
* Graphical: search "obs-studio" on Pamac Manager or Octopi
* Command-line: install it via pacman with the following command:

        sudo pacman -S obs-studio

***

### Fedora Installation (Unofficial)
* OBS Studio is included in RPM Fusion.  If you do not have it configured (if you're not sure, then you probably don't have it), you can do so with the following command:

        sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm


* Then you can install OBS with the following command (this pulls all dependencies, including NVENC-enabled ffmpeg):

        sudo dnf install obs-studio

* For NVIDIA Hardware accelerated encoding make sure you have CUDA installed (in case of an older card, install xorg-x11-drv-nvidia-340xx-cuda instead):

        sudo dnf install xorg-x11-drv-nvidia-cuda

***
### OpenMandriva Installation (Unofficial)
* OBS Studio is included in OpenMandriva Lx3 non-free repository and in restricted repository for upcoming Lx4 release - available now as Cooker.

    **For OpenMandriva Lx3:**

* Graphical: search and install "obs-studio" on "OpenMandriva Install and Remove Software" (Rpmdrake)
* Command-line: install it via terminal/konsole with the following command: "urpmi obs-studio"

    **For OpenMandriva Lx4:**

* Coming soon...

***
### openSUSE Installation (Unofficial)
  - The Packman repository contains the obs-studio package since it requires
    the fuller version of FFmpeg which is in Packman for legal reasons. If you
    do not already have the Packman repository add it as shown below.

    For openSUSE Tumbleweed:

        sudo zypper ar --refresh --priority 90 http://packman.inode.at/suse/openSUSE_Tumbleweed packman

    For openSUSE Leap 42.3:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_Leap_42.3 packman

    For openSUSE Leap 42.2:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_Leap_42.2 packman

    For openSUSE Leap 42.1:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_Leap_42.1 packman

    For openSUSE 13.2:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_13.2 packman

    It is recommended to set the priority for Packman lower so it takes
    precedence over base repositories (skip on Tumbleweed as included in initial command).

        sudo zypper mr --priority 90 packman

  - The Packman version of FFmpeg should be used for full codec support. To
    ensure any existing FFmpeg packages are switched to Packman versions
    execute the following before installing obs-studio.

        sudo zypper dup --repo packman

  - Install the obs-studio package.

        sudo zypper in obs-studio

  - Links:
    - 1 click install, direct rpm links, and download counts:
      http://packman.links2linux.org/package/obs-studio
    - Build information:
      https://pmbs.links2linux.de/package/show/Multimedia/obs-studio

***

### Gentoo Installation (Unofficial)
* Link:  https://github.com/saintdev/obs-studio-overlay

***

### NixOS Installation (Unofficial)

    nix-env -i obs-studio

See https://nixos.org/wiki/OBS for further instructions

***

### Deepin Installation (Unofficial)

Deepin 15.4 or newer is required.

* First make sure you have everything up-to-date.

        sudo apt-get update

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following command (or compile it yourself):

        sudo apt-get install ffmpeg

* Finally, install OBS Studio.

        sudo apt-get install obs-studio

***

### Debian Installation (Unofficial)

Debian 9.0 or newer is required.

* First make sure you have everything up-to-date.

        sudo apt update

* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following command (or compile it yourself):

        sudo apt install ffmpeg

* Finally, install OBS Studio.

        sudo apt install obs-studio

***

### snappy Installation (Unofficial)

* If you haven't already, [install snapd](https://docs.snapcraft.io/core/install) (ignore the Support Overview which is likely outdated).

* Install OBS Studio.

        sudo snap install obs-studio

***

## Linux Build Directions

### Red Hat/Fedora-based Build Directions
* Get RPM fusion at http://rpmfusion.org/Configuration/ ([Nux Desktop](http://li.nux.ro/repos.html) is an alternative that may include better packages for RHEL/CentOS 7 and newer Fedora)

* Set up a build environment:

        sudo dnf install gcc gcc-c++ gcc-objc cmake git

* Get the required packages:

        sudo yum install libX11-devel mesa-libGL-devel libv4l-devel \
                pulseaudio-libs-devel x264-devel freetype-devel \
                fontconfig-devel libXcomposite-devel libXinerama-devel \
                qt5-qtbase-devel qt5-qtx11extras-devel libcurl-devel \
                systemd-devel ffmpeg
  (Note: you might also need to install `ffmpeg-devel`)

* Building and installing OBS:

        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 ..
        make -j4
        sudo make install

* By default obs installs libraries in /usr/local/lib. To make sure that the loader can find them there, create a file /etc/ld.so.conf.d/local.conf with the single line  

        /usr/local/lib  

  and then run  

       sudo ldconfig

***

### Debian-based Build Directions
* Set up a build environment:

        sudo apt-get install build-essential pkg-config cmake git-core checkinstall

* Get the required packages:

        sudo apt-get install libx11-dev libgl1-mesa-dev libvlc-dev libpulse-dev libxcomposite-dev \
                libxinerama-dev libv4l-dev libudev-dev libfreetype6-dev \
                libfontconfig-dev qtbase5-dev libqt5x11extras5-dev libx264-dev \
                libxcb-xinerama0-dev libxcb-shm0-dev libjack-jackd2-dev libcurl4-openssl-dev

* FFmpeg is required, and not commonly available on debian-based distros. If you want a custom compilation with FDK AAC encoder and such, see:

        https://trac.ffmpeg.org/wiki/CompilationGuide

* Otherwise, I will only give easy and brief instructions for a very minimal FFmpeg installation (note that it does not require the inclusion of packages such as x264/etc, but you can include them if you wish):

        sudo apt-get install zlib1g-dev yasm
        git clone --depth 1 git://source.ffmpeg.org/ffmpeg.git
        cd ffmpeg
        ./configure --enable-shared --prefix=/usr
        make -j4
        sudo checkinstall --pkgname=FFmpeg --fstrans=no --backup=no \
                --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes

* Alternatively, Debian Jessie non-free, and Ubuntu 14.04 LTS multiverse have packages for FDK AAC.  Add non-free (Debian) or multiverse (Ubuntu) to your /etc/apt/sources.list.  Tested on Debian Stretch:

        sudo apt-get install libavcodec-dev libavfilter-dev libavdevice-dev libfdk-aac-dev

* Building and installing OBS:

        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo checkinstall --pkgname=obs-studio --fstrans=no --backup=no \
               --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes

***

### openSUSE Build Directions
  - See openSUSE installation instructions (above) for details on adding Packman repository.

  - Install build dependencies:

        sudo zypper in cmake \
          fontconfig-devel \
          freetype2-devel \
          gcc \
          gcc-c++ \
          libcurl-devel \
          ffmpeg2-devel \
          libjansson-devel \
          libpulse-devel \
          libqt5-qtbase-devel \
          libqt5-qtx11extras-devel \
          libudev-devel \
          libv4l-devel \
          libXcomposite-devel \
          libXinerama-devel \
          libXrandr-devel

  - Building and installing OBS:

        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo make install

***

### Linux portable mode (all distros)
* You can build in portable mode on Linux, which installs all the files to an isolated directory:

        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=0 \
                -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" ..
        make -j4 && make install

  After that you should have a portable install in ~/obs-studio-portable. Change to bin/64bit or bin/32bit and then simply run: ./obs