## {{ }} in markdown cannot be rendered

That's because this themes used Vue.js.

Solution:
Please use `v-pre` to surround the content with {{ }}.

Example:
If you want to show `{{ Something }}` in your contents, please write in markdown:
```html
<div v-pre>
{{ Something }}
</div>
```
Then you will see `{{ Something }}` on the webpage.