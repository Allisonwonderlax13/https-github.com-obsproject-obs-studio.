**Note:** If you want to make a change to these instructions, it is recommended you come to #obs-dev on Quakenet if you have questions first.

***

###Windows:
* Load git submodules (required):

         git submodule update --init

* NOTE: OBS on windows currently requires VS2013 with the latest update, as obs-studio uses both C99 and C++11.  Express might not be supported at this time (though if not, then it'll be fixed it at some point).  You can always just get VS2013 Community Edition for free.

* Download (or build) development packages of FFmpeg, x264, Qt5, cURL.

* Download windows version of cmake from: http://www.cmake.org/

* The follow variables must be used to specify dependencies, these variables can either be specified via a windows environment variable, or through a cmake variable (optionally suffixed with '32' or '64' to specify architecture):
   * DepsPath      (path to the include for all dependencies, not including Qt)  
   * FFMpegPath    (path to just FFmpeg include directory)  
   * x264Path      (path to just x264 include directory)  
   * curlPath      (path to just cURL include directory)

* NOTE: Search paths and search order for base dependency library/binary files, relative to their include directories:

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

* If you're building the GUI, the following cmake or windows environment variable must be used separately from the other dependencies to specify Qt5's location.  Like the other environment variables, append 32 or 64 if you wish to build both architectures:
> QTDIR         (path to Qt build base directory)

* NOTE: The GUI builds by default.  If you don't want to build the GUI, add a cmake boolean variable DISABLE_UI, set to true.

* NOTE: An example Qt directory you would use here if you installed Qt5 to D:\Qt would usually look something like this:
> (32bit) D:\Qt\5.3\msvc2013  
> (64bit) D:\Qt\5.3\msvc2013_64

* Run cmake-gui.  In "where is the source code", enter in the repo directory (example: D:/obs).  In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: D:/obs/build).

* NOTE: You should create one or more of the following subdirectories within the repository for building: release, debug, and build (suffixed with or without 32/64 to specify architecture).  They are excluded from the repo in .gitignore for the sake of building, so they are safe to create an use within the repository base directory.

* Press 'Configure', then enable the COPY_DEPENDENCIES option, then press 'Configure' again, and then press 'Generate' to generate visual studio project files in the 'build' subdirectory.

* Open obs-studio.sln from the 'build' subdirectory, and it should run and be good to go.  All required dependencies should be copied on compile and it should be a fully fuctional build environment.

***

###Mac OSX
* Load git submodules (required):

         git submodule update --init

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

**Ubuntu 14.04/14.10/15.04 installation**
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it with the following commands:

    For Ubuntu 14.04/14.10:

        sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
        sudo apt-get update && sudo apt-get install ffmpeg

    For Ubuntu 15.04:

        sudo apt-get install ffmpeg

* Then you can install OBS with the following commands:

        sudo add-apt-repository ppa:obsproject/obs-studio
        sudo apt-get update && sudo apt-get install obs-studio


**Arch Linux installation, unofficial**
* Link for the "Git" version:  https://aur.archlinux.org/packages/obs-studio-git
* Link for the "release" version:  https://aur.archlinux.org/packages/obs-studio


**Fedora 22 installation**
* FFmpeg is required.  If you do not have the FFmpeg installed (if you're not sure, then you probably don't have it), you can get it from the rpmfusion repos with the following commands:

        sudo rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-22.noarch.rpm

* Then you can install OBS with the following commands (This pulls all dependencies, including ffmpeg):

        sudo sudo rpm --import http://repo.tech-3.net/Fedora/TECH3-GPG-KEY.public
        sudo wget -O /etc/yum.repos.d/tech-3.repo http://repo.tech-3.net/Fedora/tech-3.repo
        sudo dnf update && sudo dnf install obs-studio


**openSUSE installation, unofficial**
  - The Packman repository contains the obs-studio package since it requires
    the fuller version of FFmpeg which is in Packman for legal reasons. If you
    do not already have the Packman repository add it as shown below.

    For openSUSE Tumbleweed:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_Tumbleweed packman

    For openSUSE 13.2:

        sudo zypper ar --refresh http://packman.inode.at/suse/openSUSE_13.2 packman

    It is recommended to set the priority for Packman lower so it takes
    precedence over base repositories.

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


**Gentoo installation, unofficial**
* Link:  https://github.com/saintdev/obs-studio-overlay


**NixOS installation**
* Get the latest Nix packages:

        git clone https://github.com/nixos/nixpkgs ~/nixpkgs

* Install:

        nix-env -f ~/nixpkgs -Q -i obs-studio


**Manually compiling on Redhat-based distros such as Fedora**
* Get RPM fusion at http://rpmfusion.org/Configuration/

* Set up a build environment (substitute yum with dnf on Fedora 22 and up):

        sudo yum install gcc gcc-c++ gcc-objc cmake git

* If you don't care much about FFmpeg features, just get it from RPM fusion:

        sudo yum install ffmpeg-devel

* Get the required packages:

        sudo yum install libX11-devel libGL-devel libv4l-devel \
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


**Manually compiling on Debian-based distros**
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

* Building and installing OBS:

        git clone https://github.com/jp9000/obs-studio.git
        cd obs-studio
        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr ..
        make -j4
        sudo checkinstall --pkgname=obs-studio --fstrans=no --backup=no \
               --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes


**Manually compiling on openSUSE**
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


**Linux portable mode**
* You can also build in portable mode on Linux, which installs all the
    files to an isolated directory:

        mkdir build && cd build
        cmake -DUNIX_STRUCTURE=0 \
                -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" ..
        make -j4 && make install

    After that you should have a portable install in ~/obs-studio-portable
    Change to bin/64bit or bin/32bit and then simply run:  ./obs