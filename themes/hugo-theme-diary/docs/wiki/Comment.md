## Enable or disable comment

Now, this theme support these comment service:
- LiveRe
- Gitalk
- Disqus
- Twikoo
- Waline
- Utterances

### Customizing existing comment service

### Enabling
#### Gitalk
Edit your `config.toml` in the hugo website's root directory.

Add the following line to the section `[params]`
```toml
enableGitalk = true
```
Then add following lines behind:
```toml
[params.gitalk]
  owner = "user"
  repo = "repo name"
  client_id = "your client id"
  client_secret = "your client secret"
```
(Modify to suit your condition.)

Notice: Gitalk will not shown in offline preview server.(Launched by `hugo server`)
#### Disqus

To add a disqus comment service, please add a line to `config.toml` in the root directory, under `[params]`:

```toml
[params]
disqusShortname = "Your disqus short name."
```

Thanks for [nicholaskajoh](https://github.com/AmazingRise/hugo-theme-diary/issues/51) and [jenlky](https://github.com/AmazingRise/hugo-theme-diary/issues/42)'s feedback.

#### LiveRe!

Edit your `config.toml` in the hugo website's root directory.

Add the following line to the section `[params]`
```toml
livereId = "xxxx"
```

"xxxx" stands for the value of `data-uid` in your LiveRe HTML code.

#### Twikoo

Edit your `config.toml` in the hugo website's root directory.

Add the following line to the section `[params]`
```toml
enableTwikoo = true
twikooEnvId = "twikoo-YourEnvId"
```

Replace YourEnvId with your actual environment id.

If your Tencent Cloud Environment is deployed in Guangzhou server, don't forget to add this:

```toml
twikooRegion = "ap-guangzhou"
```

#### Waline

Edit your `config.toml` in the hugo website's root directory.

Add the following line to the section `[params]`
```toml
walineServer = "https://yourappaddress.vercel.app"
```

`walineServer` is corresponding to `serverURL`.

#### Utterances

Edit your `config.toml` in the hugo website's root directory.

Add the following line to the section `[params]`
```toml
enableUtterances = true
```

And add a new section named `params.utterances`, like this:

```toml
[params.utterances]
repo="your repo"
term="[ENTER TERM HERE]"
label="your label"
theme="github-light"
```

#### Other

To add other services, you can edit `./layouts/partials/comment.html` and `./layouts/partials/head.html` by yourself.

And you can also open an issue, reminding me to add a feature.

---

### Disabling
#### Disable the comment in one post
If you wanna to disable the comment in a specific post, please add a line in the metadata area of the post:
```
comment : false
```
A more detailed description of the posts' meta data is [here](https://github.com/AmazingRise/hugo-theme-diary/wiki/Post's-meta-data).

#### Disable it globally
Please remove the settings item referred in "Add comment".

The comment area will not be shown.