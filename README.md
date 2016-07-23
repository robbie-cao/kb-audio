# Learning Audio

## Principle

![Sound Arch Simple Abstraction](http://www.alsa-project.org/main/images/2/22/Snd-driver-context.png)

> http://www.alsa-project.org/main/index.php/Minivosc#Understanding_ALSA_driver_architecture

## Codec

## Encoder & Decoder

### MAD: MPEG Audio Decoder

MAD is a high-quality MPEG audio decoder. It currently supports MPEG-1 and the MPEG-2 extension to lower sampling frequencies, as well as the de facto MPEG 2.5 format. All three audio layers — Layer I, Layer II, and Layer III (i.e. MP3) — are fully implemented.

MAD does not yet support MPEG-2 multichannel audio (although it should be backward compatible with such streams) nor does it currently support AAC.

MAD has the following special features:
- 24-bit PCM output
- 100% fixed-point (integer) computation
- completely new implementation based on the ISO/IEC standards
- available under the terms of the GNU General Public License (GPL)

> http://www.underbit.com/products/mad/

## Architecture

### Linux Sound Subsystem

![Linux Sound Subsystem](http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/images/ZTVyMWQ5OHB0Z2lzMC8vbWMzNWE5NC9yMzZnMjdhZ3AuaWNzaTAxLzNmM2hpZ2Y-.jpg)

- The sound core, which is a code base consisting of routines and structures available to other components of the Linux-Sound layer. Like the core layers belonging to other driver subsystems, the sound core provides a level of indirection that renders each component in the sound subsystem independent of the others. The core also plays an important role in exporting the ALSA API to higher applications. The following /dev/snd/* device nodes shown in Figure 13.3 are created and managed by the ALSA core: /dev/snd/controlC0 is a control node (that applications use for controlling volume gain and such), /dev/snd/pcmC0D0p is a playback device (p at the end of the device name stands for playback), and /dev/snd/pcmC0D0c is a recording device (c at the end of the device name stands for capture). In these device names, the integer following C is the card number, and that after D is the device number. An ALSA driver for a card that has a voice codec for telephony and a stereo codec for music might export /dev/snd/pcmC0D0p to read audio streams destined for the former and /dev/snd/pcmC0D1p to channel music bound for the latter.
- Audio controller drivers specific to controller hardware. To drive the audio controller present in the Intel ICH South Bridge chipsets, for example, use the snd_intel8x0 driver.
- Audio codec interfaces that assist communication between controllers and codecs. For AC'97 codecs, use the snd_ac97_codec and ac97_bus modules.
- An OSS emulation layer that acts as a conduit between OSS-aware applications and the ALSA-enabled kernel. This layer exports /dev nodes that mirror what the OSS layer offered in the 2.4 kernels. These nodes, such as /dev/dsp, /dev/adsp, and /dev/mixer, allow OSS applications to run unchanged over ALSA. The OSS /dev/dsp node maps to the ALSA nodes /dev/snd/pcmC0D0*, /dev/adsp corresponds to /dev/snd/pcmC0D1*, and /dev/mixer associates with /dev/snd/controlC0.
- Procfs and sysfs interface implementations for accessing information via /proc/asound/ and /sys/class/sound/.
- The user-space ALSA library, alsa-lib, which provides the libasound.so object. This library eases the job of the ALSA application programmer by offering several canned routines to access ALSA drivers.
- The alsa-utils package that includes utilities such as alsamixer, amixer, alsactl, and aplay. Use alsamixer or amixer to change volume levels of audio signals such as line-in, line-out, or microphone, and alsactl to control settings for ALSA drivers. To play audio over ALSA, use aplay.



> http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/ch13lev1sec2.html

### Linux Audio Architecture

- ALSA
- OSS
- JACK
- PulseAudio

![Linux Audio Architecutre](http://thewelltemperedcomputer.com/Linux/Pic/DiagramAlsa.jpg)

> http://thewelltemperedcomputer.com/Linux/AudioArchitecture.htm

![Linux Sound Components](https://jan.newmarch.name/LinuxSound/Sampled/Architecture/layers.png)

> https://jan.newmarch.name/LinuxSound/Sampled/Architecture/

![Linux Audio Stack](http://linux-audio.com/images/linux-audio-stack.png)

![Linux Auido Components and Relationship](http://1.bp.blogspot.com/-jLXOZF_0txQ/T6K3HZ_3JII/AAAAAAAABrw/y-4zbxrWw-E/s1600/linux_audio.png)

> http://free-electrons.com/doc/embedded_linux_audio.pdf

### Android Audio Subsystem

![Android Audio Subsystem](http://processors.wiki.ti.com/images/1/18/Audio_Stack.jpg)

> http://processors.wiki.ti.com/index.php/TI-Android-GingerBread-2.3.4-DevKit-2.1_PortingGuides#Audio

### ALSA - Advanced Linux Sound Architecture

Advanced Linux Sound Architecture (ALSA) is a software framework and part of the Linux kernel that provides an application programming interface (API) for sound card device drivers. Some of the goals of the ALSA project at its inception were automatic configuration of sound-card hardware and graceful handling of multiple sound devices in a system. ALSA is released under the GNU General Public License (GPL) and the GNU Lesser General Public License (LGPL).

![ALSA in Kernel](https://upload.wikimedia.org/wikipedia/commons/9/91/Linux_kernel_and_gaming_input-output_latency.svg)

>http://www.alsa-project.org/main/index.php/Main_Page
>
> https://en.wikipedia.org/wiki/Advanced_Linux_Sound_Architecture
>
> http://www.alsa-project.org/~frank/alsa-sequencer/

#### ALSA Structure

![Structure of ALSA](http://www.alsa-project.org/~tiwai/lk2k/archtect.gif)

> http://www.alsa-project.org/~tiwai/lk2k/lk2k.html

#### ALSA SoC Layer

The overall project goal of the ALSA System on Chip (ASoC) layer is to provide better ALSA support for embedded system on chip procesors (e.g. pxa2xx, au1x00, iMX, etc) and portable audio codecs. Currently there is some support in the kernel for SoC audio, however it has some limitations.

The ASoC layer is designed to address these issues and provide the following features:
- Codec independence. Allows reuse of codec drivers on other platforms and machines.
- Easy I2S/PCM audio interface setup between codec and SoC. Each SoC interface and codec registers it's audio interface capabilities with the core and are subsequently matched and configured when the application hw params are known.
- Dynamic Audio Power Management (DAPM). DAPM automatically sets the codec to it's minimum power state at all times. This includes powering up/down internal power blocks depending on the internal codec audio routing and any active streams.
- Pop and click reduction. Pops and clicks can be reduced by powering the codec up/down in the correct sequence (including using digital mute). ASoC signals the codec when to change power states.
- Machine specific controls: Allow machines to add controls to the sound card. e.g. volume control for speaker amp.

To achieve all this, ASoC basically splits an embedded audio system into 3 components:
- Codec driver: The codec driver is platform independent and contains audio controls, audio interface capabilities, codec dapm definition and codec IO functions.
- Platform driver: The platform driver contains the audio dma engine and audio interface drivers (e.g. I2S, AC97, PCM) for that platform.
- Machine driver: The machine driver handles any machine specific controls and audio events. i.e. turning on an amp at start of playback.

> http://www.rpsys.net/openzaurus/patches/alsa/info.html
>
> http://www.alsa-project.org/main/index.php/ASoC

![ASoC Sample AM/DM37x, OMAP35x](http://processors.wiki.ti.com/images/9/91/Asoc_architecture.png)

### PulseAudio

![PulseAudio and ALSA](http://i.stack.imgur.com/IJkHa.png)

> http://askubuntu.com/questions/581128/what-is-the-relation-between-alsa-and-pulseaudio-sound-architecture

PulseAudio runs a sound server, a background process accepting sound input from one or more sources (processes or capture devices) and redirecting it to one or more sinks (sound cards, remote network PulseAudio servers, or other processes).

One of the goals of PulseAudio is to reroute all sound streams through it, including those from processes that attempt to directly access the hardware (like legacy OSS applications). PulseAudio achieves this by providing adapters to applications using other audio systems, like aRts and ESD.

The main PulseAudio features include:

- Per-application volume controls.
- An extensible plugin architecture with support for loadable modules.
- Compatibility with many popular audio applications.
- Support for multiple audio sources and sinks.
- Low latency operation and latency measurement.
- A zero-copy memory architecture for processor resource efficiency.
- Ability to discover other computers using PulseAudio on the local network and play sound through their speakers directly.
- Ability to change which output device an application plays sound through while the application is playing sound (without the application needing to support this, and indeed without even being aware that this happened).
- A command-line interface with scripting capabilities.
- A sound daemon with command line reconfiguration capabilities.
- Built-in sample conversion and resampling capabilities.
- The ability to combine multiple sound cards into one.
- The ability to synchronize multiple playback streams.
- Bluetooth audio devices with dynamic detection.
- The ability to enable system wide equalization.

> https://en.wikipedia.org/wiki/PulseAudio

## Software

### `arecord`

Alias to `aplay -C`.

`arecord` is a command-line soundfile recorder for the ALSA soundcard driver. 
It supports several file formats and multiple soundcards with multiple devices. 
If recording with interleaved mode samples the file is automatically split before the 2GB filesize.

> http://linux.die.net/man/1/aplay

### `aplay`

`aplay` - command-line sound recorder and player for ALSA soundcard driver.

```
Usage: aplay [OPTION]... [FILE]...

-h, --help              help
    --version           print current version
-l, --list-devices      list all soundcards and digital audio devices
-L, --list-pcms         list device names
-D, --device=NAME       select PCM by name
-q, --quiet             quiet mode
-t, --file-type TYPE    file type (voc, wav, raw or au)
-c, --channels=#        channels
-f, --format=FORMAT     sample format (case insensitive)
-r, --rate=#            sample rate
-d, --duration=#        interrupt after # seconds
-M, --mmap              mmap stream
-N, --nonblock          nonblocking mode
-F, --period-time=#     distance between interrupts is # microseconds
-B, --buffer-time=#     buffer duration is # microseconds
    --period-size=#     distance between interrupts is # frames
    --buffer-size=#     buffer duration is # frames
-A, --avail-min=#       min available space for wakeup is # microseconds
-R, --start-delay=#     delay for automatic PCM start is # microseconds 
                        (relative to buffer size if <= 0)
-T, --stop-delay=#      delay for automatic PCM stop is # microseconds from xrun
-v, --verbose           show PCM structure and setup (accumulative)
-V, --vumeter=TYPE      enable VU meter (TYPE: mono or stereo)
-I, --separate-channels one file for each channel
-i, --interactive       allow interactive operation from stdin
-m, --chmap=ch1,ch2,..  Give the channel map to override or follow
    --disable-resample  disable automatic rate resample
    --disable-channels  disable automatic channel conversions
    --disable-format    disable automatic format conversions
    --disable-softvol   disable software volume control (softvol)
    --test-position     test ring buffer position
    --test-coef=#       test coefficient for ring buffer position (default 8)
                        expression for validation is: coef * (buffer_size / 2)
    --test-nowait       do not wait for ring buffer - eats whole CPU
    --max-file-time=#   start another output file when the old file has recorded
                        for this many seconds
    --process-id-file   write the process ID here
    --use-strftime      apply the strftime facility to the output file name
    --dump-hw-params    dump hw_params of the device
    --fatal-errors      treat all errors as fatal
Recognized sample formats are: S8 U8 S16_LE S16_BE U16_LE U16_BE S24_LE S24_BE U24_LE U24_BE S32_LE S32_BE U32_LE U32_BE FLOAT_LE FLOAT_BE FLOAT64_LE FLOAT64_BE IEC958_SUBFRAME_LE IEC958_SUBFRAME_BE MU_LAW A_LAW IMA_ADPCM MPEG GSM SPECIAL S24_3LE S24_3BE U24_3LE U24_3BE S20_3LE S20_3BE U20_3LE U20_3BE S18_3LE S18_3BE U18_3LE U18_3BE G723_24 G723_24_1B G723_40 G723_40_1B DSD_U8 DSD_U16_LE
Some of these may not be available on selected hardware
The available format shortcuts are:
-f cd (16 bit little endian, 44100, stereo)
-f cdr (16 bit big endian, 44100, stereo)
-f dat (16 bit little endian, 48000, stereo)
```

Exmaples:

```
aplay -c 1 -t raw -r 22050 -f mu_law foobar
```
will play the raw file "foobar" as a 22050-Hz, mono, 8-bit, Mu-Law .au file.

```
arecord -d 10 -f cd -t wav -D copy foobar.wav
```
will record foobar.wav as a 10-second, CD-quality wave file, using the PCM "copy" (which might be defined in the user's .asoundrc file as:

```
pcm.copy {
  type plug
  slave {
    pcm hw
  }
  route_policy copy
}
```

```
arecord -t wav --max-file-time 30 mon.wav
```
record from the default audio source in monaural, 8,000 samples per second, 8 bits per sample. Start a new file every 30 seconds. File names are mon-nn.wav, where nn increases from 01. The file after mon-99.wav is mon-100.wav.

```
arecord -f cd -t wav --max-file-time 3600 --use-strftime %Y/%m/%d/listen-%H-%M-%v.wav
```
Record in stereo from the default audio source. Create a new file every hour. The files are placed in directories based on their start dates and have names which include their start times and file numbers.

![aplay in linux](http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/images/MjltNHJhaS9kNy8zY3JncDA4dHMvOTMxZTZhZzU1LjhpMWYvcGdoaWZjaXMwM2c-.jpg)

### `alsamixer`

`alsamixer` - soundcard mixer for ALSA soundcard driver, with ncurses interface

> http://linux.die.net/man/1/alsamixer

### `amixer`

`amixer` - command-line mixer for ALSA soundcard driver

`amixer` allows command-line control of the mixer for the ALSA soundcard driver. amixer supports multiple soundcards.

`amixer` with no arguments will display the current mixer settings for the default soundcard and device. This is a good way to see a list of the simple mixer controls you can use.

> http://linux.die.net/man/1/amixer

### `sox`

SoX - Sound eXchange, the Swiss Army knife of audio manipulation

```
sox [global-options] [format-options] infile1
    [[format-options] infile2] ... [format-options] outfile
    [effect [effect-options]] ...

play [global-options] [format-options] infile1
    [[format-options] infile2] ... [format-options]
    [effect [effect-options]] ...

rec [global-options] [format-options] outfile
    [effect [effect-options]] ...
```

SoX reads and writes audio files in most popular formats and can optionally apply effects to them; 
it can combine multiple input sources, synthesise audio, and, on many systems, act as a general purpose 
audio player or a multi-track audio recorder. It also has limited ability to split the input in to multiple output files.

Almost all SoX functionality is available using just the `sox` command, however, to simplify playing and 
recording audio, if SoX is invoked as `play` the output file is automatically set to be the default sound 
device and if invoked as `rec` the default sound device is used as an input source. Additionally, the `soxi`(1) 
command provides a convenient way to just query audio file header information.

Examples:

```
sox recital.au recital.wav
```
translates an audio file in Sun AU format to a Microsoft WAV file, whilst
```
sox recital.au -r 12k -b 8 -c 1 recital.wav vol 0.7 dither
```
performs the same format translation, but also changes the audio sampling rate & sample size, down-mixes to mono, and applies the vol and dither effects.
```
sox -r 8k -u -b 8 -c 1 voice-memo.raw voice-memo.wav
```
converts 'raw' (a.k.a. 'headerless') audio to a self-descibing file format,
```
sox slow.aiff fixed.aiff speed 1.027
```
adjusts audio speed,
```
sox short.au long.au longer.au
```
concatenates two audio files, and
```
sox -m music.mp3 voice.wav mixed.flac
```
mixes together two audio files.
```
play "The Moonbeams/Greatest/*.ogg" bass +3
```
plays a collection of audio files whilst applying a bass boosting effect,
```
play -n -c1 synth sin %-12 sin %-9 sin %-5 sin %-2 fade q 0.1 1 0.1
```
plays a synthesised 'A minor seventh' chord with a pipe-organ sound,
```
rec -c 2 test.aiff trim 0 10
```
records 10 seconds of stereo audio, and
```
rec -M take1.aiff take1-dub.aiff
```
records a new track in a multi-track recording.
```
rec -r 44100 -2 -s -p silence 1 0.50 0.1% 1 10:00 0.1% | \
     sox -p song.ogg silence 1 0.50 0.1% 1 2.0 0.1% : \
     newfile : restart
```
records a stream of audio such as LP/cassette and splits in to multiple audio files at points with 2 seconds of silence. Also does not start recording until it detects audio is playing and stops after it sees 10 minutes of silence.

> http://linux.die.net/man/1/sox

### `mpg123`

`mpg123` - play audio MPEG 1.0/2.0/2.5 stream (layers 1, 2 and 3)

```
mpg123 [ options ] file ... | URL ... | -
```

`mpg123` reads one or more files (or standard input if ''-'' is specified) or URLs and plays them on the audio device (default) or outputs them to stdout. file/URL is assumed to be an MPEG audio bit stream.

> http://linux.die.net/man/1/mpg123

### `madplay`

`madplay` - decode and play MPEG audio stream(s)

```
madplay [options] file ...
madplay [options] -o [type:]path file ...
```

`madplay` is a command-line MPEG audio decoder and player based on the MAD library (libmad).

> http://manpages.ubuntu.com/manpages/precise/man1/madplay.1.html

### `fluidsynth`

![fluidsynth](http://tedfelix.com/linux/linux-midi.png)

> http://tedfelix.com/linux/linux-midi.html

## Awesome

### Noise Generator

**Using SoX (Sound eXchange)**

On Mac OS:
```
$ brew install sox
$ cat /dev/urandom | sox -traw -r44100 -b16 -e unsigned-integer - -tcoreaudio
```

On Linux:
```
sudo apt-get install sox
sox -traw -r44100 -b16 -e unsigned-integer /dev/urandom -d
```

Explanation:

First, we take the contents of the random generator by doing `cat /dev/urandom`, after which we pipe them to `sox`.

Sox takes a couple of options to tell it what kind of data it’s receiving:
* `-traw` Tells sox to consider incoming data as raw, uncompressed audio.
* `-r44100` Sets the input sampling rate to 44.1 Kiloherz.
* `-b16` Tells sox the bit-depth for this audio stream is 16 bits.
* `-e` unsigned-integer Says that the raw data should be considere an unsigned integer. (Which are the kind of numbers /dev/urandom outputs.)
* `-` This stray minus sign is very **IMPORTANT**! It tells sox to take the stream that was piped to it as a source.
* `-tcoreaudio` tells sox to use the ‘coreaudio’ device for output.

```
# rain-like sound
play -t sl -r48000 -c2 - synth -1 pinknoise tremolo .1 40 < /dev/zero
```

> https://blogs.fsfe.org/marklindhout/2013/02/need-to-play-high-fidelity-white-noise-listen-to-devurandom-on-osx/



**Noise in Different Colors**

  ```
  $ play -n synth 60:00 whitenoise
  ```

> http://dirk.net/2009/07/14/white-noise-generator-for-linux/


**Imitates the Soft Murmur of the Sea**

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.3 10
  ```

Explanation:

This command first generates and mixes brown noise and pink noise, which I find to be the most comfortable and natural noise.
Then it generates a sine wave of 0.3 Hz with an offset of 10% and uses this to modulate the amplitude of our mixed noises to
produce the sound of ocean waves.

Modifications:
- Timer:
  You can add a timer and limit the playback duration by specifying the number of seconds, the number of minutes and
  seconds (mm:ss) or the number of hours, minutes and seconds (hh:mm:ss) right before brownnoise. Here's an example for one hour:

  ```
  play -n synth 1:0:0 brownnoise synth pinknoise mix synth sine amod 0.3 10
  ```

- Wave frequency:
  If you want the waves to hit the beach more or less frequent, simply change the frequency of the sine wave used for amplitude
  modification (0.3 in the above command). The number represents the amount of waves per second, so a frequency of 0.1 Hz will
  cause 0.1 waves per second and therefore make one wave last for 10 seconds:

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.1 10
  ```

- Minimum background noise volume:
  The sine that is used for amplitude modulation got shifted to an offset of 10%, so the brown-pink noise will always be played
  with at least 10% volume. If you want a stronger or weaker background noise, increase or decrease this offset to your needs.
  Here's an example with 20% background noise:

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.3 20
  ```

**Using `aplay`**

  ```
  aplay --channels=2 --format=S16_LE --rate=44100 --duration=3600 /dev/urandom
  ```

**Using `ffmpeg`**

FFMpeg has an audio noise source filter. You can play it using ffplay:
  ```
  ffplay -f lavfi -showmode 0 -i 'anoisesrc=color=brown'
  ```
The arg to -i is interpreted as a lavfi filter graph, because of -f lavfi. -showmode 0 disables ffplay's default audio
visualizer window, which it shows by default for audio-only inputs.

As you can see from the output of ffmpeg -h filter=anoisesrc, you get a choice of brown/pink/white noise at whatever
amplitude and samplerate you like, optionally with a finite duration.

> http://askubuntu.com/questions/789465/generate-white-noise-to-calm-a-baby

**Star Trek Background Noise**

  ```
  play -n -c1 synth whitenoise lowpass -1 120 lowpass -1 120 lowpass -1 120 gain +14
  ```

  ```
  #!/bin/sh
  # Engage Warp Drive

  # Requires package 'sox'
  # http://www.reddit.com/r/linux/comments/n8a2k/commandline_star_trek_engine_noise_comment_from/

  # odokemono
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c36xkjx
  # original
  # play -n -c1 synth whitenoise band -n 100 20 band -n 50 20 gain +25  fade h 1 864000 1

  # noname-_-
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c373gpa
  # stereo
  # play -c2 -n synth whitenoise band -n 100 24 band -n 300 100 gain +20

  # braclayrab
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c372pyy
  # TNG
  # play -n -c1 synth whitenoise band 100 20 compand .3,.8 -1,-10 gain +20

  play -n -c1 synth whitenoise lowpass -1 120 lowpass -1 120 lowpass -1 120 gain +14
  ```

> http://askubuntu.com/questions/789465/generate-white-noise-to-calm-a-baby

## Misc

### Color of Noise

- White
- Pink
- Red (Brownian)
- Grey

![Color of Noise](https://upload.wikimedia.org/wikipedia/commons/6/6c/The_Colors_of_Noise.png)

In audio engineering, electronics, physics, and many other fields, the color of a noise signal (a signal produced by a stochastic process) is generally understood to be some broad characteristic of its power spectrum. Different colors of noise have significantly different properties: for example, as audio signals they will sound different to human ears, and as images they will have a visibly different texture. Therefore, each application typically requires noise of a specific color. This sense of 'color' for noise signals is similar to the concept of timbre in music (which is also called "tone color"); however the latter is almost always used for sound, and may consider very detailed features of the spectrum.

> https://en.wikipedia.org/wiki/Colors_of_noise

## Reference
- https://jan.newmarch.name/LinuxSound/index.html
- http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/
- http://processors.wiki.ti.com/index.php/TI-Android-GingerBread-2.3.4-DevKit-2.1_PortingGuides
- http://tedfelix.com/linux/linux-midi.html
