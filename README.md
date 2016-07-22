# Learning Audio

## Codec

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

## Awesome

### While Noise

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

## Reference

