_These features require OBS-Studio 21.0 or newer._

***

### Table of Contents:

* [Overview](#overview)    
  * [Case uses](#case-uses)
* [Setup](#setup)
* [Gaming and Streaming services support](#gaming-and-streaming-services-support)
  * [Compatible streaming services](#compatible-streaming-services)
  * [Incompatible services](#incompatible-services)
* [Streaming servers and players](#streaming-servers-and-players)
  * [Compatible streaming servers](#compatible-streaming-servers)
  * [Compatible web players](#compatible-web-players)
* [Streaming music](#streaming-music)
  * [Filters and VST](#filters-and-vst)
  * [High quality recording](#high-quality-recording)
  * [Interfacing with a DAW](#interfacing-with-a-daw)
* [Ambisonics](#ambisonics)
  * [Higher order ambisonics](#higher-order-ambisonics)
* [Multilingual streaming](#multilingual-streaming)
* [Detailed list of surround sound features](#detailed-list-of-surround-sound-features)

***

# **Overview**
OBS-Studio is the first mainstream streaming software to support **surround sound** streaming and recording.   
Traditionally this is a feature reserved to professional broadcast appliances.  
OBS-Studio can stream and record up to 8 audio channels.  
The audio channels can be surround sound channels or more general multichannel ones. 

## **Case uses:**
* **gaming**
* **music**
* **ambisonics**
* **multilingual streaming**

# **Setup**  
   
* **Settings > Audio** : just select a channel different from Mono or Stereo in the Channels list. Click OK to the Warning pop-up and restart OBS.
![Imgur](https://i.imgur.com/svd8gp2.jpg)

* **Settings > Output** : 
    * **Output Mode** : select Advanced
    * **Advanced > Audio Tab** : larger bitrates are unlocked for surround sound (up to 1024 kbs). Select a large bitrate. By default OBS-Studio selects 160 kbs for stereo. This is insufficient for surround sound. As a rule of thumb 64 kbs x number of channels should be CD quality. 
![Imgur](https://i.imgur.com/lLorSYB.jpg)
    * **Recording** :    
either standard or Custom FFmpeg recording can be selected.   
In the prior case the codec used will be aac ; in the latter case, all (free) codecs supported by FFmpeg are available.    
(check the box 'Show all codecs').   
For surround sound, will be of interest: libopus, aac (native), pcm formats (uncompressed).  
![Imgur](https://i.imgur.com/JPQhJlg.jpg)


* **IMPORTANT WARNING**: make sure to select the same channel layout as your input (if you have a 4.1 audio source do not select 7.1). If you don't, channel mixing may (or may not) occur. There is an automatic channel rematrixing when either downmixing or upmixing is mandated by a difference in channel layouts between source and output. This channel rematrixing mixes channels in general.  Or it can remove a channel (ex: 3.1 source to 4.0 output removes the LFE channel).  

* **DOLBY**
Streams can be encoded in ac3 /eac (using Output > Advanced > Custom FFmpeg recording > stream to URL ). But support of the various streaming services or web players has not been tested.    
Modify this wiki if this works.    
Capture of dolby can be tricky if the channels are lumped into two PCM channels; in order to be decoded correctly and encoded all the channels should be held in different PCM channels.    

# **Gaming and Streaming services support** 
 
## **Compatible streaming services**
As of now, the following services have been tested and are compatible with live surround sound streaming:
   
* **Twitch**,  
* **Mixer** (rtmp but NOT with ftl protocol),  
* **Smashcast** ...   
* **Facebook Live 360 with spacial audio** (requires ambisonics capture device)

 
## **Incompatible services**

* **YouTube Live** (discards channels beyond the first two).
* **Facebook Live** (downmixes all the channels).

# **Streaming servers and players**

Apart from streaming to services like Twitch or Facebook Live, you might use your own streaming server which delivers streams to your website.

### **Compatible streaming servers**
The following servers have been tested and ingest surround sound in rtmp protocol : 
* wowza
* nginx with rtmp module

Note however that server-side recordings only keep the first two channels although.

### **Compatible web players**
* html5 players (tested with hls or mpeg-dash):   
    + videojs,   
    + bitmovin,      
    + mediaelement,    
    + viblast (hls & dash),   
    + hls.js 
  
NB: Flash player accepts rtmp surround sound in Dolby; it might work ==> untested. 

# **Streaming music**

For music bands, Djing, ... , bring your production with surround sound to the World !   
Up to 7.1 surround sound is available. (For more channels (up to 16.0) check [this fork](https://github.com/pkviet/obs-studio/releases)).    

## **Filters and VST**
OBS-Studio has built-in audio filters as well as VST 2 support. The filters are compatible with surround sound; the VST also **if** they originally support multichannel. Check the [Filters Guide here](https://github.com/jp9000/obs-studio/wiki/Filters-Guide).

## **High quality recording**
Select Custom FFmpeg recording and a PCM format (e.g. pcm_s24le for 24 bit samples) for uncompressed audio.
For compressed formats, you can select also libopus and aac which will work very well (target at least 64 kbs per channel).  

## **Interfacing with a DAW**
For that you will need some application ensuring the routing.
### windows
 **Reaper** :
* [SAR](http://sar.audio/)
* rearoute (from Reaper, with up to 256 channels) if using [ASIO plugin](https://github.com/pkviet/obs-asio)
* [Voicemeeter](https://www.vb-audio.com/Voicemeeter/index.htm)
* **not working with surround sound** (as of v. 4.15) : [Virtual Audio Cable](http://software.muzychenko.net/eng/vac.htm)
![Imgur](https://i.imgur.com/9brmItb.jpg)

### MacOS
Tested with **Reaper** and the following apps ensuring the routing
* Soundflower
* Jack server
* Loopback

### Linux
No DAW tested. Update the Wiki if you tested.

# **Ambisonics** 

Although the channel layouts are tagged with position (2.1 5.1 etc), it is possible to encode your channels for ambisonics use provided your decoding application is setup to supply the ambisonics positions according to channel order.   
This is the case for Facebook Live 360 with spacial audio.   
To use the latter in Settings > Audio, select : channels > 4.0 ; aac codec is mandatory (if you use Recording > Custom FFmpeg > to URL > select flv container, with rtmp URL to FB live 360 and aac audio codec + x264 video codec).      
When streaming to FB Live 360 the four channel rtmp stream will be interpreted as carrying ambisonics of order 1 .

In order for this feature to work, obviously you will need an ambisonics capture device.   
If you have an aac or uncompressed audio recording with 4 ambisonics, you can also play it by adding a Media Source.   
(If it doesn't work, check with ffmpeg that it is decoded correctly.)   
 
## **Higher order ambisonics**    
No live streaming service supports beyond first-order ambisonics.
For recording, though for order 2 or 3, you can use this [fork](https://github.com/pkviet/obs-studio).   
If you record with libopus (Output > Advanced > Custom FFmpeg recording ) up to 255 channels are available with mkv.   
You will have to add the mapping_family=255 option though to FFmpeg audio encoder options in OBS-Studio.   


# **Multilingual streaming** 

Multichannel support in OBS allows to stream several languages simultaneously.    
This is useful for live translation (public talks ... ).

The mainstream streaming services do not support such a feature directly.    
In the broadcast industry, one usually uses mpeg-ts multi-track streams instead of a multichannel stream (in single track).

There are workarounds though allowing one to use a single track multichannel audio.   
Here is one requiring :
* **nginx** with rtmp module ;
* **ffmpeg** scripts which will be exec'd by nginx: ffmpeg will split the audio channels and create mono rtmp streams (as many as there are languages).     
* each of these mono rtmp streams can then be pushed by nginx to a service like FB or YouTube Live.    

For nginx setup with rtmp module, check elsewhere.   
(make sure to set it up with a single worker.)

The setup is the following:    
surround sound capture with each mono channel carrying a language  
(capture tested: sdi/hdmi decklink cards, reaper, Behringer X32, ASIO sound cards).      
==> obs with surround sound enabled       
==> rtmp stream to nginx which is setup with an exec script :        
` rtmp {       `
`     server {    `    
`       listen 1935;    `    
       `ping 30s;    `    
       `notify_method get; `       

       application splitter {
            live on;
            allow play all;
            exec /home/me/splitter.sh $name;
            exec_kill_signal term;
            record off;
        }

==> nginx : exec a ffmpeg script which will split the channels and redirect rtmp mono streams to nginx 

**FFmpeg example script** (for two languages and a stereo source): 
  
`#!/bin/bash`     
`echo "$(date +"%Y/%m/%d %H:%M:%S"): starting"`    
`on_die ()`    
`{`    
    `# kill all children`    
    `pkill -KILL -P $$`    
`}`    
`trap 'on_die' TERM`    
`ffmpeg -i rtmp://IP:port/splitter/ \`    
`-filter_complex "[0:a]pan=mono|c0=c0,aresample=async=1000[a0];[0:a]pan=mono|c0=c1,aresample=async=1000[a1[0:a]pan=mono|c0=c0,aresample=async=1000[a2]" \`    
`-map 0:v -copyts -start_at_zero -vcodec copy \`    
`-map [a0] -bsf:a aac_adtstoasc -copyts -start_at_zero -c:a libfdk_aac -ab 64k -ac 1 \`    
`-f flv rtmp://IP:PORT/app/stream_language1 \`    
`-map 0:v -copyts -start_at_zero -vcodec copy \`    
`-map [a1] -bsf:a aac_adtstoasc -copyts -start_at_zero -c:a libfdk_aac -ab 64k -ac 1 \`    
`-f flv rtmp://IP:PORT/app/stream_language2 &`    
`wait `


For more languages pick a corresponding channel layout and add relevant streams in FFmpeg script.   
Note that for 5.1 and 7.1 one channel (the fourth) will be encoded as LFE and so is not useable.   
For up to 16 channels support, check the following [fork]([ASIO plugin](https://github.com/pkviet/obs-studio).

# **Detailed list of surround sound features** 
* Recording and Streaming multichannel audio sources (surround sound).
* Compatible streaming services: Twitch, Mixer rtmp (not ftl), smashcast, FB 360 live
* Compatible protocols:   
    * rtmp
    * mpeg-ts tcp udp (others untested).
* Streaming servers tested:   
    * wowza,  
    * nginx-rtmp   
(rtmp with multichannel audio passes through and can be distributed by these servers to another service or cdn supporting surround sound; however, the recording feature of these servers does not work; only the first two audio channels are kept).  
 
* html5 players tested and working with live surround:   
    + videojs,   
    + bitmovin,      
    + mediaelement,    
    * viblast (hls & dash),   
    * hls.js   

* Compatible containers (for recordings):   
    * mkv   
    * mp4  
    * ts   
    * flv (others untested). 
 
* Compatible codecs:   
    * ffmpeg aac (native encoder, up to 16 channels),  
    * libfdk_aac (up to 7.1),  
    * core audio aac (up to 7.1),  
    * opus (libopus encoder, up to 255 channels),  
    * vorbis (up to 7.1),  
    * pcm (others untested). 
 
* OS: compatible with win, macOS, linux (alsa, pulse-audio).  

* Misc: 
    * Higher audio bitrates (up to 1024 kbs) unlocked to accomodate higher number of channels.  
    * audio monitoring, audio filters, VST are all working OOB. 

