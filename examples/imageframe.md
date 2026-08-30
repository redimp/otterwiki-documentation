# ImageFrame

[ImageFrame Documentation](/-/help/plugins#imageframe)

```
{{ImageFrame
[![](/Examples/ImageFrame/eagle-owl.png)](/Examples/ImageFrame/eagle-owl.png)
}}
```

{{ImageFrame
[![](/Examples/ImageFrame/eagle-owl.png)](/Examples/ImageFrame/eagle-owl.png)
}}

Lorem ipsum dolor sit amet, consectetur adipiscing elit. In in aliquet nibh. In hac habitasse platea dictumst. Duis et urna vel ligula tempor pharetra. Morbi at malesuada augue. Integer sit amet malesuada mi. Vivamus eu pretium purus. Etiam nec ornare diam. Nunc eget turpis molestie, maximus magna vitae, aliquet justo. Proin consectetur libero a sem tincidunt rhoncus. Donec non leo lacus. Nunc eget enim sit amet metus vestibulum pulvinar eget sed sem. Aenean odio enim, aliquam vitae nunc vel, hendrerit interdum purus. Suspendisse diam ligula, interdum at tellus a, tristique laoreet quam.



## Some options

```
{{ImageFrame
|caption=Eagle-owl
|position=left
[![](/Examples/ImageFrame/eagle-owl.png)](/Examples/ImageFrame/eagle-owl.png)
}}
```

{{ImageFrame
|caption=Eagle-owl
|position=left
[![](/Examples/ImageFrame/eagle-owl.png)](/Examples/ImageFrame/eagle-owl.png)
}}

Vivamus maximus sapien quis bibendum bibendum. Integer commodo urna dui, dapibus pellentesque arcu mattis at. Fusce pharetra tortor risus, vitae maximus arcu suscipit et. Mauris aliquam nisi eu mi ornare elementum. Aliquam vitae aliquam leo, eget molestie ante. Vivamus convallis nibh hendrerit maximus condimentum. Nullam enim nunc, iaculis vitae consequat nec, elementum consectetur turpis. Curabitur dapibus vestibulum tristique. Maecenas a nisl at ante malesuada vestibulum. Vivamus interdum justo tellus, non aliquam ante viverra eu.

## Embedding an image with `src=`

Instead of writing the image link inside the frame, `|src=` names the image
directly. A bare filename is an attachment on the current page (this page has
`eagle-owl.png` attached), and `|alt=` sets the alt text.

```
{{ImageFrame
|caption=Attachment on this page
|width=50%
|src=eagle-owl.png
|alt=An eagle-owl
}}
```

{{ImageFrame
|caption=Attachment on this page
|width=50%
|src=eagle-owl.png
|alt=An eagle-owl
}}

An absolute `/Page/file` path embeds an attachment from another page, here the
otter photo attached to [[AttachmentList|Examples/AttachmentList]].

```
{{ImageFrame
|caption=Attachment from another page
|width=50%
|src=/Examples/AttachmentList/asianclawlessotter.jpg
}}
```

{{ImageFrame
|caption=Attachment from another page
|width=50%
|src=/Examples/AttachmentList/asianclawlessotter.jpg
}}

`src` also accepts a full `https://` URL to embed an external image.

See the [ImageFrame documentation](/-/help/plugins#imageframe) for the full
option list.
