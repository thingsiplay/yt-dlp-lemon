# Changelog

Keep track of changes for
[yt-dlp-lemon](https://github.com/thingsiplay/yt-dlp-lemon) .

## Next

- **breaking**! Project name was changed from "ytdl" to "yt-dlp-lemon", so it
  is less confusing when researching and to distinguish itself from other
  projects. This also makes the path to the default ignore file different. So you
  should rename the directory

  - from: "~/.local/share/ytdl"
  - to: "~/.local/share/yt-dlp-lemon"

  And adapt any aliases or scripts that refer to the old executable name.

## 0.4 - Mai 31, 2024

- new: option -R to download playlists in reverse order, with last entry as
  index number 1
- changed: sanitize a little bit the produced file names by default, still not
  as strict as -r option

## 0.3 - Mai 31, 2024

- initial Changelog

- changed/new: options -s and -b now act for sponsors only, while -S and -B
  will add marks or remove all type of SponsorBlock segments
- changed/new: option -e will now embed only metadata and chapters, while -E
  will embed an extended set of data to the video file (such as thumbnail)
- changed/new: similarly option -d will only download video description and the
  info-json file, while -D will download and write extended set of additional
  files (such as thumbnail)
