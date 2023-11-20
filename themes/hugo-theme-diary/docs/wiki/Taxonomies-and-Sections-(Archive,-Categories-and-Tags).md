## Taxonomies

This theme originally supports two kind of taxonomy page: tag index and category index.

Please edit the `config.toml` in the root directory.

Modify the section `[taxonomies]` as follows.
```
[taxonomies]
   tag = "tags"
   category = "categories"
```

**You should add them to sidebar manully. Please see [Customize sidebar](https://github.com/AmazingRise/hugo-theme-diary/wiki/Customization#customize-sidebar)**

## Archive Page

Hugo implement this function as `Section`.
To visit archive page, you can directly navigate to `http://your-site/posts`
Add these to your `config.toml` in order to see `Archive` on the sidebar:
```toml
[[menu.main]]
url = "/posts"
name = "Archive"
weight = 1
```