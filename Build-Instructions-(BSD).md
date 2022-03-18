# Build Directions

* The easiest way to build OBS Studio from source is to use the [FreeBSD Ports](https://www.freebsd.org/doc/handbook/ports-using.html) and modify the `multimedia/obs-studio` port to suit your needs.

* First you have to set up the ports infrastructure on your system. See the related chapter in the [FreeBSD Handbook](https://www.freebsd.org/doc/handbook/ports-using.html).

* Once you've got your ports tree at `/usr/ports` you may edit the `multimedia/obs-studio` port to your liking. Then, you may build and install the port with:

   ```bash
   cd /usr/ports/multimedia/obs-studio
   make install clean
   ```
