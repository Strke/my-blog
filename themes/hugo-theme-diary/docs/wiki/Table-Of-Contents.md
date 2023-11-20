## Disable Table of Contents

### Globally

Add `disableToC=true` to the section of `[param]` in your `config.toml`.
Then you will not see it.

### Apply for single page

Add `disableToC: true` in the front matter of the page.
Then you will disable it in the very page.

## ~ToC auto collapse~

After 1.3.0, I've disabled this function by default.

To enable this (not recommended), please add `enableAutoCollapse=true` to the section of `[param]` in your site's `config.toml`.

**NOTE:** This function relies on jQuery, which goes with **terrible** performance.

---

For old users (version < 1.3.0), this function is enabled by default.

To disable, please add `disableAutoCollapse=true` to `[param]`.

And this can be applied to single page as [Apply for single page](#Apply-for-single-page), too.