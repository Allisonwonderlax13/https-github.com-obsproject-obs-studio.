i = int (whole number),
b = boolean (true/false, 1/0)

## General

**Usage**=transcoding, ultralowlatency, lowlatency, webcam

Default: Transcoding

Explanation: Changes a lot of settings based on usage. Highly recommend sticking to "transcoding", as all others will heavily degrade quality.
***
**Quality**=speed, balanced, quality

Default: Quality

Note: Can fall back to faster presets if throughput is too high vs max throughput.
***
**Profile**=main, high, constrained_baseline, constrained_high

Default: High

Explanation: h264 profile. "High" should work for all modern devices (think iphone 4 and later).
***
**ProfileLevel**=i

Default: 42 (4.2)

Explanation: H264 profile level. Specifies constraints for decode.
***
**MaxNumRefFrames**=i

Explanation: Limits the max number of reference frames (0-16). This max is also limited by hardware and h264 levels. More ref frames means more encoder work, but potentially better compression. Stick to 4 or less, for the sake of compatibility and profile conformity. The encoder is currently likely to use a single reference frame unless b-frames are active.
***
## Rate Control

**EnablePreAnalysis**=b

Default: false

Explanation: Pseudo 2-pass for a certain number of frames. Enabled will disable VBAQ.
***
**RateControlPreanalysisEnable**=i

Default: enabled

Explanation: Pre-encode assisted rate control.
***
**EnforceHRD**=b

Default: True

Explanation: Enforce Hypothetical Reference Decoder. Constraints on QP variation within a picture to meet HRD requirements.
***
**FillerDataEnable**=b

Default: false (true if CBR)

Explanation: Filler data. Useful for strict CBR encoding, like live streaming.
***
**VBVBufferSize**=i

Note: in bits (bps)
***
**InitialVBVBufferFullness**=i

Note: 0=0%, 64=100%
***
**PeakBitrate**=i

Note: in bits (bps)
***
**MinQP**=i

Default: 18
***
**MaxQP**=i

Default: 46

## B-Frames
**B-Frames and its features currently require RDNA2 (RX 6000 and higher). Seems to currently cause some issues, try 1 or 2.**

**MaxConsecutiveBPictures**=i

Default: 0

Explanation: Max consecutive b-frames.
***
**BPicturesPattern**=i

Default: 0

Explanation: Sets the number of consecutive b-frames. 0=off.
***
**AdaptiveMiniGOP**=b

Default: false

Explanation: Variable amount of b-frames.

Note: Might require Pre-Analysis.
***
**BReferenceEnable**=b

Explanation: Enable/Disable the use of b-frames as reference frames.

## Psy

**DeBlockingFilter**=b

Default: True

Explanation: Enable/Disable the de-blocking filter.
***
**EnableVBAQ**=b

Default: True

Explanation: Adaptive quantization. Prioritize bits to parts of the image humans care about.
***
**HighMotionQualityBoostEnable**=b

Default: False

## Motion Estimation

**HalfPixel**=b

Default: true
***
**QuarterPixel**=b

Default: true