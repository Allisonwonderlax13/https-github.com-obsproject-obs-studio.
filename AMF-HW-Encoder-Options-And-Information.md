With the release (and beta) of OBS 28, there is now a new AMD AMF Encoder implementation named "AMD HW".
You'll find it in both H.264 and HEVC flavors.

The new implementation includes a more up to date AMF version, and simplifies the interface/settings drastically.
![image](https://user-images.githubusercontent.com/50419942/183417894-f4b458b1-b8b1-4253-837e-d800fc2f4e78.png)


***
## Max Throughput 
There is a check in place to query the encoder for how much throughput its able to handle. This is mainly to guard older and lower end GPUs from performance issues.
If a user tries to do 4k resolution at 60fps, and the GPU is not able to handle it at the "Quality" preset, then it will automatically drop to a faster preset, which can be observed in the OBS log. If the driver/hardware is not able to deliver this throughput information, it defaults to "balanced".

## AMF/FFmpeg Options
It is recommend to use the defaults. There is no "best settings" that will fit every scenario, hardware and intent.

There are times where it could be useful to make some adjustments to the default, and that can be done in this field.

![image](https://user-images.githubusercontent.com/50419942/183474036-974951c4-2135-4338-bfa5-9bda8eba5a86.png)

The input needs to be formatted as such: `parameter=value parameter=value` (space between each).

For instance `BPicturesPattern=2 MaxConsecutiveBPictures=2`

[Simplified list of H264 options.](https://gist.github.com/flaeri/8d187b4ca946ce8991a85b1c79d24d58)

# Examples

### B-frames
Currently it seems that *only* RDNA2 (RX 6000+) has access to B-Frames.

There has been some reports of issues when b-frames are enabled, but if you would like to try, you can do so by specifying the following:
`BPicturesPattern=x MaxConsecutiveBPictures=x`(replace the x with the number of b-frames (1-3)). 

More b-frames is more difficult to encode, and more is not necessarily better, so try 1 or 2 first.

### Pre Analysis
This option is disabled by default, and it has some impact on GPU performance. If you're running games + obs, please be aware that in game performance will take an additional hit if this is enabled. It may also disable "Rate Control Pre-Analysis" and "VBAQ" according to the AMF documentation.

It essentially works a bit like pseudo two-pass for a short number of frames, and will analyze the incoming video frames to try to make better decisions.

Ref: [AMF PreAnalysis Doc](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/blob/master/amf/doc/AMF_Video_PreAnalysis_API.pdf)

### Other
VBAQ (Variance Based Adaptive Quantization). Set using `EnableVBAQ=true/false`.

Explanation: Adaptive quantization. Prioritize bits to parts of the image human vision tend to care about. Similar to "Psycho Visual Tuning", somewhat mixed results.

# Defaults
It is recommended to try the default settings first as it tries to strike balance between quality and performance. Should you run into performance or image quality issues, please try with the defaults first (remove any extra options).

As a baseline the encoder uses the defaults of AMF (see [AMF docs](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/blob/master/amf/doc/AMF_Video_Encode_API.pdf)).

In addition, OBS makse the following adjustments to the defaults:

## H264
Option | Value | 
-- | -- |
VBAQ | True
Enforce HRD | True
High Motion Quality Boost | False
Deblocking Filter | True
Low Latency Mode | False
CABAC | True
Pre-Encode | True
B-frames | 0

## HEVC
Option | Value | 
-- | -- |
VBAQ | True
Enforce HRD | True
High Motion Quality Boost | False
Low Latency Mode | False