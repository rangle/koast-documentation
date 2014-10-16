---
title: Mongo Mapper
layout: page
tags:
  - project
  - koast-server
  - mongo-mapper
categories:
  - koast-server
---

Mongo mapper is a helper that creates handlers to a mongo collection, with the ability mount authorization, annotators, query decorators and filters easily into the requests.

## Basic Usage ##

Provided that a schema 'tasks' is defined, to quickly setup end points to work with the tasks collection, your routes collection would look like:

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
  method: 'get',
  route: 'tasks',
  handler: mapper.get({
    model: 'tasks'
  })
}, {
  method: ['get', 'put', 'post', 'delete'],
  route: 'tasks/:_id',
  handler: mapper.auto({
    model: 'tasks'
  })
}];

exports.koastModule =
{
  defaults: defaults,
  router: koastRouter(routes,defaults)  
}

{% endhighlight %}

This will create routes that will support get - all tasks, get a task by id, adding a new task, and updating an existing task.

~~todo document mapper options, use envelope, how annotators/etc only get called if envelopes are being used, give example responses~~

## Annotators ##

Annotators are executed after a result is returned from the database, but before the result is sent to the client. These can be used to add additional meta-data to the results, or transform information into a more appropiate format.

When using annotators, the **useEnvelope** option needs to be set to true, this will wrap the response into an object containing the properties 'meta' and 'data', where data is the actual result.

{% highlight javascript %}

function isOwner(data, req) {
  if (req.user && req.user.data) {
    return data.owner == req.user.data.username;
  } else {
    return false;
  }
}

function annotator(req, item, res) {
  item.data.fullTitle = item.data.owner + ":" + item.data.description;
  item.meta.can = {
    edit: isOwner(item.data, req)
  };
  return item;
}

var routes = [{
  method: 'get',
  route: 'tasks/',
  handler: mapper.get({
    model: 'tasks',
    useEnvelope: false
  })
},
{
  method: 'get',
  route: 'tasks-plus/',
  handler: mapper.get({
    model: 'tasks',
    useEnvelope: true,
    annotator: annotator
  })
}

exports.koastModule =
{
  defaults: defaults,
  router: koastRouter(routes,defaults)  
}
{% endhighlight %}

When getting a list of tasks, it will now contain some meta-data, and have a new property called fullTitle.

`curl http://localhost:3001/api/v1/tasks` returns:

{% highlight json %}
[{
  "_id": "5417116638a36a97126b3da2",
  "owner": "alice",
  "description": "local task 1",
  "__v": 0
}]
{% endhighlight %}

`curl http://localhost:3001/api/v1/tasks-plus` returns:
{% highlight json %}
[{
  "meta": {
    "can": {
      "edit": false
    }
  },
  "data": {
    "_id": "5417116638a36a97126b3da2",
    "owner": "alice",
    "description": "local task 1",
    "__v": 0,
    "fullTitle": "alice:local task 1"
  }
}]
{% endhighlight %}
