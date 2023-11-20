There are some differences between the new version and the original version.

## New features

Features in this ported version:

- Add support for Gitalk, LiveRe, and Valine comment service.
- Tag & category page appending more easily.
- Customizable color scheme. (Some bug in original version, fixed.)
- Firefox-friendly. (CSS issue in original version, fixed.)
- `featured_image` url bug is fixed.
- A simple `Table of Contents`.

## Functions not implemented yet

These functions are neither implemented in the ori. nor the newer:

- *A themed 404 page.

(The item with * means a new feature introducing in the near future. For details, click [here](https://github.com/AmazingRise/hugo-theme-diary/projects/).)

## Difference in this new version

- The title and subtitle is the same as site title & subtitle. (In the ori., the title & subtitle is [a independent setting item](https://github.com/SumiMakito/hexo-theme-Journal#post-items-and-pages). But to unify the experience between PC and mobile, and simplify the settings, here I merged the independent setting item into global settings.)
- The setting `intro` in front matter is replaced by `description`. (Following the hugo style.) And the `truncate_len` is not implemented yet.
- The setting `no_comments : true` is turned into `comment : false`. (Following the hugo style.)