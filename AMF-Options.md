**i = int (whole number),**
**b = boolean (true/false)**

Parameters and options are written as such:

`Usage=transcoding Quality=speed`

## General

Parameter | Options | Default | Explanation
-- | -- | -- | --
**Usage**| transcoding, ultralowlatency, lowlatency, webcam | Transcoding | Changes a lot of settings based on usage. Highly recommend sticking to "transcoding".
**Quality**| speed, balanced, quality | Quality | Controls the speed vs quality. **Note: Can fall back to faster presets if throughput is too high vs max throughput.**
**Profile**| main, high, constrained_baseline, constrained_high | High | h264 profile. "High" should work for all modern devices (think iphone 4 and later).
**ProfileLevel** | i (10-62) | 42 (4.2) | H264 profile level. Specifies constraints for decode.
**MaxNumRefFrames** | i | 4 | Limits the max number of reference frames (0-16). This max is also limited by hardware and h264 levels. More ref frames means more encoder work, but potentially better compression. Stick to 4 or less, for the sake of compatibility and profile conformity. The encoder is currently likely to use a single reference frame unless b-frames are active.

## Rate Control
Parameter | Options | Default | Explanation
-- | -- | -- | --
**RateControlPreanalysisEnable** | enum (0 or 1) | 1 (enabled) | Pre-encode assisted rate control.
**EnforceHRD** | b | True | Enforce Hypothetical Reference Decoder. Constraints on QP variation within a picture to meet HRD requirements.
**FillerDataEnable** | b | False (True if CBR) | Filler data. Useful for strict CBR encoding, like live streaming.
**VBVBufferSize** | i | N/A | Note: in bits (bps)
**InitialVBVBufferFullness** | i | Note: 0=0%, 64=100%
**PeakBitrate** | i | N/A | Note: in bits (bps)
**MinQP** | i | 18 | 
**MaxQP** | i | 46 | 

## B-Frames
**B-Frames and its features currently require RDNA2 (RX 6000 and higher). Seems to currently cause some issues, try 1 or 2.**
Parameter | Options | Default | Explanation
-- | -- | -- | --
**MaxConsecutiveBPictures** | i | 3 (if supported) | Max consecutive b-frames
**BPicturesPattern** | i | 0 | Sets the number of consecutive b-frames. 0=off.
**AdaptiveMiniGOP** | b | False | Variable amount of b-frames. **Note: Might require Pre-Analysis.**
**BReferenceEnable** | b | True | Enable/Disable the use of b-frames as reference frames.

## Psy
Parameter | Options | Default | Explanation
-- | -- | -- | --
**DeBlockingFilter** | b | True | Enable/Disable the de-blocking filter.
**EnableVBAQ** | b | True (Unless CQP) | Adaptive quantization. Prioritize bits to parts of the image humans care about.
**HighMotionQualityBoostEnable** | b | False | 

## Motion Estimation
Parameter | Options | Default | Explanation
-- | -- | -- | --
**HalfPixel** | b | True | 
**QuarterPixel** | b | True

## Pre-Analysis
Parameter | Options | Default | Explanation
-- | -- | -- | --
**EnablePreAnalysis** | b | False | Pseudo 2-pass for a certain number of frames. Enabling will disable VBAQ. GPU load hit (spends traditional 3d/render resources)
**PASceneChangeDetectionEnable** | b | True | Akin to scenecut (adaptive I-frame insertion). Safest to disable for HLS transcode platforms, as is potentially risky for muxer segment/split.
**PAFrameSadEnable** | b | True | Frame SAD (Sum of Absolute Difference) algorithm
**PALookAheadBufferDepth** | i | 0 | Pre-Analysis lookahead buffer size
**PAPerceptualAQMode** | Enum 0/1 | 0 (None, off) | PA Perceptual adaptive quantization mode
**PATemporalAQMode** | Enum 0-2 | 0 (None, off) | PA Temporal adaptive quantization mode
**PAHighMotionQualityBoostMode** | Enum 0/1 | 0 (off/none) | PA High motion quality boost mode
**PACAQStrength** | Enum (0-2) | 1 (medium) | 0=low, 1=med, 2=high. Content Adaptive Quantization (CAQ) strength
