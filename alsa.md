# ALSA

## PCM Interface

### HW device

The hw device description uses the hw plugin. The three arguments (in order: CARD,DEV,SUBDEV) specify card number or identifier, device number and subdevice number (-1 means any).

Example:

```
hw
hw:0
hw:0,0
hw:supersonic,1
hw:soundwave,1,2
hw:DEV=1,CARD=soundwave,SUBDEV=2
```

### Plug->HW device

The plughw device description uses the plug plugin and hw plugin as slave. The arguments are same as for hw device.

Example:

```
plughw
plughw:0
plughw:0,0
plughw:supersonic,1
plughw:soundwave,1,2
plughw:DEV=1,CARD=soundwave,SUBDEV=2
```

### Plug device

The plug device uses the plug plugin. The one SLAVE argument specifies the slave plugin.

Example:

```
plug:mypcmdef
plug:hw
plug:'hw:0,0'
plug:SLAVE=hw
```

> http://www.alsa-project.org/alsa-doc/alsa-lib/pcm.html

## Reference

- http://www.alsa-project.org/alsa-doc/alsa-lib/pcm.html
