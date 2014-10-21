---
title: Database Configuration
layout: page
tags:
  - configuration
  - koast-server
  - database
categories:
  - koast-server
weight: 302
top: false
---

The Koast database configuration expects an array of database definitions. Currently, Koast only supports MongoDB.

{% highlight javascript %}
// sample config
{
  databases: [{
      "host": "127.0.0.1",  
      "port": "27017",
      "db": "databaseName",
      "user": "ngcourse",     // optional
      "pass": "ngcourse",     // optional
      "schemas": "./server/schemas.js",
      "handle": "_"
  }]
}

{% endhighlight %}

The username and password are optional if you have not setup your mongo server to require them.
The schemas should point to a file that contains a valid mongoose schema definition.

Currently Koast only supports one schema file, however this will change in the future to specify multipul files to be used.

### Database Handles ###

If you only require a single database, you can omit the handle value. If you need to use multipul databases, each database should provide a unique handle property.
These handles are used in the dbUtils when requesting a connection.

~~to-do: document db-utils~~

### Database Settings from Environment ###

If you do not want to keep your production database credentials checked into your source control, you are able to use shortstop protocols to instruct Koast to load the values from an environment setting.
For example:

{% highlight javascript %}
// sample config
{
  databases: [{
      "host": "env:PRODUCTION_HOST",  
      "port": "27017",
      "db": "databaseName",
      "user": "env:PRODUCTION_USER",     // optional
      "pass": "env:PRODUCTION_PASSWORD",     // optional
      "schemas": "./server/schemas.js",
      "handle": "_"
  }]
}

{% endhighlight %}
