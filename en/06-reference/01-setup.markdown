
## Setup ##

### System ###

This tab provides access to branding, IP security, and updating your Cerb5 software license.

#### Branding ####

Two settings allow you to personalize your helpdesk branding.

* **Helpdesk Title:** The title that will display on browser windows, tabs, and bookmarks when using your helpdesk.
* **Logo URL:** A public URL to your organization's logo.

#### IP Security ####

There are two special helpdesk pages that do not require a login but should still be protected from abuse.

* `/cron` is _pinged_ from an automated script to execute time-based functionality.  These scheduled tasks are defined in the [Setup -> Scheduler](##scheduler) tab.  This page can be accessed by an automated process that doesn't need to log in, but it shouldn't be freely accessible to anyone because it could be abused to create an unnecessary strain on your webserver and database; for example, by invoking hundreds of concurrent instances of the nightly maintenance task.
* `/update` is invoked automatically when the application detects a new version has been installed.  It is responsible for logging out users to ensure consistency, clearing caches, rebuilding templates, running database patches, and migrating data.  Since this process is meant to occur when everyone is logged out, an authentication method other than a helpdesk account is required.

Cerb5 uses IP-based security to protect both of these pages.  You can authorize specific IP addresses for I.T. department machines, your corporate network subnet, intranet IPs, or webserver loopback IPs.

You can enter partial IPs to approve an entire subnet, such as:

* 127.0.
* 192.168.1.

If you authorize a _loopback_ IP address (e.g. 127.0.0.1), you should use a command-line tool like `wget` or `curl` on the webserver to make requests for these pages.  This is usually the preferred option, since `/cron` will usually be requested from a cronjob on the webserver, and `/update` is used in conjunction with upgrades to the helpdesk software (which are best handled with the `git` or `svn` command-line tools).

#### Updating your product license ####

Without a product license your helpdesk will run in _Evaluation Mode_, which provides full functionality for a **single seat**.  Purchasing a license will increase the number of seats, and it may also include a subscription to future software updates.

After ordering, you'll receive a product license by email.  It will look something like this:

~~~
Licensed to: Example, Inc.
Order e-mail: a.smart.shopper@example.com

### BEGIN CERB5 LICENSE
Key: ABC1-DEF2-FED3-CBA4-ABC5
Created: 2010-05-12
Updated: 2011-02-01
Upgrades: 2012-02-01
Seats: 15
### END
~~~

To update your product license:

* Enter your company name _exactly_ as it appears on your order.  This is text after _"Licensed to:"_ from the email with your license.
* Enter your email address _exactly_ as it appears on your order.  This is the text after _"Order e-mail:"_ from the email with your license.
* Copy the text between `### BEGIN` and `### END` and paste it into the third text box.

![Updating a Cerb5 license.](images/06-update_license.png)

### Features/Plugins ###

### Storage ###

#### Disk ####

#### Database ####

#### Amazon S3 ####

### Mail Setup ###

(Explain how mail is routed into the helpdesk)

#### Incoming mail ####

(Configuring POP3)

#### Outgoing mail ####

(Reply from; signatures)

### Mail Filtering ###

### Mail Routing ###

### Mail Queue ###

### Attachments ###

### Scheduler ###

### Groups ###

### Workers ###

### Permissions ###

### Custom Fields ###
