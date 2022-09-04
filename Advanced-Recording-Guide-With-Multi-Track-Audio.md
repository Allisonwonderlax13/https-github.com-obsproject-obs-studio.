If you want to record multiple audio tracks you will need to switch to "Advanced Output" mode at the top of the window, which will give you full control of all the settings.

In order to access these options, you will need to enable the options shown in the image (pick any encoder that is **not** "same as stream".

![](https://obsproject.com/media/pages/kb/advanced-recording-guide-and-multi-track-audio/59acfcd526-1637689918/obs-adv.png)

This will allow you to record multiple audio track (up to 6), as well as choose a different encoder from the stream encoder.

In simple mode we make use of "quality based" rate control, and we will be mimicking those settings here as well.
"Indistinguishable" means its visually lossless (Visually indistinguishable from lossless).

Here is a list of settings that will close to what you get in "Simple Mode".

## x264
* Rate Control: CRF
* CRF Value:
  * Indistinguishable = 16
  * High Quality = 23
- CPU Preset: veryfast

## NVENC
* Rate Control: CQP
* CQP Value
  * Indistinguishable = 16
  * High Quality = 23
- Preset: Quality
- Profile: High
- look-ahead & Psycho Visual: off
- Max B-frames: 2

## QuickSync (Intel)
* Target Usage: Balanced
* Profile: high
* Rate Control: CQP / ICQ or LA_ICQ
* CQP/ICQ Value
  * Indistinguishable = 16
  * High Quality = 23

## AMD (AMF/VCE)
* Rate Control: CQP
* CQ Value
  * Indistinguishable = 16
  * High Quality = 23
- Preset: Balanced (speed if encoder lag)
- Profile: High
- Max B-frames: 0
- AMF/FFMPEG Options: <nothing/empty>

# Configuring multiple Audio tracks
![](https://obsproject.com/media/pages/kb/advanced-recording-guide-and-multi-track-audio/266933d6df-1637690638/rec-adv-audiotracks.png)

Not all recording formats (containers) allow for multi track audio. We suggest using mkv (you can remux later to mp4).
Once the tracks are enabled, we're free to send different audio outputs to different tracks. Most video players will only play a single track a time, but editors (assuming its supported) will allow you to manage each track separately.

In order to set up our tracks, we need to access the "advanced audio properties".

![](https://obsproject.com/media/pages/kb/advanced-recording-guide-and-multi-track-audio/a252098223-1637875066/rec-adv-audio.png)

Here we can assign which sources we want to go on which track.
In this example, I'm going to leave all my source enabled on "Track 1", because that is the default for the stream, and I want it to contain all sources.

![](https://obsproject.com/media/pages/kb/advanced-recording-guide-and-multi-track-audio/04f0e0e0a4-1637848734/rec-adv-track-setup.png)

Then I'm going to spend my remaining tracks 2-4 to have full control over the sources I want in post (editing).
* Track 2, only "comms" (discord, skype etc) is selected, so that I can add/remove, mute it later.
* Track 3, only has "desktop-game", because I might choose to change the volume, or change the speed.
* Track 4, only has "mic/aux" so that I can potentially change volume, or add effects/noise removal.
I chose to leave the music out in my setup, as I have full freedom to add/change that later.