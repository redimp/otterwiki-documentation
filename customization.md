# Customization

## Theme Customization

An Otter Wiki was not designed with the idea that user-defined topics might become necessary. However, since there is a need, a way to modify the theme has been added.

If you've made improvements that you'd like to share, don't forget, pull requests are always welcome. Or upon up an [issue](https://github.com/redimp/otterwiki/issues) post your code and a screenshot.

Check [otterwiki/docs/custom_css_example](https://github.com/redimp/otterwiki/tree/main/docs/custom_css_example) on github for ready-to-test examples.

### `custom directory`

With version **2.3.1** the template loads a `custom.css` and a `custom.js` which are empty by default. You can mount the directory into the container, for example with a `docker-compose.yaml` like this:

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
      # a custom local directory with a custom.css and a custom.js
      - ./custom:/app/otterwiki/static/custom
```

### Examples

#### Serif Pages

This is an example custom.css that uses the serif font
`Baskervville` for the content rendered in the page.

```css
@import url('https://fonts.googleapis.com/css2?family=Baskervville');

.content > .page {
    font-family: 'Baskervville', serif;
    font-weight: 400;
}
```

![](/Customization/a/baskervville-page.png)

> [!NOTE]
> In case of a local deployment the browser might cache the web app, so users are recommended to clear cookies and site data if the changes to `custom.js` or `custom.css` are not visible immediately even after restarting the docker

## Tweaking the HTML head and body

The HTML body and head of every html rendered can be tweaked via `HTML_EXTRA_HEAD` and `HTML_EXTRA_BODY`. 

This can be used for small tweaks, e.g. adding the "Fork me on github" ribbon:

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
