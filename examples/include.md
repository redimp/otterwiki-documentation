# Include

[Include Documentation](/-/help/plugins#include)

The `{{include}}` embedding pulls another page, or a single section of it, into
the current page. The examples below all include
[[the sample page|Examples/Include/Sample Page]].

## The whole page

```
{{include|src=/Examples/Include/Sample Page}}
```

{{include|src=/Examples/Include/Sample Page}}

## A single section

`section` is the heading anchor slug (lower case, spaces as dashes), matched
independently of the heading level. `children=true` (the default) pulls in the
subsections too, so this also brings along *Sea Otters*.

```
{{include|src=/Examples/Include/Sample Page|section=otters}}
```

{{include|src=/Examples/Include/Sample Page|section=otters}}

## A section, without its subsections or heading

`children=false` stops at the next subheading and `heading=false` drops the
section's own heading line, so this includes only the paragraph directly under
*Otters*.

```
{{include|src=/Examples/Include/Sample Page|section=otters|children=false|heading=false}}
```

{{include|src=/Examples/Include/Sample Page|section=otters|children=false|heading=false}}

## Things to watch out for

- **Use absolute paths inside included content.** Relative links and
  attachments (`![](image.png)`) in the source resolve against the *including*
  page, not the source page, so they break. Write `/Source/Page/image.png` in
  anything meant to be included. That is why `src` above is written absolute as
  well, though relative `src` (resolved against this page) also works.
- Headings from included content do not show up in this page's table of
  contents.
- Do not combine a `{{DataTable}}` and an `{{include}}` on the same page; the
  page's own DataTable can be left uninitialised. Keep them on separate pages.
- Includes nest at most 8 deep, and cycles (a page including itself, directly
  or through a chain) are detected and reported rather than expanded.

See the [Include documentation](/-/help/plugins#include) for the full option
list.
