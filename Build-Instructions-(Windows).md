# Requirements

* Development packages of `FFmpeg`, `x264`, `cURL`, and `mbedTLS`.
  * Pre-built versions of these dependencies for VS2019 on Windows can be found here:
    * VS2019: https://obsproject.com/downloads/dependencies2019.zip
* [Qt5](http://www.qt.io/) (Grab the MSVC package for your version of Visual Studio)
  * We currently deploy with Qt 5.15.2
    * [Qt 5.15.2 Windows](https://cdn-fastly.obsproject.com/downloads/Qt_5.15.2.7z)
* CEF Wrapper 4638 ([x64](https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_windows_x64.zip))
* Windows version of [CMake](http://www.cmake.org/) (3.16 or higher, latest preferred)
* Windows version of [Git](https://git-scm.com/download/win) (Git binaries must exist in path)
* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)
  * Ensure that the `Desktop development with C++` workload is selected when installing Visual Studio.
  * Windows 10 SDK (minimum 10.0.20348.0). [Latest SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/)

# Installation procedure

* Clone the repository and **submodules**:
  ```bash
  git clone --recursive https://github.com/obsproject/obs-studio.git
  ```

* If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.

* Create one or more of the following subdirectories within the cloned repository for building: `release`, `debug`, and `build` (suffixed with or without 32/64 to specify architecture). They are excluded from the repo in .gitignore for the sake of building, so they are safe to create and use within the repository base directory.

* Run cmake-gui, and set the following fields:
  * In "where is the source code", enter in the repo directory (example: D:/obs).
  * In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: D:/obs/build).

* Set the following variables. You can set them in cmake-gui by clicking the "Add Entry" button, or you can specify them when invoking the [cmake executable](https://cmake.org/cmake/help/v3.16/manual/cmake.1.html) via command-line with the `-DName=Value` syntax. Some variables may be able to be set as Windows Environment Variables to persist across configurations.
  * **Required**
    * `DepsPath`

      `DepsPath` is the path to the folder containing the dependencies, not including Qt. Set this to the win32 or win64 directory from the Pre-built dependencies package that you downloaded earlier.
      For example, if you extracted the dependencies .zip to C:\obs-deps, `DepsPath` should be one of these:
      * `C:\obs-deps\win32`
      * `C:\obs-deps\win64`

      If you want to specify both 32 and 64 bit dependencies to avoid changing the variable between builds, you can instead set `DepsPath32` and `DepsPath64` like so:
      * `DepsPath32`: `C:\obs-deps\win32`
      * `DepsPath64`: `C:\obs-deps\win64`

      `DepsPath` and its variants can be set as a Windows Environment Variable.

    * `QTDIR`

      `QTDIR` is the path to the Qt install directory.  The OBS UI is built by default, which requires Qt.  Set the CMake boolean variable DISABLE_UI to TRUE if you don't want the GUI and this is no longer required. Can be optionally suffixed with 32 or 64 to specify target arch.

      **NOTE**: Make sure to download Qt prebuilt components for your version of MSVC (32 or 64 bit).

      Example Qt directories you would use if you installed Qt to D:\Qt would usually look something like this:
        * `QTDIR32=D:\Qt\5.15.2\msvc2019` (32-bit)
        * `QTDIR64=D:\Qt\5.15.2\msvc2019_64` (64-bit)

      `QTDIR` and its variants can be set as a Windows Environment Variable.

    * `CEF_ROOT_DIR`

      `CEF_ROOT_DIR` is the path to an extracted CEF Wrapper. We provide a custom prebuilt wrapper to simplify the build process. This custom build includes access to hardware acceleration and additional codecs. **This enables Browser Source and Custom Browser Docks.**

      An example `CEF_ROOT_DIR` directory you might use if you extracted our provided package to your C drive would be:
        * `CEF_ROOT_DIR=C:\cef_binary_4638_windows_x64` (64-bit)

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
  * Download and install [LLVM 12.0.0](http://releases.llvm.org/).  LLVM 12.0.1+ currently introduces extra clang-format changes that we do not want to handle at this time.
  * Run VS, and go to Tools -> Options...
    * Text Editor -> C/C++ -> Code Style -> Formatting -> General
      * Enable "Use custom clang-format.exe" and enter the file name. For example:
        * C:\Program Files\LLVM\bin\clang-format.exe
  * The default command for formatting a document (Edit.FormatDocument) is Ctrl+K, Ctrl+D.