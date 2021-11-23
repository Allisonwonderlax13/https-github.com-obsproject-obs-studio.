# Advanced Recording Guide With Multi Track Audio
If you want to record multiple audio tracks you will need to switch to "Advanced Output" mode at the top which will give you full control of all the settings.

In order to access these options, you will need to enable the options shown in the image (pick any encoder that is not "same as stream".

![](https://obsproject.com/media/pages/knowledge-base/advanced-recording-guide-and-multi-track-audio/d5e9e963e6-1637689918/obs-adv.png)

This will allow you to record multiple audio track (up to 6), as well as choose a different encoder from the stream encoder.

There are some differences in terms of the quality based rate controls we recommend, although they all follow a similar paradigm of lower value = higher quality and larger files. Opposite higher numbers = lower quality, smaller files.

Here is a list of recommended values.

## x264
- Rate Control: CRF
- CRF: 15-23
- CPU Preset: veryfast or superfast

## Nvenc
- Rate Control: CQP
- CRF: 15-23
- Preset: Quality
- Profile: High
- look-ahead & Psycho Visual: off
- Max B-frames: 2

## QuickSync (Intel)
- Target Usage: quality (lower to balanced or speed if you encounter problems)
- Profile: high
- Rate Control: CQP / ICQ or LA_ICQ
- on CQP: QPI/QPP/QPB: 15-23
- on ICQ: ICQ Quality: 15-23
- on LA_ICQ: ICQ Quality: 15-23, Lookahead Depth: 40-50

## AMD (AMF/VCE)
- Preset: Indistinguishable
- Quality Preset: Balanced
- Keyframe Interval: 2 Seconds
There are also Presets for High Quality, Recording etc, which will set up the encoder for that type of usage.

# Configuring multiple Audio tracks
![](https://obsproject.com/media/pages/knowledge-base/advanced-recording-guide-and-multi-track-audio/d4b4466a56-1637690638/rec-adv-audiotracks.png)

Not all recording formats (containers) allow for multi track audio. We suggest using mkv (you can remux later to mp4).
Once the tracks are enabled, we're free to send different audio outputs to different tracks. Most video players will only play a single track a time, but editors (assuming its supported) will allow you to manage each track separately.

In order to set up our tracks, we need to access the "advanced audio properties".

![](https://obsproject.com/media/pages/knowledge-base/advanced-recording-guide-and-multi-track-audio/d14c425d07-1637690988/rec-adv-audio.png)

Here we can assign which sources we want to go on which track.
In this example, I'm going to leaver all my source enabled on "Track 1", because that is the default for the stream, and I want it to contain all sources.

![](https://obsproject.com/media/pages/knowledge-base/advanced-recording-guide-and-multi-track-audio/e0fd987744-1637691333/rec-adv-track-setup.png)

Then I'm going to spend my remaining tracks 2-4 to have full control over the sources I want in post (editing).
* Track 2, only "comms" (discord, skype etc) is selected, so that I can add/remove, mute it later.
* Track 3, only has "desktop-game", because I might choose to change the volume, or change the speed.
* Track 4, only has "mic/aux" so that I can potentially change volume, or add effects/noise removal.
I chose to leave the music out in my setup, as I have full freedom to add/change that later.