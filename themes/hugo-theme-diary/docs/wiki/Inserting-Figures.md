## Centering

To post a centering image, you can:

```markdown
<center>

![](search_result.jpg)

</center>
```

**NOTE:** The blank lines matter, don't remove them.

## Inserting Images in HTML code

To insert images in HTML, such as `<img src=... />`, there is a limit. Hugo's Markdown renderer doesn't allow raw HTML code by default.

To enable raw HTML support in Hugo, please configure your `config.toml`:

```toml
[markup.goldmark.renderer]
    unsafe = true
```

And don't raise issues on this, please. Because it's implemented by Hugo. You can give a feedback to Hugo's Markdown renderer, `GoldMark`.

## Resizing or more ...

Inspired by [this issue](https://github.com/AmazingRise/hugo-theme-diary/issues/55), I ported a shortcode from [cupper-hugo-theme](https://github.com/zwbetz-gh/cupper-hugo-theme).
Thanks for [zwbetz-gh](https://github.com/zwbetz-gh)'s code.

Additionally, I add a property `align` for aligning.

```
{{< insertFigure
img="resource_name" 
caption="This is an example figure. For details, view [here](https://github.com/AmazingRise/hugo-theme-diary/issues/55)." 
command="Resize" 
align="Right"
options="700x" >}}
```

NOTE:
- `img`: it can be a resource name; or a file's name, which is located in current directory. [How to define a source?](https://gohugo.io/content-management/page-resources/#resources-metadata-example)
- `caption`: will be shown beneath the figure (markdown supported).
- `command`: it can be `Fit`, `Fill`, `Resize`, or `Original`.
- `align`: possible value: `Left`, `Right`, `Center`, `Justify` (it is not case-sensitive).
- `options`: parameters, please see [image processing methods](https://gohugo.io/content-management/image-processing/#image-processing-methods).

In fact, this shortcode is equivalent to [Image processing methods](https://gohugo.io/content-management/image-processing/#image-processing-methods).

