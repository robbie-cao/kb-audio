# Sound Synthesis

## Overview

A synthesizer (usually abbreviated as "synth", also spelled "synthesiser") is an electronic musical instrument that generates electric signals that are converted to sound through instrument amplifiers and loudspeakers or headphones.

Synthesizers may either imitate instruments like piano, Hammond organ, flute, vocals; natural sounds like ocean waves, etc.; or generate new electronic timbres.
They are often played with a musical keyboard, but they can be controlled via a variety of other input devices, including music sequencers, instrument controllers, fingerboards, guitar synthesizers, wind controllers, and electronic drums.

Synthesizers without built-in controllers are often called sound modules, and are controlled via MIDI or CV/Gate using a controller device, often a MIDI keyboard or other controller.

Synthesizers use various methods to generate electronic signals (sounds).

Among the most popular waveform synthesis techniques are

- Subtractive synthesis
- Additive synthesis
- Wavetable synthesis
- Frequency modulation synthesis
- Phase distortion synthesis
- Physical modeling synthesis
- Sample-based synthesis.

![](https://lh6.googleusercontent.com/IBIvuP-r5QWQSxkxUH825JkhOduVZOg1KD5BC9vsVWH5BG-WZVQWGDDYG4d-rNuHUcD5PPwHtzkDbx6fyK4fZU62cpREvhf4MOwKZK4zIbTuMYYmO12WYNvU06qjiqescw)

> https://en.wikipedia.org/wiki/Synthesizer

## Wavetable Synthesis

![](https://www.music.mcgill.ca/~gary/307/week4/img4.gif)

Wavetable synthesis is a technique used in certain digital music synthesizers to produce natural tone-like sounds. The sound of an existing instrument (a single note) is sampled and parsed into a sequence of circular tables of samples or wavetables, each having one period or cycle per table. A set of wavetables with user specified harmonic content can also be generated mathematically. Upon playback, these wavetables are used to fetch samples (table-lookup) in the same manner as in a numerically-controlled oscillator to produce a waveform. However, in wavetable synthesis, the output waveform is not normally static and evolves slowly in time as one wavetable is mixed with another, creating a changing waveform. Looping occurs when the wavetable evolution is halted, slowed, or reversed in time.

Wavetable synthesizers imitate dynamic filters and other computationally expensive synthesis steps by rapidly playing successive wavetables (each with a different waveform stored therein) in sequence. If each waveform is a little duller (or brighter) than the previous, a moving filter effect can be imitated. As noted below, this creates an effect that is equivalent to additive synthesis, but with the restriction that all partials in the additive engine are harmonic (i.e. integer multiples of the fundamental frequency). Wavetable synthesis can also related to frequency modulation synthesis; using wavetables, however, significantly reduces the amount of hardware needed, since the sum of partials at each step of each partial's envelope (the part requiring the most compute power) has already been calculated, and all the CPU needs to do is interpolate between them. By contrast, a basic analog additive or FM synthesizer would need several discrete oscillators, envelope generators and volume controls per voice, and a digital version would require a very fast CPU (that wasn't available when the technology was first being developed), or special (and much more complex) hardware to do the math. For example, the Yamaha DX7 has 6 operators per voice, and 16 voice polyphony, so a total of 96 separate oscillator, EG, and VCA modules would be needed to build an equivalent modular synthesizer. This setup would take several equipment racks to hold. It would also only be useful as an additive synthesizer, since FM requires highly stable oscillators to work properly. (The DX7 actually uses phase modulation -- a sine-wave DCO behind a programmable digital delay line -- combined with two intermediate registers and a fast time division multiplexing system to compute each operator, thus reducing the number of DCOs needed per voice to 1.)

This method differs from simple sample playback in that the output waveform is always generated in real time as the CPU processes the sequence of wavetables, and the data in each table normally represent one period or cycle of the waveform. The name "wavetable" as applied to soundcards and sample-playback synthesizers is a misnomer; see below for an in-depth discussion.jk

> http://en.wikiaudio.org/Wavetable_synthesis

Wavetable Synthesis employs the use of a table with various switchable frequencies played in certain orders (wavetables). As a key is pressed, the sound moves in order through the wavetable, not spontaneously changing the waveform, but smoothly changing its shape into the various waves in the table.

This method produces sounds that can evolve really quickly and smoothly. The method was intended to create digital sounding noises, so it is not used for instrument replication very often, but is an effective way to create pads or harsh-sounding tones like bells or digital sounds.

> https://theproaudiofiles.com/sound-synthesis-basics/

Wavetable synthesis provides an excellent balance between realism and resource use. Because its samples are recorded from real instruments, wavetable synthesis has a substantial advantage over synthesis methods that emulate instrument sounds using simple waveshapes generated in real time. For example, while FM synthesis is known for its excellent bell tones and horns, it has great difficulty producing the rich, complex waveforms characteristic of instruments such as the acoustic piano, guitar, drums and other percussion instruments.

Each instrument definition for a wavetable synthesizer consists of samples and sets of parameters that determine how the synthesizer will articulate them, i.e. shape their playback in terms of volume, pitch, and tone. Seamless automatic repetition of the body of the samples (looping) allows notes to continue sounding indefinitely. Thus a relatively small number of short samples may be used to produce notes of arbitrary pitch, volume and length.

> http://www.qsound.com/technology/player-synthesizer.htm

## General MIDI

General MIDI or GM is a standardized specification for music synthesizers that respond to MIDI messages.

GM imposes several requirements beyond the more abstract MIDI 1.0 specification. While MIDI 1.0 by itself provides a communications protocol which ensures that different instruments can interoperate at a fundamental level (e.g., that pressing keys on a MIDI keyboard will cause an attached MIDI sound module to play musical notes), GM goes further in two ways: it requires that all GM-compatible synthesizers meet a certain minimal set of features, such as being able to play at least 24 notes simultaneously (polyphony), and it attaches specific interpretations to many parameters and control messages which were left under-specified in the MIDI 1.0 spec, such as defining instrument sounds for each of the 128 possible program numbers.

GM synthesizers are required to be able to:

- Allow 24 voices to be active simultaneously (including at least 16 melodic and 8 percussive voices)
- Respond to note velocity
- Support all 16 channels simultaneously (with channel 10 reserved for percussion)
- Support polyphony (multiple simultaneous notes) on each channel

> https://en.wikipedia.org/wiki/General_MIDI

## DLS Format

DLS (from Downloadable Sounds), is standardized file formats for digital musical instrument sound banks (collections of virtual musical instrument programs). The DLS standards also include detailed specifications for how MIDI protocol-controlled music synthesizers should render the instruments in a DLS file. As a result, some people consider DLS primarily a synthesizer specification and only secondarily a file format.


The Downloadable Sounds Level 1 Architecture enables the author to completely define an instrument by combining a recorded waveform with articulation information. An instrument designed this way can be downloaded onto any hardware device that supports the standard and then played like any standard MIDI synthesizer.

Together with MIDI, it delivers the following benefits:
- A common playback experience, unlike GM.
- An unlimited sound palette for both instruments and sound effects, unlike GM.
- True audio interactivity, unlike digital audio.
- MIDI storage compression, unlike digital audio.

The DLS Level 1 synthesizer consists of the following basic elements for each voice:
- A sampled sound source with loop points
- Two 4-segment envelope generators characterized as ADSR (Attack-Decay-Sustain-Release)
- One Low Frequency Oscillator (LFO) generators
- Standardized response to MIDI controllers

The DLS Level 2 synthesizer consists of the following basic elements for each voice:
- A sampled sound source with loop and release
- Two 6-segment envelope generators characterized as DAHDSR (Delay-Attack-Hold-Decay-Sustain-Release)
- Two Low Frequency Oscillator (LFO) generators
- A low pass filter with resonance and dynamic filter cutoff frequency
- Standardized response to MIDI controllers

> http://amei.or.jp/midistandardcommittee/Recommended_Practice/dls1v11b.pdf

> http://amei.or.jp/midistandardcommittee/Recommended_Practice/dls2v10a.pdf

## SoundFont

SoundFont is a brand name that collectively refers to a file format and associated technology that uses sample-based synthesis to play MIDI files.

MIDI files do not contain any sounds, only instructions to render them. To render such files, sample-based MIDI synthesizers use recordings of instruments and sounds stored in a file or ROM chip. SoundFont-compatible synthesizers allow users to use SoundFont banks with custom samples to render their music.

A SoundFont bank contains base samples in PCM format (similar to WAV files) that are mapped to sections on a musical keyboard. A SoundFont bank also contains other music synthesis parameters such as loops, vibrato effect, and velocity-sensitive volume changing.

SoundFont banks can conform to standard sound sets such as General MIDI, or use other wholly custom sound-set definitions.

> https://en.wikipedia.org/wiki/SoundFont

## Synthesizer Software

### FluidSynth

FluidSynth is a real-time software synthesizer based on the SoundFont 2 specifications and has reached widespread distribution.  FluidSynth itself does not have a graphical user interface, but due to its powerful API several applications utilize it and it has even found its way onto embedded systems and is used in some mobile apps.

Features:

- Cross platform (Linux, Mac OSX and Windows to name a few)
- SoundFont 2 support
- Realtime effect control using SoundFont 2.01 modulators
- Playback of MIDI files
- Shared library which can be used in other programs
- Built in command line shell

> http://www.fluidsynth.org

> https://sourceforge.net/projects/fluidsynth/

### TiMidity++

TiMidity++ is a software synthesizer. It can play MIDI files by converting them into PCM waveform data; give it a MIDI data along with digital instrument data files, then it synthesizes them in real-time, and plays. It can not only play sounds, but also can save the generated waveforms into hard disks as various audio file formats.

Features:

- Plays MIDI files without any external MIDI instruments at all
- Understands SMF, MOD, RCP/R36/G18/G36, MFI
- Converts MIDI files into various audio file formats: .wav, .au, .aiff, .ogg and so on
- Uses Gravis Ultrasound compatible patch files and/or SoundFonts as the voice data
- Displays information about the music that is now playing
- Various user interfaces: ncurses, gtk, Win32-GUI, and others
- Plays remote MIDI files over the network
- Plays MIDI files in archive files
- Displays sound spectrogram for the playing music
- Trace playing

> http://timidity.sourceforge.net

> https://en.wikipedia.org/wiki/TiMidity%2B%2B

### Csound

Csound is a sound and music synthesis system, providing facilities for composition and performance over a wide range of platforms. It is not restricted to any style of music, having been used for many years in at least classical, pop, techno, ambient.

Csound can most accurately be described as a compiler. What is a compiler? A compiler is a software that takes textual instructions in the form of source code and converts them into object code. This object code then gets converted into some kind of executable binary in the form of a computer program. Csound works in more or less the same way, only its object code is a stream of numbers representing audio.

> https://sourceforge.net/projects/csound

> http://csound.github.io/

### Nyquist

Nyquist is a language for sound synthesis and music composition. Unlike score languages that tend to deal only with events, or signal processing languages that tend to deal only with signals and synthesis, Nyquist handles both in a single integrated system. Nyquist is also flexible and easy to use because it is based on an interactive Lisp interpreter.

With Nyquist, you can design instruments by combining functions (much as you would using the orchestra languages of Music V, cmusic, or Csound). You can call upon these instruments and generate a sound just by typing a simple expression. You can combine simple expressions into complex ones to create a whole composition.

> https://sourceforge.net/projects/nyquist

> http://www.cs.cmu.edu/~rbd/doc/nyquist/

## Reference

- https://en.wikipedia.org/wiki/Synthesizer
- https://en.wikipedia.org/wiki/Software_synthesizer
- https://en.wikipedia.org/wiki/Sample-based_synthesis
- https://en.wikipedia.org/wiki/Wavetable_synthesis
- https://en.wikipedia.org/wiki/DLS_format
- https://en.wikipedia.org/wiki/General_MIDI
- https://en.wikipedia.org/wiki/SoundFont
- https://www.sfu.ca/sonic-studio/handbook/Sound_Synthesis.html
- http://www.acoustics.salford.ac.uk/acoustics_info/sound_synthesis/
- http://www.qsound.com/technology/player-synthesizer.htm
- http://www.docin.com/p-1042378707.html
- http://www.nyu.edu/classes/bello/FMT_files/10_MIDI_soundcontrol.pdf
- http://www.synthfont.com/sfspec24.pdf
