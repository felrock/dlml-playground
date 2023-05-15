# Creating subtitles to Never gonna give you up

## Get song

use yt-dlp, download the song

```
./yt-dlp.sh <youtube-link>
```

## Convert to .wav

Run ffmpeg to convert the song into .wav, since whisper.cpp takes sound input as 16k .wav

```
ffmpeg -i <video-file> -ar 16000 <wav-name>
```

## Create chunks

Seperate into 10s chunks, using ffmpeg

```
ffmpeg -i <wav-name> -threads 3 \
       -acodec copy -f segment -segment_time 00:20 \
       -reset_timestamps 1 \
       chunk_%02d.wav
```

## Feed into whisper.cpp

First install and build whisper. Run main with some chunk, this can be slow depending on
what machine you are on.

e.g
```
./main -m models/ggml-base.en.bin -f chunk_03.wav -osrt
```
