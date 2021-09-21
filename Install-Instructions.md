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
    * [Xcode Project](#macos-xcode-project)
  * [Linux](#linux)
    * [Install](#linux-install-directions)
      * [Ubuntu/Mint](#ubuntumint-installation)
      * [Arch Linux (Unofficial)](#arch-linux-installation-unofficial)
      * [Manjaro (Unofficial)](#manjaro-installation-unofficial)
      * [Fedora (Unofficial)](#fedora-installation-unofficial)
      * [OpenMandriva Installation (Unofficial)](#openmandriva-installation-unofficial)
      * [Solus Installation (Unofficial)](#solus-installation-unofficial)
      * [openSUSE (Unofficial)](#opensuse-installation-unofficial)
      * [Gentoo (Unofficial)](#gentoo-installation-unofficial)
      * [NixOS (Unofficial)](#nixos-installation-unofficial)
      * [UOS/Deepin (Unofficial)](#uosdeepin-installation-unofficial)
      * [Debian/LMDE Installation (Unofficial)](#debianlmde-installation-unofficial)
      * [Void Installation (Unofficial)](#void-installation-unofficial)
      * [snappy (Unofficial)](#snappy-installation-unofficial)
    * [Build from source](#linux-build-directions)
      * [Red Hat-Based](#red-hat-based-build-directions)
      * [Fedora](#fedora-build-directions)
      * [Debian-based](#debian-based-build-directions)
      * [openSUSE](#opensuse-build-directions)
      * [Linux portable mode (all distros)](#linux-portable-mode-all-distros)
  * [FreeBSD](#freebsd)
    * [Install](#freebsd-install-directions)
    * [Build from source](#freebsd-build-directions)

***

# Windows

### Windows Install Directions:
Pre-built Windows versions can be found here: https://github.com/obsproject/obs-studio/releases/

The full .exe installer and .zip contains OBS Studio 32bit, 64bit, Browser Source, and Intel® RealSense™ plugin. You will be prompted during install for the Browser Source and RealSense plugin to be installed if using the .exe installer, otherwise the components are included in the .zip.

The small .exe installer contains the base OBS Studio 32bit, 64bit, Intel® RealSense™ plugin, but does not contain the Browser Source plugin.

NOTE: If using the .zip method for either the full or small install and installing to a non-standard program location (i.e. outside Program Files), you will need to add the security group ALL APPLICATION PACKAGES to have full control over the main OBS Studio directory and sub-directories. Certain features may not function properly without these security rights (primarily, the ability to use game capture on UWP apps).

***

### Windows Build Directions:
* **Requirements for building OBS on Windows**
  * Development packages of `FFmpeg`, `x264`, `cURL`, and `mbedTLS`.
    * Pre-built versions of these dependencies for VS2019 on Windows can be found here:
      * VS2019: https://obsproject.com/downloads/dependencies2019.zip
  * [Qt5](http://www.qt.io/) (Grab the MSVC package for your version of Visual Studio)
    * We currently deploy with Qt 5.15.2
      * [Qt 5.15.2 Windows](https://cdn-fastly.obsproject.com/downloads/Qt_5.15.2.7z)
  * CEF Wrapper 3770 ([x64](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows64_minimal.zip), [x86](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows32_minimal.zip))
  * Windows version of [CMake](http://www.cmake.org/) (3.16 or higher, latest preferred)
  * Windows version of [Git](https://git-scm.com/download/win) (Git binaries must exist in path)
  * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)
    * Windows 10 SDK (minimum 10.0.20348.0). [Latest SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/)

* **Installation Procedure**
  * Clone the repository and **submodules**:

    ```bash
    git clone --recursive https://github.com/obsproject/obs-studio.git
    ```

  * If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.

  * Create one or more of the following subdirectories within the cloned repository for building: `release`, `debug`, and `build` (suffixed with or without 32/64 to specify architecture). They are excluded from the repo in .gitignore for the sake of building, so they are safe to create and use within the repository base directory.

  * Run cmake-gui, and set the following fields:
    * In "where is the source code", enter in the repo directory (example: D:/obs).
    * In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: D:/obs/build).

  * Set the following variables. You can set them in cmake-gui, or you can set them as Windows Environment Variables to persist across configurations.
    * **Required**
      * `DepsPath`

        `DepsPath` is the path to the folder containing the dependencies, not including Qt. Set this to the win32 or win64 directory from the Pre-built dependencies package that you downloaded earlier.
        For example, if you extracted the dependencies .zip to C:\obs-deps, `DepsPath` should be one of these:
        * `C:\obs-deps\win32`
        * `C:\obs-deps\win64`

        If you want to specify both 32 and 64 bit dependencies to avoid changing the variable between builds, you can instead set `DepsPath32` and `DepsPath64` like so:
        * `DepsPath32`: `C:\obs-deps\win32`
        * `DepsPath64`: `C:\obs-deps\win64`

      * `QTDIR`

        `QTDIR` is the path to the Qt install directory.  The OBS UI is built by default, which requires Qt.  Set the CMake boolean variable DISABLE_UI to TRUE if you don't want the GUI and this is no longer required. Can be optionally suffixed with 32 or 64 to specify target arch.

        **NOTE**: Make sure to download Qt prebuilt components for your version of MSVC (32 or 64 bit).

        Example Qt directories you would use if you installed Qt to D:\Qt would usually look something like this:
          * `QTDIR32=D:\Qt\5.15.2\msvc2019` (32-bit)
          * `QTDIR64=D:\Qt\5.15.2\msvc2019_64` (64-bit)

      * `CEF_ROOT_DIR`

        `CEF_ROOT_DIR` is the path to an extracted CEF Wrapper. We provide a custom prebuilt wrapper to simplify the build process. This custom build includes access to hardware acceleration and additional codecs. **This enables Browser Source and Custom Browser Docks.**

        **Be sure to also enable the CMake flag `BUILD_BROWSER` otherwise this will do nothing**

    * **Optional**
      * `VIRTUALCAM_GUID` - Set to any random GUID value.  This must be set to build the Virtual Camera features.

      (If these components below share the same directory as DepsPath, they do not need to be individually specified.)
      * `FFmpegPath` - Path to just FFmpeg include directory.
      * `x264Path` - Path to just x264 include directory.
      * `curlPath` - Path to just cURL include directory.


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
Visual Studio 16 2019, **or their 64bit equivalents** if you want to build the 64bit version of OBS
      * NOTE: If you need to change your dependencies from a build already configured, you will need to uncheck COPIED_DEPENDENCIES and run Configure again.

  * If you did not set up Environment Variables earlier you can now configure the DepsPath and if necessary the x264, FFmpeg and cUrl path in the cmake-gui.

  * In cmake-gui, press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.

  * Open obs-studio.sln from the subdirectory you specified under "where to build the binaries" (e.g. D:/obs/build) in Visual Studio (or click the "Open Project" button from within cmake-gui).

  * The project should now be ready to build and run. All required dependencies should be copied on compile and it should be a fully functional build environment. The output is built in the 'rundir/[build type]' directory of your 'build' subdirectory.

  * The PACKAGE target uses Wix Installer and seems to be obsolete. Discussion on discord indicates that the current installer uses NSIS, but the process seems not to be fully documented.

  * If you want to use the Virtual Camera from this build, you will need to run its install script.  If you already have a standard OBS Studio installation, you will need to uninstall its Virtual Camera first.

    To uninstall an OBS Virtual Camera:
    1. Close any applications that were using the OBS Virtual Camera.
    2. In the obs-studio installation directory, run `data\obs-plugins\win-dshow\virtualcam-uninstall.bat` as administrator.
    
    To install an OBS Virtual Camera:
    1. In the obs-studio installation directory (for Visual Studio builds, this is '[build dir]/rundir/[build type]'), run `data\obs-plugins\win-dshow\virtualcam-install.bat` as administrator.

    Don't forget to uninstall your build's virtual camera before cleaning/deleting your build files.

* **Integrating clang-format into Visual Studio**
  * clang-format is required for pull requests, and our CI uses a specific version that may not match the one VS2019 ships with.
  * Download and install [LLVM 10.0.0](http://releases.llvm.org/).  LLVM 11+ currently introduces extra clang-format changes that we do not want to handle at this time.
  * Run VS, and go to Tools -> Options...
    * Text Editor -> C/C++ -> Code Style -> Formatting -> General
      * Enable "Use custom clang-format.exe" and enter the file name. For example:
        * C:\Program Files\LLVM\bin\clang-format.exe
  * The default command for formatting a document (Edit.FormatDocument) is Ctrl+K, Ctrl+D.

# macOS

### macOS Install Directions

Pre-built macOS versions can be found here: https://github.com/obsproject/obs-studio/releases

Simply run the installer and follow the on-screen directions to install OBS Studio.

***

### macOS Build Directions
* Clone the repository and **submodules**:

  ```bash
  git clone --recursive https://github.com/obsproject/obs-studio.git
  ```

* If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.

#### macOS Full Build Script

To get a self-built OBS up and running, a default build and packaging script is provided. This script only requires Homebrew (https://brew.sh) to be installed on the build system already:

* To build OBS as-is with full browser-source support, simply run `./CI/full-build-macos.sh` from the checkout directory (The script will take care of downloading all necessary dependencies).
* To *create an app-bundle* after building OBS, run the script with the `-b` flag: `./CI/full-build-macos.sh -b`
* To *create a disk image* after building OBS, run the script with the `-p` flag: `./CI/full-build-macos.sh -b -p`
* To *notarize* an app-bundle after building and bundling OBS, run the script with the `-n` flag: `./CI/full-build-macos.sh -b -n`
* To create an app-bundle *without building OBS again*, run the script with the `-s` flag: `./CI/full-build-macos.sh -s -b`

The last option is helpful if custom `cmake` flags have been used, but a proper app bundle is desired.

#### macOS Custom Builds

Custom build configurations require a set of dependencies to be installed on the build system. Some dependencies need to be installed via Homebrew (https://brew.sh):

* FFmpeg
* x264
* cmake
* freetype
* mbedtls
* swig
* Qt5

If you need SRT support, either use FFmpeg provided by `obs-deps` or install FFmpeg from a custom tap instead of the default homebrew FFmpeg:

```bash
brew tap homebrew-ffmpeg/ffmpeg
brew install homebrew-ffmpeg/ffmpeg/ffmpeg --with-srt
```

#### Pre-Built Dependencies

These dependencies are also available via `obs-deps` (https://github.com/obsproject/obs-deps) as pre-compiled binaries, which are assured to be compatible with current OBS code (as OBS is built against specific versions of some packages while Homebrew delivers most recent stable builds).

* **When using obs-deps**, extract both archives from the macOS release to `/tmp/obsdeps` to assure compatibility with app bundling later (due to the way `dylib`s are identified and linked).
* Create a build directory inside the `obs-studio` directory, change to it, then configure the project via `cmake`:

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DQTDIR="/tmp/obsdeps" -DSWIGDIR="/tmp/obsdeps" -DDepsPath="/tmp/obsdeps" -DDISABLE_PYTHON=ON ..
   ```

* Build OBS by running `make`.

#### Configuring and Building

* If not already handled by the Homebrew installation, install a current macOS platform SDK (only macOS High Sierra 10.13.4 or later is supported): `xcode-select --install`
* Create a build directory inside the `obs-studio` directory, change to it, then configure the project via `cmake`:

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DDISABLE_PYTHON=ON ..
   ```

* *NOTE*: `cmake` might require additional parameters to find `Qt5` libraries present on this system, this can either be provided via `-DQTDIR="/usr/local/opt/qt"` or setting an environment variable, e.g.: `export QTDIR=/usr/local/opt/qt`
* Build OBS by running `make`
* Run OBS from the `/rundir/RelWithDebInfo/bin` directory in your build directory, by running `./obs` from a Terminal
* *NOTE*: If you are running via command prompt, you *must* be in the 'bin' directory specified above, otherwise it will not be able to find its files relative to the binary.

*** 

### macOS Xcode Project

To create an Xcode project for OBS, `cmake` must be run with additional flags. Follow the build instructions above to create a working configuration setup, then add `-G Xcode` to the `cmake` command, e.g.:

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DDISABLE_PYTHON=ON -G Xcode ..
   ```

This will create an `obs-studio.xcodeproj` project file in the build directory as well as Xcode project files for all build dependencies. To build a full macOS build, the build target `ALL_BUILD` can be used, but must be configured first:

* Select `ALL_BUILD` from available build schemes in Xcode, then press `CMD+B` to build the project at least once
* Then select `Edit Scheme...` from the same menu.
* Under the `Info` tab, click on the dropdown for `Executable`, then click on `other`.
* Navigate to the `/rundir/debug/bin` bin folder that the previous Xcode build process should have created and select the `obs` binary found there.
* Next, switch to the `Options` tab and check the box to `Use custom working directory` and select the same `/rundir/debug/bin` directory in your Xcode build directory.

**NOTE:** When running OBS directly from Xcode be aware that browser sources will not be available (as CEF requires to be run as part of an application bundle in macOS) and accessing the webcam will lead to a crash (as macOS requires a permission prompt text set in an application bundle's `Info.plist` which is, of course, not available).

To debug OBS on macOS with all plugins and modules loaded, follow these steps:

* Build (but do not run) OBS with Xcode.
* Run `BUILD_DIR="YOUR_XCODE_BUILD_DIR" BUILD_CONFIG="Debug" ../CI/full-build-macos.sh -d -s -b` to bundle OBS build by Xcode, replace `YOUR_XCODE_BUILD_DIR` with the directory where you ran `cmake` to create the Xcode project.
* Next, create a new Xcode project, select `macOS` as platform and `Framework` as type.
* Give your project any arbitrary name and place it in any folder you like.
* With the new project open, click on your current build scheme in Xcode and select `Edit Scheme...`.
* For the `Run` step, go to the `Info` tab and select `Other...` in the dropdown for `Executable`.
* Browse to your OBS Xcode build directory and select the `OBS.app` application bundle created by the script.

You can now run OBS with Xcode directly attached as debugger. You can debug the visual stack as well as trace crashes and set breakpoints.

**NOTE:** Breakpoints set in the actual Xcode project do not carry over to this "helper" project and vice versa.

# Linux

Any installation directions marked Unofficial are not maintained by the OBS Studio author and may not provide all the features and/or be up to date or stable.

**NOTE:** OpenGL 3.3 or later is required to use OBS Studio on Linux. You can check what version of OpenGL is supported by your system by typing `glxinfo | grep "OpenGL"` on Terminal.

## Linux Install Directions

### Ubuntu/Mint Installation

**Please note that OBS Studio is not fully working on Chrome OS and features like Screen and Window Capture do not work.**  
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

***

### Arch Linux Installation (Unofficial)
* "Release" version is available on the community repository:
   ```bash
   sudo pacman -S obs-studio
   ```
* Some [AUR](https://aur.archlinux.org/packages/?O=0&K=obs-studio-) packages are also available.

***

### Manjaro Installation (Unofficial)
* Graphical: search "obs-studio" on Pamac Manager or Octopi
* Command-line: install it via pacman with the following command:
   ```bash
   sudo pacman -S obs-studio
   ```
* Some [AUR](https://aur.archlinux.org/packages/?O=0&K=obs-studio-) packages are also available.
***

### Fedora Installation (Unofficial)
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
***

### OpenMandriva Installation (Unofficial)
* OBS Studio is included in OpenMandriva Lx3 non-free repository and in restricted repository for upcoming Lx4 release - available now as Cooker.

#### For OpenMandriva Lx3
* Graphical: search and install "obs-studio" on "OpenMandriva Install and Remove Software" (Rpmdrake)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   urpmi obs-studio
   ```

#### For OpenMandriva Lx4
* Graphical: search and install "obs-studio" on "OpenMandriva Software Management" (dnfdragora)
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:

   ```bash
   dnf install obs-studio
   ```

***

### Solus Installation (Unofficial)
* Graphical: search `obs-studio` in Software Center
* Command-line: install it as root (su or sudo) via terminal/konsole with the following command:
   ```bash
   eopkg install obs-studio
    ```

***

### openSUSE Installation (Unofficial)
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

***

### Gentoo Installation (Unofficial)
Command-line: can be installed using portage by the following command:

```bash
sudo emerge media-video/obs-studio
```

See https://packages.gentoo.org/packages/media-video/obs-studio for available versions and more information.

***

### NixOS Installation (Unofficial)
Command-line: can be installed by the following command:

```bash
nix-env -i obs-studio
```

See https://nixos.org/wiki/OBS for further instructions

***

### UOS/Deepin Installation (Unofficial)
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

***

### Debian/LMDE Installation (Unofficial)
Debian 9.0 or newer is required.  
**Please note that OBS Studio is not fully working on Chrome OS and features like Screen and Window Capture do not work.**

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

***

### Void Installation (Unofficial)
* First make sure your repositories are up-to-date. OBS is available on the `multilib` repos if you need the 32-bit build.

   ```bash
   sudo xbps-install -S
   ```

* Then install OBS Studio. Any missing dependencies will be installed automatically.
  * If it refuses to install, try running `sudo xbps-install -Su` to update everything first.

   ```bash
   sudo xbps-install obs
   ```

***

### snappy Installation (Unofficial)
* If you haven't already, [install snapd](https://docs.snapcraft.io/core/install) (ignore the Support Overview which is outdated).

* Install OBS Studio.

   ```bash
   sudo snap install obs-studio
   ```

***

## Linux Build Directions

Note: as of May 1, 2019, [Facebook live now mandates the use of RTMPS](https://developers.facebook.com/docs/graph-api/changelog/breaking-changes/#live-api-4-24). That functionality requires your distro's [mbed TLS](https://tls.mbed.org/) package, which [obs-studio/cmake/Modules/FindMbedTLS.cmake script](https://github.com/obsproject/obs-studio/blob/master/cmake/Modules/FindMbedTLS.cmake) searches for at compile time.

Note: Do not use the GitHub source .tar as it does not include all the required source files. Always use the appropriate Git tag with the associated submodules.

### Red Hat-based Build Directions
* Get RPM Fusion at http://rpmfusion.org/Configuration/ ([Nux Desktop](http://li.nux.ro/repos.html) is an alternative that may include better packages for RHEL/CentOS 7)

* Get the required packages:
   ```bash
   sudo yum install \
          cmake \
          ffmpeg-devel \
          fontconfig-devel \
          freetype-devel \
          gcc \
          gcc-c++ \
          gcc-objc \
          git \
          glib2-devel \
          libcurl-devel \
          libglvnd-devel \
          libv4l-devel \
          libX11-devel \
          libXcomposite-devel \
          libXinerama-devel \
          luajit-devel \
          make \
          mbedtls-devel \
          pipewire-devel \
          pulseaudio-libs-devel \
          python3-devel \
          qt5-qtbase-devel \
          qt5-qtsvg-devel \
          qt5-qtwayland-devel \
          qt5-qtx11extras-devel \
          speexdsp-devel \
          swig \
          systemd-devel \
          wayland-devel \
          x264-devel
   ```

  * If `speexdsp-devel` is not available, it can be built from source (https://gitlab.xiph.org/xiph/speexdsp)

* Building and installing OBS:

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4280_linux64.tar.bz2
      tar -xjf ./cef_binary_4280_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4280_linux64" ..
      make -j4
      sudo make install
      ```

  * If building without browser source:

      ```bash
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=OFF ..
      make -j4
      sudo make install
      ```

* By default OBS installs libraries in /usr/local/lib. To make sure that the loader can find them there, create a file /etc/ld.so.conf.d/local.conf with the single line

   ```bash
   
   ```

  and then run

   ```bash
   sudo ldconfig
   ```
### Fedora Build Directions
   ```bash
   sudo dnf install \
          cmake \
          ffmpeg-devel \
          fontconfig-devel \
          freetype-devel \
          gcc \
          gcc-c++ \
          gcc-objc \
          git \
          glib2-devel \
          libcurl-devel \
          libglvnd-devel \
          libv4l-devel \
          libX11-devel \
          libXcomposite-devel \
          libXdamage \
          libXinerama-devel \
          luajit-devel \
          make \
          mbedtls-devel \
          pipewire-devel \
          pulseaudio-libs-devel \
          python3-devel \
          qt5-qtbase-devel \
          qt5-qtbase-private-devel \
          qt5-qtsvg-devel \
          qt5-qtwayland-devel \
          qt5-qtx11extras-devel \
          speexdsp-devel \
          swig \
          systemd-devel \
          wayland-devel \
          x264-devel
   ```

* Building and installing OBS:

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4280_linux64.tar.bz2
      tar -xjf ./cef_binary_4280_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4280_linux64" ..
      make -j$(nproc)
      sudo make install
      sudo echo "/usr/local/lib" >> /etc/ld.so.conf.d/local.conf
      sudo ldconfig
      ```

  * If building without browser source:

      ```bash
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=OFF ..
      make -j$(nproc)
      sudo make install
      sudo echo "/usr/local/lib" >> /etc/ld.so.conf.d/local.conf
      sudo ldconfig
      ```

***

### Debian-based Build Directions
* Get the required packages:

   ```bash
   sudo apt-get install \
           build-essential \
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
           libglvnd-dev \
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
           libwayland-dev \
           libx11-dev \
           libx264-dev \
           libxcb-shm0-dev \
           libxcb-xinerama0-dev \
           libxcomposite-dev \
           libxinerama-dev \
           pkg-config \
           python3-dev \
           qtbase5-dev \
           qtbase5-private-dev \
           libqt5svg5-dev \
           swig \
           libxcb-randr0-dev \
           libxcb-xfixes0-dev \
           libx11-xcb-dev \
           libxcb1-dev \
           libxss-dev \
           qtwayland5 \
           libgles2-mesa \
           libgles2-mesa-dev
   ```

* Building and installing OBS:

  * If you have configured your environment with a umask 077 or a noexec home dir you may 
    want to change your `CMAKE_INSTALL_PREFIX`, as it is reported `/usr` may change permissions on `/usr/bin` and `/usr/lib`. 

  * If building with browser source:
   ```bash
   sudo apt install libnss3-dev
   wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4280_linux64.tar.bz2
   tar -xjf ./cef_binary_4280_linux64.tar.bz2
   git clone --recursive https://github.com/obsproject/obs-studio.git
   cd obs-studio
   mkdir build && cd build
   # Note Ubuntu 20.04/Debian 10 must set ENABLE_PIPEWIRE=OFF and do not support wayland capture.
   # Modern platforms can use the default/enable pipewire for wayland capture support.
   cmake -DUNIX_STRUCTURE=1 -DENABLE_PIPEWIRE=OFF -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4280_linux64" ..
   make -j$(nproc)
   sudo make install
   ```
   If you have `checkinstall` use this instead of `make install`
   ```
   sudo checkinstall --default --pkgname=obs-studio --fstrans=no --backup=no --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes
   ```

  * If building without browser source:

   ```bash
   git clone --recursive https://github.com/obsproject/obs-studio.git
   cd obs-studio
   mkdir build && cd build
   # Note Ubuntu 20.04/Debian 10 must set ENABLE_PIPEWIRE=OFF and do not support wayland capture.
   # Modern platforms can use the default/enable pipewire for wayland capture support.
   cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DENABLE_PIPEWIRE=OFF -DBUILD_BROWSER=OFF ..
   make -j$(nproc)
   sudo make install
   ```
   If you have `checkinstall` use this instead of `make install`
   ```
   sudo checkinstall --default --pkgname=obs-studio --fstrans=no --backup=no --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes
   ```

***

### openSUSE Build Directions
* See openSUSE installation instructions (above) for details on adding Packman repository.

* Install build dependencies:

   ```bash
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
                python3-devel \
                libxss-dev
   ```

* Building and installing OBS:

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4280_linux64.tar.bz2
      tar -xjf ./cef_binary_4280_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4280_linux64" ..
      make -j4
      sudo make install
      ```

  * If building without browser source:

      ```bash
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=OFF ..
      make -j4
      sudo make install
      ```

***

### Linux portable mode (all distros)
* Please note that you need to install the build dependencies for your distribution before following this steps. See above.

* You can build in portable mode on Linux, which installs all the files to an isolated directory.

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4280_linux64.tar.bz2
      tar -xjf ./cef_binary_4280_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4280_linux64" ..
      make -j4 && make install
      ```

  * If building without browser source:

      ```bash
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" -DBUILD_BROWSER=OFF ..
      make -j4 && make install
      ```

* After that, you should have a portable install in `~/obs-studio-portable`. Change to `bin/64bit` or `bin/32bit` and then simply run: `./obs`

# FreeBSD

### FreeBSD Installation (Unofficial)

* Install OBS Studio:

   ```bash
   pkg install obs-studio
   ```

***

### FreeBSD Build Directions
* The easiest way to build OBS Studio from source is to use the [FreeBSD Ports](https://www.freebsd.org/doc/handbook/ports-using.html) and modify the `multimedia/obs-studio` port to suit your needs.

* First you have to set up the ports infrastructure on your system. See the related chapter in the [FreeBSD Handbook](https://www.freebsd.org/doc/handbook/ports-using.html).

* Once you've got your ports tree at `/usr/ports` you may edit the `multimedia/obs-studio` port to your liking. Then, you may build and install the port with:

   ```bash
   cd /usr/ports/multimedia/obs-studio
   make install clean
   ```
