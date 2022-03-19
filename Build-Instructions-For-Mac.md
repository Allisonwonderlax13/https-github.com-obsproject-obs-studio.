# Option A: Automatic macOS builds

Automatic macOS builds allow building OBS with minimal input and setup - necessary dependencies are installed automatically, build flags use a sane default and the generated OBS build uses the application's full feature set.

## Prerequisites

* macOS 10.13 or newer for x86_64-based builds
* macOS 11.0 or newer for arm64-based builds
* Command Line Tools (CLT) for Xcode:
    * Install by running `xcode-select --install` in Terminal, **or**
    * Download from https://developer.apple.com/downloads, **or**
    * Install [Xcode](https://itunes.apple.com/us/app/xcode/id497799835)

**NOTE:** Automatic build scripts use [Homebrew](https://brew.sh) to automatically install additional build dependencies. If Homebrew is already installed on the system, that Homebrew environment will be used.

## Build procedure

* Clone the repository including **submodules**:

    `git clone --recursive https://github.com/obsproject/obs-studio.git`

* To do a **fully automated** build, open Terminal, switch to the checkout directory then run one of the following commands:

```
    # Download and set up dependencies, then build OBS for local host 
    # architecture with common feature set
    CI/build-macos.sh

    # Skip download and setup of dendencies
    CI/build-macos.sh --skip-dependency-checks

    # Create a relocatable OBS.app application bundle
    CI/build-macos.sh --bundle

    # Create a distributable disk image
    CI/build-macos.sh --package

    # Ask for and use available developer ID for codesigning
    CI/build-macos.sh --codesign

    # Notarize application bundle or distributable disk image
    CI/build-macos.sh --notarize

    # Compile and package a codesigned and notarized build of OBS
    CI/build-macos.sh --package --notarize

    # Create a relocatable and codesigned OBS.app application bundle without 
    # checking for dependencies
    CI/build-macos.sh --skip-dependency-checks --codesign --bundle

    # Show all available options
    CI/build-macos.sh --help
```

# Option B: Custom macOS builds

Custom macOS builds allow full customization of the desired build configuration but also require manual setup and preparation. Available CMake configuration variables can be found in the [CMake build system documentation](https://github.com/PatTheMav/obs-studio/wiki/OBS-Build-System).

## Prerequisites

* macOS 10.15 or newer
* Command Line Tools (CLT) for Xcode:
    * Install by running `xcode-select --install` in Terminal, **or**
    * Download from https://developer.apple.com/downloads, **or**
    * Install [Xcode](https://itunes.apple.com/us/app/xcode/id497799835)
    * CMake
    * Ninja
    * *Optional:* CCache to improve compilation speeds on consecutive builds
    * Pre-Built OBS dependencies (includes `FFmpeg`, `x264`, `mbedTLS`, `Qt` and more)
        * Download the latest version for your desired architecture from https://github.com/obsproject/obs-deps/releases
        * Unpack the contents of the archive into `~/development/obs-build-dependencies/obs-deps`
    * For browser source and browser panel support, the pre-built CEF framework is needed:
        * Chromium Embedded Framework (CEF) [Intel x86_64](https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_macos_x86_64.tar.xz)
        * Chromium Embedded Framework (CEF) [Apple M1](https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_macos_arm64.tar.xz)
        * Preparation of CEF for use in OBS requires some fixups and compilation of the static wrapper library (see below)
    * Alternatively download and setup the following dependencies manually:
        * FFmpeg
        * x264
        * freetype
        * mbedtls
        * swig
        * Qt5 (requires additional patches, check the [obs-deps](https://github.com/obsproject/obs-deps) repository for more details)

## Build procedure

### 1. Get the source code

1. Open a Terminal window, create and switch to a directory you want to have OBS checked out
2. Clone the repository including **submodules**: `git clone --recursive https://github.com/obsproject/obs-studio.git`

(If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.)

### 2. Get the dependencies

* To download and set up most preconditions mentioned above, you can also run the script `CI/macos/01_install_dependencies.sh` from the checkout directory (run it with the `--help` switch to see all available options). 

**NOTE:** The directory where the script will download and setup the dependencies in cannot be changed.

### 3. Build Chromium Embedded Framework Wrapper

1. Switch to the directory where the pre-compiled Chromium Embedded Framework was downloaded and extracted
2. Fix the CMake build scripts:
    * `/usr/bin/sed -i '.orig' '/add_subdirectory(tests\/ceftests)/d' ./CMakeLists.txt`
    * `/usr/bin/sed -E -i '' 's/"10.(9|10)"/"'10.13'"/' ./cmake/cef_variables.cmake`
3. Run CMake to generate the build scripts for the static wrapper library:
    ```
    cmake -S . -B build -G Ninja \
        -DPROJECT_ARCH=x86_64 \
        -DCEF_COMPILER_FLAGS="-Wno-deprecated-copy" \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_CXX_FLAGS="-std=c++11 -stdlib=libc++ -Wno-deprecated-declarations -Wno-unknown-warning-option" \
        -DCMAKE_EXE_LINKER_FLAGS="-std=c++11 -stdlib=libc++" \
        -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13
    ```

4. Build the static wrapper library by running `cmake --build build`

**NOTE:** This build requirement can be disabled by setting `-DENABLE_BROWSER_SOURCE` to `OFF` in the next step.

### 4. Set up the build project

1. Run CMake to generate a build environment

```
cmake -S . -B build -G Xcode
    -DCEF_ROOT_DIR="~/development/obs-build-dependencies/cef_binary_4638_macos_x86_64" \
    -DCMAKE_PREFIX_PATH="~/development/obs-build-dependencies/obs-deps" \
    -DCMAKE_OSX_DEPLOYMENT_TARGET=11.0 \
    -DCMAKE_OSX_ARCHITECTURES="x86_64" \
```

**Optional Settings:**

2. Specify a codesigning identity using `-DOBS_BUNDLE_CODESIGN_IDENTITY="[YOUR_IDENTITY]"`
3. Specify the output directory for creating application bundles using `-DCMAKE_INSTALL_PREFIX=[YOUR INSTALL DESTINATION]`
4. To build OBS without Xcode, change `Xcode` to `Ninja` as the `-G` option
5. Specify the build configuration e.g. using `-DCMAKE_BUILD_TYPE=RelWithDebInfo` (Ninja or GNU Makefiles only)

### 5. Open the Xcode project

1. Open `obs-studio.xcodeproj` from the build directory (or any other directory specified via the `-B` switch above)
2. Build OBS using `OBS` schema, recognisable by the OBS app icon.

### 6. Install OBS.app bundle

Installation will use the directory specified via `-DCMAKE_INSTALL_PREFIX` or can be customised with the `--prefix` switch for Ninja-based builds.

* If using Xcode:
    * Open the generated Xcode project
    * Install OBS using the `install` schema

* If using the command line (Xcode, Ninja, or GNU Makefiles):
    * Build OBS by running `cmake --build build`
    * Install OBS by running `cmake --install build`