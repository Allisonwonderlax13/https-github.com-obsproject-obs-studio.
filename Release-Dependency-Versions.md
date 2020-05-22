This page is intended to document the version of dependencies shipped with OBS on Windows (and eventually, macOS). These are the versions that the release builds downloaded from [obsproject.com/download](https://obsproject.com/download) are built with, but not necessarily the only compatible versions. Please use your best judgement when selecting a version to build with. 

That said, it is recommended that when building on Windows yourself to use our provided dependency zip file [located here](https://obsproject.com/downloads/dependencies2017.zip) using Visual Studio 2017/2019.

*Note: This is a rough first draft and not all dependencies are currently listed.*

## Windows

| lib | git commit | version |
| :--- | :---: | :---: |
| cURL | | 7.58.0 |
| CEF |  | 75.0.3770 [x86](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows32_minimal.zip) [x64](https://cdn-fastly.obsproject.com/downloads/cef_binary_75.1.16+g16a67c4+chromium-75.0.3770.100_windows64_minimal.zip) |
| FFmpeg | [192d1d34eb](https://github.com/FFmpeg/FFmpeg/commit/192d1d34eb3668fa27f433e96036340e1e5077a0) | n4.2.2 |
| VLC | | [3.0.0-git](https://cdn-fastly.obsproject.com/downloads/vlc.zip) |
| FreeType2 | | 2.9.0 |
| libogg | [df53eebf72](https://github.com/xiph/ogg/commit/df53eebf72e86eb179465f53dd77297ae72ae233) | [no version] |
| libopus | [e85ed7726d](https://github.com/xiph/opus/commit/e85ed7726db5d677c9c0677298ea0cb9c65bdd23) | v1.3.1 |
| libpng | [a40189cf88](https://github.com/glennrp/libpng/commit/a40189cf881e9f0db80511c382292a5604c3c3d1) | v1.6.37
| libvorbis | [9eadeccdc4](https://github.com/xiph/vorbis/commit/9eadeccdc4247127d91ac70555074239f5ce3529) | [no version] |
| libvpx | | [v1.8.1](https://chromium.googlesource.com/webm/libvpx/+/refs/tags/v1.8.1) |
| libwebsockets | | 3.1.99 |
| LuaJIT | | 2.0.5 |
| mbed TLS | [04a049bda1](https://github.com/ARMmbed/mbedtls/commit/04a049bda1ceca48060b57bc4bcf5203ce591421) | [2.16.3](https://github.com/ARMmbed/mbedtls/releases/tag/mbedtls-2.16.3) |
| Python | [5fd33b5926](https://github.com/python/cpython/commit/5fd33b5926eb8c9352bf5718369b4a8d72c4bb44) | [3.6.2](https://github.com/python/cpython/releases/tag/v3.6.2) |
| Qt | [no git] | [5.10.1](https://cdn-fastly.obsproject.com/downloads/Qt_5.10.1.7z) |
| Speex | | |
| x264 | [72db437770](https://github.com/mirror/x264/commit/72db437770fd1ce3961f624dd57a8e75ff65ae0b) | 0.157.2945 r2945 |
| zlib | [cacf7f1d4e](https://github.com/madler/zlib/commit/cacf7f1d4e3d44d871b605da3b647f07d718623f) | [1.2.11](https://github.com/madler/zlib/releases/tag/v1.2.11) |

## Linux (Ubuntu PPA)

| lib | git commit | version |
| :--- | :---: | :---: |
| CEF | gf19c584 | CEF 76.1.13 & chromium-76.0.3809.13 |