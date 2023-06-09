* [Dropped frames](#dropped-frames)
  * [Possible causes of dropped frames](#possible-causes-of-dropped-frames)
* [Solutions](#solutions)
  * Network-related
    * [Dynamic bitrate](#dynamic-bitrate)
    * [Lower your bitrate](#lower-your-bitrate)
    * [Change stream server](#change-stream-server)
    * [Test using a different service](#test-using-a-different-service)
    * [Enable network optimizations](#enable-network-optimizations)
    * [Bind to IP settings](#bind-to-ip-settings)
  * Software-related
    * [Check your anti-virus/internet security software](#check-your-anti-virusinternet-security-software)
    * [Check bundled network software](#check-bundled-network-software)
    * [Update network drivers](#update-network-drivers)
  * Hardware-related
    * [Avoid streaming over Wi-Fi](#avoid-streaming-over-wi-fi)
    * [Check your firewall/router](#check-your-firewallrouter)
    * [Bad router or bad networking hardware](#bad-router-or-bad-networking-hardware)
* [If you still cannot solve the issue](#if-you-still-cannot-solve-the-issue)
  * [Contact your ISP](#contact-your-isp)
  * [Get support](#get-support)

***

# Dropped frames

"Dropped frames" means that your connection to remote server isn't stable or you can't keep up with your set bitrate. Because of this, the program was forced to drop some of the video frames in order to compensate. If you drop too many frames, you may be disconnected from the streaming server.

Speed tests provide very rough estimate - they mean very little with regards to streaming. Just because a speed test says you have 5 Mb/s upload doesn't mean you can upload to anything at a *stable* 5Mb/s since connections across the internet rarely maintain such a consistent speed.

Your "stable" bitrate is more likely about 70-75% of your "estimated" speed test upload (and that's only if you're not being throttled). If anything, a speed test will tell you the theoretical maximum speed that you could stream at under perfectly ideal conditions, but conditions are never perfect.

## Possible causes of dropped frames

Possible reasons for dropped frames are:

* Your computer's firewall/anti-virus/security software interferes with the connection
* You are behind a firewall or proxy that moderates your connection's upload speed
* Your router is the slow, old, faulty, or misconfigured
* Somebody else or another of your devices on your network is using a lot of your upload bandwidth. For example:
  * There is a large file being uploaded
  * There are torrents actively being seeded on your network
  * Somebody else is streaming

### OBS Studio is unlikely to be the cause

It is nearly impossible for OBS Studio to cause dropped frames. Bear this in mind if you have recently updated OBS Studio and believe it is the cause of your dropped frames.

For a detailed, technical explanation on what dropped frames are, please check [this post](https://gist.github.com/jp9000/5793a3f4ae15913c858913d6a00824b7) written by Lain.

# Solutions

## Network-related

### Dynamic Bitrate

OBS Studio 24.0 introduced a new feature called Dynamic Bitrate. This feature detects when your internet connection is limited, and will automatically reduce your bitrate to compensate rather than dropping frames. Once any congestion disappears, it will automatically raise your bitrate back to its original value.

To enable Dynamic Bitrate, first ensure you are using OBS Studio 24 or higher by looking at your version number in the title bar of the program. Then, go to **Settings > Advanced > Network** and check the box next to the option that says "Dynamically change bitrate to manage congestion".

### Lower your bitrate

Network conditions aren't always the same from day to day; what worked yesterday is not guaranteed to work today. The bitrate you have selected may be too high for the current conditions.

* Lower your stream bitrate in `Settings -> Output -> Streaming`.
  * To maintain a good picture quality, you may also need to lower your output resolution/framerate in `Settings -> Video`.

### Change stream server

The server to which you are streaming might be overloaded. Switching another server will resolve many dropped frames/connection issues. Even the server the appears to be located closest to you or is shown to have the lowest ping may not give you the best connection. In fact, there have been times when Europeans have found US servers more reliable to stream though than ones located in Europe.

* Switch from a manually-selected service to Auto.
* Try selecting a few servers manually and comparing performance.
  * **Twitch users**: You can use [TwitchTest](https://r1ch.net/projects/twitchtest) to find the server to which you have the best connection, and the max bitrate at which you can stream to that server.
  * For the best results, set Duration to Medium and uncheck any regions you're not in. After the test runs, look for the server with the highest quality rating. If two or more are tied, use the one with the higher bitrate. Note that a quality score of at least 80 is the general baseline for a stable stream.

### Test using a different service

It can be helpful to try a different streaming service just to make sure the issue isn't with the stream service you're using.

* If you stream to Twitch, try streaming to YouTube or Hitbox instead.
  * If the issues disappear, the problem might be with the streaming service.
  * If the problem remains, then the issue is more likely with your connection in general.

### Enable network optimizations

The optional network optimizations setting makes OBS use an event-based API instead of non-blocking sockets. While this should technically behave no differently to the regular network code, some users have reported that this helps with their dropped frames or disconnection issues. The network optimizations code also includes detailed logging of network events which can help when you're posting a log file.

* On Windows only, go to `Settings -> Advanced -> Network` and click Enable network optimizations.

> Note: this setting is not available on macOS.

### Bind to IP settings

In Settings -> Advanced, there is a setting to Bind to IP. By and large, this should be left at Default and not changed unless you know exactly what you are doing and why you need to. Make sure the setting is correct (this usually means Default). If you bind it to a specific IP address, and then that IP address changes on your PC, OBS Studio will fail to connect to any services.

* Change `Settings -> Advanced -> Network -> Bind to IP` to Default.

## Software-related

### Check your anti-virus/internet security software

In some cases, anti-virus or firewall/security software can interfere with the connection.

* Try temporarily disabling your security software.
  * If disabling it works, simply add an exception for obs32.exe/obs64.exe (Windows) or OBS Studio.app (macOS) to your anti-virus and then re-enable it.

> The process for adding disabling your network and/or adding an exception will vary. Please consult the software vendor's website for instructions. Remember to add exceptions for both 32 bit and 64 bit versions of OBS Studio.

### Check bundled network software

In rare cases, some software/drivers/programs claiming to "optimize" or "enhance" your network connection can actually cause more problems. For instance, they might try to configure things that help with a game; however, that configuration might interfere with OBS Studio's operation.

* Try uninstalling any software related to your network card other than the core driver that needs to be installed for Windows.
* Avoid using any "optimization" or "tweak" programs.
* Certain network cards come with custom configuration utilities (most notably Killer Networks) that can cause issues and need to be removed for a driver-only install.

### Update network drivers

In some rare cases, dropped frames can be caused by an old network adapter driver doing a poor job of handing the high speeds being consumed.

* Check Windows Update for more recent drivers.
* On a Mac, the latest drivers come with the most up-to-date version of macOS.
* Check your computer manufacturer's website for more recent drivers.

## Hardware-related

### Avoid streaming over Wi-Fi

In many cases, Wi-Fi connections can cause issues because of their unstable nature. Streaming requires a stable connection.

* If possible, switch to a wired connection such as over Ethernet.

### Check your firewall/router

If you are getting disconnected and you've already tried other servers, check that your firewall/router/antivirus software is not interfering with the connection. 

* If you suspect the problem is your firewall/router, make sure outbound TCP port number 1935 (the default port used for RTMP, but note that your service may use a different port) is allowed.

> Note: you do **not** need to use any kind of port-forwarding to stream.

### Bad router or bad networking hardware

As a cause of dropped frames, faulty hardware is usually quite rare. This can include the router, any repeaters, the network card in your computer, or even the cabling. If you have another network card available (including the one in a laptop or other PC you might have lying around) try that plugged into the modem to check for network card problems on your streaming PC. Try using different Ethernet cables too.

* Try bypassing the router by connecting your computer directly to your modem, if possible.
* If you have another network card, try using that.
* If you are using a Ethernet connection, try using a different cable.

***

# If you still cannot solve the issue

Remember, OBS is unlikely be the direct cause of connection issues or dropped frames. If you have tried all of the above and still have issues, the problem may be out of your direct control.

## Contact your ISP

Tt is common for Internet Service Providers (ISPs) to run maintenance or change things on their end that can cause you as a subscriber to have issues. Your connection may also be throttled by the ISP.

* Contact your ISP and explaining the issue to them.
  * ***Be detailed***. Tell them exactly what you're trying to do (stream to your streaming service (Twitch/YouTube/etc.) and what's happening (connection is unstable and dropping packets).

## Get support

***If you tried everything in this guide and are still having issues, please make a post on the [forums or stop by the OBS Discord server](https://obsproject.com/help).***
