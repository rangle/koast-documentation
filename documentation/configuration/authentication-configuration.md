---
title: Authentication Configuration
layout: page
tags:
  - configuration
  - koast-server
categories:
  - koast-server
weight: 303
top: false
---

Please see the [Authentication]({{ '/documentation/authentication/authentication.html' | prepend: site.baseurl }}) for full details. Valid configuration options are below:

{% highlight javascript %}
// config/app.json
{
"app" : { /* other config settings */},
"authentication":
  {
    /* valid options: password, social, disabled */
    "strategy": "password" ,
    /* valid options: cookie, token */
    "maintenance": "cookie"
  },
  "secrets":
  {
    /* replace secret with your own */
    "cookieSecret": "catsaretheinternet",   // only needed for cookies
    "authTokenSecret": "catsaretheinternet" // only needed for token
  },
  "oauth":
  {
    /* only needed for oauth - detailed later */
  }
}
{% endhighlight %}
