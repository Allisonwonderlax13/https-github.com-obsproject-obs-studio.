If you want to develop for OBS, please visit our [Discord](https://obsproject.com/discord) and get to know the devs or have questions answered!

Also, if there is something in this guide you want to change/improve on, it is recommended that you talk about it with the devs in Discord or IRC first.

Please note that any install directions/packages for Linux/FreeBSD distributions listed as *Unofficial* means that they are community provided, and any support for those packages should be directed at the appropriate distro/package maintainers.

***

### Table of Contents:

  * [Windows](#windows)
    * [Install](#windows-install-directions)
    * [Build from source](#windows-build-directions)
  * [macOS](#macos)
    * [Install](#macos-install-directions)
    * [Build from source](#macos-build-directions)
  * [Linux](#linux)
    * [Install](#linux-install-directions)
      * [Ubuntu](#ubuntu-installation)
      * [Arch Linux (Unofficial)](#arch-linux-installation-unofficial)
      * [Manjaro (Unofficial)](#manjaro-installation-unofficial)
      * [Fedora (Unofficial)](#fedora-installation-unofficial)
      * [OpenMandriva Installation (Unofficial)](#openmandriva-installation-unofficial)
      * [openSUSE (Unofficial)](#opensuse-installation-unofficial)
      * [Gentoo (Unofficial)](#gentoo-installation-unofficial)
      * [NixOS (Unofficial)](#nixos-installation-unofficial)
      * [Deepin (Unofficial)](#deepin-installation-unofficial)
      * [Debian Installation (Unofficial)](#debian-installation-unofficial)
      * [Void Installation (Unofficial)](#void-installation-unofficial)
      * [snappy (Unofficial)](#snappy-installation-unofficial)
    * [Build from source](#linux-build-directions)
      * [Red Hat/Fedora-based](#red-hatfedora-based-build-directions)
      * [Debian-based](#debian-based-build-directions)
      * [openSUSE](#opensuse-build-directions)
      * [Linux portable mode (all distros)](#linux-portable-mode-all-distros)
  * [FreeBSD](#freebsd)
    * [Install](#freebsd-install-directions)
    * [Build from source](#freebsd-build-directions)

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
  * Development packages of `FFmpeg`, `x264`, `cURL`, and `mbedTLS`.
    * Pre-built windows dependencies for VS2017 can be found here:
      * VS2017: https://obsproject.com/downloads/dependencies2017.zip
  * [Qt5](http://www.qt.io/) (Grab the MSVC package for your version of Visual Studio)
    * We currently deploy with 5.10.1, which is no longer available from Qt. A download is provided [here](https://cdn-fastly.obsproject.com/downloads/Qt_5.10.1.7z).
  * Windows version of [cmake](http://www.cmake.org/)
  * [Visual Studio 2017 (latest updates recommended)](https://visualstudio.microsoft.com/vs/older-downloads/)
    * Windows 10 SDK (minimum version of 10.0.18362.0)

* **Installation Procedure**
  * Clone the repository and **submodules**:

             git clone --recursive https://github.com/obsproject/obs-studio.git

  * If you do not know what submodules are, or you are not using git from the command line, **PLEASE make sure to fetch the submodules too**.

  * Create one or more of the following subdirectories within the cloned repository for building: `release`, `debug`, and `build` (suffixed with or without 32/64 to specify architecture). They are excluded from the repo in .gitignore for the sake of building, so they are safe to create an use within the repository base directory.

  * Run cmake-gui, and set the following fields:
    * In "where is the source code", enter in the repo directory (example: D:/obs).
    * In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: D:/obs/build).

  * Set the following variables in cmake-gui (alternatively, you can set them as Windows Environment Variables):
    * **Required**
      * `DepsPath` (Path to the include for all dependencies, not including Qt.).
        * An example path if you extracted the dependancies .zip to c:\obs-deps would be:
          * `c:\obs-deps\win32`
          * `c:\obs-deps\win64`
        * If you wish to specify both 32 and 64 bit dependencies (for multi-arch building), you can use `DepsPath32` and `DepsPath64` to their respective include folders.
      * `QTDIR` (Path to Qt build base directory. GUI is built by default. Set the cmake boolean variable DISABLE_UI to TRUE if you don't want the GUI and this is no longer required. Can be optionally suffixed with 32 or 64 to specify target arch).
        * **NOTE**: Make sure to download Qt 5.X.X prebuilt components for your version of MSVC (64 or 32 bit).
        * Example Qt directories you would use here if you installed Qt5 to D:\Qt would usually look something like this:
          * `(32bit) QTDIR=D:\Qt\5.10.1\msvc2017`
          * `(64bit) QTDIR64=D:\Qt\5.10.1\msvc2017_64`
    * **Optional** (If these share the same directory as DepsPath, they do not need to be individually specified.)
      * `FFmpegPath` (Path to just FFmpeg include directory.)
      * `x264Path` (Path to just x264 include directory.)
      * `curlPath` (Path to just cURL include directory.)

    * **INFORMATIONAL NOTE**: Search paths and search order for base dependency library/binary files, relative to their include directories:

      Library files
      * ../lib
      * ../lib32 (if 32bit)
      * ../lib64 (if 64bit)
      * ./lib
      * ./lib32 (if 32bit)
      * ./lib64 (if 64bit)

      Binary files:
      * ../bin
      * ../bin32 (if 32bit)
      * ../bin64 (if 64bit)
      * ./bin
      * ./bin32 (if 32bit)
      * ./bin64 (if 64bit)

  * In cmake-gui, press 'Configure' and select the generator that fits to your installed VS Version:
Visual Studio 15 2017, **or their 64bit equivalents** if you want to build the 64bit version of OBS
      * NOTE: If you need to change your dependencies from a build already configured, you will need to uncheck COPIED_DEPENDENCIES and run Configure again.

  * If you did not set up Environment Variables earlier you can now configure the DepsPath and if necessary the x264, ffmpeg and curl path in the cmake-gui.

  * In cmake-gui, press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.

  * Open obs-studio.sln from the subdirectory you specified under "where to build the binaries" (e.g. D:/obs/build) in Visual Studio (or click the Open Project button from the cmake-gui in 3.7+).

  * The project should now be ready to build and run. All required dependencies should be copied on compile and it should be a fully functional build environment. The output is built in the 'rundir/[build type]' directory of your 'build' subdirectory.

* **Integrating clang-format into Visual Studio**
  * clang-format is required for pull requests, and OBS uses a newer version than the one VS2017 ships with.
  * Download and install [LLVM 8.0.0](http://releases.llvm.org/)
  * Run VS, and go to Tools -> Options...
    * Text Editor -> C/C++ -> Formatting -> General
      * Enable "Use custom clang-format.exe" and enter the file name. For example:
        * C:\Program Files\LLVM\bin\clang-format.exe
  * The default command for formatting a document (Edit.FormatDocument) is Ctrl+K, Ctrl+D.

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

* Use macports or homebrew and install FFmpeg, x264, Qt5, cmake, mbedtls and swig (if you want scripting).

  * NOTE: Qt5 can also be downloaded/installed via the Qt website, though keep in mind that you will have to set the QTDIR environment variable to the Qt5 build base directory.
    * For example: `export QTDIR=/usr/local/opt/qt`
  * NOTE: If you need SRT support, you might need to install FFmpeg from a custom tap instead of the default homebrew FFmpeg, like so:

        brew tap homebrew-ffmpeg/ffmpeg
        brew install homebrew-ffmpeg/ffmpeg/ffmpeg --with-srt


* Make sure to have the OSX 10.9 or newer SDK installed (comes with recent versions of Xcode)

* In a terminal, go to the obs-studio directory create a 'build' sub directory and change to it, then to build, type:

    on OSX 10.9 or newer:

        cmake .. && make

    on OSX 10.8:

        MACOSX_DEPLOYMENT_TARGET=10.8 cmake -DCMAKE_OSX_SYSROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/ .. && make

* It builds in a modular structure by default.  To run it via terminal, go to `build/rundir/RelWithDebInfo/bin`, then run using command `./obs`

* NOTE: If you are running via command prompt, you *must* be in the 'bin' directory specified above, otherwise it will not be able to find its files relative to the binary.

* Currently app bundles are created on CI with the before deploy script, may require tweaking to work locally. https://github.com/obsproject/obs-studio/blob/master/CI/before-deploy-osx.sh#L20
  * We are currently working to improve this process

# Linux

Any installation directions marked Unofficial are not maintained by the OBS Studio author and may not be up to date or stable.

**NOTE:** OpenGL 3.3 or later is required to use OBS Studio on Linux. You can check what version of OpenGL is supported by your system by typing the following into the terminal:
* glxinfo | grep "OpenGL"

## Linux Install Directions

### Ubuntu Installation
* xserver-xorg version 1.18.4 or newer is recommended to avoid potential performance issues with certain features in OBS, such as the fullscreen projector.
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following commands:

        sudo apt install ffmpeg

* Then you can install OBS with the following commands, make sure you enabled the multiverse repo in Ubuntu's software center (NOTE: On newer versions of ubuntu adding a repository automatically apt updates.):

        sudo add-apt-repository ppa:obsproject/obs-studio
        sudo apt update
        sudo apt install obs-studio

***

### Arch Linux Installation (Unofficial)
* "Release" version is available on community repository:

        sudo pacman -S obs-studio

* "Git" version is available on AUR: https://aur.archlinux.org/packages/obs-studio-git

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

#### For OpenMandriva Lx3
* Graphical: search and install "obs-studio" on "OpenMandriva Install and Remove Software" (Rpmdrake)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

        urpmi obs-studio

#### For OpenMandriva Lx4
* Graphical: search and install "obs-studio" on "OpenMandriva Software Management" (dnfdragora)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

        dnf install obs-studio

***

### openSUSE Installation (Unofficial)
* The Packman repository contains the obs-studio package since it requires
  the fuller version of FFmpeg which is in Packman for legal reasons. If you
  do not already have the Packman repository add it as shown below.
  * For openSUSE Tumbleweed:

        sudo zypper ar --refresh --priority 90 http://packman.inode.at/suse/openSUSE_Tumbleweed packman

  * For openSUSE Leap 15.0:

        sudo zypper ar --refresh --priority 90 http://packman.inode.at/suse/openSUSE_Leap_15.0 packman

  * For openSUSE Leap 42.3:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_Leap_42.3 packman

* It is recommended to set the priority for Packman lower so it takes
  precedence over base repositories (skip on Tumbleweed as included in initial command).

        sudo zypper mr --priority 90 packman

* The Packman version of FFmpeg should be used for full codec support. To
  ensure any existing FFmpeg packages are switched to Packman versions
  execute the following before installing obs-studio.

        sudo zypper dup --repo packman

* Install the obs-studio package.

        sudo zypper in obs-studio

Links:
* 1 click install, direct rpm links, and download counts:
    http://packman.links2linux.org/package/obs-studio
* Build information:
    https://pmbs.links2linux.de/package/show/Multimedia/obs-studio

***

### Gentoo Installation (Unofficial)
Command-line: can be installed using portage by the following command:

        sudo emerge media-video/obs-studio

See https://packages.gentoo.org/packages/media-video/obs-studio for available versions and more information.

***

### NixOS Installation (Unofficial)
Command-line: can be installed by the following command:

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

### Void Installation (Unofficial)
* First make sure your repositories are up-to-date. OBS is available on the `multilib` repos if you need the 32-bit build.

        sudo xbps-install -S

* Then just install OBS Studio. Any missing dependencies will be installed automatically.
  * If it refuses to install, try running `sudo xbps-install -Su` to update everything first.

        sudo xbps-install obs

Though unofficial, the package is actively maintained and functional!

***

### snappy Installation (Unofficial)
* If you haven't already, [install snapd](https://docs.snapcraft.io/core/install) (ignore the Support Overview which is outdated).

* Install OBS Studio.

        sudo snap install obs-studio

***

## Linux Build Directions

Note: as of May 1, 2019, [Facebook live now mandates the use of RTMPS](https://developers.facebook.com/docs/graph-api/changelog/breaking-changes/#live-api-4-24). That functionality requires your distro's [mbed TLS](https://tls.mbed.org/) package, which [obs-studio/cmake/Modules/FindMbedTLS.cmake script](https://github.com/obsproject/obs-studio/blob/master/cmake/Modules/FindMbedTLS.cmake) searches for at compile time.

### Red Hat/Fedora-based Build Directions
* Get RPM fusion at http://rpmfusion.org/Configuration/ ([Nux Desktop](http://li.nux.ro/repos.html) is an alternative that may include better packages for RHEL/CentOS 7)
* Get the required packages:

        sudo yum install \
               make \
               gcc \
               gcc-c++ \
               gcc-objc \
               cmake \
               git \
               libX11-devel \
               mesa-libGL-devel \
               libv4l-devel \
               pulseaudio-libs-devel \
               libspeexdsp-devel \
               x264-devel \
               freetype-devel \
               fontconfig-devel \
               libXcomposite-devel \
               libXinerama-devel \
               qt5-qtbase-devel \
               qt5-qtx11extras-devel \
               qt5-qtsvg-devel \
               libcurl-devel \
               systemd-devel \
               ffmpeg \
               ffmpeg-devel \
               luajit-devel \
               python3-devel \
               mbedtls \
               mbedtls-devel \
               swig

  * If `libspeexdsp-devel` is not available, it can be built from source (https://git.xiph.org/?p=speexdsp.git;a=summary)
* Building and installing OBS:
  * If building with browser source:

        wget https://cdn-fastly.obsproject.com/downloads/cef_binary_3770_linux64.tar.bz2
        tar -xjf ./cef_binary_3770_linux64.tar.bz2
        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_3770_linux64" ..
        make -j4
        sudo make install

  * If building without browser source

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
* Get the required packages:

        sudo apt-get install \
                   build-essential \
                   checkinstall \
                   cmake \
                   git \
                   libmbedtls-dev \
                   libasound2-dev \
                   libavcodec-dev \
                   libavdevice-dev \
                   libavfilter-dev \
                   libavformat-dev \
                   libavutil-dev \
                   libcurl4-openssl-dev \
                   libfdk-aac-dev \
                   libfontconfig-dev \
                   libfreetype6-dev \
                   libgl1-mesa-dev \
                   libjack-jackd2-dev \
                   libjansson-dev \
                   libluajit-5.1-dev \
                   libpulse-dev \
                   libqt5x11extras5-dev \
                   libspeexdsp-dev \
                   libswresample-dev \
                   libswscale-dev \
                   libudev-dev \
                   libv4l-dev \
                   libvlc-dev \
                   libx11-dev \
                   libx264-dev \
                   libxcb-shm0-dev \
                   libxcb-xinerama0-dev \
                   libxcomposite-dev \
                   libxinerama-dev \
                   pkg-config \
                   python3-dev \
                   qtbase5-dev \
                   libqt5svg5-dev \
                   swig \
                   libxcb-randr0-dev \
                   libxcb-xfixes0-dev \
                   libx11-xcb-dev \
                   libxcb1-dev

* Building and installing OBS:
  * If building with browser source:

        wget https://cdn-fastly.obsproject.com/downloads/cef_binary_3770_linux64.tar.bz2
        tar -xjf ./cef_binary_3770_linux64.tar.bz2
        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_3770_linux64" ..
        make -j4
        sudo checkinstall --default --pkgname=obs-studio --fstrans=no --backup=no --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes

  * If building without browser source

        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo checkinstall --default --pkgname=obs-studio --fstrans=no --backup=no --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes

***

### openSUSE Build Directions
* See openSUSE installation instructions (above) for details on adding Packman repository.
* Install build dependencies:

        sudo zypper in cmake \
                     fontconfig-devel \
                     freetype2-devel \
                     gcc \
                     gcc-c++ \
                     libcurl-devel \
                     ffmpeg2-devel \
                     libjansson-devel \
                     libpulse-devel \
                     libspeexdsp-devel \
                     libqt5-qtbase-devel \
                     libqt5-qtx11extras-devel \
                     libudev-devel \
                     libv4l-devel \
                     libXcomposite-devel \
                     libXinerama-devel \
                     libXrandr-devel \
                     luajit-devel \
                     mbedtls \
                     swig \
                     python3-devel

* Building and installing OBS:
  * If building with browser source:

        wget https://cdn-fastly.obsproject.com/downloads/cef_binary_3770_linux64.tar.bz2
        tar -xjf ./cef_binary_3770_linux64.tar.bz2
        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_3770_linux64" ..
        make -j4
        sudo make install

  * If building without browser source

        git clone --recursive https://github.com/obsproject/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo make install

***

### Linux portable mode (all distros)
* Please note that you need to install the build dependencies for your repo before following this steps. See above.
* You can build in portable mode on Linux, which installs all the files to an isolated directory.
  * If building with browser source:

         wget https://cdn-fastly.obsproject.com/downloads/cef_binary_3770_linux64.tar.bz2
         tar -xjf ./cef_binary_3770_linux64.tar.bz2
         git clone --recursive https://github.com/obsproject/obs-studio.git
         cd obs-studio
         mkdir build && cd build
         cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_3770_linux64" ..
         make -j4 && make install

  * If building without browser source:

      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" ..
      make -j4 && make install

* After that you should have a portable install in ~/obs-studio-portable. Change to bin/64bit or bin/32bit and then simply run: ./obs

# FreeBSD

### FreeBSD Installation (Unofficial)

* Install OBS Studio:

        pkg install obs-studio

***

### FreeBSD Build Directions
* The easiest way to build OBS Studio from source is to use the [FreeBSD Ports](https://www.freebsd.org/doc/handbook/ports-using.html) and modify the `multimedia/obs-studio` port to suite your needs.
* First you have to set up the ports infrastructure on your system. See the related chapter in the [FreeBSD Handbook](https://www.freebsd.org/doc/handbook/ports-using.html).
* Once you've got your ports tree at `/usr/ports` you may edit the `multimedia/obs-studio` port to your liking. Then, you may build and install the port with:

      cd /usr/ports/multimedia/obs-studio
      make install clean
