## Quickstart

### 1. Run the auto-configutation wizard

When you load OBS Studio for the first time you should see the Auto-Configuration Wizard. If you're new to OBS or just want to get started as quickly as possible, follow the steps to get good starting settings for your setup. If you only see the main OBS Studio window, you can access the Auto-Config Wizard in the Tools menu at the top. This wizard will automatically select your best settings - streaming or recording, resolution, bitrate, encoder, streaming provider and more. You can always modify the settings manually later.

### 2. Set up your audio devices

By default, OBS Studio is set to capture your system default desktop audio device and microphone. You can verify this by looking at the volume meters in the mixer section of the main OBS Studio window, and see if they are active. If they aren't moving, or you suspect the wrong device is being captured, click on Settings -> Audio and select the devices manually.

**macOS users**: If you're on macOS, [you'll need an extra app](https://obsproject.com/forum/resources/os-x-capture-audio-with-ishowu-audio-capture.505/) to capture desktop audio. This is due to limitations in macOS that provide no direct capture methods for desktop audio devices.

### 3. Add your sources for video

Next, you'll see that the preview is a black screen. OBS does not capture any video by default. To get started capturing, you need to add a Source. At the bottom of the window is a box called 'Sources'. Click on the + (or right click inside the box) and pick the source you want. As a few examples, select Game Capture if you're capturing a game, Window for non-game applications, or Video Capture Device for a webcam or capture card.

Sources and Scenes are the bread and butter of OBS Studio, and can be super powerful. [Click here to read more about them](https://obsproject.com/wiki/OBS-Studio-Overview#scenes-and-sources). 

### 4. Test your stream and record settings

Double check that all your settings are how you want them in Settings -> Output. Then, just hit Start Recording or Start Streaming. We strongly encourage running a test for a few minutes to make sure that there are no issues, rather than just jumping in to your first stream or recording. If you run into any issues, or need further help, take a peak at our [help portal](https://obsproject.com/help)

Once you're satisfied, you can go on to creating great content. That's all there is to it!

If you want to read a more in-depth guide about the power of OBS Studio, jump into the [Overview wiki](OBS-Studio-Overview).

## FAQ

### Where are my recordings saved?

Once your recording is done, you can find it using File -> Show Recordings. You can change this in File -> Settings -> Output -> Recording.

### I need my recordings in MP4!

File -> Remux Recordings will quickly and easily convert your video files into MP4.

### Help, my video is laggy!

Depending on the kind of lag, this could be related to a slow internet connection, your game using too many resources, or incorrect settings. Read the troubleshooting guides linked below.

### I need help with other issues!

- [Troubleshooting Guides](Troubleshooting-Guides)
- [Community Chat & more](http://obsproject.com/help)