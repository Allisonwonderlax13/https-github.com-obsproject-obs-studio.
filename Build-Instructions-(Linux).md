# Build instructions

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
          libxkbcommon-devel \
          make \
          mbedtls-devel \
          pciutils-devel \
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
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2
      tar -xjf ./cef_binary_4638_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4638_linux64" ..
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
   /usr/local/lib
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
          libdrm-devel \
          libglvnd-devel \
          libv4l-devel \
          libX11-devel \
          libXcomposite-devel \
          libXdamage \
          libXinerama-devel \
          libxkbcommon-devel \
          luajit-devel \
          make \
          mbedtls-devel \
          pciutils-devel \
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
          vlc-devel \
          wayland-devel \
          x264-devel
   ```

* Building and installing OBS:

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2
      tar -xjf ./cef_binary_4638_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4638_linux64" ..
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
           libgles2-mesa-dev \
           libpci-dev
   ```

* Building and installing OBS:

  * If you have configured your environment with a umask 077 or a noexec home dir you may 
    want to change your `CMAKE_INSTALL_PREFIX`, as it is reported `/usr` may change permissions on `/usr/bin` and `/usr/lib`. 

  * If building with browser source:
   ```bash
   sudo apt install libnss3-dev
   wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2
   tar -xjf ./cef_binary_4638_linux64.tar.bz2
   git clone --recursive https://github.com/obsproject/obs-studio.git
   cd obs-studio
   mkdir build && cd build
   # Note Ubuntu 20.04/Debian 10 must set ENABLE_PIPEWIRE=OFF and do not support wayland capture.
   # Modern platforms can use the default/enable pipewire for wayland capture support.
   cmake -DUNIX_STRUCTURE=1 -DENABLE_PIPEWIRE=OFF -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4638_linux64" ..
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
   # Note Ubuntu 20.04/Debian 10 must set ENABLE_PIPEWIRE=OFF and do not support wayland capture.
   # Modern platforms can use the default/enable pipewire for wayland capture support.
   cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DENABLE_PIPEWIRE=OFF -DBUILD_BROWSER=OFF -Bbuild
   make -j$(nproc) -Cbuild
   sudo make install -Cbuild
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
                libxss-dev \
                pciutils-devel
   ```

* Building and installing OBS:

  * If building with browser source:

      ```bash
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2
      tar -xjf ./cef_binary_4638_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=1 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4638_linux64" ..
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
      wget https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2
      tar -xjf ./cef_binary_4638_linux64.tar.bz2
      git clone --recursive https://github.com/obsproject/obs-studio.git
      cd obs-studio
      mkdir build && cd build
      cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_4638_linux64" ..
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

### FreeBSD Installation ([Unofficial](#about-unofficial-builds))

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
