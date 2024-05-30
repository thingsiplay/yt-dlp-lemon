# ytdl

Simple wrapper to yt-dlp with only a subset of options.

- Author: [Tuncay D.](https://github.com/thingsiplay)
- Source: [Github](https://github.com/thingsiplay/ytdl)
- License: [MIT](LICENSE)

## What is this?

**ytdl** is a small script for Linux as an alternative interface to
[yt-dlp](https://github.com/yt-dlp/yt-dlp) (which itself is a fork from
_youtube-dl_, to download YouTube videos). My goal is to make some of its
functionality a bit more accessible for the daily usage. This includes
predefined settings and narrowing it down to options I care most about.

## Installation

This is a Bash script. It does not need an installation. Just download and give
it the executable bit.

### Requirements

Linux and Bash only. Besides a few standard GNU utilities, only
[yt-dlp](https://github.com/yt-dlp/yt-dlp) is a hard requirement. Which itself
obviously requires [Python](https://www.python.org/),
[ffmpeg](https://ffmpeg.org) and when reading or writing audio metadata, also
[mutagen](https://github.com/quodlibet/mutagen).

#### Arch (includes Manjaro, EndeavourOS)

```bash
pacman -S yt-dlp ffmpeg python-mutagen
```

### Download

There is no release or build version of the script. Just get the source, make
the script executable and put the file in a folder that is found in the $PATH
variable.

```bash
git clone https://github.com/thingsiplay/ytdl
cd ytdl
chmod +x ytdl
```

## Usage

Just run the script with an URL to a YouTube video to start downloading.

```bash
ytdl [options] [url...]
```

Show most common list of options with `-h` (short help) or all available
options and additional notes with `-H` (long help):

```bash
ytdl -h
ytdl -H
```

By default, every downloaded video id will be added to an ignore list
automatically. This will prevent re downloading the same video again.

```bash
ytdl https://youtu.be/jNQXAC9IVRw
```

### Skip or force download

Skip downloading the video and just output information about the online video.
Even if it's already in the local ignore list:

```bash
ytdl -x https://youtu.be/jNQXAC9IVRw
```

You can also combine it with option `-q` to compact the output:

```bash
ytdl -xq https://youtu.be/jNQXAC9IVRw
```

The standard location of the ignore file is at
"~/.local/share/ytdl/ignore.lst". This file can be set to an alternative path
with option `-i FILE` . For the following examples the option `-I` (uppercase
"i") is used consistently, so no ignore file is used and video download is
enforced for demonstration purposes:

```bash
ytdl -I https://youtu.be/jNQXAC9IVRw
```

### Set limits

Usually the best available quality of a video is downloaded. Set a max allowed
size with option `-m HEIGHT` , to get a smaller sized video format. We will
make use of this option many times, so the examples are quickly done. `HEIGHT`
is the pixel dimension for the video height:

```bash
ytdl -I -m 160 https://youtu.be/jNQXAC9IVRw
```

If you have limited internet bandwidth and don't want to slow the entire
connection down until download process is finished, then just limit the
download speed with `-l MB`:

```bash
ytdl -I -l 1.2 https://youtu.be/jNQXAC9IVRw
```

When downloading and saving a video, then it is automatically named including
its title and other information. This can get rather very long or contain weird
characters. Option `-r LEN` will restrict those characters to ASCII set and
truncate its length to whatever is specified:

```bash
ytdl -I -r 8 https://youtu.be/jNQXAC9IVRw
```

### Playlist or audio mode

Playlist mode `-p` downloads all videos from a playlist. In addition a sub
directory is created and the filenames include an index. For testing, let's
combine this with option `-m` to download small versions of the video. This
results in 4 videos and 22 MB (at time of writing this document):

```bash
ytdl -I -m 240 -p https://youtube.com/playlist?list=PL6-7feKhsltT9ZTElq6V2Z2EZN71wyxrX
```

Extract and keep an audio file format only with option `-a` . Useful for music
or podcasts, where the visual part is not important or present at all.

```bash
ytdl -I -a https://youtu.be/jNQXAC9IVRw
```

### Sponsor and chapter marks

Split video by it's chapter marks and create separate files in a sub directory
with option `-c`:

```bash
ytdl -I -c https://youtu.be/jNQXAC9IVRw
```

Did you know `yt-dlp` supports [SponsorBlock](https://sponsor.ajay.app/)
directly? A free community based service for YouTube videos to recognize and
add chapter marks for sponsored segments. If you have a video player (such as
VLC player) then you can view and select those marks directly. Option `-s` will
add chapter marks into the video, and recognizes sponsors automatically.

```bash
ytdl -I -m 160 -s https://youtu.be/9Jxxbh4HbtE
```

Sponsored segments can be automatically removed and blocked in the final video
file too. But `-b` does not add any marks and handles sponsors only. In the
following example 2 segments are recognized and removed, cutting total video
length from 9:42 to 8:51 minutes.

```bash
ytdl -I -m 160 -b https://youtu.be/9Jxxbh4HbtE
```

Uppercase `-S` and `-B` does the same respectively, but recognizes more type of
segments such as filler and interaction reminder. In following example `-B`
will recognize 7 segments from SponsorBlock database and reduce total video
length to 8:18 minutes.

```bash
ytdl -I -m 160 -B https://youtu.be/9Jxxbh4HbtE
```

Combine `-S` and `-b` in example to block sponsors from final output file and
add rest of the chapter marks in addition to get the best of both worlds:

```bash
ytdl -I -m 160 -Sb https://youtu.be/9Jxxbh4HbtE
```

### Metadata and additional files

There are also bunch of additional meta, thumbnail images and extra files to
each video. Option `-d` will download these video descriptions and other
metadata as separate files. Note the video file in this example has the
".webm" extension.

```bash
ytdl -I -m 160 -d https://youtu.be/jNQXAC9IVRw
```

Metadata, thumbnail image, subtitles and more can be embed into the actual
video file with `-e` . Media players can display or interact with those
elements. Not all container formats support all of these extras, so ".mkv" will
be used automatically:

```bash
ytdl -I -m 160 -e https://youtu.be/jNQXAC9IVRw
```

### Output format

Specify container format for output file with option `-f` . This will do a
simple conversion, meaning content is not re-encoded, just re-packaged. This
can fail, if the container does not support the audio or video codecs or
any specific feature. On the other hand this is lossless conversion and fast:

```bash
ytdl -I -f mp4 https://youtu.be/jNQXAC9IVRw
```

A re-encoding of the video content can be forced with uppercase `-F` if needed.
This has much more freedom to choose output format than `-f` and will take much
longer time to finish:

```bash
ytdl -I -F mp3 https://youtu.be/jNQXAC9IVRw
```

Have a great rest of your day.
