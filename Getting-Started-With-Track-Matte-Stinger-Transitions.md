Stinger Transitions in OBS Studio overlay a video over a cut-transition between two sources. Since the inception of Stinger Transitions in OBS, there has been two modes so far: milliseconds cut point or frame number cut point.

Starting with OBS Studio 27.0, Stinger transitions can now use a third mode: Track Matte. In this mode, no "cut point" is necessary. The transition between the current scene and the next scene is managed by an animated "mask video", with the stinger video overlayed on top of it.

A selection of [free track matte stingers](https://cdn-fastly.obsproject.com/downloads/TrackMatteStingers.zip) have been made available for those wishing to try out the feature.

An example: using a Track Matte in a Stinger transition can make the next scene appear from within a circle overlayed over the current scene. 

![Track Matte Result](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/track-matte/track-matte-result.gif)

In the mask video, black pixels tell OBS where to show pixels from the current scene, where white pixels tell OBS where to show pixels from the next scene. Pixels inbetween plain black and plain white are allowed too, and allow for crossfades from the current scene to the next scene.

![Track Matte example](https://raw.githubusercontent.com/wiki/obsproject/obs-studio/images/track-matte/track-matte-example.gif)

*Example of a side-by-side track matte video file. The left half is the "overlay" portion and the right half tells OBS which scene to display, where black = "from" scene, white = "to" scene, gray = degree of fade between "from" scene and "to" scene*

Track Mattes for a Stinger Transition can be provided in three ways:

- **Everything in the same video file, side-by-side**: the stinger video and track matte video are merged into the same file, with the stinger video on the left and the track matte video on the right. If both the stinger video and track matte video have a 1920x1080 resolution, the combined video file must have a 3840x1080 resolution (2x 16:9 side-by-side).

  <!-- TODO example image: side-by-side video file -->

- **Everything in the same video file, stacked**: same as side-by-side mode, but with a vertical layout. The stinger video is at the top, and the track matte video is at the bottom. If both the stinger video and track matte video have 1920x1080 resolution, the combined video file must have a 1920x2160 resolution (2x 16:9 on top of each other).

  <!-- TODO example image: stacked video file -->

- ~~**In two separate video files**: the first file being the stinger video and the second file being the track matte video.~~ **NOTE:** Since OBS currently does not support lock-step video decoding, we have disabled this feature for the time being due to the fact that two separate videos cannot be guaranteed to play back in perfect sync. We recommend using the single-file video methods (side-by-side or stacked) instead. You can stack video files side-by-side using the following FFmpeg command: `ffmpeg -i left.mp4 -i right.mp4 -filter_complex hstack output.mp4`

  <!-- TODO example image: two separate files -->

The side-by-side and stacked options give the best results. The "separate files" option is OK for testing and experimentation but doesn't guarantee synchronized playback of the stinger & track matte videos (which can result in problems where the stinger & track matte masking doesn't line up).