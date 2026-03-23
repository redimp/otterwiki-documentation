# Customization

## How to customize your wiki

An Otter Wiki was not designed with the idea that user-defined styles and code might become necessary. However, since there is a need, a way to add CSS, JS and HTML has been added.

There are currently two ways to customize the wiki:
- Using `custom` directory to be able to add custom CSS, JS and HTML 
- Using env variables for small HTML tweaks

Both methods work independently of each other.

### Using `custom` Directory

The template automatically loads several custom files which are empty by default. You can mount a directory into the container to provide the following customizations:
- `custom.css` - custom CSS styles (from version **2.3.1**)
- `custom.js` - custom JavaScript (from version **2.3.1**)
- `customHead.html` - custom HTML injected into the `<head>` section (from version **2.15.0**)
- `customBody.html` - custom HTML injected into the `<body>` section (from version **2.15.0**)

For example, with a `docker-compose.yaml` like this:  
```yaml
version: '3'
services:
  otterwiki:
    image: redimp/otterwiki:2
    restart: unless-stopped
    ports:
      - 8080:80
    volumes:
      - ./app-data:/app-data
      # a custom local directory with a custom.css, custom.js, customHead.html and customBody.html
      - ./custom:/app/otterwiki/static/custom
```

> [!NOTE]
> In case of a local deployment the browser might cache the web app, so users are recommended to clear cookies and site data if the changes to `custom.js` or `custom.css` are not visible immediately even after restarting the docker

### Using Environment Variables to Tweak HTML

The HTML body and head of every html rendered can be tweaked via `HTML_EXTRA_HEAD` and `HTML_EXTRA_BODY` environment variables.

This can be used for small tweaks that don't require big amounts of code.


## Examples

If you've made improvements that you'd like to share, don't forget, pull requests are always welcome. Or upon up an [issue](https://github.com/redimp/otterwiki/issues) post your code and a screenshot.

Check [otterwiki/docs/custom_css_example](https://github.com/redimp/otterwiki/tree/main/docs/custom_css_example) on github for ready-to-test examples.

### Serif Pages

This is an example `custom.css` that uses the serif font
`Baskervville` for the content rendered in the page.

```css
@import url('https://fonts.googleapis.com/css2?family=Baskervville');

.content > .page {
    font-family: 'Baskervville', serif;
    font-weight: 400;
}
```

![](/Customization/a/baskervville-page.png)

### Adding the "Fork me on github" ribbon

This is an example of adding the "Fork me on github" ribbon using env variables for custom HTML.

```yaml
services:
  otterwiki:
    image: redimp/otterwiki:2
    restart: unless-stopped
    ports:
        - 8080:80
    environment:
      HTML_EXTRA_HEAD: <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-fork-ribbon-css/0.2.3/gh-fork-ribbon.min.css" />
      HTML_EXTRA_BODY: <a class="github-fork-ribbon right-bottom" href="https://url.to-your.repo" data-ribbon="Fork me on GitHub" title="Fork me on GitHub">Fork me on GitHub</a>
    volumes:
      - ./app-data:/app-data
```

![](/Customization/github-ribbon.jpg)

### Custom Fancy Block Styles

If a non-standard style name is used for a Fancy Block, it will be rendered with the class `alert-[style name]`. This allows you to define your own styles in `custom.css`.

```md
::: extradanger
## Watch Out!

This Fancy Block has custom styles
:::
```

You will likely need to add styles for both light and dark modes, since the dark mode styles for the base alert will override your styles for light mode.

```css
.alert-extradanger {
    background-color: yellow;
    border: 5px dashed black;
    border-radius: 0;
}

.dark-mode .alert-extradanger {
    background-color: #666600;
    border: 5px dashed yellow;
    border-radius: 0;
}
```

![](/Customization/custom-fancyblock.png)

### Styling individual pages or categories

A data attribute set to the path is set on the `<body>` of each page (`data-page-path`) and index (`data-index-path`) to allow styling or JavaScript functionality on individual pages.

The path is turned into a slug in each section, for example _My Category/My Page_ becomes `my-category/my-page`.

These are some examples of potential style customisations:

#### Target a Specific Page

Hides the sidebar on the homepage only

```css
body[data-page-path="home"] .extra-nav {
    display: none;
}
```

#### Style all pages in a category

Applies a different colour to pages in _My Category_.

```css
body[data-page-path^="my-category/"] h1 {
    color: darkred;
    font-weight: bold;
}
```

#### Style a Category Index

> [!NOTE]
> The whole wiki page index has a `data-index-path` of `/`

Turns the index listing into a one-column list for _My Category_.

```css
/* body[data-index-path^="my-category/"]
   to apply to subcategories as well*/
body[data-index-path="my-category"] .pageindex-columns {
  columns: 1
}
```
