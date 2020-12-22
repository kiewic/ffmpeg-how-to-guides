# FFmpeg command line tool how-tos

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

## Add one line of text

```
./ffmpeg -i video.mp4 -vf "drawtext=text='Happy New Year':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2:" -codec:a copy output.mp4
```

## Add multiple lines of text

```
./ffmpeg -i video.mp4 -vf "[in]drawtext=text='Have A':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2-55, drawtext=text='Happy Christmat':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2-20, drawtext=text='And A':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2+20, drawtext=text='Happy New Year':fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:fontcolor=white:fontsize=30:x=(w-text_w)/2:y=(h-text_h)/2+55[out]" -codec:a copy output.mp4
```
