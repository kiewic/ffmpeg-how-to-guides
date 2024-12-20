# FFmpeg command line tool how-to guides

## Converting video from MOV to MP4

Use following command to:

* Convert from MOV to MP4
* Remove audio (`-an`)
* Scale down the video (`-vf "scale=iw/3:ih/3"` or `-vf "scale=iw/4:ih/4"`)
* Select the start position (`ss`)
* Set duration (`t`)

```
./ffmpeg -i input.mov -an -vf "scale=iw/3:ih/3" -ss 00:00:05 -t 00:00:18 a_third_the_frame_size.mp4
```

## How to speed up a video

To double the speed:

```
./ffmpeg -i input.mov -filter:v "setpts=0.5*PTS" -filter:a "atempo=2.0" output.mov
```

* Double the speed of video (`-filter:v "setpts=0.5*PTS"`)
* Double the speed of audio (`-filter:a "atempo=2.0"`)
* Remove audio (`-an`)

Source: [FFmpeg wiki](https://trac.ffmpeg.org/wiki/How%20to%20speed%20up%20/%20slow%20down%20a%20video)

## Increase audio volume on an MP4

It seems to only work with mp4 (or at least it is not working with mov).

```
./ffmpeg -i input.mp4 -c:v copy -af "volume=8" output.mp4
```

## Concatenate multiple videos

First create a `list.txt` file like this:

```
file '/Users/tcook/Pictures/GoPro/GH010356.MP4'
file '/Users/tcook/Pictures/GoPro/GH020356.MP4'
file '/Users/tcook/Pictures/GoPro/GH030356.MP4'
```

Then:

```
./ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4
```

The `-safe 0` is not required if the paths are relative.

The `-c copy` sets all codec operations to copy, i.e., video, audio, subtitles, data and attachments, if any.

## Add one line of text

```
./ffmpeg -i video.mp4 -vf "drawtext=text='Happy New Year':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2:" -codec:a copy output.mp4
```

## Add multiple lines of text

```
./ffmpeg -i video.mp4 -vf "[in]drawtext=text='Have A':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2-55, drawtext=text='Happy Christmat':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2-20, drawtext=text='And A':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2+20, drawtext=text='Happy New Year':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2+55[out]" -codec:a copy output.mp4
```

## Merge video from one file and audio from another

```
ffmpeg -i video.mp4 -i audio.mp4 -map 0:v:0 -map 1:a:0 -c:v copy -c:a copy output.mp4
ffmpeg -i "/Users/gilberto/Pictures/Sabrina Carpenter Outside Lands Festival 2024 - 1080.mp4" -i "/Users/gilberto/Pictures/Sabrina Carpenter Outside Lands Festival 2024 - Audio.mp4" -map 0:v:0 -map 1:a:0 -c:v copy -c:a copy "/Users/gilberto/Pictures/Sabrina Carpenter Outside Lands Festival 2024 - 1080 With Audio.mp4"
```

1. `-i video.mp4`: Specifies the input file containing the video
2. `-i audio.mp4`: Specifies the input file containing the audio
3. `-map 0:v:0`: Maps the video stream from the first input (video.mp4)
4. `-map 1:a:0`: Maps the audio stream from the second input (audio.mp4)
5. `-c:v copy`: Copies the video stream without re-encoding
6. `-c:a copy`: Copies the audio stream without re-encoding
7. `output.mp4`: The output file with the video from video.mp4 and the audio from audio.mp4
