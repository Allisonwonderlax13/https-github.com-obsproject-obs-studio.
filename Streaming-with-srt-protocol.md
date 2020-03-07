_**This feature requires OBS Studio 25.0 or newer.**_

***

### Table of Contents:
* [General Overview](#general-overview)
* [Can srt be used with Twitch or my favourite service ?](#can-srt-be-used-with-Twitch-or-my-favourite-service-?)
  * [services](#services)
  * [encoders](#encoders)
  * [servers](#servers)
  * [players](#players)
* [How to set up OBS-Studio](#how-to-set-up-obs-studio)
  * [Option 1: Stream srt using the Streaming output](#option-1:-stream-srt-using-the-streaming-output)
  * [Option 2: stream srt using the Custom FFmpeg Record output](#option-2:-stream-srt-using-the-custom-ffmpeg-record-output)

***

# General Overview
* Secure Reliable Transport (or SRT in short) is a relatively recent open source streaming protocol, originally developped by Haivision (first demo in 2013) and promoted by the [SRT Alliance](https://www.srtalliance.org/) which includes many big players in the streaming/video/telco industry. It promises:   
(1) better resilience to network issues (jitter, lost packets, delay, bandwidth fluctuations) with mechanisms for packet recovery (retransmission) and    
(2) low latency (as low as twice the round-trip between encoder and ingest server, with sub-second latency usually).  
  
For a video demo, see for instance :      
<p align="center"> <a href="https://www.youtube.com/watch?v=I6G2iaTMuQo"><img src="https://img.youtube.com/vi/I6G2iaTMuQo/0.jpg"></img></a></p> 

* It's mostly used in the broadcast and corporate world for now, see for instance NAB 2018 srt panel with ESPN, NFL, Microsoft speakers talking about their use of srt :   
<p align="center"> <a href="https://www.youtube.com/watch?v=PuGn2cjIt04"><img src="https://img.youtube.com/vi/PuGn2cjIt04/0.jpg"></img></a></p>    

 The NAB 2019 NAB srt panel can be watched there: [https://www.haivision.com/resources/webinars/broadcast-panel](https://www.haivision.com/resources/webinars/broadcast-panel). (Hey Haivision, friendly suggestion: it'd be nice to have this vid posted on YouTube instead of having to enter personal infos ! :P )    

* Unlike rtmp it is an open source protocol: the source code can be found on [github](https://github.com/Haivision/srt). While rtmp development has been abandoned since 2012, srt development is still very much active.

* As a protocol, it is content agnostic, although the industry uses it along with an mpegts container, which is the de facto standard 
in broadcast industry. (In terms of comparison, RTMP protocol relies on the flv container.)
The mpegts container is usually used along with udp protocol, which makes it fast but very unreliable and prone to packet loss.
It can also be used with tcp which is more reliable but has larger latency. SRT adds up to these two protocols to transport mpegts, with the best of two worlds, reliability of tcp, and lower latency of udp. It also supports encryption and bonding.   

* Other competing new protocols are webrtc, zixi (closed source) and RIST; the latter two are quite similar to SRT and all go beyond rtmp. 

* For further technical details, we recommend this video by Alex Converse, a Twitch engineer :   
     <p align="center"> <a href="https://youtu.be/m1FABQtAjgU"><img src="https://img.youtube.com/vi/m1FABQtAjgU/0.jpg"></img></a></p>        
There is also a white paper which can be found [here](https://www.haivision.com/resources/white-paper/srt-protocol-technical-overview/)
or [there](https://github.com/Haivision/srt/issues/479). The API is fully documented on [github](https://github.com/Haivision/srt).   


***

# Can srt be used with Twitch or my favourite service ?
_Short answer:_ **NO** ! (or not yet?)
## Services
_Long answer:_ **none** of the main streaming services support the srt protocol for ingest. Most still use rtmp (Twitch, YouTube, Facebook...). (Mixer though relies on webrtc through its proprietary ftl protocol which OBS already supports).    
So if you're using exclusively these services, no need to read further.   

At this stage of the adoption of srt protocol, you'll have to be technically inclined if you want to use SRT.    
If you are able to setup your own streaming server, maybe redirecting your streams to the main services like Twitch or YouTube and are interested in achieving low-latency with improved network resilience, read on.    

The other category of users who could potentially be interested belong obviously to the professional broadcast industry.    
So this wiki entry can be considered as fairly advanced in that it requires access to a server and being able to set it up.    

The configuration of obs itself ranges from easy to medium in terms of difficulty. The server setup is more challenging since it requires system/network admin knowledge ... 

## Encoders
Live software encoders : 
* **FFmpeg**,
* **obs-studio** which relies on the FFmpeg libraries,
* vmix,
* srt-live-transmit (which is a demo app from libsrt developpers; needs to be compiled from source)...    
Hardware encoders are also available at various vendors.
  
## Servers
The following servers support srt ingest:
* wowza
* nimble server
* srt live server
The latter is free and **[open source](https://github.com/Edward-Wu/srt-live-server)** ! but only serves srt streams and does not transmux to HLS/RTMP/DASH ... it is much more rudimentary than the other servers at this stage but it is FOSS and works fine with obs-studio in our tests !    

Nimble server is nominally free; it is used along with a non free dashboard which is in all fairness quite convenient. But it can also be used without the dashboard and just requires modifying an xml config file.    

Both wowza and nimble server ingest in srt but can distribute in the main protocols/formats (rtmp/hls/dash ...).

## Players
The following players can be used to watch an srt stream :
* vlc 4,
* ffplay (part of ffmpeg tools),
* last but not least, obs-studio Media Source :) (an obvious case-use is to broadcast from any srt source to an obs instance).    

### vlc 4 usage    

Download vlc 4  [here](https://nightlies.videolan.org/)(warning: this is the development version of vlc).    
If you just want to test without disturbing your current vlc install, we advise you to download a portable install (zip).    
Go to `Media > Open Network Stream`:    

![](https://i.imgur.com/SJmLpVf.png)   

enter the srt IP which has the form `srt://IP:port`.    

![](https://i.imgur.com/kxPnPVv.png) 

### ffplay usage    

Requires to be compiled with libsrt support.   
To the extent of our knowledge there does not seem to be any such binary widely available, although there are no license constraints.   
Just launch the command-line:     
`ffplay srt://IP:port`

# How to set up OBS-Studio
There are two ways of setting up obs-studio to connect to a server.    
The first is simpler but gives less options at the moment.    
The second is a bit more difficult to setup but gives more fine tuning capabilities (and at the moment of this writing is more stable).   
**Note that while the discussion focuses on srt protocol, udp or tcp can also be used instead.**   

## Option 1: Stream srt using the Streaming output
_Credit_: Aaron Boxer, Collabora (SRT Alliance) author of the new srt output    

This is extremely simple : go to Settings > Stream and in Service , select Custom.
Enter the srt URL in the form: srt://IP:port (obs-studio will also accept any protocol relying on mpegts container and supported by FFmpeg, therefore udp, tcp, rtp ...)

![](https://i.imgur.com/ZPT2PXm.png)

OBS-Studio will accept options in the syntax : `srt://IP:port?option1=value1&option2=value2`.
The full list of options is those supported by FFmpeg : [http://ffmpeg.org/ffmpeg-protocols.html#srt](http://ffmpeg.org/ffmpeg-protocols.html#srt).

The most important option is **latency** in milliseconds (ms). It has a default value of 120 ms and should be at least 2.5 the round-trip between encoder and ingest server.

Another sometimes required option is the **mode** which can be caller, listener or rendez-vous. _caller_ opens client connection. _listener_ starts server to listen for incoming connections. _rendezvous_ use Rendez-Vous connection mode which is a bi-directional link where the first to initiate handshake is considered caller. The default value is caller and usually need not be set for obs-studio since it'll be in caller mode normally.

A case where it's useful to  set the mode to listener is when sending a stream to vlc : obs-studio then acts as a server to vlc which is the client. On a lan for instance, set obs-studio to `srt://127.0.0.1:port?mode=listener` to establish a connexion to vlc which you point to `srt://127.0.0.1:port`.

![](https://i.imgur.com/PhH7why.png)

**Known issues:**
* At this time (v25 RC2), a bug with mpegts muxer makes the stream not completely compliant with mpegts spec. So srt decoding fails when transmuxing srt to another protocol/container than the combination srt/mpegts : this is the case for instance with nimble server and makito X decoder... A dump of the mpegts stream therefore creates a non conformant file (this can probably be fixed though by a remuxing with FFmpeg). This bug is under investigation.    
* However transmuxing to rtmp works with wowza server and more generally to udp or tcp /mpegts. In particular, vlc, wowza, srt live server, ffplay work well when selecting this simpler option to stream with srt.

## Option 2: stream srt with the Custom FFmpeg Record output
The second option is a bit more complicated. It relies on the `Advanced: Custom FFmpeg Recording output`.
1. Go to `Settings > Output > Output mode` dropdown: select `Advanced`.   
2. Click on `Recording Tab`.
3. In the `Type` dropdown, select: `Custom Output (FFmpeg)`
4. In the `FFmpeg Output Type` dropdown, select: `Output to URL`
5. In the `File Path or URL` box, type the srt  URL: `srt://IP:port` (options like latency are entered with the syntax `srt://IP:port?option1=value1&option2=value2` ).   
For a list of some of these options and a discussion, see the previous section or refer to FFmpeg documentation : [http://ffmpeg.org/ffmpeg-protocols.html#srt](http://ffmpeg.org/ffmpeg-protocols.html#srt).   
6. In `Container Format` dropdpwn, select: `mpegts`.
7. `Muxer Settings` can be left blank; or you can use the (mpegts FFmpeg muxer options)[http://ffmpeg.org/ffmpeg-formats.html#mpegts-1] with the following syntax `option1=value1 option2=value2`; the `option=value` pairs must be separated by a space.    
8. The other settings are self-explanatory. Check the box `show all codecs` to display all codecs available to FFmpeg.  
 
Note that several audio tracks can be selected. They can be identified on the ingest server side by what is called a PID. On default value, the video track has `pid 0x100` (=256), and the other audio tracks have `pid 0x101` etc. If you need to change the pid of your tracks, use the muxer option mpegts_start_pid in 7.    

![](https://i.imgur.com/QwQ8vzR.png)    

**Known issues:**
None.

**Pros/Cons in comparison with Option 1:**

_Pros:_ 
* mpegts is not malformed and can be dumped into a recording without issues.
* works with more servers, clients : wowza, nimble server, srt live server, ffmpeg, ffplay, vlc etc ...
* several audio tracks can be streamed (for instance track 1: main track, track 2: background music, track 3: commentary etc ...) while in option 1 only a single track can be selected. Obs-studio supports up to 6 audio tracks.

_Cons:_
* More complex to set up.
* One can not record the stream since one leverages the Record Output to stream ...

