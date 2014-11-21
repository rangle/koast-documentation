---
title: Email
layout: page
tags:
  - project
  - koast-server
  - email
  - mailMaker
categories:
  - koast-server
weight: 500
anchors:
  setting-up-configuration: Setting up Configuration
  declaring-mailmaker: Declaring MailMaker
  declaring-mailoptions: Declaring MailOptions
  sending-emails: Sending Emails
---

## Setting up Configuration ##

In configuration file of your application add the mailer configuration block as shown below.

{% highlight javascript %}
"adminEmails": "email@mail.com",
"mailer": {
  "smtp": {
    "service": "EmailService",
    "auth": {
      "user": "username",
      "pass": "password"
    }
  }
}
{% endhighlight %}

## Declaring MailMaker ##

Create the mailMaker object where you want to make use of the email madule to send the emails.

{% highlight javascript %}
var adminEmails = koast.config.getConfig('adminEmails');
var mailerMaker = koast.mailer.mailerMaker();
{% endhighlight %}

## Declaring MailOptions ##

To send email mailOptions must be configured as shown below.

{% highlight javascript %}
var mailOptions = {
  from: adminEmails, // sender address
  to: 'sumit@rangle.io', // list of receivers
  subject: 'Hello', // Subject line
  text: 'Hello world', // plaintext body
  html: '<b>Hello world </b>' // html body
};
{% endhighlight %}

## Sending Emails ##

Last to send emails just call the mailMaker's sendEmail method of mailMaker with callback function.

{% highlight javascript %}
mailerMaker.sendMail(mailOptions, function (error, info) {
  if (error) {
    console.log(error);
  } else {
    console.log('Message sent: ' + info);
  }
});
{% endhighlight %}
