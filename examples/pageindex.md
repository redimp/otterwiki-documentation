# PageIndex

[PageIndex Documentation](/-/help/plugins#pageindex)

The `{{PageIndex}}` embedding lists the pages below the current page. This
example page has three subpages (*Giant Otter*, *River Otter*, *Sea Otter*),
so the instances below all list those.

## Default, letter-grouped

```
{{PageIndex}}
```

{{PageIndex}}

## As a plain list, with page headings

`style=list` renders an unordered list instead of the letter-grouped layout,
and `toc=true` nests each page's headings underneath it.

```
{{PageIndex|style=list|toc=true}}
```

{{PageIndex|style=list|toc=true}}

## Filtered by a glob pattern

`src` filters the listed pages by title. Here only the pages ending in
*Otter* that start with *S* are kept.

```
{{PageIndex|src=S* Otter}}
```

{{PageIndex|src=S* Otter}}

See the [PageIndex documentation](/-/help/plugins#pageindex) for `toggle` and
the remaining options.
