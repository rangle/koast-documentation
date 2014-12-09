---
title: Authentication
layout: page
tags:
  - project
  - koast-server
  - authentication
categories:
  - koast-server
weight: 400
top: true

anchors:
  authentication-options : Authentication Options
  example-projects: Example Projects
  password---tokens : Password - Tokens
  oauth-setup : OAuth Setup
  oauth---tokens : OAuth Tokens
  oauth---custom-callback : OAuth Custom Callback
---

# Authentication Options #

Koast supports local/password, and oauth/social authentication. You are also able to configure your site to use cookies or tokens for session maintenance.
For authentication to work, you will need to include the appropiate user schemas.

The configuration options that are required:

{% highlight javascript %}

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

For authentication to work, please include the userProviderAccounts and users
in your schema file. ~~note: this will be refactored/improved soon~~

{% highlight javascript %}

exports.schemas = [{
  // Only used for social login. We'll remove this later.
  name: 'userProviderAccounts',
  properties: {
    username: {
      type: String
    }, // Assigned by us
    provider: {
      type: String,
      enum: ['google', 'twitter', 'facebook'],
      required: true
    },
    idWithProvider: {
      type: String,
      required: true
    }, // Assigned by the provider
    emails: [{
      type: String
    }],
    displayName: {
      type: String
    },
    oauthToken: {
      type: String
    },
    oauthSecret: {
      type: String
    },
    tokenExpirationDate: {
      type: Date
    }
  }
}, {
  // Represents a user of a system. Needed for password auth
  name: 'users',
  properties: {
    username: String,
    password: String,
    displayName: String
  }
}
}];

{% endhighlight %}

### Example Projects ###

A few examples have been created that demonstrate the various configuration options and are available at the [koast-examples](https://github.com/rangle/koast-examples) repository.

The examples that have been setup for authentication are

- [koast-todo](https://github.com/rangle/koast-examples/tree/master/koast-todo) - local auth with cookies
- [koast-todo-social-cookie](https://github.com/rangle/koast-examples/tree/master/koast-todo-social-cookie) - social authentication with cookies, api and client run on the same server
- [koast-todo-social-sessionless](https://github.com/rangle/koast-examples/tree/master/koast-todo-social-sessionless) - social authentication with tokens and sessionless, client and api can run on different servers

### Password - Tokens ###

When using Token maintenance, after logging in your response object will contain
a user record, and a meta-data containing the token. You will need to store this token, and ensure that it is sent along with all subsequent requests to the server.
If you are using koast-angular, the `koast.user.login()` will handle this for you.

### OAuth Setup ###

To setup OAuth, you will need to populate the OAuth of your configuration with the provider, and the needed keys. For example, to setup authentication with FaceBook:

{% highlight javascript %}
// app.json
{
"oauth": {
    "facebook": {
      "clientID": "YOUR_CLIENT_ID",
      "clientSecret": "YOUR_CLIENT_SECRET",
      "callbackURL": "http://localhost:8080/auth/facebook/callback"
    }
  }
}
{% endhighlight %}

### OAuth - Tokens ###

If you are using token authentication with oauth, you will also need to specify a clientReturnUrl under the authentication settings.

{% highlight javascript %}
{
  "authentication": {
    "strategy": "social",
    "maintenance": "token",
    "clientReturnUrl": "http://localhost:8081"
  }
}
{% endhighlight %}

This allows you to setup your client application, and your api server on seperate servers.
If you are using koast-angular: `koast.user.initiateOauthAuthentication('facebook', '/tasks');`, it will take care of redirecting, managing your tokens.

If you are not using koast-angular, the api server will redirect to the clientReturnUrl with a queryString 'redirectToken'. This is a short-lived token that you will need to refresh soon.

~~todo: example implementation~~

### OAuth - Custom Callback ###

If you need to perform steps after the OAuth authentication process Koast makes it possible to provide a custom authentication module.
This module will be able to handle the <b>success</b> callback from an OAuth provider.  This allows for custom steps to be performed and additional security constraints to be enforced.  

{% highlight javascript %}
{
  "authentication": {
    "strategy": "social",
    "maintenance": "token",
    "clientReturnUrl": "http://localhost:8081",
    "authenticationModule": "path:../server/authentication"
    "whiteListAccounts": "nick@rangle.io, dean@rangle.io, yuri@rangle.io",
  }
}
{% endhighlight %}

To handle an OAuth callback simply provide a path to your module.  The 'path:' syntax shown above in the <b>authenticationModule</b> element is a shortstop handler.  It builds a path relative to your app.js file.  Once your custom callback is called you can apply additional security contraints.  In the example above we have a config element of whitelisted accounts.  Imagine we want to restrict access to just the top brass at Rangle.  Anyone not on this list can't login.  The OAuth provider does not know this and will permit anyone with valid Google/Facebook/etc credentials to log in.  Please note that when your authentication module is invoked the user is already logged in.  At this point you can check apply your additional security contraints and if the user should not be given access you can simply log them back out like so:

{% highlight javascript %}
req.logout();
{% endhighlight %}

You also have the option to redirect them.  Possibly to explain why they could not log in.

{% highlight javascript %}
res.redirect('/AuthenticationRestricted');
{% endhighlight %}

This notion of a whitelist is just one example.  There are any number of possible implementations.  We can apply domain restriction using this approach.  For example only allowing @rangle.io Gmail accounts.  We could perform some sort of logging of the user login process, just recording who logs in and not adding any additional security contraints.  We could take this time to pull a user settings file from the DB and keep it in memory for subsequent requests.  The custom authentication module is an 'inversion of control' putting the application developer in the drivers seat.
