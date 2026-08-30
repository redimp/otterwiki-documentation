# Plugins

An Otter Wiki can be extended with plugins. It uses
[pluggy](https://pluggy.readthedocs.io/en/stable/) to expose a set of function
hooks that a plugin implements to add or change behaviour, from rewriting
markdown before it is rendered to injecting HTML into the sidebar or reacting
to page changes.

> [!NOTE]
> The plugin API is experimental and still limited. If you want to write a
> plugin and cannot find a matching hook, please open an
> [issue](https://github.com/redimp/otterwiki/issues).

## Example plugins

Same example plugins live in
[docs/plugin_examples](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples)
in the source repository. Each is a self-contained package you can copy as a
starting point, and they come with a `docker-compose.yaml` for trying them out
and a test suite.

- [noemojis](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_noemojis) removes all emojis from pages.
- [htmlinjection](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_htmlinjection) demonstrates the HTML injection and per-element rendering hooks.
- [referencingpages](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_referencingpages) shows which pages reference the current page via WikiLinks.
- [sidebarpageindex](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_sidebarpageindex) demonstrates the sidebar page index filter and sort hooks.
- [authorsignature](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_authorsignature) adds a footer with the original author and last editor.
- [redlinks](https://github.com/redimp/otterwiki/tree/main/docs/plugin_examples/plugin_redlinks) marks WikiLinks to non-existent pages in red, like MediaWiki's redlinks.


## What a plugin is

A plugin is a normal, pip-installable Python package that registers one or more
hook implementations. In practice that means three things:

- a class whose methods are decorated with `@hookimpl`,
- a call to `plugin_manager.register(...)` so the wiki finds it,
- an `otterwiki` entry point in the package metadata.

The smallest complete example, the `noemojis` plugin, strips emojis from every
page before it is rendered:

```python
from otterwiki.plugins import hookimpl, plugin_manager

class NoEmojiPlugin:
    @hookimpl
    def renderer_markdown_preprocess(self, md):
        return self.emojis.sub('', md)

# needed so the plugin_manager finds the plugin
plugin_manager.register(NoEmojiPlugin())
```

with a `pyproject.toml` that declares the entry point:

```toml
[project.entry-points.otterwiki]
noemojis = "otterwiki_noemojis"
```

A plugin that surfaces `info()` and `help()` also appears in the in-app plugin
help at [/-/help/plugins](/-/help/plugins), next to the built-in embeddings.

## Installing a plugin

A plugin must be installed into the same (virtual) environment that runs the
flask app.

### In the docker image

On container start `entrypoint.sh` installs every plugin directory it finds
under `/app-data/plugins/` and `/plugins/` with `pip install -U .`. So there
are two ways to deploy a plugin:

- drop the plugin directory into `plugins/` inside the volume that already
  holds your `app-data` (alongside `db.sqlite` and `repository`), or
- keep your plugins in a separate directory and mount it into the container at
  `/plugins`.

For example, mounting a plugins directory next to the usual app-data volume:

```yaml
services:
  otterwiki:
    image: redimp/otterwiki:2
    volumes:
      - ./app-data:/app-data
      - ./plugins:/plugins
```

> [!NOTE]
> A plugin with dependencies that are not already in the image will fail to
> install. In that case build a custom image that bundles the plugin and its
> requirements. In environments with `SELINUX=enforcing` the bind mounts need
> adjusting, see the [[FAQ|FAQ#environments-with-selinux]].

### From a source install

Activate the virtual environment that runs the app and install the plugin
directory into it:

```bash
venv/bin/pip install .
```

### Uninstalling

From a source install run `pip uninstall <plugin-name>`. For the docker image,
remove the plugin directory and recreate the container for a clean environment.

## Developing Plugins

TODO

### Available hooks

The authoritative list of hooks, with full signatures and documentation, is the
`OtterWikiPluginSpec` class in
[otterwiki/plugins.py](https://github.com/redimp/otterwiki/blob/main/otterwiki/plugins.py).
A plugin implements only the hooks it needs. Grouped by purpose:

**Setup**

- `setup(app, db, storage)` receives the Flask app, database and git storage to initialise the plugin.

**Rendering (pre/post processing)**

- `renderer_markdown_preprocess(md)` transforms the raw markdown before rendering (chained across plugins).
- `renderer_html_postprocess(html)` transforms the HTML after the page has been rendered.
- `renderer_javascript()` adds JavaScript to the rendered page.
- `page_view_htmlcontent_postprocess(html, page)` transforms a page's rendered content just before display.
- `page_render_context(page, preview)` receives the page currently being rendered, and whether it is a preview.

**Embeddings**

- `embedding_parse(embedding, options, args)` parses an embedding and returns its HTML.
- `embedding_render(embedding, args)` renders a `{{name ...}}` embedding to HTML.

**Template injection points**

- `template_html_head_inject(page)` injects HTML into the `<head>`.
- `template_html_body_inject(page)` injects HTML before the closing `</body>`.
- `template_html_sidebar_left_inject(page)` appends HTML to the left sidebar (menu and page index).
- `template_html_sidebar_right_inject(page)` appends HTML to the right sidebar (the "On this page" block).

**Per-element rendering**

- `renderer_process_link(...)` modifies each rendered markdown link.
- `renderer_process_image(...)` modifies each rendered image.
- `renderer_process_heading(...)` modifies each rendered heading.
- `renderer_process_wikilink(...)` modifies each rendered WikiLink.

**Repository and page events**

- `repository_changed(changed_files)` reacts to any repository change, including the git web server and automatic pulls (read-only).
- `page_saved(pagepath, content, author, message)` runs after a page's content changed.
- `page_deleted(pagepath, author, message)` runs after a page was deleted.
- `page_renamed(old_pagepath, new_pagepath, author, message)` runs after a page was renamed.

**Info and help**

- `info()` returns `(name, description, category)` used to group the plugin in the user help.
- `help(plugin)` returns the plugin's documentation shown under `/-/help`.
- `help_category_prelude(category)` returns introductory text for a help category.

**Static CSS**

- `static_css()` returns CSS added to every page via the layout template.

**URL routes**

- `url_request(plugin, extra, method, values)` handles requests to `/-/plugin/<name>/<extra>`.
- `url_admin_request(plugin, extra, method, values)` handles admin requests to `/-/admin/plugin/<name>/<extra>`.

**Sidebar page index**

- `sidebar_page_index_filter_entries(entries, mode)` filters the sidebar page index entries in place.
- `sidebar_page_index_sort_entries(entries, mode)` sorts the sidebar page index entries in place.
