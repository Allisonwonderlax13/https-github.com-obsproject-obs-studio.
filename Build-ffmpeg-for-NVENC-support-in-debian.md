These are the steps to build a ffmpeg deb package with NVENC support to make hardware acceleration available for OBS
Required an nVIDIA card with a reasonably updated driver

Install nv encoder headers matching your driver:
```
git clone --branch <version branch> https://github.com/FFmpeg/nv-codec-headers
cd nv-codec-headers
sudo make install
```

Uninstall existing ffmpeg if applicable

Get the ffmpeg source
```
apt source ffmpeg
```

add "--enable-nonfree --enable-nvenc" to the debian/rules file in the source directory with your favorite editor

Run debuild to run the package, takes a bit of time, grab a glass of water
```
debuild -us -uc -b
```

Install the generated packages that are in .. relative to where you ran debuild
```
sudo apt install ./*.deb
```
Note, the packages labeled "extra" might not install, you can just omit those

Now NVENC should be available in OBS, and of course to ffmpeg. Try encoding a video with -c:v nvenc_h264 see if it works.

