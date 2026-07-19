# Editing

https://prose.io/#edmomot/edmomot.github.io

# Image format
Resize in windows to "large (1920x1080)", upload via git. Leave original filename.

# Image link format

`![Title](/images/directory/image.jpg)`

# Gifs

Use ffmpeg exe in same directory as file to convert named `input.mp4`

`.\ffmpeg.exe -ss 8 -t 9 -i input.mp4 -vf "fps=30,scale=600:-1:flags=lanczos,crop=600:800:0:0,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 output.gif`

Produces `output.gif` in same directory

Start time of gif in seconds from the source video" `ss 8`
Length of gif in seconds: `-t 9`
Frames per second: `fps=30`
Scaled output size in pixels, shortest axis: `scale=600`
Crop video to 600 pixels wide, 800 pixels tall, with the top left corner of the crop aligned with the top left corner of the source video: `crop=600:800:0:0`

# Open links in new tab

`[Text description](https://link.com){:target="_blank"}`

# Jekyll Now

Website built with [jekyll-now](https://github.com/barryclark/jekyll-now)
