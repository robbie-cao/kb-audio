# GStreamer

## What is GStreamer

GStreamer is a framework for creating streaming media applications. It is a pipeline-based multimedia framework that links together a wide variety of media processing systems to complete complex workflows. For instance, GStreamer can be used to build a system that reads files in one format, processes them, and exports them in another. The formats and processes can be changed in a plug and play fashion.

![](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/manual/html/images/gstreamer-overview.png)

> https://gstreamer.freedesktop.org/data/doc/gstreamer/head/manual/html/chapter-gstreamer.html

![](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/GStreamer_example_pipeline.svg/960px-GStreamer_example_pipeline.svg.png)

> https://en.wikipedia.org/wiki/GStreamer

## Gstreamer vs FFmpeg

GStreamer - Multimedia Framework. Plain GStreamer can't do anything without plugins, GStreamer requires various plugins like, source(file,http,ftp,..), demux(mp4,3gp,flv,TS,..) & decoder(H.264,H.263,MP3,AAC).

FFMPEG - Container parser, Software Implementation of Audio/Video decoders, SW Scaling.

GStreamer is a broader library, and can actually use FFmpeg plugins. For simple and typical transcoding jobs, maybe FFmpeg is easier to use. But for more complex operations, GStreamer is super powerful.

> https://www.quora.com/Multimedia-Which-is-better-FFmpeg-or-GStreamer-Why

![](http://processors.wiki.ti.com/images/7/7b/GstBuildDependancies.png)

> http://processors.wiki.ti.com/index.php/ARM_Multimedia_Users_Guide

## Reference

- https://en.wikipedia.org/wiki/GStreamer
- https://gstreamer.freedesktop.org
- https://github.com/GStreamer/gstreamer/tree/master/docs
