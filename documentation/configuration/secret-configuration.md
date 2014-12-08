---
title: Secrets
layout: page
tags:
  - configuration
  - koast-server
categories:
  - koast-server
weight: 305
top: false
---

The secret is used to salt the hash of the user session.  It protects against session hijacking and has important security considerations to be followed.

#1) <b>Secrets should never be in your source code.</b>

Use the shortstop 'env' handler.  This allows us to set an environment variable for the secret and shortstop will pull it in to the config.

{% highlight javascript %}
"secrets": {
  "cookieSecret": "env: COOKIE_SECRET"
  },
  {% endhighlight %}

#2) <b>Length matters.</b>

Use a long sentence rather than a UUID.

"This is my funky secret oh my god it has ninja turtles"
