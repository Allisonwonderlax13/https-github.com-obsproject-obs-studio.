# Installation procedure

* Clone the repository and **submodules**:

  ```bash
  git clone --recursive https://github.com/obsproject/obs-studio.git
  ```

* If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.

## macOS Full Build Script

To get a self-built OBS up and running, a default build and packaging script is provided. This script only requires Homebrew (https://brew.sh) to be installed on the build system already:

* To build OBS as-is with full browser-source support, simply run `./CI/full-build-macos.sh` from the checkout directory (The script will take care of downloading all necessary dependencies).
* To *create an app-bundle* after building OBS, run the script with the `-b` flag: `./CI/full-build-macos.sh -b`
* To *create a disk image* after building OBS, run the script with the `-p` flag: `./CI/full-build-macos.sh -b -p`
* To *notarize* an app-bundle after building and bundling OBS, run the script with the `-n` flag: `./CI/full-build-macos.sh -b -n`
* To create an app-bundle *without building OBS again*, run the script with the `-s` flag: `./CI/full-build-macos.sh -s -b`

The last option is helpful if custom `cmake` flags have been used, but a proper app bundle is desired.

## macOS Custom Builds

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

## Pre-Built Dependencies

These dependencies are also available via `obs-deps` (https://github.com/obsproject/obs-deps) as pre-compiled binaries, which are assured to be compatible with current OBS code (as OBS is built against specific versions of some packages while Homebrew delivers most recent stable builds).

* **When using obs-deps**, extract both archives from the macOS release to `/tmp/obsdeps` to assure compatibility with app bundling later (due to the way `dylib`s are identified and linked).
* Create a build directory inside the `obs-studio` directory, change to it, then configure the project via `cmake`:

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DQTDIR="/tmp/obsdeps" -DSWIGDIR="/tmp/obsdeps" -DDepsPath="/tmp/obsdeps" -DDISABLE_PYTHON=ON ..
   ```

* Build OBS by running `make`.

# Configuring and Building

* If not already handled by the Homebrew installation, install a current macOS platform SDK (only macOS High Sierra 10.13.4 or later is supported): `xcode-select --install`
* Create a build directory inside the `obs-studio` directory, change to it, then configure the project via `cmake`:

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DDISABLE_PYTHON=ON ..
   ```

* *NOTE*: `cmake` might require additional parameters to find `Qt5` libraries present on this system, this can either be provided via `-DQTDIR="/usr/local/opt/qt5"` or setting an environment variable, e.g.: `export QTDIR=/usr/local/opt/qt5`
* Build OBS by running `make`
* Run OBS from the `/rundir/RelWithDebInfo/bin` directory in your build directory, by running `./obs` from a Terminal
* *NOTE*: If you are running via command prompt, you *must* be in the 'bin' directory specified above, otherwise it will not be able to find its files relative to the binary.

*** 

## macOS Xcode Project

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

**NOTE:** When running OBS directly from Xcode be aware that browser sources will not be available (as CEF requires to be run as part of an application bundle in macOS) and accessing the webcam will lead to a crash (as macOS requires a permission prompt text set in an application bundle's `Info.plist` which is, of course, not available). Breakpoints and stack traces are available though for debugging purposes if you don't need browser source. In that case, make sure to add -DBUILD_BROWSER=OFF, e.g. : 

   ```bash
   cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DBUILD_BROWSER=OFF -DDISABLE_PYTHON=ON -G Xcode ..
   ```

**NOTE 2:** Configuration / generation can also be done with cmake-gui if one prefers to use a GUI.

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