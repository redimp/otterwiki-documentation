# Video Attachment

## Using the Video Embedding

[Video Documentation](/-/help/plugins#video)

```
{{Video
|autoplay=on
|muted=true
|controls=true
/examples/Video%20Attachment/example.mp4
}}
```

{{Video
|autoplay=on
|muted=true
|controls=true
/examples/Video%20Attachment/example.mp4
}}

## YouTube

The same embedding renders a YouTube URL (`youtube.com/watch?v=…` or
`youtu.be/…`) as an embedded player. The playback flags map onto the YouTube
iframe parameters, so `controls`, `autoplay`, `muted` and `loop` still apply.

```
{{Video
|width=100%
https://www.youtube.com/watch?v=aqz-KE-bpKQ
}}
```

{{Video
|width=100%
https://www.youtube.com/watch?v=aqz-KE-bpKQ
}}

## Via HTML

This page has a video file [example.mp4](/examples/Video%20Attachment/example.mp4) attached. You can add the video to the wiki page using a html block:

```html
<video width='100%' controls>
    <source src="/examples/Video%20Attachment/example.mp4" type="video/mp4">
</video>
```

And get it ready to view like this:

<video width='100%' controls>
    <source src="/examples/Video%20Attachment/example.mp4" type="video/mp4">
</video>
