# Option A: Automatic FreeBSD builds

Automatic FreeBSD builds allow building OBS with minimal input and setup - necessary dependencies are installed automatically, build flags use a sane default and the generated OBS build uses the application's full feature set.

## Prerequisites

* FreeBSD 13
* Git
* Pkg

## Build procedure

* Clone the repository including **submodules**:

    `git clone --recursive https://github.com/obsproject/obs-studio.git`

* To do a **fully automated** build, open Terminal, switch to the checkout directory then run one of the following commands:

```
    # Download and set up dependencies, then build OBS for local host 
    # architecture with common feature set
    CI/build-freebsd.sh

    # Skip download and setup of dendencies
    CI/build-freebsd.sh --skip-dependency-checks

    # Show all available options
    CI/build-freebsd.sh --help

    # Use `my_build_dir` prefix as build directory
    CI/build-freebsd.sh --build-dir my_build_dir
```

# Option B: Custom FreeBSD builds

Custom FreeBSD builds allow full customization of the desired build configuration but also require manual setup and preparation. Available CMake configuration variables can be found in the [CMake build system documentation](https://github.com/PatTheMav/obs-studio/wiki/OBS-Build-System).

**NOTE:** FreeBSD is not officially supported by the OBS team and is provided as-is.

## Prerequisites

* FreeBSD 13 or newer
* CMake 3.16 or newer
* Git
* Ninja
* *Optional:* CCache to improve compilation speeds on consecutive builds
* Several additional dependencies (see step 2 below)

## Build Procedure

### 1. Get the source code

1. Open a Terminal window, create and switch to a directory you want to have OBS checked out
2. Clone the repository including **submodules**: `git clone --recursive https://github.com/obsproject/obs-studio.git`

(If you do not know what submodules are, or you are not using Git from the command line, **PLEASE make sure to fetch the submodules too**.)

### 2. Get the dependencies

* To download and set up most preconditions mentioned above, you can also run the script `CI/freebsd/01_install_dependencies.sh` from the checkout directory (run it with the `--help` switch to see all available options). 

**NOTE:** The directory where the script will download and setup the dependencies in cannot be changed.

* Alternatively the required dependencies can be installed using `apt`:
    * Build system dependencies
    ```
    sudo pkg install cmake ninja pkgconf curl ccache
    ```

    * OBS dependencies (core):
    ```
    sudo pkg install ffmpeg libx264 mbedtls mesa-libs jansson lua52 luajit python37 libX11 xorgproto libxcb libXcomposite libXext libXfixes libXinerama libXrandr swig dbus jansson libICE libSM libsysinfo
    ```

    * OBS dependencies (UI):
    ```
    sudo pkg install qt5-buildtools qt5-qmake qt5-imageformats qt5-core qt5-gui qt5-svg qt5-widgets qt5-xml
    ```

    * Plugin dependencies:
    ```
    sudo pkg install v4l_compat fdk-aac fontconfig freetype2 speexdsp libudev-devd libv4l vlc audio/jack pulseaudio sndio
    ```

### 3. Set up the build project

1. Run CMake to generate a build environment

```
cmake -S . -B <YOUR_BUILD_DIRECTORY> -G Ninja \
    -DLINUX_PORTABLE=ON \
    -DENABLE_PIPEWIRE=OFF
```

**Optional Settings:**

2. To change the build type, pass either `Debug`, `Release`, `RelWithDebInfo`, or `MinSizeRel` as `-DCMAKE_BUILD_TYPE`

**NOTE:** When building OBS with `LINUX_PORTABLE` disabled, OBS expects GNU-based install paths (e.g. `/usr/local/[bin,lib,share]`) and is built for a single architecture only. To create separate builds for 32-bit and 64-bit architectures, always enable portable builds.

### 4. Build the project

1. Run `cmake --build <YOUR_BUILD_DIRECTORY>` to build the entire OBS project
2. Run `cmake --build <YOUR_BUILD_DIRECTORY> -t libobs` to build only libobs or any other valid target
3. Run `cmake --build <YOUR_BUILD_DIRECTORY> -t clean` to clean your current build directory

### 6. Install the project

Installation will use the directory specified via `-DCMAKE_INSTALL_PREFIX` or can be customised with the `--prefix` switch:

1. Run `cmake --install <YOUR_BUILD_DIRECTORY>` to install OBS to the prefix the project was configured with
2. Run `cmake --install <YOUR_BUILD_DIRECTORY> --prefix <YOUR_INSTALL_LOCATION>` to install OBS to a custom location

### 7. Create installer package

1. Run `cmake --package <YOUR_BUILD_DIRECTORY>` - CMake will handle all operations necessary to create a installer package (compressed archive as well as shell script with embedded binary)
