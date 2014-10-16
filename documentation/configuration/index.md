---
title: Configuration
layout: page
tags:
  - configuration
  - koast-server
  - shortstop
categories:
  - koast-server
weight: 300
top: true
---

Koast uses [shortstop](https://github.com/krakenjs/shortstop) and [confit](https://github.com/krakenjs/confit)  to help manage the configuration, and environment specific configuration.

Your application should have a configuration structure like:

```
├── config
│   └── app.json
│   └── development.json
│   └── staging.json
│   └── production.json
```

the app.json contains common configuration that will be used across all environment as the base default settings. Each environment can have custom configuration in the appropriate .json file.

Koast will pick up the environment from the NODE_ENV setting, if none is found it will default to development.

A full app.json might look something like:
{% highlight json %}
{
  "app": {
    "indexHtml": "path:../client/index.html",
    "portNumber": 3001,
    "routes": [{
      "route": "/api/v1",
      "type": "module",
      "module": "server/lib/api"
    }]
  },
  "authentication": {
    "strategy": "password",
    "maintenance": "token"
  },
  "cors": {

    "origin": "*",
    "headers": "X-Requested-With, Content-Type, Authorization",
    "methods": "GET,POST,PUT,DELETE,OPTIONS, Authorization",
    "credentials": true

  },
  "databases": [{
    "host": "127.0.0.1",
    "port": "27017",
    "db": "erg",
    "schemas": "./server/schemas.js",
    "handle": "_"
  }],
  "secrets": {
    "authTokenSecret": "catsaretheinternet"
  }
}
{% endhighlight %}

When Koast loads up the configuration for other environments, it will merge the settings from the other .json files. You only need to include the settings that you want to override, and not the entire file.

For example, if you want to launch koast on port 8080 on development, your development.json would only need to contain

```json
{
  "app":
  {
    "portNumber": "8080"
  }
}
```
All of the other settings from app.json will remain the same.

Since Koast is using [shortstop](https://github.com/krakenjs/shortstop) to load the configuration files, we are able to specify 'protocols' as values for keys. What this allows you to do is split your configuration into multipul files, pull in values from enviroment settings, etc.

For example, if you wanted to keep your database configuration in a seperate file, you could configure it like:

```json
{
  "databases": "import:./path/to/database.json"
}
```

This will load up your database.json and set it as the value for databases. If you want to pull in a value from an environment variable:

```json
{
  "databases": [{
    "host": "env:HOST",
    "port": "27017",
    "db": "erg",
    "schemas": "./server/schemas.js",
    "handle": "_"
  }]
}
```

and it will pull in the value of the HOST environment variable, and set that as the value.

Please see the documentation for [shortstop](https://github.com/krakenjs/shortstop) and [shortstop-handlers](https://github.com/krakenjs/shortstop-handlers) for more information.
