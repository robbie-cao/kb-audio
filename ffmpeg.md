# FFmpeg

**FFmpeg** is a free software project that produces libraries and programs for handling multimedia data.

FFmpeg includes:

- Libraries
  * libavcodec is a library containing all of the native FFmpeg audio/video encoders and decoders. Most codecs were developed from scratch to ensure best performance and high code reusability.
  * libavformat is a library containing demuxers and muxers for audio/video container formats.
  * libavutil is a helper library containing routines common to different parts of FFmpeg. This library includes hash functions (Adler-32, CRC, MD5, RIPEMD, SHA-1. SHA-2, MurmurHash3, HMAC MD-5, HMAC SHA-1 and HMAC SHA-2), ciphers (DES, RC4, AES, AES-CTR, TEA, XTEA, Blowfish, CAST-128, Twofish and Camellia), LZO decompressor and Base64 encoder/decoder.
  * libavresample is a library containing audio resampling routines from the Libav project, similar to libswresample from ffmpeg.
  * libswresample is a library containing audio resampling routines.
  * libpostproc is a library containing older h263 based video postprocessing routines.
  * libswscale is a library containing video image scaling and colorspace/pixelformat conversion routines.
  * libavfilter is the substitute for vhook which allows the video/audio to be modified or examined between the decoder and the encoder. Filters have been ported from many projects including MPlayer and avisynth.
- Command line tools
  * ffmpeg is a command-line tool that converts audio or video formats. It can also capture and encode in real-time from various hardware and software sources such as a TV capture card.
  * ffserver is an HTTP and RTSP multimedia streaming server for live and recorded broadcasts. It can also be used to time shift live broadcasts.
  * ffplay is a simple media player utilizing SDL and the FFmpeg libraries.
  * ffprobe is a command-line tool to display media information (text, CSV, XML, JSON).


## Usage

```
usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...
```

## Examples

## Reference

- https://en.wikipedia.org/wiki/FFmpeg
- https://www.ffmpeg.org/documentation.html
