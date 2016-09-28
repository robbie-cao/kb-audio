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


## Tips

### Record sound from ALSA device with `ffmpeg`

1. Issue the arecord -L command.
2. Look for the plughw values which corresponds to your hardware. Please note that `plughw` represents a hardware which has been plugged into the computer.
3. Crosscheck the values in /dev/snd/by-id directory.

   When you unplug the webcam(s), the by-id sub directory will vanish from the `/dev/snd` directory.
   This is an excellent test to confirm which plughw is an externally plugged in device.

4. The sound device(s) ID listed there will be the same as or correspond to one of the values obtained from the `arecord -L` command.
5. The command to use is `ffmpeg: -f alsa -i plughw`.
6. Please note, do NOT enclose the `plughw` value in quotes.
7. A working example for me was:

   ```
   ffmpeg -f alsa -i plughw:CARD=U0x46d0x821,DEV=0 -acodec libmp3lame -t 20 output.mp4
   ```

8. You may add the video portion to the above command by adding:

   ```
   -f video4linux2
   ```

> http://askubuntu.com/questions/167360/how-to-format-the-ffmpeg-command-to-record-sound-from-my-webcam

### Live audio stream using `ffmpeg`

```
fmpeg -ac 1 -f alsa -i hw:0,0 -acodec libmp3lame -ab 32k -ac 1 -re -f rtp rtp://localhost:1234

arecord -f cd -D plughw:1,0 | ffmpeg -i - -acodec libmp3lame -ab 32k -ac 1 -re -f rtp rtp://234.5.5.5:1234
```

> http://raspberrypi.stackexchange.com/questions/1466/live-audio-stream-using-ffmpeg

### Record sound from microphone

```
# using alsa-util
arecord -D plughw:0,0 -f S16_LE -c1 -r22050 -t raw | lame -r -s 22.05 -m m -b 64 - mic-input.mp3

# using ffmpeg
ffmpeg -f alsa -ac 2 -i pulse -acodec pcm_s16le -vcodec libx264 -vpre lossless_ultrafast -threads 0 -y voice.wav
# convert wav to mp3 using lame
lame -r -s 22.05 -m m -b 64 voice.wav mic-input.mp3
```

> http://www.pc-freak.net/blog/linux-record-audio-from-console-terminal-arecord-ffmpeg-recliving-2nd-3rd-world-country-blessing/

## Reference

- https://en.wikipedia.org/wiki/FFmpeg
- https://www.ffmpeg.org/documentation.html
- https://trac.ffmpeg.org/wiki/Capture/ALSA
