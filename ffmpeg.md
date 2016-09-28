# FFmpeg

**FFmpeg** is a free software project that produces libraries and programs for handling multimedia data.

FFmpeg includes:

- Libraries
  - *libavcodec* is a library containing all of the native FFmpeg audio/video encoders and decoders. Most codecs were developed from scratch to ensure best performance and high code reusability.
  - *libavformat* is a library containing demuxers and muxers for audio/video container formats.
  - *libavutil* is a helper library containing routines common to different parts of FFmpeg. This library includes hash functions (Adler-32, CRC, MD5, RIPEMD, SHA-1. SHA-2, MurmurHash3, HMAC MD-5, HMAC SHA-1 and HMAC SHA-2), ciphers (DES, RC4, AES, AES-CTR, TEA, XTEA, Blowfish, CAST-128, Twofish and Camellia), LZO decompressor and Base64 encoder/decoder.
  - *libavresample* is a library containing audio resampling routines from the Libav project, similar to libswresample from ffmpeg.
  - *libswresample* is a library containing audio resampling routines.
  - *libpostproc* is a library containing older h263 based video postprocessing routines.
  - *libswscale* is a library containing video image scaling and colorspace/pixelformat conversion routines.
  - *libavfilter* is the substitute for vhook which allows the video/audio to be modified or examined between the decoder and the encoder. Filters have been ported from many projects including MPlayer and avisynth.
- Command line tools
  - `ffmpeg` is a command-line tool that converts audio or video formats. It can also capture and encode in real-time from various hardware and software sources such as a TV capture card.
  - `ffserver` is an HTTP and RTSP multimedia streaming server for live and recorded broadcasts. It can also be used to time shift live broadcasts.
  - `ffplay` is a simple media player utilizing SDL and the FFmpeg libraries.
  - `ffprobe` is a command-line tool to display media information (text, CSV, XML, JSON).


## Usage

```
usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...
```


## Data Flow

`ffmpeg` reads from an arbitrary number of input "files" (which can be regular files, pipes, network streams, grabbing devices, etc.), specified by the "-i" option, and
writes to an arbitrary number of output "files", which are specified by a plain output filename. Anything found on the command line which cannot be interpreted as an
option is considered to be an output filename.

Each input or output file can, in principle, contain any number of streams of different types (video/audio/subtitle/attachment/data). The allowed number and/or types of
streams may be limited by the container format. Selecting which streams from which inputs will go into which output is either done automatically or with the "-map"
option (see the Stream selection chapter).

The transcoding process in `ffmpeg` for each output can be described by the following diagram:

```
         _______              ______________
        |       |            |              |
        | input |  demuxer   | encoded data |   decoder
        | file  | ---------> | packets      | -----+
        |_______|            |______________|      |
                                                   v
                                               _________
                                              |         |
                                              | decoded |
                                              | frames  |
                                              |_________|
         ________             ______________       |
        |        |           |              |      |
        | output | <-------- | encoded data | <----+
        | file   |   muxer   | packets      |   encoder
        |________|           |______________|
```

`ffmpeg` calls the libavformat library (containing demuxers) to read input files and get packets containing encoded data from them. When there are multiple input files,
`ffmpeg` tries to keep them synchronized by tracking lowest timestamp on any active input stream.


## Examples

## Reference

- https://en.wikipedia.org/wiki/FFmpeg
- https://www.ffmpeg.org/documentation.html
