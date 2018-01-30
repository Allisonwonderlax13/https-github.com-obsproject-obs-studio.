To make it easier for new users, here's an updated quickstart guide. I'll paste in the existing one below and we can tweak from there.

* New users struggle most with the fact that a source needs to be added first
* The old guide doesn't mention the auto-config wizard
* Quickstart should be its own page (this one can be renamed once it's ready), and relevant bits that can be expanded upon should link to their respective sections of the OBS Studio Overview page

## Quickstart

Getting started with OBS Studio is relatively simple, with only a few steps needed before you're on your way to creating content.

1. Select your Streaming Service in Settings -> Stream, and enter your stream key information. If you are only going to be recording, you can ignore this step.
2. Select your Base (Canvas) and Output (Scaled) resolutions and FPS in Settings -> Video. The Base (Canvas) should be set to your primary monitor's resolution by default, and this is usually safe to leave alone. Output (Scaled) is the resolution that the stream or recording will be output as. For most cases, we recommend starting with 720p (1280x720) at 30 FPS as the starting point. 
3. Set your streaming bitrate or recording quality in Settings -> Output:
  - For Streaming, set the bitrate you wish to stream at. Recommended for 720p 30 FPS is 2500. If your internet cannot support this much bitrate, you may need to downscale resolution further to accommodate a lower bitrate.
  - For Recording, select a Recording Quality from the dropdown menu. Indistinguishable Quality is our recommended starting point. If you have an available hardware encoder (NVENC, QSV, or AMF), you can select that here as well.
4. Add your [Scenes and Sources](#scenes-and-sources) (use the + symbol under the Sources list, or right click and select **Add**) for the content you wish to stream or record.
5. Click on Start Stream or Start Recording, and enjoy!
