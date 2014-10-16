---
title: Setting up routes
layout: page
tags:
  - project
  - koast-server
  - routing
categories:
  - koast-server
---

In the **app** section of your configuration, there is a section called routes. Koast will mount two types of routes for you - static, and module.

{% highlight javascript %}
{
  "app": {
    "routes": [{
      "route": "/api/v1", // base url of the route
      "type": "module",   // type of the route - module or static
      "module": "server/lib/api" // if a module, location to find the module
    }]
  }
}
{% endhighlight %}

## static ##

~~todo: complete static documentation~~

## module ##

The module definition in Koast 0.4.x has changed from 0.2.x. Previously, Koast expected a module to export the following

{% highlight javascript %}
'use strict';

export.defaults = {};
export.defaults.authorication = function(req, res) { return true; };
export.routes = [{
    method: ['get', 'put'],
    route: 'tasks/:_id',
    handler: mapper.auto({
        model: 'tasks'
    })
}]
{% endhighlight %}

Koast 0.4.x now expects the format:

{% highlight javascript %}
var router = require('express').Router();
// setup express 4 router

exports.koastModule =
{
  defaults: {
    authorization: function(req,res) {
      return true;
    }
  },
  router: router

}
{% endhighlight %}

Koast will still load 0.2.x style of modules, but will display a warning message that this will be deprecated.

## Migrating older Koast applications ##

Being able to define your routes as an array is still supported however via the koast-router module. To migrate to a 0.4.x application:

{% highlight javascript %}
var koastRouter = require('koast').koastRouter;

var defaults =
{
  authorization: function(req,res)
  {
    return true;
  }
};
var routes = [{
    method: ['get'],
    route: 'tasks/:_id',
    handler: mapper.get({
        model: 'tasks'
    })
}];

exports.koastModule =
{
  defaults: defaults,
  router: koastRouter(routes,defaults)  
}
{% endhighlight %}
