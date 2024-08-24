- [Google Summer of Code 2024 Report](#google-summer-of-code-2024-report)
- [Project Overview](#project-overview)
	- [Background](#background)
	- [Project Status](#project-status)
    - [Minimal PU21 filter](#minimal-pu21-filter)
      - [Paramaters](#parameters)
    - [Future Projected Enhancements](#future-projected-enhancements)
- [Experience](#experience)
	- [Challenges and approach](#challenges-and-approach)
	- [Acknowledgements](#acknowledgements)

## Google Summer of Code 2024 Report

I am Rajiv Harlalka, a final year under-graduate student at Indian Institute of 
Technology, Kharagpur (IIT KGP). I was selected as a **Student Contributor** for Google Summer of Code 2024 
at FFmpeg for the large-sized project [Addition of PU21 encoding filter](https://summerofcode.withgoogle.com/programs/2024/projects/Z2tq722P).

## Project Overview

### Background

FFmpeg is a open-source multimedia to decode, encode, transcode, mux, demux, stream, filter and play audio and video. The package consists of ffmpeg, the main converting tool, ffprobe, a tool to analyse media metadata and understand it's background, and ffplay, to stream a media on the fly with various filter applied.
FFmpeg contains various filters for applying various type of filters on the video and audio supporting numerous encoders and decoders as well as container types.

The aim of this Google Summer of Code Project was addition of an additional filter for PU21 Transform Encoding.

PU21 (Perceptual Uniform)[^1] has been developed to convert absolute high dynamic range (HDR) linear color values into approximately perceptually uniform (PU) values, which can be used with standard quality metrics. This transformation would eventually help evaluate HDR videos based on quality tests known for SDR ones.

### Project Status

#### Minimal PU21 filter

With the addition of the [vf_pu21.c](https://github.com/rajivharlalka/FFmpeg/blob/pu21_transform/libavfilter/vf_pu21.c) to the list of FFmpeg's list of filters, an HDR video can be converted from 10bit HDR image/video to PU21 domain.

The encoded media can now be used in parallel with metrics such as PSNR, SSIM, defined originally in the SDR media domain, to be used for quatifying media similarity.

The following command can be used to generate an output EXR image from an input HDR video at a timestamp of 2 sec of one frame.
`ffmpeg -ss 00:00:02 -i hdr_video.MOV -frames:v 1 -vf "zscale=t=linear,pu21" output.EXR`

##### Paramaters

The following are the parameters that the filter supports.

- multiplier: Muliplier to scale the absolute pixel value by a factor. (max: 10000, min:1, default: 1)
- mode: The original PU21 paper mentions 4 modes of the encoding with different parametric values. The values would be 
        0: BANDING
        1: BANDING GLARE (DEFAULT)
        2:PEAKS
        3:PEAKS GLARE

#### Future Projected Enhancements

##### Linearization using EOTF/OOTF
Currently the pu21 filter requires the media to be in linear domain instead of the known PQ/HLG formats.
An EOTF/OOTF[^3] conversion is being implemented in the PU21 filter itself that would remove the need pass the media through the zscale filter for linearization.

##### Parallelization using slicing

Currently the filter does not use the benefit of parallelization of the encoding by slicing the frame. Having this as an enhancement would lead to making the encoding process faster.

## Experience
 
Google Summer of Code 2024 was a great learning experience for me. It provided me a unique opportunity to explore the multimedia background and the **FFmpeg** ecosystem.

### Challenges and approach

- Coming from a background without any multimedia experience was quite tough given that I had to understand each terminology, meaning of every format to really understand what each line of my code would do. This was one of the biggest challenge for me. The [Digital Video Introduction Repository](https://github.com/leandromoreira/digital_video_introduction) was very helpful in understanding a lot of the basic of video/audio encoding/decoding

- Little to No Documentation on the FFmpeg's internal functions made it quite complicated to write code and it became more of a hit and trial to understand which flags or which functions to use. A Document on writing-filters was my starting point and reading through other filters enabled some feasibility in understanding.

- Documents mostly used of multimedia jargons and expected the reader prior experience in my opinion, which made it even harder to understand documents such as the ITU-R BT REC: 2100[^2] which defines the EOTF/OETF for HLG and PQ. Reading the document multiple times, discussions with Cosmin and some online research helped bridge some of this gap.

[^1]: Original Github Repository for the research on PU21: [link](https://github.com/gfxdisp/pu21) 
[^2]: Rec BT 2100 Document on HDR media [document](https://www.itu.int/dms_pubrec/itu-r/rec/bt/R-REC-BT.2100-1-201706-S!!PDF-E.pdf)
[^3]: Table 4: of Rec 2100 Document [document](https://www.itu.int/dms_pubrec/itu-r/rec/bt/R-REC-BT.2100-1-201706-S!!PDF-E.pdf)

### Acknowledgements

I am grateful to my mentor, [Cosmin](https://github.com/cosminaught), for guiding me throughout the project and helping me whenever I got stuck. He provided me with valuable review suggestions and helped me overcome many of my tiny issues. His approach of taking *baby steps* or what is generally known as *first principle approach* enabled me to progress my project without getting too much anxious about the complexity of the project.

I would also like to thank the Thilo and FFmpeg Organization for giving me this opportunity to work on such an interesting project and I look forward to contributing to the FFmpeg project in the future.

