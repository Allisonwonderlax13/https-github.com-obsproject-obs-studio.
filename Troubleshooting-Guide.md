This section will be updated as new troubleshooting guides become available. See the table of contents for the planned topics and progress. If you have any suggestions or ideas for further topics, join us in #obsproject on irc.quakenet.org.

#### Table of Contents

Completed Sections:
* [Dropped Frames/Connection Issues](#dropped-frames-and-general-connection-issues)
* [Stream Buffering/Lag](#stream-buffering-issues)
* [Laptop Performance/Black Screen](#laptop-performance-issues)

Work in Progress:
* [Source Capture Issues/black screen](#source-capture-issues)
* [Audio Capture Issues](#audio-capture-issues)
* [USB Device Issues](#usb-device-issues)
* [Buffering](#buffering)
* [Encoding Issues](#encoding-issues)

# Dropped Frames and General Connection Issues
This guide contains ***every*** piece of dropped frames / disconnect / network related advice we can give. If you really truly honestly super duper actually 100% tried everything in this guide (including replacing hardware), and you ***still*** have issues then the problem is somewhere along the route between you and whichever server you are trying to stream to. In this case, there will not be anything you can do to resolve the issue as an end user, and it is recommended that you contact your Internet Service Provider (ISP). Also, please note that dropped frames are near impossible to be caused by OBS itself. This means if you just updated and you're seeing dropped frames, they are **not** related, even if you think they are.

"Dropped frames" means that your connection to the server isn't stable, or you can't keep up with your set bitrate. Because of this, the program was forced to drop some of the video frames in order to compensate. If you drop too many frames, you may be disconnected from the streaming server. Again, dropped frames are nearly impossible to be caused by OBS itself. You may also have connection problems such as random disconnections due to firewall / anti-virus / security software, routers, etc. This guide covers the most common solutions to these issues.

### Try changing servers 

If you think the issue is not with your connection speed, the first thing you should do when trying to diagnose a dropped frames/disconnection issue is to try other ingest servers to see if you can get a stable connection. Sometimes the server you are trying to stream to is having an off day or is overloaded when you are trying to stream. Switching another server will resolve many dropped frames/connection issues. Just because the server you have selected is the "closest" to you, or pings the best to you, does not mean it will give you the best connection. In fact, there have been times where Europeans have found US servers most reliable to stream though. So give a wide variety of servers a try, and make sure you try several servers, not just one or two before you give up.

Twitch Users: You can use this tool to find the server that you have the best bandwidth to, and the max bit rate you can stream to that server at. For the best results, set Duration to Medium and uncheck any regions you're not in. After the test runs, look for the server with the highest quality rating. If two or more are tied, use the one with the most bitrate. Note that a quality score of at least 80 is the general baseline for a stable stream. TwitchTest can be downloaded [here](http://www.teamliquid.net/forum/tech-support/478845-twitchtest-twitch-bandwidth-tester) 

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

### Bad router or bad networking hardware
Faulty hardware is usually quite rare, but if you suspect your hardware is malfunctioning, plug in to your modem directly, bypassing the router, so you can check to see if that's the issue. If you have another network card available (including the one in a laptop or other PC you might have lying around) try that plugged into the modem to check for network card problems on your streaming PC. Try using different Ethernet cables too.

***

# Stream Buffering Issues
To answer this question we first have to ask another one: Are **we** dropping frames? Check the counter at the bottom of the OBS Studio main window:

![Dropped Frame Counter](http://i.imgur.com/ckL3wKb.png)

If this counter is showing dropped frames, the issue is likely with your own connection and you should try our [general connection issue troublehsooting](#dropped-frames-and-general-connection-issues) steps.

Often, you will not drop frames and still have viewers complaining about lag, buffering, or the stream constantly loading. Why is that and what can we do against it? First of all let us take a look at the **why**.

### Why does my stream lag/buffer/load for my viewers?
![Connection Paths](http://www.helping-squad.com/wp-content/uploads/2014/08/provider_differences_small.png)

Let's analyze the above picture, which shows two possible scenarios. 
- Provider A has no (or does not always do) balancing of its streams. This means that all streams are served to all viewers from a single server. Twitch.tv for instance, does not use its full Content Delivery Network (CDN) for non-partnered streams. This can lead to very mixed results. User Z can watch your stream just fine, because the route from your Provider to him is very fast or no server on the route is overloaded. But User X might experience problems. He could live in the same country as you, but if the route between him and the provider is too long or is overloaded, he might have problems watching your stream.
- Provider B has different servers all around the world (YouTube, for example) and can send the stream within their own system to their servers. When User Y asks to watch your stream, Provider B will automatically choose the best route (in most cases) to ensure there is no buffering or lag.

There are more ways for your streaming provider to handle the streams, but these two examples are the most commonly used. Provider C might use a combination of both systems, or some form of centralized load balancing other than a CDN.

There's also another simple explanation on why your stream might be buffering:

***YOU USED TOO MUCH BITRATE***

This is a very common mistake that new streamers make. Streamers will tend to use as much bitrate as they have upload available, with no regard to how that might affect their viewers. Of course, we understand you want your stream to look good. Upping your bitrate is a simple way to accomplish that, but it must be within reason. Check the information here provided by Akamai and summarized by OBS forum member RytoEX:

>According to [Akamai's Q4 2016 State of the Internet Connectivity Report](https://www.akamai.com/us/en/multimedia/documents/state-of-the-internet/q4-2016-state-of-the-internet-connectivity-report.pdf), in Q4 2016, 63% of Internet connections in USA were above 10 Mb/s. The average connection speed in USA was 17.2 Mb/s. Average mobile speeds in USA were 5.1 Mb/s. Even mobile users who have access to fast mobile networks would still need to be concerned about bitrate if they are on a data plan with limits and the stream(s) they are watching does not have transcoding.
>
>As bad as that may sound, especially when compared to South Korea or Singapore (or any other nation in the top 10 in any category), connections in much of the rest of the world are still further below those levels (most of the Asia Pacific region - including China and India - most of Europe, all of Africa, all of the Middle East, all of Central America, and all of South America). Russia's average Internet connection speed only clocks in at 11.6 Mb/s with 48% of their connections above 10 Mb/s. Germany's average average Internet connection speed is only 14.6 Mb/s with 50% of their connections above 10 Mb/s.

Basically, this means that just because you can upload 20mb/s constantly without dropping a frame, it does not mean your viewers will be able to download it. Most streaming services impose bitrate limits in part due to this.

In the end while your 1080p 60fps 9mb/s stream might look glorious, and 3 people can watch it fine, either your stream provider or the rest of your viewers very well might have issues. 

And finally...

### What can I do to fix this?
There is unfortunately no perfect cure for this. Let us make it clear once more: **Unless you drop frames, the stream you send out arrived at the server of your provider**. From this point, it is between the provider and your viewers and out of your control.

But we have a few options we can try:
- Lower your bitrate (and if necessary resolution/framerate)
- Try different servers of the same Provider (will probably not help, but especially with Twitch this sometimes can)
- Try a different Provider (might have a better balancing or content distribution)
- Accept that some viewers can encounter problems
- Try again later. The time of day, or the usage amounts of your Provider, can cause your viewers to watch the stream fine one day but not the next.

The Internet is a big amount of highways connected with junctions, at each of the junctions, something can possibly go wrong. We can only make sure to use reasonable bitrate values and not drop frames. Then it is on our providers to do the best they can.

***

# Laptop Performance Issues
Laptops are notoriously difficult to work with when it comes to capture applications like OBS Studio. Since most modern laptops will ship with two GPUs - typically an Intel iGPU and a discrete graphics chip (either NVIDIA or AMD) - this means that certain applications have the possibility to run on either GPU, where as OBS can only run on a single GPU at a time. An applications that is, say, running on the Intel GPU will not be able to be captured by OBS running on the discrete (NVIDIA or AMD) GPU. Additionally, if OBS is not running on the discrete GPU, you might run into performance issues. Unfortunately, this is not an issue with OBS, but rather a design choice by laptop manufacturers and there's little that can be done on our side. However, we do have several troubleshooting suggestions to try and assist with any issues.


### If you are getting a black screen with Window or Game Capture Sources or are otherwise having performance issues with OBS on your laptop, read the following:
#### If you have an NVIDIA discrete graphics card
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on "Manage 3D Settings" if it is not already selected
- Under the Program Settings tab, click the **Add** button under where it says "Select a program to customize:"
- Navigate to the .exe path for OBS and add it to the list
  - Default paths are: C:\Program Files (x86)\obs-studio\bin\32bit\obs32.exe and C:\Program Files (x86)\obs-studio\bin\64bit\obs64.exe)
- Make sure it is selected in the drop down list
- Then, under where it says "Select the preferred graphics processor for this program" open the drop down and select "High-performance NVIDIA processor"
- Re-open OBS and test again

Alternate directions, but note this sets the global default so all applications will run on the NVIDIA:
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on Manage 3D Settings on the left, and then the Global tab on the right
- Click the drop down box below that, select "High-performance NVIDIA processor" and click Apply, then OK
- Re-open OBS and test again

**NOTE:**
If you are planning on using QuickSync (QSV), Set QSVHelper.exe to run on the the Intel instead of the NVIDIA.

#### If you have an AMD discrete graphics card
A guide can be found here to set OBS to run on the proper GPU if you are experiencing issues:
- https://community.amd.com/docs/DOC-1581#jive_content_id_Configuring_Switchable_Graphics

### Window and Game Capture and "multi-adapter compatibility" mode
If you cannot set the GPU (AMD laptops typically), or wish to cross-capture an image from the other GPU after that (example, League of Legends lobby window), use Window or Game Capture with the "multi-adapter compatibility mode" option enabled to force a capture. "Multi-adapter compatibility mode" requires a bit more CPU usage, however.
Compatibility mode is not recommended for capturing games, but it basically guarantees a capture.

### Special note from Jim
I know it's annoying. I'm not happy that this is the case either. Unfortunately, there's nothing anyone can really do about it. This is just the way laptops are designed.