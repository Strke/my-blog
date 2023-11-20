RSS Feed is located at `http://your-site/index.xml`.
To customize the contents of RSS Feed, please edit `/index.rss.xml`

To add it to the sidebar:
Edit `config.toml`
```toml
[[menu.main]]
url = "/index.xml"
name = "RSS Feed"
weight = 4
```

**Notice: RSS Feed only generate the very lastest 10 posts.**
(You can edit `/index.rss.xml` by yourself to change this limit.)