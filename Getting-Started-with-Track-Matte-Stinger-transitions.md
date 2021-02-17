Stinger Transitions in OBS Studio overlay a video over a cut-transition between two sources. Since the inception of Stinger Transitions in OBS, there has been two modes so far: milliseconds cut point or frame number cut point.

Starting with OBS Studio 27.0, Stinger transitions can now use a third mode: Track Matte. In this mode, no "cut point" is necessary. The transition between the current scene and the next scene is managed by an animated "mask video", with the stinger video overlayed on top of it.

An example: using a Track Matte in a Stinger transition can make the next scene appear from within a circle overlayed over the current scene. 

_TODO animated gif: track matte stinger example_

In this mask video, black pixels tell OBS where to show pixels from the current scene, where white pixels tell OBS where to show pixels from the next scene. Pixels inbetween plain black and plain white are allowed too, and allow for crossfades from the current scene to the next scene.

_TODO static image: how black and white pixels are mapped to the current scene and the next scene_

Track Mattes for a Stinger Transition can be provided in three ways:

- **In two separate video files**: the first file being the stinger video and the second file being the track matte video.

  _TODO example image: two separate files_

- **Everything in the same video file, side-by-side**: the stinger video and track matte video are merged into the same file, with the stinger video on the left and the track matte video on the right. If both the stinger video and track matte video have a 1920x1080 resolution, the combined video file must have a 3840x1080 resolution (2x 16:9 side-by-side).

  _TODO example image: side-by-side video file_

- **Everything in the same video file, stacked**: same as side-by-side mode, but with a vertical layout. The stinger video is at the top, and the track matte video is at the bottom. If both the stinger video and track matte video have 1920x1080 resolution, the combined video file must have a 1920x2160 resolution (2x 16:9 on top of each other).

  _TODO example image: stacked video file_

The side-by-side and stacked options give the best results. The "separate files" option is OK for testing and experimentation but doesn't guarantee synchronized playback of the stinger & track matte videos (which can result in problems where the stinger & track matte masking doesn't line up).