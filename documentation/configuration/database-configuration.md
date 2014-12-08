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

Currently Koast only supports one schema file, however this will change in the future to specify multiple files to be used.

### Database Handles ###

If you only require a single database, you can omit the handle value. If you need to use multiple databases, each database should provide a unique handle property.
These handles are used in the dbUtils when requesting a connection.

### DB Utils ###

DB Utils is an abstraction over basic MongoDB & Mongoose functions.  It is exposed as Koast.db and has the following public interface:

<b>createSingleConnection:</b> which takes these arguments

* @param  {String}   handle       A handle for the connection.
* @param  {Object}   dbConfig     A database configuration object.
* @param  {Array}    schemas      An array of schema definitions.

Returns a promise that resolves to a ready Mongoose connection.  The schemas get added to the database.  This allows a koast application to delegate all of it's persistence needs to koast-db-utils.

<b>createConfiguredConnections:</b> which takes these arguments

* @param  {Array}    subset       A list of keys specifying which databases to load.                                                                                  (Optional, defaults to all.)
* @param  {Function} callback     A callback function called after every connection. (Optional.)
* @param  {object}   config       koast config module

This method relies on Koast config and uses it to get an array of databases and their associated schemas.  For each database in the config it calls createSingleConnection.
A promise is returned which resolves to an array of ready Mongoose connections.  The schemas get added to the associated databases.  

<b>getConnectionHandles:</b>

Returns an array of strings representing connection handles.

<b>getConnectionNow:</b>

* @param  {String} handle         A connection handle.

Returns a connection for a given handle. The connection is returned immediately, whether or not it is ready.

<b>closeAllConnectionsNow:</b>

Closes all connections immediately (without waiting for them to be finalized).

Returns a promise that resolves when all connections have been closed.

<b>getConnectionPromise:</b>

@param  {String} handle         A connection handle.

Returns a promise for a connection identified by a handle. The promise will resolve to a connection when the connection is ready.

<b>reset:</b>  This wipes out all state clearing away all connections and connectionPromises.

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
