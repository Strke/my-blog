Here is an example of a post.
```
title: "When You Have Too Much to Do"
date: 2018-03-18T02:01:58+05:30
description: "You have a to-do list that scrolls on for days. You are managing multiple projects, getting lots of email and messages on different messaging systems, managing finances and personal health habits and so much more."
tags: [Primer, todo]
featured_image: "/images/notebook.jpg"
categories: Todo
comment : false
```

`date` is the post's publish date.(Optional)

`description` will be shown in the index page as an abstract of the post.

`featured_image` If featured_image presents, the image specified will show up in the post item, also, the feature image will show up in the detailed post's or page's page.

`tags` is an array of tags.

`categories` is a single element variable. So please don't add a racket outside it.

*(If you make it an array like `["Life"]`, it is also okay. But it will result in some small issues.)*

`comment` if it is `false`, you will get a hint of "Comment diabled." in the bottom of the post.
