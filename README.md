# Learning Audio

## Codec

## Software

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

