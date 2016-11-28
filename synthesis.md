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

## Reference

- https://en.wikipedia.org/wiki/Synthesizer
- https://en.wikipedia.org/wiki/Software_synthesizer
- https://en.wikipedia.org/wiki/Sample-based_synthesis
- https://en.wikipedia.org/wiki/Wavetable_synthesis
- https://www.sfu.ca/sonic-studio/handbook/Sound_Synthesis.html
- http://www.acoustics.salford.ac.uk/acoustics_info/sound_synthesis/
- http://www.qsound.com/technology/player-synthesizer.htm
- http://www.docin.com/p-1042378707.html
