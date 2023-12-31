# Current Limitation
1. CEF hardware acceleration does not work.
Uncheck "Settings" -> "Advanced" -> "Sources" -> "Enable Browser Source Hardware Acceleration" to use software rendering.
5. There is no installer available, yet. 
3. Virtual Camera should work, in theory. A manual registration of Virtual Camera is needed.
4. Video Source is not available, unless there's a good enough MinGW toolchain targeting ARM64 Windows, AND the VideoLAN is willing to use it. 
6. There is no hardware encoding, yet. Neither Microsoft nor Qualcomm released any documentation on this topic.

Windows ARM64 binary build can be found here: https://github.com/tommyvct/obs-studio/releases   
[![CI Multiplatform Build](https://github.com/tommyvct/obs-studio/actions/workflows/main.yml/badge.svg?branch=windows-arm64)](https://github.com/tommyvct/obs-studio/actions/workflows/main.yml)

# Prerequisites
- ARM64 fork of `obs-studio`: https://github.com/tommyvct/obs-studio/
- VS2022 with arm64 C++ toolchain, ATL, and Spectre mitigation (optional).  
  Reference `.vsconfig`
  <details>



  ``` json
    {
        "version": "1.0",
        "components": [
            "Microsoft.VisualStudio.Component.CoreEditor",
            "Microsoft.VisualStudio.Workload.CoreEditor",
            "Microsoft.VisualStudio.Component.TypeScript.TSServer",
            "Microsoft.VisualStudio.Component.TypeScript.SDK.4.4",
            "Microsoft.VisualStudio.ComponentGroup.WebToolsExtensions",
            "Microsoft.VisualStudio.Component.Roslyn.Compiler",
            "Microsoft.Component.MSBuild",
            "Microsoft.VisualStudio.Component.TextTemplating",
            "Microsoft.VisualStudio.Component.Debugger.JustInTime",
            "Microsoft.VisualStudio.Component.IntelliCode",
            "Microsoft.VisualStudio.Component.VC.CoreIde",
            "Microsoft.VisualStudio.Component.VC.Tools.x86.x64",
            "Microsoft.VisualStudio.Component.Graphics.Tools",
            "Microsoft.VisualStudio.Component.VC.DiagnosticTools",
            "Microsoft.VisualStudio.Component.VC.Redist.14.Latest",
            "Microsoft.VisualStudio.ComponentGroup.NativeDesktop.Core",
            "Microsoft.VisualStudio.Component.VC.Tools.ARM64",
            "Microsoft.VisualStudio.ComponentGroup.WebToolsExtensions.CMake",
            "Microsoft.VisualStudio.Component.VC.CMake.Project",
            "Microsoft.VisualStudio.Component.VC.ATL",
            "Microsoft.VisualStudio.Component.VC.ATLMFC",
            "Microsoft.VisualStudio.Component.VC.ASAN",
            "Microsoft.VisualStudio.Component.Windows11SDK.22000",
            "Microsoft.VisualStudio.Component.VC.Tools.ARM64EC",
            "Microsoft.VisualStudio.Component.VC.CoreBuildTools",
            "Microsoft.VisualStudio.Workload.NativeDesktop",
            "Microsoft.VisualStudio.Component.VC.ATL.ARM64",
            "Microsoft.VisualStudio.Component.VC.ATL.ARM64EC",
            "Microsoft.VisualStudio.Component.VC.ATL.Spectre",
            "Microsoft.VisualStudio.Component.VC.ATL.ARM64.Spectre",
            "Microsoft.VisualStudio.Component.VC.ATL.ARM64EC.Spectre",
            "Microsoft.VisualStudio.Component.VC.ATLMFC.Spectre",
            "Microsoft.VisualStudio.Component.VC.MFC.ARM64.Spectre",
            "Microsoft.VisualStudio.Component.VC.MFC.ARM64EC.Spectre",
            "Microsoft.VisualStudio.Component.VC.Runtimes.ARM64.Spectre",
            "Microsoft.VisualStudio.Component.VC.Runtimes.ARM64EC.Spectre",
            "Microsoft.VisualStudio.Component.VC.MFC.ARM64",
            "Microsoft.VisualStudio.Component.VC.MFC.ARM64EC"
        ]
    }
  ```



  </details>

- CMake  
    - When using CMake GUI, there will be a prompt to choose the generator and the platform to use will appear before the configure begins. Choose `Visual Studio 17 2022` as the generator, and ARM64 as the platform.   
    - If using CMake from command line, use `-G"Visual Studio 17 2022" -A"ARM64"`.  
    - This applies to everything using CMake in this guide.

The following prerequisites are for building dependencies:

- MSYS2 with `make`, `diffutils`, and `yasm` installed.
- A Unix environment, either MSYS2, WSL or a real Linux with `mingw64-binutils` installed.  
- Perl
- Ruby
- The [patch](https://github.com/RytoEX/obs-deps/tree/build-windows-deps-v02/CI)
- Cross MSYS2 bash
	1. Use this Windows Terminal profile:
        ``` json
        {
            "commandline": "%comspec% /k \"C:\\\"Program Files\"\\\"Microsoft Visual Studio\"\\2022\\Community\\VC\\Auxiliary\\Build\\vcvarsall.bat x64_arm64 & \"C:\\msys64\\msys2_shell.cmd\"  -defterm -no-start -use-full-path\"",
            "icon": "C:\\msys64\\msys2.ico",
            "name": "MSYS2 MSVC ARM64",
            "startingDirectory": null,
        },
        ```
        Or save the following content to a batch file:
		``` bat
		call "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvars64.bat" x64_arm64
		call "C:\msys64\msys2_shell.cmd"
		```
	2. Open MSYS2 console  
		``` bash
		mv /usr/bin/link.exe /usr/bin/link.exe.bak
		```
		We won't use the linker built-in with MSYS2.
- MSVC ARM64 Developer Command Prompt / Powershell  
    - Use this Windows Terminal profile:
        ``` json
        {
            "commandline": "powershell.exe -noe -c \"&{$vsPath = &(Join-Path ${env:ProgramFiles(x86)} '\\Microsoft Visual Studio\\Installer\\vswhere.exe') -prerelease -property installationpath; Import-Module (Join-Path $vsPath 'Common7\\Tools\\Microsoft.VisualStudio.DevShell.dll'); Enter-VsDevShell -VsInstallPath $vsPath  -SkipAutomaticLocation -Arch arm64 -hostArch amd64;}\"",
            "icon": "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\Common7\\IDE\\Assets\\VisualStudio.70x70.contrast-standard_scale-80.png",
            "name": "MSVC ARM64",
        },
        ```
        Or save the following content to a batch file:
        ``` bat
        call "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvars64.bat" x64_arm64
        ```
# Dependencies

Prebuilt dependencies can be downloaded here: https://github.com/tommyvct/obs-deps/releases

Almost all dependencies used here are using the latest development version instead of certain old version unless specified. This is to minimize the potential incompatibility against Windows ARM64 platfrom/toolchain.

## A rough workflow
Have a copy of [Intel version of prebuilt dependencies](https://obsproject.com/downloads/dependencies2019.zip) for reference.

Grab the dependencies from their upsteram and compile. If there's a built-in sln file then use it. If the sln didn't work or there is no sln, then use CMake. The worst situation is there is no cmake and sln, then you need to use MSYS2 commandline AND MSVC toolchain. See Cross MSYS2 bash above.

The header files are usually gathered in the `include` directory. 

Make sure you put the produced files in the same hierarchy as the intel one.

## Compile Qt
Download Qt 5.15.2 here: https://download.qt.io/archive/qt/

> Only the following folders is needed in the source folder: `gnuwin32`, `qtbase`, `qtsvg` and `qtwinextras`, remove the rest to speed things up.  
> Use `jom` instead of `nmake`. `nmake` is single-threaded only, and it will take days to compile qt.  
> Qt can figure out which compiler to use by itself, so just use a normal `x64` VS Developer Command Prompt instead of `x64_arm64` one

``` batch
"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvarsall.bat" x64

cd qt\

configure.bat -opensource -platform win32-msvc -xplatform win32-arm64-msvc2017 -prefix "C:\qt"  -confirm-license  -schannel -nomake examples -nomake tests -no-compile-examples -no-dbus -no-freetype -no-harfbuzz -no-icu -no-feature-concurrent -no-feature-itemmodeltester -no-feature-printdialog -no-feature-printer -no-feature-printpreviewdialog -no-feature-printpreviewwidget -no-feature-sql -no-feature-sqlmodel -no-feature-testlib -no-sql-db2 -no-sql-ibase -no-sql-mysql -no-sql-oci -no-sql-odbc -no-sql-psql -no-sql-sqlite2 -no-sql-sqlite -no-sql-tds -DQT_NO_PDF -DQT_NO_PRINTER -mp

jom -j12
jom install -j12
```

## Compile mbedTLS  
Use CMake instead of buildin vs sln. 

### Crypto Library DLL  
#### Prepare `mbedcrypto.def`  
> Unix shell required  
> Tested on openSUSE 15.2 WSL2  
> Make sure you have related packages installed

Run the following script:
``` bash
#/bin/bash
# Credit https://github.com/RytoEX/obs-deps/blob/build-windows-deps/win64.sh

# make directories
rm -rf win64
mkdir win64
cd win64
mkdir bin
mkdir include
mkdir lib
cd ..
mkdir -p win64/lib/pkgconfig

# set build prefix
WORKDIR=$PWD
PREFIX=$PWD/win64
export PKG_CONFIG_PATH="$PREFIX/lib/pkgconfig"

# set variables
SKIP_PROMPTS=true


#---------------------------------


# start mbedTLS
if [ "$SKIP_PROMPTS" != true ]; then
	read -n1 -r -p "Press any key to build mbedtls..." key
fi

# download mbedTLS
curl --retry 5 -L -o mbedtls-2.25.0.tar.gz https://github.com/ARMmbed/mbedtls/archive/v2.25.0.tar.gz
tar -xf mbedtls-2.25.0.tar.gz
mv mbedtls-2.25.0 mbedtls

# build mbedTLS
# Enable the threading abstraction layer and use an alternate implementation
sed -i -e "s/\/\/#define MBEDTLS_THREADING_C/#define MBEDTLS_THREADING_C/" \
-e "s/\/\/#define MBEDTLS_THREADING_ALT/#define MBEDTLS_THREADING_ALT/" mbedtls/include/mbedtls/config.h
cp -p patch/mbedtls/threading_alt.h mbedtls/include/mbedtls/threading_alt.h


mkdir -p mbedtlsbuild/win64
cd mbedtlsbuild/win64
rm -rf *
cmake ../../mbedtls -DCMAKE_SYSTEM_NAME=Windows -DCMAKE_C_COMPILER=x86_64-w64-mingw32-gcc -DCMAKE_INSTALL_PREFIX=$PREFIX -DCMAKE_RC_COMPILER=x86_64-w64-mingw32-windres -DCMAKE_SHARED_LINKER_FLAGS="-static-libgcc -Wl,--strip-debug" -DUSE_SHARED_MBEDTLS_LIBRARY=ON -DUSE_STATIC_MBEDTLS_LIBRARY=OFF -DENABLE_PROGRAMS=OFF -DENABLE_TESTING=OFF
make -j$(nproc)

x86_64-w64-mingw32-dlltool -z mbedcrypto.orig.def --export-all-symbols library/libmbedcrypto.dll # export all symbols
grep "EXPORTS\|mbedtls" mbedcrypto.orig.def > mbedcrypto.def # keep lines with either "EXPORTS" or "mbedtls"
sed -i -e "/\\t.*DATA/d" -e "/\\t\".*/d" -e "s/\s@.*//" mbedcrypto.def # trim ordinals
```

Now bring `mbedcrypto.def` back to Windows.

#### Compile
Open CMake GUI, specify where the mbedTLS source code and the build directory is, then press Configure.  

Choose`Visual Studio 16 2019` as generator and `ARM64` as platform, then press Finish.  

Modify the cache entry as follows:
|Entry|Value|
|---:|:---|
|ENABLE_TESTING|OFF|
|ENABLE_PROGRAMS|OFF|
|USE_SHARED_MBEDTLS_LIBRARY|ON|
|USE_STATIC_MBEDTLS_LIBRARY|ON|

Click Generate, then Open Project.

In Visual Studio, right-click on the `mbedcrypto` project, then select Properties. 
In Configuration Properties > Linker > Input, put in the absolute path of `mbedcrypto.def`, then click OK. 

Now build the whole solution with **Release** configuration. 
> Debug configuration will cause more link errors. 

Link errors are expected. Open `mbedcrypto.def` file, find and delete the unresolved external symbol from `mbedcrypto.def`, then build again.

## Compile libsrt  
Use CMake. 

|Cache|Value|
|---:|:---|
|`USE_ENCLIB`|`mbedtls`|
|`USE_OPENSSL_PC`|`OFF`|
|`ENABLE_STDCXX_SYNC`|`ON`|

Set mbedTLS library path accordingly.
CMake may failed to find mbedTLS include path, add the include path in Visual Studio instead.


## Compile opus
Use CMake.

```BUILD_SHARED_LIBS=ON```


## Compile zlib  
Use built-in vs sln.

Right click on `zlibstat` project, click Properties, choose Configuration Properties > Librarian > All Options, frem Additional options, remove `/MACHINE:X64`. Make sure to build with `ReleaseWithoutAsm` configuration.

Build projects one-by-one.

Remember to patch the header **after** compile.


## Compile libpng
Use built-in vs sln.

Unload the built-in zlib project, copy the previously pre-compiled `zlib.lib` to the output directory.  

Spectre Migitation warning as error is a bug in vs2019, manual warning mute is required.

## Compile libogg
Use built-in vs sln.

ReleaseDLL configuration for OBS,  
Release configuration for dependency of libvorbis.


## Compile libvorbis  
Use built-in vs sln (`vorbis_dynamic.sln`).

Put previously compiled `libogg.lib` at `libogg/win32/VS2010/ARM64/Release`, where `libogg` directory is on the same level of vorbis source code directory.

Right click on `vorbisenc` Project, click Properties. Change Configuration type to `dll`.
In Configuration Properties > Linker > Input, add Module Definition File `../../vorbisenc.def`.

Build all Projects. Rename `vorbisenc.exe` to `vorbisenc.dll`.


## Compile x264 & ffmpeg
### Prep

Open Cross MSYS2 bash

Get the sources for x264 and ffmpeg
``` bash
mkdir tmp
cd tmp
mkdir sources
mkdir build
cd sources
 
git clone --depth 1 https://code.videolan.org/videolan/x264.git
git clone --depth 1 git://source.ffmpeg.org/ffmpeg.git
```

Fix the build scripts for x264  
https://github.com/FFmpeg/gas-preprocessor  
Replace `gas-preprocessor.pl` at `x264/tools`, then have another copy at `/usr/bin`

### Compile x264
``` bash
cd tmp/build
mkdir x264
cd x264

CC=cl ./../../sources/x264/configure --prefix=./../../installed --enable-shared --disable-avs --disable-ffms --disable-gpac --disable-interlaced --disable-lavf
make -j 8
make install
mv ./../../installed/lib/libx264.dll.lib ./../../installed/lib/libx264.lib
```

### Compile ffmpeg  
Apply patch first.  
Add previously compiled `zlib.dll`, `zlib.lib` and `libpng16-16.dll` to `installed/bin/`.  
Add previously compiled `zlib.dll` and `zlib.lib` to `installed/lib/`.  
Add `png.h`, `pngconf.h` and `pnglibconf.h` to `installed/include/libpng16/`.  
Add `zconf.h` and `zlib.h` to `installed/include/`.  

``` bash
cd tmp/build
mkdir ffmpeg
cd ffmpeg

export CC=cl

./../../sources/ffmpeg/configure --prefix=./../../installed --toolchain=msvc --arch=aarch64 --target-os=win32 --enable-asm --enable-shared --disable-postproc --disable-programs  --enable-decoder=png --enable-zlib --enable-libx264 --enable-gpl --extra-ldflags="-LIBPATH:./../../installed/lib/"   --extra-cflags="-I./../../installed/include/" --enable-cross-compile --enable-runtime-cpudetect --enable-d3d11va --disable-dxva2 --disable-doc --disable-debug


make V=1 -j
make install
```

## Compile curl  
https://medium.com/@chuy.max/compile-libcurl-on-windows-with-visual-studio-2017-x64-and-ssl-winssl-cff41ac7971d


## Compile Freetype  
open `builds\windows\vc2010\freetype.sln`  
new solution platform ARM64, Copy settings from x64  
Switch to Release Static solution configuration  
Right click `freetype` **project**, then click properties  
Click Librarian on the left, change Target Machine to MachineARM64  

## Compile Detours
https://github.com/Microsoft/Detours
Use the built-in sln.

## Compile libSpeex
https://gitlab.xiph.org/xiph/speexdsp
Use the build-in sln.

## Prepare Python
Grab the latest release from https://github.com/jay0lee/CPython-Windows-ARM64.
Copy the `include` folder to `DepsARM64\include`, then rename it to `python`.
Copy `python3.lib` and `python39.lib` to `DepsARM64\bin`.
It may complain about `Pyconfig.h` not found. Grab one from https://github.com/python/cpython/blob/main/PC/pyconfig.h.


## Prepare swig
From the Intel version of deps, copy the entire `swig` folder over.
`swig` is a compile-time tool, and it's not linked or bundled to the artifact.

## Prepare CEF

Download CEF at https://cef-builds.spotifycdn.com/index.html#windowsarm64

unzip, create a folder `build` inside.
Run CMake and point the build directory to the `build` folder created last step, build the wrapper.

## Gather files  
Gather the binaries in `bin` folder and headers in `include` folder.   

<details> <summary>It should be look like this:</summary>

```
DepsARM64
│   tree.txt
│   
├───bin
│       avcodec-58.dll
│       avcodec.lib
│       avdevice-58.dll
│       avdevice.lib
│       avfilter-7.dll
│       avfilter.lib
│       avformat-58.dll
│       avformat.lib
│       avutil-56.dll
│       avutil.lib
│       cmocka.dll
│       cmocka.lib
│       detours.lib
│       freetype.lib
│       libcurl.dll
│       libcurl.lib
│       libmbedcrypto.dll
│       libogg.dll
│       libopus-0.dll
│       libpng16-16.dll
│       libsrt.dll
│       libvorbis-0.dll
│       libvorbisenc-2.dll
│       libvorbisfile-3.dll
│       libx264-161.dll
│       libx264.lib
│       mbedcrypto.lib
│       mbedtls.lib
│       mbedx509.lib
│       python3.lib
│       python39.lib
│       swresample-3.dll
│       swresample.lib
│       swscale-5.dll
│       swscale.lib
│       x264.exe
│       zlib.dll
│       zlib.lib
│       
├───cef
│   │   ...
│               
├───include
│   │   detours.h
│   │   ft2build.h
│   │   x264.h
│   │   x264_config.h
│   │   zconf.h
│   │   zlib.h
│   │   
│   ├───cmocka
│   │       cmocka.h
│   │       cmocka_pbc.h
│   │       
│   ├───curl
│   │       curl.h
│   │       curlver.h
│   │       easy.h
│   │       mprintf.h
│   │       multi.h
│   │       options.h
│   │       stdcheaders.h
│   │       system.h
│   │       typecheck-gcc.h
│   │       urlapi.h
│   │       
│   ├───freetype
│   │   │   freetype.h
│   │   │   ftadvanc.h
│   │   │   ftbbox.h
│   │   │   ftbdf.h
│   │   │   ftbitmap.h
│   │   │   ftbzip2.h
│   │   │   ftcache.h
│   │   │   ftchapters.h
│   │   │   ftcid.h
│   │   │   ftcolor.h
│   │   │   ftdriver.h
│   │   │   fterrdef.h
│   │   │   fterrors.h
│   │   │   ftfntfmt.h
│   │   │   ftgasp.h
│   │   │   ftglyph.h
│   │   │   ftgxval.h
│   │   │   ftgzip.h
│   │   │   ftimage.h
│   │   │   ftincrem.h
│   │   │   ftlcdfil.h
│   │   │   ftlist.h
│   │   │   ftlzw.h
│   │   │   ftmac.h
│   │   │   ftmm.h
│   │   │   ftmodapi.h
│   │   │   ftmoderr.h
│   │   │   ftotval.h
│   │   │   ftoutln.h
│   │   │   ftparams.h
│   │   │   ftpfr.h
│   │   │   ftrender.h
│   │   │   ftsizes.h
│   │   │   ftsnames.h
│   │   │   ftstroke.h
│   │   │   ftsynth.h
│   │   │   ftsystem.h
│   │   │   fttrigon.h
│   │   │   fttypes.h
│   │   │   ftwinfnt.h
│   │   │   t1tables.h
│   │   │   ttnameid.h
│   │   │   tttables.h
│   │   │   tttags.h
│   │   │   
│   │   ├───config
│   │   │       ftconfig.h
│   │   │       ftheader.h
│   │   │       ftmodule.h
│   │   │       ftoption.h
│   │   │       ftstdlib.h
│   │   │       integer-types.h
│   │   │       mac-support.h
│   │   │       public-macros.h
│   │   │       
│   │   └───internal
│   │       │   autohint.h
│   │       │   cffotypes.h
│   │       │   cfftypes.h
│   │       │   compiler-macros.h
│   │       │   ftcalc.h
│   │       │   ftdebug.h
│   │       │   ftdrv.h
│   │       │   ftgloadr.h
│   │       │   fthash.h
│   │       │   ftmemory.h
│   │       │   ftobjs.h
│   │       │   ftpsprop.h
│   │       │   ftrfork.h
│   │       │   ftserv.h
│   │       │   ftstream.h
│   │       │   fttrace.h
│   │       │   ftvalid.h
│   │       │   psaux.h
│   │       │   pshints.h
│   │       │   sfnt.h
│   │       │   t1types.h
│   │       │   tttypes.h
│   │       │   wofftypes.h
│   │       │   
│   │       └───services
│   │               svbdf.h
│   │               svcfftl.h
│   │               svcid.h
│   │               svfntfmt.h
│   │               svgldict.h
│   │               svgxval.h
│   │               svkern.h
│   │               svmetric.h
│   │               svmm.h
│   │               svotval.h
│   │               svpfr.h
│   │               svpostnm.h
│   │               svprop.h
│   │               svpscmap.h
│   │               svpsinfo.h
│   │               svsfnt.h
│   │               svttcmap.h
│   │               svtteng.h
│   │               svttglyf.h
│   │               svwinfnt.h
│   │               
│   ├───libavcodec
│   │       ac3_parser.h
│   │       adts_parser.h
│   │       avcodec.h
│   │       avdct.h
│   │       avfft.h
│   │       bsf.h
│   │       codec.h
│   │       codec_desc.h
│   │       codec_id.h
│   │       codec_par.h
│   │       d3d11va.h
│   │       dirac.h
│   │       dv_profile.h
│   │       dxva2.h
│   │       jni.h
│   │       mediacodec.h
│   │       packet.h
│   │       qsv.h
│   │       vaapi.h
│   │       vdpau.h
│   │       version.h
│   │       videotoolbox.h
│   │       vorbis_parser.h
│   │       xvmc.h
│   │       
│   ├───libavdevice
│   │       avdevice.h
│   │       version.h
│   │       
│   ├───libavfilter
│   │       avfilter.h
│   │       buffersink.h
│   │       buffersrc.h
│   │       version.h
│   │       
│   ├───libavformat
│   │       avformat.h
│   │       avio.h
│   │       version.h
│   │       
│   │       
│   ├───libavutil
│   │       adler32.h
│   │       aes.h
│   │       aes_ctr.h
│   │       attributes.h
│   │       audio_fifo.h
│   │       avassert.h
│   │       avconfig.h
│   │       avstring.h
│   │       avutil.h
│   │       base64.h
│   │       blowfish.h
│   │       bprint.h
│   │       bswap.h
│   │       buffer.h
│   │       camellia.h
│   │       cast5.h
│   │       channel_layout.h
│   │       common.h
│   │       cpu.h
│   │       crc.h
│   │       des.h
│   │       dict.h
│   │       display.h
│   │       dovi_meta.h
│   │       downmix_info.h
│   │       encryption_info.h
│   │       error.h
│   │       eval.h
│   │       ffversion.h
│   │       fifo.h
│   │       file.h
│   │       film_grain_params.h
│   │       frame.h
│   │       hash.h
│   │       hdr_dynamic_metadata.h
│   │       hmac.h
│   │       hwcontext.h
│   │       hwcontext_cuda.h
│   │       hwcontext_d3d11va.h
│   │       hwcontext_drm.h
│   │       hwcontext_dxva2.h
│   │       hwcontext_mediacodec.h
│   │       hwcontext_opencl.h
│   │       hwcontext_qsv.h
│   │       hwcontext_vaapi.h
│   │       hwcontext_vdpau.h
│   │       hwcontext_videotoolbox.h
│   │       hwcontext_vulkan.h
│   │       imgutils.h
│   │       intfloat.h
│   │       intreadwrite.h
│   │       lfg.h
│   │       log.h
│   │       lzo.h
│   │       macros.h
│   │       mastering_display_metadata.h
│   │       mathematics.h
│   │       md5.h
│   │       mem.h
│   │       motion_vector.h
│   │       murmur3.h
│   │       opt.h
│   │       parseutils.h
│   │       pixdesc.h
│   │       pixelutils.h
│   │       pixfmt.h
│   │       random_seed.h
│   │       rational.h
│   │       rc4.h
│   │       replaygain.h
│   │       ripemd.h
│   │       samplefmt.h
│   │       sha.h
│   │       sha512.h
│   │       spherical.h
│   │       stereo3d.h
│   │       tea.h
│   │       threadmessage.h
│   │       time.h
│   │       timecode.h
│   │       timestamp.h
│   │       tree.h
│   │       twofish.h
│   │       tx.h
│   │       version.h
│   │       video_enc_params.h
│   │       xtea.h
│   │       
│   ├───libpng16
│   │       png.h
│   │       pngconf.h
│   │       pnglibconf.h
│   │       
│   ├───libswresample
│   │       swresample.h
│   │       version.h
│   │       
│   ├───libswscale
│   │       swscale.h
│   │       version.h
│   │       
│   ├───mbedtls
│   │       aes.h
│   │       aesni.h
│   │       arc4.h
│   │       aria.h
│   │       asn1.h
│   │       asn1write.h
│   │       base64.h
│   │       bignum.h
│   │       blowfish.h
│   │       bn_mul.h
│   │       camellia.h
│   │       ccm.h
│   │       certs.h
│   │       chacha20.h
│   │       chachapoly.h
│   │       check_config.h
│   │       cipher.h
│   │       cipher_internal.h
│   │       cmac.h
│   │       compat-1.3.h
│   │       config.h
│   │       config_psa.h
│   │       ctr_drbg.h
│   │       debug.h
│   │       des.h
│   │       dhm.h
│   │       ecdh.h
│   │       ecdsa.h
│   │       ecjpake.h
│   │       ecp.h
│   │       ecp_internal.h
│   │       entropy.h
│   │       entropy_poll.h
│   │       error.h
│   │       gcm.h
│   │       havege.h
│   │       hkdf.h
│   │       hmac_drbg.h
│   │       md.h
│   │       md2.h
│   │       md4.h
│   │       md5.h
│   │       md_internal.h
│   │       memory_buffer_alloc.h
│   │       net.h
│   │       net_sockets.h
│   │       nist_kw.h
│   │       oid.h
│   │       padlock.h
│   │       pem.h
│   │       pk.h
│   │       pkcs11.h
│   │       pkcs12.h
│   │       pkcs5.h
│   │       pk_internal.h
│   │       platform.h
│   │       platform_time.h
│   │       platform_util.h
│   │       poly1305.h
│   │       psa_util.h
│   │       ripemd160.h
│   │       rsa.h
│   │       rsa_internal.h
│   │       sha1.h
│   │       sha256.h
│   │       sha512.h
│   │       ssl.h
│   │       ssl_cache.h
│   │       ssl_ciphersuites.h
│   │       ssl_cookie.h
│   │       ssl_internal.h
│   │       ssl_ticket.h
│   │       threading.h
│   │       threading_alt.h
│   │       timing.h
│   │       version.h
│   │       x509.h
│   │       x509_crl.h
│   │       x509_crt.h
│   │       x509_csr.h
│   │       xtea.h
│   │       
│   ├───ogg
│   │       config_types.h.in
│   │       Makefile.am
│   │       ogg.h
│   │       os_types.h
│   │       
│   ├───opus
│   │       opus.h
│   │       opus_custom.h
│   │       opus_defines.h
│   │       opus_multistream.h
│   │       opus_projection.h
│   │       opus_types.h
│   │       
│   ├───psa
│   │       crypto.h
│   │       crypto_accel_driver.h
│   │       crypto_compat.h
│   │       crypto_config.h
│   │       crypto_driver_common.h
│   │       crypto_entropy_driver.h
│   │       crypto_extra.h
│   │       crypto_platform.h
│   │       crypto_se_driver.h
│   │       crypto_sizes.h
│   │       crypto_struct.h
│   │       crypto_types.h
│   │       crypto_values.h
│   │       
│   ├───python
│   │   │   abstract.h
│   │   │   asdl.h
│   │   │   ast.h
│   │   │   bitset.h
│   │   │   bltinmodule.h
│   │   │   boolobject.h
│   │   │   bytearrayobject.h
│   │   │   bytesobject.h
│   │   │   cellobject.h
│   │   │   ceval.h
│   │   │   classobject.h
│   │   │   code.h
│   │   │   codecs.h
│   │   │   compile.h
│   │   │   complexobject.h
│   │   │   context.h
│   │   │   datetime.h
│   │   │   descrobject.h
│   │   │   dictobject.h
│   │   │   dynamic_annotations.h
│   │   │   enumobject.h
│   │   │   errcode.h
│   │   │   eval.h
│   │   │   exports.h
│   │   │   fileobject.h
│   │   │   fileutils.h
│   │   │   floatobject.h
│   │   │   frameobject.h
│   │   │   funcobject.h
│   │   │   genericaliasobject.h
│   │   │   genobject.h
│   │   │   graminit.h
│   │   │   grammar.h
│   │   │   import.h
│   │   │   interpreteridobject.h
│   │   │   intrcheck.h
│   │   │   iterobject.h
│   │   │   listobject.h
│   │   │   longintrepr.h
│   │   │   longobject.h
│   │   │   marshal.h
│   │   │   memoryobject.h
│   │   │   methodobject.h
│   │   │   modsupport.h
│   │   │   moduleobject.h
│   │   │   namespaceobject.h
│   │   │   node.h
│   │   │   object.h
│   │   │   objimpl.h
│   │   │   odictobject.h
│   │   │   opcode.h
│   │   │   osdefs.h
│   │   │   osmodule.h
│   │   │   parsetok.h
│   │   │   patchlevel.h
│   │   │   picklebufobject.h
│   │   │   pyarena.h
│   │   │   pycapsule.h
│   │   │   pyctype.h
│   │   │   pydebug.h
│   │   │   pydtrace.d
│   │   │   pydtrace.h
│   │   │   pyerrors.h
│   │   │   pyexpat.h
│   │   │   pyfpe.h
│   │   │   pyframe.h
│   │   │   pyhash.h
│   │   │   pylifecycle.h
│   │   │   pymacconfig.h
│   │   │   pymacro.h
│   │   │   pymath.h
│   │   │   pymem.h
│   │   │   pyport.h
│   │   │   pystate.h
│   │   │   pystrcmp.h
│   │   │   pystrhex.h
│   │   │   pystrtod.h
│   │   │   Python-ast.h
│   │   │   Python.h
│   │   │   pythonrun.h
│   │   │   pythread.h
│   │   │   pytime.h
│   │   │   py_curses.h
│   │   │   rangeobject.h
│   │   │   setobject.h
│   │   │   sliceobject.h
│   │   │   structmember.h
│   │   │   structseq.h
│   │   │   symtable.h
│   │   │   sysmodule.h
│   │   │   token.h
│   │   │   traceback.h
│   │   │   tracemalloc.h
│   │   │   tupleobject.h
│   │   │   typeslots.h
│   │   │   ucnhash.h
│   │   │   unicodeobject.h
│   │   │   warnings.h
│   │   │   weakrefobject.h
│   │   │   
│   │   ├───cpython
│   │   │       abstract.h
│   │   │       bytearrayobject.h
│   │   │       bytesobject.h
│   │   │       ceval.h
│   │   │       code.h
│   │   │       dictobject.h
│   │   │       fileobject.h
│   │   │       fileutils.h
│   │   │       frameobject.h
│   │   │       import.h
│   │   │       initconfig.h
│   │   │       interpreteridobject.h
│   │   │       listobject.h
│   │   │       methodobject.h
│   │   │       object.h
│   │   │       objimpl.h
│   │   │       pyerrors.h
│   │   │       pylifecycle.h
│   │   │       pymem.h
│   │   │       pystate.h
│   │   │       sysmodule.h
│   │   │       traceback.h
│   │   │       tupleobject.h
│   │   │       unicodeobject.h
│   │   │       
│   │   └───internal
│   │           pegen_interface.h
│   │           pycore_abstract.h
│   │           pycore_accu.h
│   │           pycore_atomic.h
│   │           pycore_byteswap.h
│   │           pycore_bytes_methods.h
│   │           pycore_call.h
│   │           pycore_ceval.h
│   │           pycore_code.h
│   │           pycore_condvar.h
│   │           pycore_context.h
│   │           pycore_dtoa.h
│   │           pycore_fileutils.h
│   │           pycore_gc.h
│   │           pycore_getopt.h
│   │           pycore_gil.h
│   │           pycore_hamt.h
│   │           pycore_hashtable.h
│   │           pycore_import.h
│   │           pycore_initconfig.h
│   │           pycore_interp.h
│   │           pycore_object.h
│   │           pycore_pathconfig.h
│   │           pycore_pyerrors.h
│   │           pycore_pyhash.h
│   │           pycore_pylifecycle.h
│   │           pycore_pymem.h
│   │           pycore_pystate.h
│   │           pycore_runtime.h
│   │           pycore_sysmodule.h
│   │           pycore_traceback.h
│   │           pycore_tupleobject.h
│   │           pycore_warnings.h
│   │           
│   ├───srt
│   │   │   access_control.h
│   │   │   api.h
│   │   │   buffer.h
│   │   │   cache.h
│   │   │   channel.h
│   │   │   common.h
│   │   │   congctl.h
│   │   │   core.h
│   │   │   crypto.h
│   │   │   epoll.h
│   │   │   fec.h
│   │   │   group.h
│   │   │   handshake.h
│   │   │   list.h
│   │   │   logger_defs.h
│   │   │   logging.h
│   │   │   logging_api.h
│   │   │   md5.h
│   │   │   netinet_any.h
│   │   │   packet.h
│   │   │   packetfilter.h
│   │   │   packetfilter_api.h
│   │   │   packetfilter_builtin.h
│   │   │   platform_sys.h
│   │   │   queue.h
│   │   │   srt.h
│   │   │   srt_compat.h
│   │   │   sync.h
│   │   │   threadname.h
│   │   │   udt.h
│   │   │   utilities.h
│   │   │   window.h
│   │   │   
│   │   └───win
│   │           syslog_defs.h
│   │           unistd.h
│   │           wintime.h
│   │           
│   └───vorbis
│           codec.h
│           Makefile.am
│           vorbisenc.h
│           vorbisfile.h
│           
├───qt
│   |   ...
│               
├───swig
│   │   swig.exe
│   |   ...
│
├───VulkanSDK
│   ├───Include
    │   └───...
    │   vulkan-1.lib
```

</details>

# Compile OBS
## Generate `.sln`
Use CMake.

|Cache|Value|
|---:|:---|
|`DepsPath`|`DepsARM64`|
|`QTDIR`|`DepsARM64/qt`|
|`PYTHON_LIB`|`DepsARM64/bin/python310.lib`|
|`ENABLE_BROWSER`|`ON`|
|`CEF_ROOT_DIR`|`DepsARM64/cef`|


Note that CPython have support for Windows ARM64 only since version 3.9, while the x64 version of OBS Studio is still using version 3.6.

## Compile
Select `Release` or `RelWithDebInfo` configuration, and build. 
