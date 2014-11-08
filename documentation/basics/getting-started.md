---
title: Basics
layout: page
tags:
  - project
  - koast-server
categories:
  - koast-server
weight: 200
top: true
anchors:
  getting-started: Getting Started
  koast-init: Koast Init
  manual-creation : Manual Creation
---

# Getting Started #

To get started with Koast, either install via npm:

`npm install koast` or `npm install koast -g`

Or, clone the repo and link it locally

```
git clone https://github.com/rangle/koast.git koast && cd $_
npm link
```

and in your application directory:

`npm link koast`

# Koast Init #

Koast includes an initializer option. Simply type `koast init` from  your project directory and it will prompt you for a few basic questions.

```
 _                   _
 | | _____   __ _ ___| |_
 | |/ / _ \ / _` / __| __|
 |   < (_) | (_| \__ \ |_
 |_|\_\___/ \__,_|___/\__| Is now initializing your app <3

? Package name: koast-app
? Description: A koast app
? Include frontend: Yes

```

This will generate a bare-bones application, that once completed starts the application - `node server/app.js` and point your browser at http://localhost:3000/api/koast

## Structure Created ##

```
├── bower_components -- only if 'include front end'
├── client           -- only if 'include front end'
├── config
│   ├── app.json     -- global configuration
│   └── dev
|     └── dev.json
├── server
|   ├── app.js       -- server application
|   └── api.js       -- example api

```


# Manual Creation #

A basic server/app.js would look like:

{% highlight javascript %}
'use strict';

var koast = require('koast');

koast.configure();
kaost.serve();

{% endhighlight %}

Fire up the application with `node server/app.js`, and you should land on a basic page.

You do not need to explcitly set any configurations or defaults, as koast will provide a few default settings that you can later override.
