This page is intended to document the version of dependencies shipped with OBS on Windows (and eventually, macOS). These are the versions that the release builds downloaded from [obsproject.com/download](https://obsproject.com/download) are built with, but not necessarily the only compatible versions. Please use your best judgement when selecting a version to build with. 

That said, it is recommended that when building on Windows yourself to use our provided dependency zip file [located here](https://cdn-fastly.obsproject.com/downloads/dependencies2019.zip) using Visual Studio 2019.

*Note: This is a rough first draft and not all dependencies are currently listed.*

## Windows

| lib | git commit | version |
| :--- | :---: | :---: |
| AMF | [802f92ee52](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/commit/802f92ee52b9efa77bf0d3ea8bfaed6040cdd35e) | 1.4.16.1 |
| cURL | | 7.73.0 |
| CEF |  | 75.0.3770 [x86](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows32_minimal.zip) [x64](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows64_minimal.zip) |
| FFmpeg | [f9f95ceebf](https://github.com/FFmpeg/FFmpeg/commit/f9f95ceebfbd7b7f43c1b7ad34e25d366e6e2d2b) | n4.2.4 |
| VLC | | [3.0.0-git](https://cdn-fastly.obsproject.com/downloads/vlc.zip) |
| FreeType2 | | 2.10.4 |
| libogg | [31bd3f2707](https://github.com/xiph/ogg/commit/31bd3f2707fb7dbae539a7093ba1fc4b2b37d84e) | [no version] |
| libopus | [e85ed7726d](https://github.com/xiph/opus/commit/e85ed7726db5d677c9c0677298ea0cb9c65bdd23) | v1.3.1 |
| libpng | [a40189cf88](https://github.com/glennrp/libpng/commit/a40189cf881e9f0db80511c382292a5604c3c3d1) | v1.6.37 |
| libsrt | | [v1.4.2](https://github.com/Haivision/srt/releases/tag/v1.4.2) |
| libvorbis | [83a82dd929](https://github.com/xiph/vorbis/commit/83a82dd9296400d811b78c06e9ca429e24dd1e5c) | [no version] |
| libvpx | [e56e8dcd6f](https://chromium.googlesource.com/webm/libvpx/+/e56e8dcd6fc9e2b04316be5144c18ca6772f6263) | v1.9.0-git |
| LuaJIT | | 2.0.5 |
| mbed TLS | [523f0554b6](https://github.com/ARMmbed/mbedtls/commit/523f0554b6cdc7ace5d360885c3f5bbcc73ec0e8) | [2.24.0](https://github.com/ARMmbed/mbedtls/releases/tag/v2.24.0) |
| pthread-win32| [19fd5054b2](https://github.com/GerHobbelt/pthread-win32/commit/19fd5054b29af1b4e3b3278bfffbb6274c6c89f5) | 2.10.0.0 |
| Python | [5fd33b5926](https://github.com/python/cpython/commit/5fd33b5926eb8c9352bf5718369b4a8d72c4bb44) | [3.6.2](https://github.com/python/cpython/releases/tag/v3.6.2) |
| Qt | [no git] | [5.15.2](https://cdn-fastly.obsproject.com/downloads/Qt_5.15.2.7z) |
| Speex | | |
| x264 | [d198931a63](https://github.com/mirror/x264/commit/d198931a63049db1f2c92d96c34904c69fde8117) | 0.161.3020 r3020 |
| zlib | [cacf7f1d4e](https://github.com/madler/zlib/commit/cacf7f1d4e3d44d871b605da3b647f07d718623f) | [1.2.11](https://github.com/madler/zlib/releases/tag/v1.2.11) |

## macOS

| lib | git commit | version |
| :--- | :---: | :---: |
|libpng||1.6.37|
|libopus||1.31|
|libogg||68ca3841567247ac1f7850801a164f58738d8df9|
|librnnoise|[90ec41e](https://github.com/xiph/rnnoise/commit/90ec41ef659fd82cfec2103e9bb7fc235e9ea66c)||
|libvorbis||1.3.6|
|libvpx||1.8.12|
|libjansson||2.12|
|libx264|[d198931](https://github.com/mirror/x264/commit/d198931a63049db1f2c92d96c34904c69fde8117)||
|libmbedtls||2.16.5|
|libsrt||1.4.1|
|ffmpeg||4.2.2|
|libluajit||2.1.0-beta3|
|libfreetype||2.10.1|
|libpcre||8.44|
|swig||3.0.12|
|Qt||5.14.1|

## Linux (Ubuntu PPA)

| lib | git commit | version |
| :--- | :---: | :---: |
| CEF | gf19c584 | CEF 76.1.13 & chromium-76.0.3809.13 |