---
title: App Configuration
layout: page
tags:
  - configuration
  - koast-server
categories:
  - koast-server
weight: 301
top: false
---

Application settings are stored in the "app" portion of your app.json file.

{% highlight javascript %}
// config/app.json
{
  "app": {
    "indexHtml": "path:../client/index.html", // optional
    "lessPaths": [
      ["/css","client/less"] // optional
    ],
    "portNumber": 8080,
    "routes": [{
      "route": "/api/v1",
      "type": "module",
      "module": "server/lib/tasks"
    }, {
      "route": "/app",
      "type": "static",
      "path": "path:../client/app"
    }, {
      "route": "/css",
      "type": "static",
      "path": "path:../client/css"
    }, {
      "route": "/bower_components",
      "type": "static",
      "path": "path:../client/bower_components"
    }]

  }
}
{% endhighlight %}

This section specifies the express settings that the app should run with such as port number, routes, less files, etc.

- app.portNumber
  - port number to run the application on
  - if none is specified, will default to the PORT environment variable
- app.indexHtml
  - optional index file to serve to a web browser
  - if none is specified, koast will provide a basic default file
- app.lessPaths
  - an collection paths to mount the less-middleware on
  - each element should be an array, with the first element being the url you wish to mount the route as
  - the second element is the path to the less files
  - *note* the mount path, ie: "/css" will still need to be added to the routes collection as a static route

Please see the [Routing documentation]({{ '/documentation/routing/setting-up-routes.html' | prepend: site.baseurl }}) for more information on the various types of routes and configuration options.
