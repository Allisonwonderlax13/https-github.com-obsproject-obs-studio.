This page is intended to document the version of dependencies shipped with OBS on Windows (and eventually, macOS). These are the versions that the release builds downloaded from [obsproject.com/download](https://obsproject.com/download) are built with, but not necessarily the only compatible versions. Please use your best judgement when selecting a version too build with. 

That said, it is recommended that when building on Windows yourself to use our provided dependency zip file [located here](https://obsproject.com/downloads/dependencies2017.zip) using Visual Studio 2017/2019.

*Note: This is a rough first draft and not all dependencies are currently listed.*

## Windows

| lib | git commit | version |
| :--- | :---: | :---: |
| curl | | 7.58.0 |
| cef |  | 75.0.3770 |
| FFmpeg | [192d1d34eb](https://github.com/FFmpeg/FFmpeg/commit/192d1d34eb3668fa27f433e96036340e1e5077a0) | n4.2.2 |
| freetype2 | | |
| libogg | [df53eebf72](https://github.com/xiph/ogg/commit/df53eebf72e86eb179465f53dd77297ae72ae233) | [no version] |
| libpng | a40189cf88 | v1.6.37
| libvorbis | [9eadeccdc4](https://github.com/xiph/vorbis/commit/9eadeccdc4247127d91ac70555074239f5ce3529) | [no version] |
| libvpx | e11f218af1 | v1.8.1 |
| libwebsockets | | |
| luajit | | |
| mbedtls | | |
| ogg | | |
| opus | [e85ed7726d](https://github.com/xiph/opus/commit/e85ed7726db5d677c9c0677298ea0cb9c65bdd23) | v1.3.1 |
| python | | |
| Qt | [no git] | 5.10.1 |
| speex | | |
| vpx | | |
| x264 | [72db437770](https://github.com/mirror/x264/commit/72db437770fd1ce3961f624dd57a8e75ff65ae0b) | 0.157.2945 r2945 |
| zlib | [no git] | 1.2.11 |

## Linux (Ubuntu PPA)

| lib | git commit | version |
| :--- | :---: | :---: |
| cef | gf19c584 | CEF 76.1.13 & chromium-76.0.3809.13 |