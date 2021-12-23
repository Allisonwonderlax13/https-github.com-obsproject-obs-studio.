Just follow these **4 steps** to start streaming or recording!

### 1. Run the auto-configuration wizard

If you're new to OBS or just want to get started as quickly as possible, follow the steps in the Auto-Configuration Wizard. This wizard will automatically test your system and attempt to find settings that your PC can handle. This includes streaming or recording, resolution, bitrate, encoder, streaming provider and more. You can always modify the settings manually later.

The wizard will show the first time you run OBS. If you need to run it again, click on Tools -> Auto-Configuration Wizard.

### 2. Set up your audio devices

By default, OBS Studio is set to capture your desktop audio and microphone. You can verify this by looking at the volume meters in the mixer section of the main OBS Studio window. If they aren't moving, or you suspect the wrong device is being captured, click on Settings -> Audio and select the devices manually.

**macOS users**: If you're on macOS, [you'll need an extra app](https://obsproject.com/forum/resources/mac-desktop-audio-using-blackhole.1191/) to capture desktop audio. This is due to limitations in macOS that provide no direct capture methods for desktop audio devices.

### 3. Add your sources for video

Next, you'll see that the preview is a black screen. OBS does not capture any video by default. To get started capturing, **you need to add a source**.

At the bottom of the window is a box called 'Sources'. Click on the + (or right click inside the Sources box) and pick the source you want. For example:
- select [Display Capture](https://obsproject.com/wiki/Sources-Guide#display-capture) to record everything visible on a monitor
- select [Game Capture](https://obsproject.com/wiki/Sources-Guide#game-capture) (Windows only) if you're capturing a game
- select [Window Capture](https://obsproject.com/wiki/Sources-Guide#window-capture) for non-game applications
- or select [Video Capture Device](https://obsproject.com/wiki/Sources-Guide#video-capture-device) for a webcam or capture card

Sources and scenes are the bread and butter of OBS Studio, and can be super powerful. [Click here to read more about them](https://obsproject.com/wiki/OBS-Studio-Overview#scenes-and-sources). 

**Laptop users**: [Here's](Laptop-Troubleshooting) our troubleshooting guide if your game/window/display capture sources still show a black screen.

### 4. Test your stream and record settings

Double check that all your settings are how you want them in Settings -> Output. **Then, just hit Start Recording or Start Streaming.**

We strongly encourage running a test for a few minutes to make sure that there are no issues, rather than just jumping in to your first stream or recording. If you run into any issues, or need further help, take a peek at our [help portal](https://obsproject.com/help).

Once you're satisfied, you can go on to creating great content. That's all there is to it!

If you want to read a more in-depth guide about the power of OBS Studio, jump into the [Overview wiki](OBS-Studio-Overview).

## FAQ

### Where are my recordings saved?

Once your recording is done, you can find it using File -> Show Recordings. You can change this in File -> Settings -> Output -> Recording.

### I need my recordings in MP4!

To ensure that your recordings don't become corrupted, it is recommended to keep your video recording format as MKV. When you've finished recording and need to convert the MKV file to MP4, click on File -> Remux Recordings.

### Help, my video is laggy!

Depending on the kind of lag, this could be related to a slow internet connection, your game using too many resources, or incorrect settings. Read the troubleshooting guides linked below.

### I need help with other issues!

- [Troubleshooting Guides](Troubleshooting-Guides)
- [Community Chat & more](http://obsproject.com/help)