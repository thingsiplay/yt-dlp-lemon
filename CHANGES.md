# Changelog

Keep track of changes for [ytdl](https://github.com/thingsiplay/ytdl) .

## Next

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
