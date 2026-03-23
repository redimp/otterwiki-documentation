# AttachmentList

[AttachmentList Documentation](/-/help/plugins#attachmentlist)

```
{{AttachmentList}}
```

{{AttachmentList}}


## Filter and options

Display just files ending in `*.txt` show file `details` but not Author and Date, hide the icons.

```
{{AttachmentList
|caption=Attachments
|filter=\*txt
|format=details
|icons=false
}}
```

{{AttachmentList
|caption=Attachments
|filter=\*txt
|format=details
|icons=false
}}

---

Display just the filenames and icons/thumbnails of files beginning with in `asianclawless*` 

```
{{AttachmentList
|filter=asianclawless\*
|format=minimal
|icons=true
}}
```

{{AttachmentList
|filter=asianclawless\*
|format=minimal
|icons=true
}}
