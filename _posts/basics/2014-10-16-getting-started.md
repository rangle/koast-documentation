---
title: Getting Started
layout: page
tags:
  - project
  - koast-server
categories:
  - koast-server
---


To get started with Koast, either install via npm:

`npm install koast` or `npm install koast -g`

Or, clone the repo and link it locally

```
git clone https://github.com/rangle/koast.git koast && cd $_
npm link
```

and in your application directory:

`npm link koast`

A basic server/app.js would look like:

{% highlight javascript %}
'use strict';

var koast = require('koast');

koast.configure();
kaost.serve();

{% endhighlight %}

Fire up the application with `node server/app.js`, and you should land on a basic page.
