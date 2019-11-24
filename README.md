# docker-videotools

## Description

Image with various tools to edit and convert video files

Tools included:

* [ffmpeg](http://ffmpeg.org/) - A complete, cross-platform solution to record, convert and stream audio and video.
* qt-faststart - Enable streaming and pseudo-streaming of Quicktime and MP4 files by moving metadata and offset information to the front of the file
* [vid.stab](http://public.hronopik.de/vid.stab/) - ffmpeg video stabilization plugin

## How to use this image

```
docker run -it --rm -v $HOME/Videos:/videos:rw fr3nd/videotools bash
```

## How to use vid.stub
See [this article](https://www.imakewebsites.ca/posts/2018/02/17/stabilizing-gopro-video-with-ffmpeg-and-vid.stab/) or these commands:

1. Prepocess
```
ffmpeg -t 5 -i GOPR7182.MP4 -vf vidstabdetect=stepsize=32:shakiness=10:accuracy=10:result=transform_vectors.trf -f null - 
```

2. Preview
```
ffmpeg -t 5 -i GOPR7182.MP4 -y -vf vidstabtransform=input=transform_vectors.trf:zoom=0:smoothing=10,unsharp=5:5:0.8:3:3:0.4,scale=480:-1 -vcodec libx264 -tune film -an stabilized.mp4
```

3. Final run
```
ffmpeg -i GOPR7182.MP4 -vf vidstabtransform=input=transform_vectors.trf:zoom=0:smoothing=10,unsharp=5:5:0.8:3:3:0.4 -vcodec libx264 -tune film -acodec copy -preset slow stabilized.mp4
```
