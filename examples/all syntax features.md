# All Syntax Features

This page should be an example for all the syntax features supported 
by An Otter Wiki. You display the markdown code via 
<span class="help-button"><span class="btn btn-square btn-sm">
<i class="fas fa-ellipsis-v"></i></span> <i class="fas fa-caret-right"></i>
<span class="btn btn-square btn-sm"><i class="fab fa-markdown"></i></span>
View Source</span> or click [here](/Examples/All%20Syntax%20Features/source).

## Markdown paragraphs and text formatting

In markdown paragraphs are seperated by a blank line.

Without an empty
line, a block of
words will be rendered
as paragraph.

Individuals words or sentences can be emphasized as `monospace`, **bold**, _italic_ 
or **_bold and italic_**. Or go wild and ==mark== or ~~strike out~~ words.

An Otter Wiki stores all pages in UTF-8 in a git repository. With UTF-8 you get emojis
like 🥳 and 🎆, that all modern browsers can display.

If you don't want to user a header for seperating paragraphs,
or a header is just not enough, add a horizontal line:

---

## Links

Some example for links in the middle of a paragraph.
[A link to the otterwiki github project](https://github.com/redimp/otterwiki), 
followed by an auto linked url to http://example.com, followed by a mail address 
<mailto:mail@example.com>. Commonly used are links inside the wiki, e.g. 
one pointing [[Home]] or to another example 
[[Syntax Highlighting|Examples/Syntax Highlighting]].

## Footnotes

Here's a sentence with a footnote.[^1]

[^1]: This is the footnote.

## Quotes

> One quoted line.

Got more to quote?

> You can highlight paragraphs as a quote.
> 
> The quote can span multiple lines!
>> And it can nest more quotes.
>> 
>> **Markdown syntax will be rendered inside quotes.**

## List examples

Itemized lists look like

* this one
* with three
* items.

A numbered list

1. first item
2. second item
3. third item

And a task list

- [ ] a unchecked item
- [x] and **bold** checked item

*The task list can only be mofied by editing it, not by clicking the checkboxes while viewing it.*

## Tables

A wide table with different aligend columns:

| Very wide column without expliciit alignment | Left aligned column | Centered column | Rght aligned Column |
| -------------------------------------------- |:------------------- |:---------------:| -------------------:|
| Cell with Text                               | Cell with Text      |  Cell with Text |      Cell with Text |
| Cell with <br> two lines.                    | Cell with Text      |  Cell with Text |      Cell with Text |


A table with some formatting and an emoji.

| Alpha    | Bravo             | Charlie  |
| -------- | ----------------- | -------- |
| `D`elta  | Echo              | Foxtrott |
| Golf     | **Hotel**         | India    |
| Juliett  | Kilo              | _Lima_   |
| ~~Mike~~ | November          | Oscar    |
| Papa     | Quebec            | Romeo    |
| Sierra   | Tango             | Uniform  |
| Victor   | [Whisky](#tables) | X-Ray    |
| Yankee   | Zulu              | 🦦       |


## Code blocks

Markdown is often used for documentation, so it's not a
surprise that it should be excellent in displaying code and configurations.
For example a minimal `docker-compose.yaml` for running An Otter Wiki:

```yaml
version: '3'
services:
  otterwiki:
    image: redimp/otterwiki:2.0
    ports:
    - 8080:80
```

This and many other examples for syntax highlighting can be found in [[Examples/Syntax Highlighting|/Examples/Syntax Highlighting]].

## Lists can nest blocks

List can nest lists and other block.

1. For example
    * an unordered list
    * with two items
2. or
    1. an new level
    2. of an ordered list
    3. with three items.
3. Somtimes you might have to add

	an entire paragraph to a list. To do that indent it with 
    4 spaces and add an empty line before and after the paragraph.

5. Maybe you want to add a fenced block of code

    ```python
	class Example:
        """Finding examples is hard."""
        var = True
    ```
4. or a quote

    > You get the concept.

## Math or LaTeX

If you want to share math formulas in your wiki, [MathJax](https://www.mathjax.org/) comes
to the rescue. For example:

When `$a \ne 0$`, there are two solutions to `$ax^2 + bx + c = 0$` and they are
```math
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
```

## Special blocks

An Otter Wiki supports also some special blocks, whose syntax is derived
from various dialects.

You can create spoiler blocks:

>! ![](/static/img/otterhead-100.png)
>! Not a massive spoiler, but useful to show that you can hide images and text inside
>! a spoiler block.

---

Blocks with summary, that unfold the details on click:

>| # Do you want to know more?
>| 
>| Detail blocks could be used for handling spoilers, too.

---

In case you have to highligh important informations,
An Otter Wiki provides special blocks in different flavors.

:::info
# An info block

with some content.
:::

and in the flavor of a 

:::warning 
# Warning block (in yellow)

With _some_ `formatting` in it.
:::

or a

:::danger 
# Danger block (in red)

In case a warning not warning enough.
:::
