_**This feature requires OBS Studio 25.0 or newer.**_

***

### Table of Contents:
* [General Overview](#general-overview)
* [Can SRT be used with Twitch or my favorite service?](#can-srt-be-used-with-twitch-or-my-favorite-service)
  * [Services](#services)
  * [Encoders](#encoders)
  * [Servers](#servers)
  * [Players](#players)
* [How to set up OBS Studio](#how-to-set-up-obs-studio)
  * [Option 1: Stream SRT using the Streaming output](#option-1-stream-srt-using-the-streaming-output)
  * [Option 2: Stream SRT using the Custom FFmpeg Record output](#option-2-stream-srt-with-the-custom-ffmpeg-record-output)    
* [Examples of setups](#examples-of-setups)
  * [Relay server to Twitch](#relay-server-to-twitch)

***

# General Overview
* Secure Reliable Transport (or SRT in short) is a relatively recent open source streaming protocol, originally developped by Haivision (first demo in 2013) and promoted by the [SRT Alliance](https://www.srtalliance.org/) which includes many big players in the streaming/video/telco industry. It promises:  
(1) better resilience to network issues (jitter, lost packets, delay, bandwidth fluctuations) with mechanisms for packet recovery (retransmission) and  
(2) low latency (as low as twice the round-trip between encoder and ingest server, with sub-second latency usually).  
  
For a video demo, see here:  
<p align="center"> <a href="https://www.youtube.com/watch?v=I6G2iaTMuQo"><img src="https://img.youtube.com/vi/I6G2iaTMuQo/0.jpg"></img></a></p> 

* SRT is mostly used in the broadcast and corporate world at the moment. See the NAB 2018 SRT panel with ESPN, NFL, Microsoft speakers talking about their use of SRT:   
<p align="center"> <a href="https://www.youtube.com/watch?v=PuGn2cjIt04"><img src="https://img.youtube.com/vi/PuGn2cjIt04/0.jpg"></img></a></p>    

 The NAB 2019 NAB SRT panel can be watched here: [https://www.haivision.com/resources/webinars/broadcast-panel](https://www.haivision.com/resources/webinars/broadcast-panel). (Hey Haivision, friendly suggestion: it'd be nice to have this vid posted on YouTube instead of having to enter personal info! :P )

* Unlike RTMP, SRT is an open source protocol, and the source code can be found on [GitHub](https://github.com/Haivision/srt). While RTMP development has been abandoned since 2012, SRT development is still very much active.

* As a protocol, it is content agnostic, although the industry uses it along with an MPEGTS container, which is the de facto standard in broadcast industry. (In terms of comparison, RTMP protocol relies on the FLV container.) The MPEGTS container is usually used along with UDP protocol, which makes it fast, but very unreliable and prone to packet loss. It can also be used with TCP, which is more reliable but has larger latency. SRT adds up to these two protocols to transport MPEGTS, with the best of two worlds: the reliability of TCP, and the lower latency of UDP. It also supports encryption and bonding.

* Other competing new protocols are WebRTC, Zixi (closed source) and RIST; the latter two are quite similar to SRT and all go beyond RTMP. 

* For further technical details, we recommend this video by Alex Converse, a Twitch engineer:
     <p align="center"> <a href="https://youtu.be/m1FABQtAjgU"><img src="https://img.youtube.com/vi/m1FABQtAjgU/0.jpg"></img></a></p>  
There is also a white paper which can be found [here](https://www.haivision.com/resources/white-paper/srt-protocol-technical-overview/)
or [there](https://github.com/Haivision/srt/issues/479). The API is fully documented on [GitHub](https://github.com/Haivision/srt).  
A very good source of info is the [SRT Cookbook](https://srtlab.github.io/srt-cookbook/).


***

# Can SRT be used with Twitch or my favorite service?
_Short answer:_ **NO** (or not yet?)
## Services
_Long answer:_ **None** of the main streaming services support the SRT protocol for ingest. Most still use RTMP (Twitch, YouTube, Facebook...). (Mixer though relies on WebRTC through its proprietary FTL protocol which OBS already supports). If you're using exclusively these services, no need to read further.

At this stage of the adoption of SRT protocol, you'll have to be technically inclined if you want to use SRT. If you are able to set up your own streaming server, maybe redirecting your streams to the main services like Twitch or YouTube and are interested in achieving low-latency with improved network resilience, read on.

The other category of users who could potentially be interested belong obviously to the professional broadcast industry. This wiki entry can be considered as fairly advanced in that it requires access to a server and being able to set it up.

The configuration of OBS itself ranges from easy to medium in terms of difficulty. The server setup is more challenging since it requires system/network admin knowledge.

## Encoders
Live software encoders : 
* **FFmpeg**,
* **OBS Studio** which relies on the FFmpeg libraries,
* [vMix](https://www.vmix.com/),
* srt-live-transmit (which is a demo app from libsrt developpers; needs to be compiled from source)  
* [Larix Broadcaster](https://softvelum.com/larix/)/Screencaster which can broadcast on Android or iOS.   
Hardware encoders are also available at various vendors.

  
## Servers
The following servers support SRT ingest:
* [Wowza](https://www.wowza.com/) (paid service)
  * Supports SRT ingest and transmuxing/distributing in RTMP/HLS/DASH
* [Nimble Streamer](https://wmspanel.com/nimble) (free, closed source)
  * While Nimble Streamer is nominally free, it is used along with a non free dashboard which is, in all fairness, quite convenient. But it can also be used without the dashboard and just requires modifying a JSON config file.
  * Supports SRT ingest and transmuxing/distributing in RTMP/HLS/DASH
* [SRT Live Server](https://github.com/Edward-Wu/srt-live-server) (free, open source)
  * This option only *serves* SRT streams and does **not** transmux to HLS/RTMP/DASH. It is much more rudimentary than the other servers at this stage but it is FOSS and works fine with OBS Studio in our tests.

Additionally, though it is technically not a server, FFmpeg can be used in listener mode to ingest an SRT stream. It won't be able to serve the stream as a real genuine server would do. But it could be used to transmux to RTMP and route to nginx-rtmp for instance, which can then handle the ingest to Twitch/YouTube/Facebook/etc.  
ex: `ffmpeg -i srt://IP:port?mode=listener -c copy -f flv rtmp://IP:1935/app/streamName`.    
In the same way srt-live-transmit can be used to listen to an srt (or udp) stream and relay to a final srt URL.    
ex: `srt-live-transmit srt://IPsrc:port srt://IPdest:port`.    

## Players
The following players can be used to watch an SRT stream :
* [VLC](https://www.videolan.org/vlc/index.html) (version 3.0+ on Mac/Linux, version 4.0+ on Windows)
* ffplay (part of [ffmpeg tools](http://ffmpeg.org/download.html))
* OBS Studio Media Source (an obvious case-use is to broadcast from any SRT source to an OBS instance).
* [Haivision Play Pro](https://apps.apple.com/tr/app/haivision-play-pro/id1482925169) (iPhone only)
* [Larix Player](https://softvelum.com/player/) for Android, Android TV and iOS.

### Receive srt stream within OBS
This could be useful to two pc setups (although NDI is probably a more common solution).    
In a Media Source, uncheck 'Local File'.    
For 'Input', enter the srt URL. If the stream is received from a server (in listemer mode), the srt connexion will be in mode=caller (which is the default one so the option can be omitted). If however the stream is received straight from an encoder in caller mode, add the mode=listener to the URL (see screenshot).    
For 'Input Format', enter mpegts.

![](https://i.imgur.com/CMS57Jx.png)

### VLC usage

Download VLC 3.0 [here](https://www.videolan.org/vlc/index.html) or VLC 4.0 [here](https://nightlies.videolan.org/) (warning: this is the development version of VLC).  
If you just want to test without disturbing your current VLC install, we advise you to download a portable install (zip).

Go to `Media > Open Network Stream`:

![](https://i.imgur.com/SJmLpVf.png)

enter the SRT IP which has the form `srt://IP:port`.

![](https://i.imgur.com/kxPnPVv.png) 

### ffplay usage

It is required that ffplay be compiled with libsrt support. To the extent of our knowledge, there does not seem to be any such binary widely available, although there are no license constraints.

Just launch the command-line:  
`ffplay srt://IP:port`

# How to set up OBS Studio

There are two ways of setting up OBS Studio to connect to a server. The first is simpler but gives less options at the moment. The second is a bit more difficult to setup but gives more fine tuning capabilities (and at the moment of this writing is more stable). Note: ffmpeg may not come with SRT support in older distributions of Linux, so check the repository sources to ensure that ffmpeg comes with libsrt support, as there is no easy way of getting OBS Studio to reference a custom build of ffmpeg.

**Note that while the discussion focuses on SRT protocol, UDP or TCP can also be used instead.**

## Option 1: Stream SRT using the Streaming output
_Credit_: Aaron Boxer, Collabora (SRT Alliance) author of the new SRT output

1. Go to `Settings > Stream`
2. In the `Service` drowdown, select `Custom`.
3. Enter the SRT URL in the form: `srt://IP:port` (OBS Studio will also accept any protocol relying on MPEGTS container and supported by FFmpeg, therefore UDP, TCP, RTP, etc.)

![](https://i.imgur.com/ZPT2PXm.png)

OBS Studio will accept options in the syntax: `srt://IP:port?option1=value1&option2=value2`. The full list of options is those supported by FFmpeg: [http://ffmpeg.org/ffmpeg-protocols.html#srt](http://ffmpeg.org/ffmpeg-protocols.html#srt).

The most important option is **latency** in microseconds (μs). It has a default value of 120 ms = 120 000 μs and should be at least 2.5 * (the round-trip time between encoder and ingest server, in ms).    
Ex: for a latency of 1 sec, set latency=1000000 .    

Another sometimes required option is the **mode**, which can be `caller`, `listener` or `rendez-vous`. `caller` opens client connection. `listener` starts server to listen for incoming connections. `rendezvous` use Rendez-Vous connection mode which is a bi-directional link where the first to initiate handshake is considered caller. The default value is caller and usually need not be set for OBS Studio since it'll be in caller mode normally.

A case where it's useful to set the mode to `listener` is when sending a stream to VLC. OBS Studio then acts as a server to VLC, which is the client. On a LAN for instance, set OBS Studio to `srt://127.0.0.1:port?mode=listener` to establish a connection to VLC which you point to `srt://127.0.0.1:port`.

![](https://i.imgur.com/PhH7why.png)

**Known issues:**
* At this time (v25 RC2 and later), a bug with MPEGTS muxer makes the stream not completely compliant with MPEGTS spec. In this case, SRT decoding fails when transmuxing SRT to another protocol/container than the combination SRT/MPEGTS. This is the case for instance with Nimble Streamer and Makito X Decoder. A dump of the MPEGTS stream therefore creates a non-conformant file (this can probably be fixed though by a remuxing with FFmpeg). This bug is under investigation.    
    * bug fixed in this PR: [PR 2665](https://github.com/obsproject/obs-studio/pull/2665)  
* However, transmuxing to RTMP works with Wowza, and more generally to UDP or TCP /MPEGTS. In particular, VLC, Wowza, SRT Live Server, and ffplay work well when selecting this simpler option to stream with SRT.

## Option 2: Stream SRT with the Custom FFmpeg Record output

This option is a bit more complicated. It relies on the `Advanced: Custom FFmpeg Recording` output.

1. Go to `Settings > Output`
2. In the `Output mode` dropdown, select `Advanced`.
3. Click on `Recording Tab`.
4. In the `Type` dropdown, select `Custom Output (FFmpeg)`
5. In the `FFmpeg Output Type` dropdown, select `Output to URL`
6. In the `File Path or URL` box, type the SRT URL: `srt://IP:port` (options like latency are entered with the syntax `srt://IP:port?option1=value1&option2=value2`).  
For a list of some of these options and a discussion, see the previous section or refer to FFmpeg documentation : [http://ffmpeg.org/ffmpeg-protocols.html#srt](http://ffmpeg.org/ffmpeg-protocols.html#srt).   
7. In `Container Format` dropdown, select `mpegts`.
8. `Muxer Settings` can be left blank, or you can use the [MPEGTS FFmpeg muxer options](http://ffmpeg.org/ffmpeg-formats.html#mpegts-1) with the following syntax `option1=value1 option2=value2` (the `option=value` pairs must be separated by a space).
9. The other settings are self-explanatory. Check the box `show all codecs` to display all codecs available to FFmpeg.  
 
Note that several audio tracks can be selected. They can be identified on the ingest server side by what is called a PID. On default value, the video track has `pid 0x100` (=256), and the other audio tracks have `pid 0x101` etc. If you need to change the pid of your tracks, use the muxer option MPEGTS_start_pid in 7.    

![](https://i.imgur.com/QwQ8vzR.png)

**Known issues:**
There can be issues with Makito X Decoder. (under investigation)

**Pros/Cons in comparison with Option 1:**

_Pros:_ 
* MPEGTS is not malformed and can be dumped into a recording without issues.
* Works with more servers, clients: Wowza, Nimble Streamer, SRT Live Server, FFmpeg, ffplay, VLC, etc.
* Several audio tracks can be streamed (for instance, track 1: main track, track 2: background music, track 3: commentary etc.) while in option 1 only a single track can be selected. OBS Studio supports up to 6 audio tracks.

_Cons:_
* More complex to set up.
* One can not record the stream since one leverages the Record Output to stream.    

# Examples of setups
## Relay server to Twitch

One can use ffmpeg to easily relay an input SRT stream to a standard RTMP compatible with Twitch (or even other streaming services provider like YouTube ou Facebook). This is especially interesting if you have a bad and unstable connection between OBS and the service. Note that the server you use should have a fast and reliable connection to benefit from this (but this is usually the case). Also note that using a server to proxy stream consumes a lot of bandwidth (consider twice as much as the bandwidth of your video stream) and this can be expensive on some providers like Amazon, GCS or Digital Ocean (it doesn't need a fast CPU though, it's only receiving, rewraping for RTMP and sending).
This approach has the advantage of being really easy to setup on OBS's side if you prepare a server for someone else.
For example the following command can be used on the server:  

```ffmpeg -i srt://:1234?mode=listener&transtype=live&latency=3000000&ffs=128000&rcvbuf=100058624 -c copy -f flv rtmp://live-cdg.twitch.tv/app/streamKey```  

In this command you can change:
- `1234`: This is the listening port used by the SRT server. You will need to send your stream to this port. It can be anything, provided the ffmpeg can bind to it.
- `latency=3000000`: The latency in microseconds. Here we use 3000 milliseconds (3 seconds). This is a simple calculus: the higher it is, the more reliable it will be if you drop packets or your latency increases and your viewer will have to wait more to receive your frames. The lower it is, the more sensible it will be to connection problems, but your viewers will also receive your content faster.
- `ffs` and `rcvbuf` are complicated numbers that indicates the sizes of the differents buffers. They are already set for a latency of 3000ms. Basically, when you increase the latency, you'll need to increase these values (because SRT needs to store more content in memory to recompose the puzzle afterwards). You can have informations on how to calculate these here: https://github.com/Haivision/srt/issues/703#issuecomment-495570496
- `live-cdg.twitch.tv`: Nearest Twitch's ingest server. Change to your nearest one found here: [Twitch Ingest Informations](https://stream.twitch.tv/ingests/)
- `streamKey`: This is your Twitch.tv stream key. You will need it to replace this with yours so they know who is streaming to where. Do not leak it, anyone with this key can stream to your account.

On OBS, you just need to configure your stream settings to "Custom", and specify your server with this template:
```srt://ipofyourserver:4444```  
You can leave the stream key empty, you don't need it.


Run the ffmpeg command you saw above, it will wait indefinitely for an input stream, and automatically stop at the end of the stream. I leave you the choice of managing the restart with systemd or in a Docker container!