This guide contains ***every*** piece of dropped frames / disconnect / network related advice we can give. If you really truly honestly super duper actually 100% tried everything in this guide (including replacing hardware), and you ***still*** have issues then the problem is somewhere along the route between you and whichever server you are trying to stream to. In this case, there will not be anything you can do to resolve the issue as an end user, and it is recommended that you contact your Internet Service Provider (ISP). Also, please note that dropped frames are near impossible to be caused by OBS itself. This means if you just updated and you're seeing dropped frames, they are **not** related, even if you think they are.

"Dropped frames" means that your connection to the server isn't stable, or you can't keep up with your set bitrate. Because of this, the program was forced to drop some of the video frames in order to compensate. If you drop too many frames, you may be disconnected from the streaming server. Again, dropped frames are nearly impossible to be caused by OBS itself. You may also have connection problems such as random disconnections due to firewall / anti-virus / security software, routers, etc. If you want a more detailed, technical explanation on what dropped frames are, please check this post written by Jim [here](https://gist.github.com/jp9000/5793a3f4ae15913c858913d6a00824b7). 

This guide covers the most common solutions to these issues.


### Try changing servers

If you think the issue is not with your connection speed, the first thing you should do when trying to diagnose a dropped frames/disconnection issue is to try other ingest servers to see if you can get a stable connection. Sometimes the server you are trying to stream to is having an off day or is overloaded when you are trying to stream. Switching another server will resolve many dropped frames/connection issues. Just because the server you have selected is the "closest" to you, or pings the best to you, does not mean it will give you the best connection. In fact, there have been times where Europeans have found US servers most reliable to stream though. So give a wide variety of servers a try, and make sure you try several servers, not just one or two before you give up.

**TWITCH.TV USERS**: You can use [TwitchTest](https://r1ch.net/projects/twitchtest) to find the server that you have the best bandwidth to, and the max bit rate you can stream to that server at. For the best results, set Duration to Medium and uncheck any regions you're not in. After the test runs, look for the server with the highest quality rating. If two or more are tied, use the one with the most bitrate. Note that a quality score of at least 80 is the general baseline for a stable stream.

### Try lowering bitrate

The next thing to do is lower bitrate until the dropped frames stop. Network conditions aren't always the same from day to day, and what worked yesterday isn't guaranteed to work today. Sometimes there's just not much else you can do except lower bitrate to compensate for the poor connection at the given time.

### Don't stream over wireless

In many cases, wireless connections can cause issues because of their unstable nature. Streaming really requires a stable connection. Often wireless connections are fine, but if you have problems, then we are going to be very unlikely to be able to help you diagnose it if you're on a wireless just because it adds yet another variable. We recommend streaming on wired connections.

### Try another streaming service (Just as a test)

It can be helpful to try a different streaming service just to make sure the issue isn't with the provider you're trying to use. For example, if you are having connection problems with Twitch.tv, try streaming on YouTube.com or Hitbox.tv to see if you have the same issues. If the issues disappear, the problem might be with the streaming service. If the problem remains, then the issue is more likely with your connection in general.

### Check your firewall / router

If you are getting disconnected and you've already tried other servers, then another thing to check is your firewall/router/antivirus software and make sure that they're not interfering with the connection. If you suspect the problem is your firewall/router, make sure outbound TCP port number 1935 (the default port used for RTMP, but note that your service may use a different port) is allowed. Note that you do NOT need to use any kind of port-forwarding to stream.

### Check your anti-virus / internet security software

In some cases, anti-virus or firewall / security software can be a cause. You can usually temporarily disable it or add an exception for obs32.exe/obs64.exe to check to see whether it's the problem. If disabling it works, simply add an exception for obs32.exe/obs64.exe to your anti-virus and then re-enable it. (The process for adding an exception will vary, you will have to find out from the vendor's website or Google on how to do so. Remember to add exceptions for both 32 bit and 64 bit versions of OBS).

### Check bundled network software

In rare cases, some software/drivers/programs claiming to "optimize" or "enhance" your network connection can actually cause more problems. Try uninstalling any extra software / drivers related to your network card other than the core driver that needs to be installed for Windows. Avoid using any "optimization" or "tweak" programs.  Certain network cards come with custom configuration utilities (most notably Killer Networks) that can cause issues and need to be removed for a drivers only install.

### Speed Testing

Speed tests are a very rough estimate - they mean very little with regards to streaming. Just because a speed test says you have 5Mb/s upload doesn't mean you can upload to anything at a *stable* 5Mb/s. That's just not how the internet works unfortunately. You're never guaranteed to be able to maintain a stable connection to a server if the server or routing points to the server are unstable. Your "stable" bitrate is more likely about 70-75% of your "estimated" speed test upload (and that's only if you're not being throttled). If anything, a speed test will tell you the theoretical maximum speed that you could stream at under perfectly ideal conditions, but conditions are never perfect.

### Update Network Adapter Driver

In some rare cases, dropped frames can be caused by an old network adapter driver doing a poor job of handing the high speeds being consumed. It's not a bad idea to just check to see if there are any new drivers for your network adapter, just to be sure.

### Try the "new network code"

In the advanced settings of OBS Studio is an option to use new network code. Enabling this makes OBS use an event based API instead of non-blocking sockets. While this should technically behave no differently to the regular network code, some users have reported that this helps with their dropped frames or disconnection issues. The new network code also includes detailed logging of network events which can help when you're posting a log file.

### Check the Bind to IP settings

In Settings -> Advanced, there is a setting to Bind to IP. By and large, this should be left at Default and not changed unless you know exactly what you are doing and why you need to. Make sure the setting is correct (this usually means Default). If you bind it to a specific IP address, and then that IP address changes on your PC, OBS will fail to connect to any services.

### Bad router or bad networking hardware

Faulty hardware is usually quite rare, but if you suspect your hardware is malfunctioning, plug in to your modem directly, bypassing the router, so you can check to see if that's the issue. If you have another network card available (including the one in a laptop or other PC you might have lying around) try that plugged into the modem to check for network card problems on your streaming PC. Try using different Ethernet cables too.

### Contact your ISP

Lastly, it is common for Internet Service Providers (ISPs) to run maintenance or change things on their end that can cause you as a subscriber to have issues. If you've tried everything in this guide already, and none of it as helped, we recommend calling your ISP and explaining the issue to them. ***Be detailed***. This means telling them exactly what you're trying to do (stream to your streaming service (Twitch/YouTube/etc.) and what's happening (connection is unstable and dropping packets). They should help you to determine any issues. Again, to reiterate the previous message at the top of this guide, OBS *cannot* be the direct cause of connection issues or dropped frames. You can check the post written by Jim [here](https://gist.github.com/jp9000/5793a3f4ae15913c858913d6a00824b7). 