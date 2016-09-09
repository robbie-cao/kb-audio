# `amixer` usage

**`amixer`** - command-line mixer for ALSA soundcard driver

**`amixer`** allows command-line control of the mixer for the ALSA soundcard driver. amixer supports multiple soundcards.

**`amixer`** with no arguments will display the current mixer settings for the default soundcard and device. **This is a good way to see a list of the simple mixer controls you can use.**

**`alsamixer`** is an ncurses mixer program for use with the  ALSA  soundcard drivers.

![alsamixer](https://upload.wikimedia.org/wikipedia/commons/8/86/Alsamixer.png)

## Usage

```
Usage: amixer <options> [command]

Available options:
  -h,--help       this help
  -c,--card N     select the card
  -D,--device N   select the device, default 'default'
  -d,--debug      debug mode
  -n,--nocheck    do not perform range checking
  -v,--version    print version of this program
  -q,--quiet      be quiet
  -i,--inactive   show also inactive controls
  -a,--abstract L select abstraction level (none or basic)
  -s,--stdin      Read and execute commands from stdin sequentially
  -R,--raw-volume Use the raw value (default)
  -M,--mapped-volume Use the mapped volume

Available commands:
  scontrols       show all mixer simple controls
  scontents       show contents of all mixer simple controls (default command)
  sset sID P      set contents for one mixer simple control
  sget sID        get contents for one mixer simple control
  controls        show all controls for given card
  contents        show contents of all controls for given card
  cset cID P      set control contents for one control
  cget cID        get control contents for one control
```

## Examples

Examples

**`amixer -c 1 sset Line,0 80%,40% unmute cap`**

  will set the second soundcard's left line input volume to 80% and right line input to 40%, unmute it, and select it as a source for capture (recording).

**`amixer -c 1 -- sset Master playback -20dB`**

  will set the master volume of the second card to -20dB. If the master has multiple channels, all channels are set to the same value.

**`amixer -c 1 set PCM 2dB+`**

  will increase the PCM volume of the second card with 2dB. When both playback and capture volumes exist, this is applied to both volumes.

**`amixer -c 2 cset iface=MIXER,name='Line Playback Volume",index=1 40%`**

  will set the third soundcard's second line playback volume(s) to 40%

**`amixer -c 2 cset numid=34 40%`**

  will set the 34th soundcard element to 40%

> http://linux.die.net/man/1/amixer

## Advanced

**`(amixer get Master | grep off > /dev/null && amixer -q set Master unmute) || amixer -q set Master mute`**

  Toogle state (mute / unmute)

**`mixer -c 0 set Master toggle | tail -1 | awk '{print $4}' | sed "s/[^0-9]//g" ; amixer -c 0 set Speaker toggle >/dev/null; amixer -c 0 set Front toggle >/dev/null`**

  Unmuting master channel, printing only percent value, while suppressing other outputs

**`amixer -c 0 set Master toggle | sed -n "$ p" | awk '{print $4}' | sed "s/[^0-9]//g"`**

  Mute Master sound channel, printing only the percent volume

**`check(){ power=$(acpi -a) ; if [[ $power == *on-line* ]] ; then echo "supply is on"; else echo "somebody is steeling your laptop"; amixer -c0 set Master 100+ unmute ; mpg123 nuclear-alarm.mp3 ; fi } ;while true; do check ; sleep 2 ; done`**

  Power Supply Triggered Alert

  Checks whether your power supply is still plugged in. If not it will trigger an alarm at maximum volume.

**`on="off"; off="on"; now=$(amixer get Master | tr -d '[]' | grep "Playback.*%" |head -n1 |awk '{print $7}'); amixer sset Master ${!now}`**

  This toggles mute on the Master channel of an alsa soundcard

> http://www.commandlinefu.com/commands/using/amixer

## Steps

To use `amixer` to configure ALSA soundcard driver, you need understand those parameters you are going to set. Simple steps as below:

1. Check `amixer` command and options with `amixer --help`

   Refer to [`amixer` usage](#usage).

2. Check interfaces and contents (provided by the sound system on target) which you can control with `amixer scontrols` / `amixer scontents` or `amixer controls` / `amixer contengs`

   **It's important step as different board may provide different interface and contents to control.**

   Example:

   ```
   # amixer controls
   numid=10,iface=MIXER,name='Headphone Playback ZC Switch'
   numid=9,iface=MIXER,name='Headphone Playback Volume'
   numid=15,iface=MIXER,name='PCM Playback -6dB Switch'
   numid=39,iface=MIXER,name='Mono Output Mixer Left Switch'
   numid=40,iface=MIXER,name='Mono Output Mixer Right Switch'
   numid=17,iface=MIXER,name='ADC High Pass Filter Switch'
   numid=34,iface=MIXER,name='ADC PCM Capture Volume'
   numid=16,iface=MIXER,name='ADC Polarity'
   numid=2,iface=MIXER,name='Capture Volume ZC Switch'
   numid=3,iface=MIXER,name='Capture Switch'
   numid=1,iface=MIXER,name='Capture Volume'
   numid=8,iface=MIXER,name='Playback Volume'
   numid=21,iface=MIXER,name='3D Filter Lower Cut-Off'
   numid=20,iface=MIXER,name='3D Filter Upper Cut-Off'
   numid=23,iface=MIXER,name='3D Switch'
   numid=22,iface=MIXER,name='3D Volume'
   numid=31,iface=MIXER,name='ALC Attack'
   numid=30,iface=MIXER,name='ALC Decay'
   numid=24,iface=MIXER,name='ALC Function'
   numid=28,iface=MIXER,name='ALC Hold Time'
   numid=25,iface=MIXER,name='ALC Max Gain'
   numid=27,iface=MIXER,name='ALC Min Gain'
   <...>
   ```

   ---

   ```
   # amixer contents
   numid=10,iface=MIXER,name='Headphone Playback ZC Switch'
     ; type=BOOLEAN,access=rw------,values=2
     : values=on,on
   numid=9,iface=MIXER,name='Headphone Playback Volume'
     ; type=INTEGER,access=rw---R--,values=2,min=0,max=127,step=0
     : values=115,115
     | dBscale-min=-121.00dB,step=1.00dB,mute=1
   numid=15,iface=MIXER,name='PCM Playback -6dB Switch'
     ; type=BOOLEAN,access=rw------,values=1
     : values=off
   numid=39,iface=MIXER,name='Mono Output Mixer Left Switch'
     ; type=BOOLEAN,access=rw------,values=1
     : values=off
   numid=40,iface=MIXER,name='Mono Output Mixer Right Switch'
     ; type=BOOLEAN,access=rw------,values=1
     : values=off
   numid=17,iface=MIXER,name='ADC High Pass Filter Switch'
     ; type=BOOLEAN,access=rw------,values=1
     : values=off
   numid=34,iface=MIXER,name='ADC PCM Capture Volume'
     ; type=INTEGER,access=rw---R--,values=2,min=0,max=255,step=0
     : values=195,195
     | dBscale-min=-97.00dB,step=0.50dB,mute=0
   numid=16,iface=MIXER,name='ADC Polarity'
     ; type=ENUMERATED,access=rw------,values=1,items=4
     ; Item #0 'No Inversion'
     ; Item #1 'Left Inverted'
     ; Item #2 'Right Inverted'
     ; Item #3 'Stereo Inversion'
     : values=0
   numid=2,iface=MIXER,name='Capture Volume ZC Switch'
     ; type=INTEGER,access=rw------,values=2,min=0,max=1,step=0
     : values=0,0
   numid=3,iface=MIXER,name='Capture Switch'
     ; type=BOOLEAN,access=rw------,values=2
     : values=off,off
   numid=1,iface=MIXER,name='Capture Volume'
     ; type=INTEGER,access=rw---R--,values=2,min=0,max=63,step=0
     : values=46,46
     | dBscale-min=-97.00dB,step=0.50dB,mute=0
   numid=8,iface=MIXER,name='Playback Volume'
     ; type=INTEGER,access=rw---R--,values=2,min=0,max=255,step=0
     : values=255,255
     | dBscale-min=-127.00dB,step=0.50dB,mute=1
   <...>
   ```

3. Understand controls, contents and parameters to be set

   In summary, use `cget` / `sget` command to check interface/contents and then use `cset` / `sset` command to set the interface with proper parameters you want.

   Example:

   If you want to set **'Line In Volume'**:

   a) Get in *cID* with `amixer controls`:

      ```
      # amixer controls
      numid=49,iface=MIXER,name='Headphone Mixer Aux Playback Volume'
      numid=43,iface=MIXER,name='Headphone Mixer Beep Playback Volume'
      numid=32,iface=MIXER,name='Headphone Playback ZC Switch'
      numid=4,iface=MIXER,name='Headphone Playback Switch'
      numid=3,iface=MIXER,name='Headphone Playback Volume'
      numid=6,iface=MIXER,name='PCM Playback Volume'
      numid=5,iface=MIXER,name='Line In Volume'        <-- interface to be set
      <...>
      ```

   b) Check the contents which can be set for 'Line In Volume' with `amixer cget`:

      ```
      # amixer cget numid=5,iface=MIXER,name='Line In Volume'
      numid=5,iface=MIXER,name='Line In Volume'
        ; type=INTEGER,access=rw---R--,values=2,min=0,max=31,step=0
        : values=23,23
        | dBscale-min=-34.50dB,step=1.50dB,mute=0
      ```

   c) The result shows the volume setting is *0~31* and current setting is *23*. If you want to set the volume to *25*, command as below:

      ```
      # amixer cset numid=5,iface=MIXER,name='Line In Volume' 25
      numid=5,iface=MIXER,name='Line In Volume'
        ; type=INTEGER,access=rw---R--,values=2,min=0,max=31,step=0
        : values=25,25
        | dBscale-min=-34.50dB,step=1.50dB,mute=0
      ```

   Similarly, use `scontrols` / `scontents` and `sget` / `sset` command pairs for simple control. For example, to set volume for 'Headphone', steps as below:

   a) Get in *sID* with `amixer scontrols`:

      ```
      # amixer scontrols
      Simple mixer control 'Headphone',0           <-- interface to be set
      Simple mixer control 'Headphone Playback ZC',0
      Simple mixer control 'Speaker',0
      Simple mixer control 'Speaker AC',0
      Simple mixer control 'Speaker DC',0
      Simple mixer control 'Speaker Playback ZC',0
      Simple mixer control 'PCM Playback -6dB',0
      Simple mixer control 'Mono Output Mixer Left',0
      Simple mixer control 'Mono Output Mixer Right',0
      Simple mixer control 'Playback',0
      Simple mixer control 'Capture',0
      Simple mixer control '3D',0
      Simple mixer control '3D Filter Lower Cut-Off',0
      Simple mixer control '3D Filter Upper Cut-Off',0
      Simple mixer control 'ADC High Pass Filter',0
      Simple mixer control 'ADC PCM',0
      <...>
      ```

   b) Check the simple contents which can be set for 'Headphone' with `amixer sget`:

      ```
      # amixer sget 'Headphone',0
      Simple mixer control 'Headphone',0
      Capabilities: pvolume
      Playback channels: Front Left - Front Right
      Limits: Playback 0 - 127
      Mono:
      Front Left: Playback 127 [100%] [6.00dB]
      Front Right: Playback 127 [100%] [6.00dB]
      ```
   c) Set volume with `amixer sset`:

      ```
      # amixer -c 0 sset Headphone,0 120
      Simple mixer control 'Headphone',0
      Capabilities: pvolume
      Playback channels: Front Left - Front Right
      Limits: Playback 0 - 127
      Mono:
      Front Left: Playback 120 [94%] [-1.00dB]
      Front Right: Playback 120 [94%] [-1.00dB]
      ```

      -or-

      ```
      # amixer sset 'Headphone',0 80%
      Simple mixer control 'Headphone',0
      Capabilities: pvolume
      Playback channels: Front Left - Front Right
      Limits: Playback 0 - 127
      Mono:
      Front Left: Playback 102 [80%] [-19.00dB]
      Front Right: Playback 102 [80%] [-19.00dB]
      ```

4. Well done!

A more detailed guide in Chinese: http://blog.csdn.net/yimiyangguang1314/article/details/7755815

## Save Settings

After configure soundcard with `amixer` or `alsamixer`, you can save settings with the following command:

```
$ sudo alsactl store
```

This should save alsamixer configurations to `/var/lib/alsa/asound.state` which gets loaded every startup.

You could also save the mixer settings into a custom file with:

```
$ alsactl --file ~/.config/asound.state store
```

Reloading:

```
$ alsactl --file ~/.config/asound.state restore
```

> http://askubuntu.com/questions/50067/howto-save-alsamixer-settings

> http://linux.die.net/man/1/alsactl

## Reference

- http://linux.die.net/man/1/amixer
- http://www.commandlinefu.com/commands/using/amixer
- http://blog.csdn.net/yimiyangguang1314/article/details/7755815
- http://linuxcommand.org/man_pages/alsamixer1.html
