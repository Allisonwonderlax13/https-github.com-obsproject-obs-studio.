If you want to develop for OBS, please visit #obs-dev on Quakenet and get to know the devs or have questions answered!

Also, if there is something in this guide you want to change/improve on, it is recommended that you talk about it with the devs in IRC first.

***

### Table of Contents:
* [Windows](#windows)
* [Mac OSX](#mac-osx)
* [Linux](#linux)
    * [Ubuntu installation (14.04 and following)](#ubuntu-installation)
    * [Arch Linux installation, unofficial](#arch-linux-installation-unofficial)
    * [Manjaro installation, unofficial](#manjaro-installation-unofficial)
    * [Fedora 22+ installation, unofficial](#fedora-installation-unofficial)
    * [openSUSE installation, unofficial](#opensuse-installation-unofficial)
    * [Gentoo installation, unofficial](#gentoo-installation-unofficial)
    * [NixOS installation](#nixos-installation)
    * [Manually compiling on Red Hat based distros such as Fedora](#manually-compiling-on-red-hat-based-distros-such-as-fedora)
    * [Manually compiling on Debian-based distros](#manually-compiling-on-debian-based-distros)
    * [Manually compiling on openSUSE](#manually-compiling-on-opensuse)
    * [Linux portable mode](#linux-portable-mode)

***

###Windows:
* **Requirements for building OBS on windows**
  * Development packages of `FFmpeg`, `x264`, and `cURL`.  
    * Pre-built windows dependencies for VS2013 and VS2015 can be found here:  http://code.fosshub.com/OBS/downloads
  * [Qt5](http://www.qt.io/)
  * Windows version of [cmake](http://www.cmake.org/)
  * Visual Studio 2013 (Latest update) or Visual Studio 2015

* **Installation Procedure**
  * Clone the repository and submodules:

         `git clone --recursive https://github.com/jp9000/obs-studio.git`

  * Create one or more of the following subdirectories within the cloned repository for building: `release`, `debug`, and `build` (suffixed with or without 32/64 to specify architecture). They are excluded from the repo in .gitignore for the sake of building, so they are safe to create an use within the repository base directory.

  * You can set the following Variables in cmake-gui or set them as Windows Environment Variables (optionally suffixed with '32' or '64' to specify architecture):
    * **Required**
      * `DepsPath` (Path to the include for all dependencies, not including Qt.)
	  * `QTDIR` (Path to Qt build base directory. GUI is built by default. Set the cmake boolean variable DISABLE_UI to TRUE if you don't want the GUI and this is no longer required.)
	    * NOTE: An example Qt directory you would use here if you installed Qt5 to D:\Qt would usually look something like this:
          * `(32bit) D:\Qt\5.3\msvc2013`
          * `(64bit) D:\Qt\5.3\msvc2013_64`
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
Visual Studio 12 2013 or Visual Studio 14 2015 or their 64bit equivalents if you want to build the 64bit version of OBS
  
  * If you did not set up Environment Variables earlier you can now configure the DepsPath and if necessary the x264, ffmpeg and curl path in the cmake-gui.

  * Enable the COPY_DEPENDENCIES option, then press 'Configure' again. 
  
  * Press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.

  * Open obs-studio.sln from the 'build' subdirectory in Visual Studio, and it should run and be good to go.  All required dependencies should be copied on compile and it should be a fully fuctional build environment.  The output is built in the 'rundir/[build type]' directory of your 'build' subdirectory.

***

###Mac OSX
* Clone the repository and submodules:

         git clone --recursive https://github.com/jp9000/obs-studio.git

* Use macports or homebrew and install FFmpeg, x264, Qt5, and cmake.

* NOTE: Qt5 can also be downloaded/installed via the Qt website, though keep in mind that you will have to set the QTDIR environment variable to the Qt5 build base directory.

* Make sure to have the OSX 10.9 or newer SDK installed (comes with recent versions of Xcode)

* In a terminal, go to the obs-studio directory create a 'build' sub directory and change to it, then to build, type:

    on OSX 10.9 or newer:

        cmake .. && make

    on OSX 10.8:

        MACOSX_DEPLOYMENT_TARGET=10.8 cmake -DCMAKE_OSX_SYSROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/ .. && make

* It builds in a modular structure by default.  To run it via terminal, go to build/rundir/RelWithDebInfo/bin, then type ./obs to run.

* NOTE: If you are running via command prompt, you *must* be in the 'bin' directory specified above, otherwise it will not be able to find its files relative to the binary.

* To create an app bundle instead, use the command: `make package`. This will create a .dmg file with an app bundle inside.

***

###Linux

**NOTE:** OpenGL 3.2 or later is required to use OBS Multiplatform on Linux. You can check what version of OpenGL is supported by your system by typing the following into the terminal:
* glxinfo | grep "OpenGL"

***

### Ubuntu installation
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following commands:

    **For Ubuntu 14.04 LTS or 14.10**, FFmpeg is not officially included so you will need a specific PPA:

        sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
        sudo apt-get update && sudo apt-get install ffmpeg

    **For Ubuntu 15.04 and following versions**, FFmpeg is officially included:

        sudo apt-get install ffmpeg

* Then you can install OBS with the following commands:

        sudo add-apt-repository ppa:obsproject/obs-studio
        sudo apt-get update && sudo apt-get install obs-studio

***

### Arch Linux installation, unofficial
* "Release" version is available on community repository:

        sudo pacman -S obs-studio

* "Git" version is available on AUR:  https://aur.archlinux.org/packages/obs-studio-git


***

### Manjaro installation, unofficial
* Graphical: search "obs-studio" on Pamac Manager or Octopi
* Command-line: install it via pacman with the following command:

        sudo pacman -S obs-studio

***

### Fedora installation, unofficial
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it from the rpmfusion repos with the following commands (replace the XX with your Fedora version number, e.g. 23):

        sudo rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-XX.noarch.rpm


* Then you can install OBS with the following commands (This pulls all dependencies, including ffmpeg):

        sudo rpm --import http://repo.tech-3.net/Fedora/TECH3-GPG-KEY.public
        sudo wget -O /etc/yum.repos.d/tech-3.repo http://repo.tech-3.net/Fedora/tech-3.repo
        sudo dnf update && sudo dnf install obs-studio

* To do updates, you may need to do the following:

        sudo dnf clean all && sudo dnf update -y

* For Hardware Acceleration support, choose an FFmpeg build that has NVidia nvenc or Intel QSync enabled in the build options
* FFmpeg with hardware acceleration: http://negativo17.org/handbrake-makemkv-ffmpeg-and-skype-available-for-centosrhel-7/

        sudo dnf config-manager --add-repo=http://negativo17.org/repos/fedora-handbrake.repo
        sudo dnf install ffmpeg --setopt=install_weak_deps=True
        ffmpeg -codecs | grep nvenc

        sudo rpm --import http://repo.tech-3.net/Fedora/TECH3-GPG-KEY.public
        sudo dnf config-manager --add-repo http://repo.tech-3.net/Fedora/tech-3.repo
        sudo dnf clean all && sudo dnf update -y
        sudo dnf install obs-studio
***

### openSUSE installation, unofficial
  - The Packman repository contains the obs-studio package since it requires
    the fuller version of FFmpeg which is in Packman for legal reasons. If you
    do not already have the Packman repository add it as shown below.

    For openSUSE Tumbleweed:

        sudo zypper ar --refresh --priority 90 http://packman.inode.at/suse/openSUSE_Tumbleweed packman

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

### Gentoo installation, unofficial
* Link:  https://github.com/saintdev/obs-studio-overlay

***

### NixOS installation

    nix-env -i obs-studio

See https://nixos.org/wiki/OBS for further instructions
***

### Manually compiling on Red Hat based distros such as Fedora
* Get RPM fusion at http://rpmfusion.org/Configuration/

* Set up a build environment (substitute yum with dnf on Fedora 22 and up):

        sudo yum install gcc gcc-c++ gcc-objc cmake git

* If you don't care much about FFmpeg features, just get it from RPM fusion:

        sudo yum install ffmpeg-devel

* Get the required packages:

        sudo yum install libX11-devel mesa-libGL-devel libv4l-devel \
                pulseaudio-libs-devel x264-devel freetype-devel \
                fontconfig-devel libXcomposite-devel libXinerama-devel \
                qt5-qtbase-devel qt5-qtx11extras-devel libcurl-devel \
                systemd-devel

* Building and installing OBS:

        git clone https://github.com/jp9000/obs-studio.git
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

### Manually compiling on Debian-based distros
* Set up a build environment:

        sudo apt-get install build-essential pkg-config cmake git checkinstall

* Get the required packages:

        sudo apt-get install libx11-dev libgl1-mesa-dev libpulse-dev libxcomposite-dev \
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

        git clone https://github.com/jp9000/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo checkinstall --pkgname=obs-studio --fstrans=no --backup=no \
               --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes

***

### Manually compiling on openSUSE
  - See openSUSE installation instructions (above) for details on adding Packman repository.

  - Install build dependencies:

        sudo zypper in cmake \
          fontconfig-devel \
          freetype2-devel \
          gcc \
          gcc-c++ \
          libcurl-devel \
          libffmpeg-devel \
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

        git clone https://github.com/jp9000/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo make install

***

### Linux portable mode
* You can also build in portable mode on Linux, which installs all the
    files to an isolated directory:

        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=0 \
                -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" ..
        make -j4 && make install

    After that you should have a portable install in ~/obs-studio-portable
    Change to bin/64bit or bin/32bit and then simply run:  ./obs