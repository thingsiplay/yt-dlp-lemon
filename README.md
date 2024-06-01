# yt-dlp-lemon

Simple wrapper to yt-dlp with only a subset of options.

- Author: [Tuncay D.](https://github.com/thingsiplay)
- Source: [Github](https://github.com/thingsiplay/yt-dlp-lemon)
- License: [MIT](LICENSE)

## Important: Project name changed

Name of project changed from `ytdl` to `yt-dlp-lemon` . If you used it before
name change, then you will most likely have an ignore file automatically
saved and updated in `~/.local/share/ytdl/` . Rename the directory
to `~/.local/share/yt-dlp-lemon/` .

---

## What is this?

**yt-dlp-lemon** is a small script for Linux as an alternative interface to
[yt-dlp](https://github.com/yt-dlp/yt-dlp) (which itself is a fork from
_youtube-dl_, to download YouTube videos). My goal is to make some of its
functionality a bit more accessible for the daily usage. This includes
predefined settings and narrowing it down to options I care most about.

### Why lemon in name?

As in "easy peasy lemon squeezy"; a phrase for easy to use. I needed something
that is a little bit descriptive and distinguishes itself from existing
projects.

## Installation

This is a Bash script. It does not need an installation. Just download and give
it the executable bit. I personally recommend creating an alias to shorten the
name to `ytdl` in example.

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
git clone https://github.com/thingsiplay/yt-dlp-lemon
cd yt-dlp-lemon
chmod +x yt-dlp-lemon
```

## Usage

Just run the script with an URL to a YouTube video to start downloading.

```bash
yt-dlp-lemon [options] [url...]
```

Show most common list of options with `-h` (short help) or all available
options and additional notes with `-H` (long help):

```bash
yt-dlp-lemon -h
yt-dlp-lemon -H
```

By default, every downloaded video id will be added to an ignore list
automatically. This will prevent re downloading the same video again.

```bash
yt-dlp-lemon https://youtu.be/jNQXAC9IVRw
```

All downloaded file names will be sanitized a little by default. Characters
such ":" and "?" are stripped out, but many other characters such as "!" or "ä"
are still allowed.

### Skip or force download

Skip downloading the video and just output information about the online video.
Even if it's already in the local ignore list:

```bash
yt-dlp-lemon -x https://youtu.be/jNQXAC9IVRw
```

You can also combine it with option `-q` to compact the output:

```bash
yt-dlp-lemon -xq https://youtu.be/jNQXAC9IVRw
```

The standard location of the ignore file is at
"~/.local/share/yt-dlp-lemon/ignore.lst". This file can be set to an alternative path
with option `-i FILE` . For the following examples the option `-I` (uppercase
"i") is used consistently, so no ignore file is used and video download is
enforced for demonstration purposes:

```bash
yt-dlp-lemon -I https://youtu.be/jNQXAC9IVRw
```

### Set limits

Usually the best available quality of a video is downloaded. Set a max allowed
size with option `-m HEIGHT` , to get a smaller sized video format. We will
make use of this option many times, so the examples are quickly done. `HEIGHT`
is the pixel dimension for the video height:

```bash
yt-dlp-lemon -m 160 -I https://youtu.be/jNQXAC9IVRw
```

If you have limited internet bandwidth and don't want to slow the entire
connection down until download process is finished, then just limit the
download speed with `-l MB`:

```bash
yt-dlp-lemon -l 1.2 -I https://youtu.be/jNQXAC9IVRw
```

When downloading and saving a video, then it is automatically named including
its title and other information. This can get rather very long or contain weird
characters. Option `-r LEN` will restrict those characters to ASCII set and
truncate its length to whatever is specified:

```bash
yt-dlp-lemon -r 8 -I https://youtu.be/jNQXAC9IVRw
```

### Playlist or audio mode

Playlist mode `-p` downloads all videos from a playlist. In addition a sub
directory is created and the filenames include an index. For testing, let's
combine this with option `-m` to download small versions of the video. This
results in 4 videos and 22 MB (at time of writing this document):

```bash
yt-dlp-lemon -p -m 240 -I https://youtube.com/playlist?list=PL6-7feKhsltT9ZTElq6V2Z2EZN71wyxrX
```

For some playlists, the top most entry will always be the first newest added
one. If you periodically check the playlist for new entries, this would mess
with the numbering system. For such cases you can download and number the files
in reverse order with `-R`. There is also an option uppercase `-P` to avoid
numbering of downloaded filenames entirely.

```bash
yt-dlp-lemon -R -m 240 -I https://youtube.com/playlist?list=PL6-7feKhsltT9ZTElq6V2Z2EZN71wyxrX
```

Extract and keep an audio file format only with option `-a` . Useful for music
or podcasts, where the visual part is not important or present at all.

```bash
yt-dlp-lemon -a -I https://youtu.be/jNQXAC9IVRw
```

### Sponsor and chapter marks

Split video by it's chapter marks and create separate files in a sub directory
with option `-c`:

```bash
yt-dlp-lemon -c -I https://youtu.be/jNQXAC9IVRw
```

Did you know `yt-dlp` supports [SponsorBlock](https://sponsor.ajay.app/)
directly? A free community based service for YouTube videos to recognize and
add chapter marks for sponsored segments. If you have a video player (such as
VLC player) then you can view and select those marks directly. Option `-s` will
add chapter marks into the video, and recognizes sponsors automatically.

```bash
yt-dlp-lemon -s -m 160 -I https://youtu.be/9Jxxbh4HbtE
```

Sponsored segments can be automatically removed and blocked in the final video
file too. But `-b` does not add any marks and handles sponsors only. In the
following example 2 segments are recognized and removed, cutting total video
length from 9:42 to 8:51 minutes.

```bash
yt-dlp-lemon -b -m 160 -I https://youtu.be/9Jxxbh4HbtE
```

Uppercase `-S` and `-B` does the same respectively, but recognizes more type of
segments such as filler and interaction reminder. In following example `-B`
will recognize 7 segments from SponsorBlock database and reduce total video
length to 8:18 minutes.

```bash
yt-dlp-lemon -B -m 160 -I https://youtu.be/9Jxxbh4HbtE
```

Combine `-S` and `-b` in example to block sponsors from final output file and
add rest of the chapter marks in addition to get the best of both worlds:

```bash
yt-dlp-lemon -Sb -m 160 -I https://youtu.be/9Jxxbh4HbtE
```

### Metadata and extra files

Additional video description and info files are downloaded alongside the video
with option `-d`:

```bash
yt-dlp-lemon -d -m 160 -I https://youtu.be/jNQXAC9IVRw
```

Metadata and chapter marks can be embedded into the video file itself with
option `-e`:

(_NOTE: Chapter marks seem not to be added with `-e` or `-E` . Reportedly this
works with ffmpeg 6.0 or older, but not with 6.1. I compiled and tested with
ffmpeg 7.0.1, but it seems to be not working either. ffmpeg is the software
used by yt-dlp itself._)

```bash
yt-dlp-lemon -e -m 160 -I https://youtu.be/jNQXAC9IVRw
```

To write even more of these extra files, such as thumbnail image or a desktop
link file (shortcut to original YouTube link), use uppercase `-D` . An
uppercase `-E` will similarly include much more data to embed, such as
thumbnail image and subtitles. Not all container formats are suited for all
type of embedded data. Therefore the output file format will be "mkv" to
support everything.

```bash
yt-dlp-lemon -DE -m 160 -I https://youtu.be/jNQXAC9IVRw
```

### Output format

Specify container format for output file with option `-f` . This will do a
simple conversion, meaning content is not re-encoded, just re-packaged. This
can fail, if the container does not support the audio or video codecs or
any specific feature. On the other hand this is lossless conversion and fast:

```bash
yt-dlp-lemon -f mp4 -I https://youtu.be/jNQXAC9IVRw
```

A re-encoding of the video content can be forced with uppercase `-F` if needed.
This has much more freedom to choose output format than `-f` and will take much
longer time to finish. In the example here we combine `-F` with `-a` to
actually get an audio file in mp3 format, otherwise its still a video.

```bash
yt-dlp-lemon -a -F mp3 -I https://youtu.be/jNQXAC9IVRw
```

Have a great rest of your day.
