## Handling email replies ##

Cerb5 will determine the appropriate `From:` address to use on outgoing email based on the group or bucket.  These are collectively referred to as _Receipts_ addresses_.

For example:

* The Billing group may send mail as `billing@example.com`
* The Billing group's _Receipts_ bucket may send mail as `receipts@example.com`
* The Sales group may send mail as `sales@example.com`

To ensure that you receive customer replies, it is very important that your mail servers route these reply-to addresses to a mailbox that is checked by Cerb5.

You can add new reply-to addresses from Setup:

1. Click _setup_ in the top right.
1. Click the _Mail_ menu.
1. Select the _Reply-To Addresses_ menu item.

![Navigating to the helpdesk addresses setup page.](images/checklist/menu_setup_mail_replyto.png)

Click the _Add_ button to add a new reply-to address, and enter the following information:

* **Send replies as email:** This specifies an email address to use as the sender in `From:`.
* **Send replies as name:** This allows you to provide a personal name that will display next to the sender address when your recipient reads their email.  For example, `billing@example.com` may use the personal name _"Widget Factory - Billing Dept."_.  This gives the recipient an idea of who you are; especially if they don't recognize your email address.
* **Default signature template:** This defines the default worker signature that will be used when mail is sent from this email address.  You can use placeholders like the current worker's name or title, which will automatically fill in the appropriate values depending on who is sending the email.  You can also use conditional logic and modifiers in this template.  Refer to the section about Snippets for more information.
* **Make default:** If you check this box the current address will become the default reply-to address.  That means it will be used in situations where there is no group or bucket; for example, sending out reminders for forgotten passwords.

![Configuring a new reply-to address.](images/checklist/setup_mail_replyto_add.png)

You can repeat this process for any additional reply-to addresses you want to add.

There is nothing wrong with using a single reply-to address for all outgoing mail -- in fact, that may simplify your mail server configuration considerably, and you can still personalize the sender name for each group or bucket.  If you have multiple products, brands, or initiatives, you may want to configure a reply-to address for each of them so your audience can instantly recognize who you are and why you're contacting them.  Otherwise, a busy or inattentive recipient may unintentionally delete your message as spam because they don't recognize the sender.

## Creating accounts for your co-workers ##

The people who log in to Cerb5 to accomplish work and represent your organization to your audience are called _workers_.  These workers might work in customer service, billing, sales, I.T., or marketing; or they may be executives, developers, or system administrators.

If this is a brand new helpdesk, the only worker that exists at the moment is yourself.  This is the account that was created for you during the installation process.

1. Click _setup_ in the top right.
1. Click the _Workers & Groups_ menu.
1. Select the _Workers_ menu item.

First, let's update your own account information.  Click the link on the first or last name of _Super User_ in the _Workers_ list.

![Configuring a worker account.](images/checklist/adding_workers.png)

* In **Contact Information** you can update a worker's name, title, and email address.  Keep in mind that if you change a worker's primary email address it will become their new helpdesk login.

* **Authentication** is used to update a worker's password, and you can also promote them to _Administrator_, which will give them the permission to do absolutely everything in the helpdesk; they are like the `root` account on Unix-based systems, or the `Administrator` account in Windows.  Only workers who are administrators can enter _Setup_.  Grant administrator access judiciously.

* Lastly, the **Memberships** box defines the worker's _membership_ to each of your groups.  You can also designate the worker as a _manager_ of a group.  Managers can perform group-level administration: adding other workers as new group _members_, creating _buckets_ for organizing the group's work, etc.

When you've finished with your changes, click the _Save Changes_ button.

Start collaborating by inviting your co-workers with the _Add Worker_ button.  In the next step we'll organize your workers into groups.

![A list of helpdesk workers.](images/checklist/workers_list.png)

## Creating groups ##

Workers are organized into _groups_. Groups are a flexible concept that can be based on anything: brand, product, department, timezone, language, etc. By default you will have three groups based on common departments: Dispatch, Support, and Sales. The defaults were chosen because they're common departments across most industries, not because they're all-encompassing. You are free to modify these groups to suit your needs.

If you stick with the defaults, this is the proposed workflow:

* **Dispatch** catches all mail that isn't explicitly routed somewhere. Some companies prefer to have a human dispatcher assign work -- to verify support eligibility and route issues based on worker skills -- and this group is an easy way to achieve that. Because Cerb5 doesn't require incoming mail to always match a specific group, the Dispatch group is also the best way to spot incoming email addresses (like `billing@example.com`) that you may want to route directly to a particular group.
* **Support** collects issues related to support: product support, customer service, FAQs, billing, etc.
* **Sales** collects issues related to sales: leads, new orders, refunds, resellers, etc.

The workers inside groups can either be _managers_ or _members_. Groups are designed to be autonomous; a group manager has the power to make most configuration changes related to the group without requiring help from an administrator. Managers can add new members to the group, manage buckets, and configure workflow using the group's Virtual Attendant.

The common practice is to use groups for building a roster of fairly interchangeable workers.  Work can be given to the group from the outside with the confidence that any member knows what to do with it. Groups share an organization system based on _buckets_, but workers outside the group aren't expected to know how other groups organize their work (and it shouldn't matter to them). Instead, new work is given to groups through their _inbox_, and the group's own filters will decide how work is routed or assigned from there.

Let's add a couple new groups.  At this point you're also welcome to change the default groups.

* Click _setup_ in the top right.
* Click the _Workers & Groups_ menu.
* Click the _Groups_ menu item.
* Click the name of a group on the left, or click _add a new group_ to create one.

![Configuring a group.](images/checklist/groups_add.png)

* **Name** is what the group will be referred to as throughout the interface.
* **Members** is the list of workers that you would like to designate as members or managers.

From this point, the managers of your new group can take over.  They should refer to the [Quick Start Guide for Group Managers](#quick-start-guide-for-group-managers).

## Friendly URLs ##

![This URL isn't very pretty.](images/checklist/friendly_urls.png)

You may notice that your URLs look a bit ugly with the omnipresent `/index.php/` in the path.  Cerb5 can use _URL rewriting_ to make URLs shorter and more user-friendly, but this requires webserver support. 

![This looks much nicer with friendly URLs enabled!](images/checklist/friendly_urls_done.png)

### Enabling friendly URLs with Apache ###

If you're using the Apache web server you can enable _"friendly URLs"_ with the following commands:

	$ cd /path/to/cerb5
	$ cp .htaccess-dist .htaccess
	
For this to work you will need `mod_rewrite` to be enabled in your Apache configuration.  This is usually the case, unless you have just compiling or installed it.

If `mod_rewrite` isn't enabled, you can enable it with the following command on many Linux-based servers:

	$ sudo a2enmod rewrite
	$ /etc/init.d/apache2 reload
	
### Enabling friendly URLs with nginx ###

[nginx](http://wiki.nginx.org/) is another popular lightweight webserver for serving PHP content.  It doesn't support the Apache `.htaccess` file format, but you can add the following directives to your server configuration to get the same effect:

	location / {
	    if (!-e $request_filename) { 
	        rewrite ^/(.+)$ /index.php/$1 last;
	    }
	}
	location ~ ^(.+.php)(.*)$ {
	    fastcgi_split_path_info ^(.+.php)(.*)$;
	    fastcgi_pass 127.0.0.1:9000;
	    fastcgi_index  index.php;
	    fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;
	    fastcgi_param  PATH_INFO        $fastcgi_path_info;
	    include fastcgi_params;
	    fastcgi_param  QUERY_STRING     $query_string;
	    fastcgi_param  REQUEST_METHOD   $request_method;
	    fastcgi_param  CONTENT_TYPE     $content_type;
	    fastcgi_param  CONTENT_LENGTH   $content_length;
	    fastcgi_intercept_errors        on;
	    fastcgi_ignore_client_abort     off;
	    fastcgi_connect_timeout 60;
	    fastcgi_send_timeout 180;
	    fastcgi_read_timeout 180;
	    fastcgi_buffer_size 128k;
	    fastcgi_buffers 4 256k;
	    fastcgi_busy_buffers_size 256k;
	    fastcgi_temp_file_write_size 256k;
	}
	
This assumes that Cerb5 is installed in the root of the webserver.  Otherwise, just prepend the path in your configuration.  You also will want to create a blank `.htaccess` file so Cerb5 will automatically generate friendly URLs for you.

## Security ##

Traditionally, when you access a URL like `http://www.example.com/pages/help.html` from your browser there is corresponding file on the webserver with the name `help.html` in the `pages` directory.  This same process is used for HTML, images, Javascript, CSS, and other files available for download.

Cerb5 uses a different approach for serving content, which is known to web application developers as the **Model-View-Controller (MVC)** [^wikipedia-mvc] architectural pattern.  As a consequence of this, all the public interaction with your helpdesk occurs with two main files in the root directory of your Cerb5 installation. The pages that your workers interact with are _virtual_.  This means that there isn't a file on your webserver that corresponds with each URL.

[^wikipedia-mvc]: Wikipedia: _Model-View-Controller (MVC)_  
	<http://en.wikipedia.org/wiki/Model-View-Controller>

*	**index.php** returns _responses_ for "full-cycle" HTTP _requests_.  These requests occur when you type a URL into your browser, click a link, or request a resource (e.g. image, script, stylesheet, file download).  The typical response is to render a new page of output, often including a header, top-level menu, body content, and footer.
*	**ajax.php** returns _responses_ for **Asynchronous Javascript And XML (Ajax)** [^wikipedia-ajax] _requests_.  _"Asynchronous"_ refers to the fact that these requests happen silently in the background, and your browser won't clear the existing page contents as it would during a full-cycle request.  Ajax requests are used to provide functionality aimed at making web applications feel more responsive and interactive (in a way that only desktop applications used to be).  These requests generally only affect one part of the greater whole.

[^wikipedia-ajax]: _Wikipedia: Asynchronous Javascript and XML (Ajax)_  
	<http://en.wikipedia.org/wiki/Ajax_(programming)>

Here are some common examples of Ajax functionality: 

* Autocomplete suggestions from the address book when you begin typing an email address.
* A list that refreshes its contents after sorting or paging without redrawing the entire page.
* Pulling up supplemental information from the server without leaving the page you're on.  For example, if you hover over an email address the interface may display a tooltip with the past 10 conversations you've had with that particular contact.  This information is being downloaded from the server in real-time, as it's usually far too much information to send in advance for all possible interface interactions.

### Protecting filesystem access ###

You only need to expose three files to the outside world.  The application will transparently manage access to other resources, such as images or file attachment downloads.

You should make the following files available to web requests:

* `ajax.php`
* `index.php`
* `favicon.ico`

Browser access to the following locations should be forbidden:

* `.svn/`
* `.git/`
* `api/`
* `features/`
* `libs/`
* `storage/`

If you're using the provided `.htaccess` file for _friendly URLs_, then we've already given you some defaults for blocking access to these directories:

~~~
RewriteRule ^(.*/)?\.svn(/|$) - [F,L]
RewriteRule ^(.*/)?\.git(/|$) - [F,L]
RewriteRule ^(.*/)?api(/|$) - [F,L]
RewriteRule ^(.*/)?features(/|$) - [F,L]
RewriteRule ^(.*/)?libs(/|$) - [F,L]
RewriteRule ^(.*/)?storage(/|$) - [F,L]
~~~

With Apache, you can use directives in your virtual host configuration to protect these directories:

* <http://httpd.apache.org/docs/2.0/sections.html>
* <http://httpd.apache.org/docs/2.0/misc/security_tips.html#protectserverfiles>

With nginx, you can use the following directive in your server configuration:

	location ~ ^/cerb5/(api|features|libs|storage)/ {
	    return 403;
	}

### Considerations for HTTP authentication and IP-based security ###

#### Workers ####

Your organization may have a requirement that all worker access should be password protected or restricted to requests coming from inside your corporate network.  That's a fine policy, but there are a few things you need to consider when implementing a firewall, HTTP authentication, or IP-based security restrictions.

#### Scheduled tasks ####

All scheduled tasks are triggered by automated requests to the `/cerb5/cron` URL.  If these requests are coming from a cronjob on the same webserver then this usually doesn't present a problem.  However, once you've established IP-based restrictions you should test the scheduled automated access to that URL to make sure it can get through.  These scripts will also need to provide HTTP authentication details if you're enforcing them.

#### Web-API ####

If you have applications that use the **Web-API** (<http://wiki.cerb5.com/wiki/Web-API>) to integrate with your helpdesk then you'll need to make sure they can make requests to the `/cerb5/rest/*` path.  You'll need to provide some extra code to handle HTTP authentication.

#### Community portals ####

Community portals also make requests to Cerb5.  If you install the Support Center, or another community portal, on an external webserver then you need to make sure that machine can make requests to the `/cerb5/portal/*` path.  The default community portal script doesn't provide a mechanism for HTTP authentication, but you could provide an override by IP address.  This feature is on the project wishlist [^chd679].

[^chd679]: Feature request: _Community Tools should support HTTP authentication if the parent helpdesk is password protected._  
	<http://wgmdev.com/jira/browse/CHD-679>

## Performance ##

### PHP opcode caching with XCache ###

When a webserver receives an HTTP request for a PHP script, the human-readable source code is compiled into a machine-friendly format called _opcodes_ [^wikipedia-opcode]. By default, opcodes are only kept around long enough to serve an individual request and then they're disposed of to free up resources.

[^wikipedia-opcode]: Wikipedia: _Opcode_  
	<http://en.wikipedia.org/wiki/Opcode>

The default behavior makes sense in a shared hosting environment where dozens of scripts and applications may be sharing the same system resources. However, as you can imagine, when a single PHP web-app receives a disproportionately large amount of the traffic then the process of constantly recompiling its source code can become a performance bottleneck.

**XCache** (<http://xcache.lighttpd.net/>) is an extension for PHP which caches the most frequently-accessed scripts in the machine-friendly opcode format. Typically, XCache is a moderate, turnkey performance-boost that doesn't require any application-level changes to benefit from the cache.

![XCache flowchart](images/checklist/xcache_flowchart.png)

#### Installing XCache ####

Installing XCache is an advanced topic. If you don't have administrator-level control of your server then it's likely not something you're going to be able to do.

XCache is not found in the **PHP Extension and Application Repository (PEAR)** (<http://pear.php.net/>), so you'll need to install it through a package manager or compile it manually.

On Debian and Ubuntu systems you can install XCache with `apt-get`:

	$ sudo apt-get install php5-xcache

Depending on your installation method the XCache configuration will be at the end of your `php.ini` file, or in a `conf.d/xcache.ini` file.  

If you have other applications on your webserver that you don't want to be managed by XCache, you can disable caching by default with the following settings in your `php.ini` or `conf.d/xcache.ini` file:

	xcache.cacher = Off
	
Then you can add the following option in your virtual host configuration or an `.htaccess` file:

	php_flag xcache.cacher On
	
#### Don't use Alternative PHP Cache (APC) ####

The **Alternative PHP Cache (APC)** [^php-apc] is a popular choice for opcode caching because it's part of PEAR, which makes installing it really simple.  However, APC suffers from some major drawbacks.  It has limited support for recent versions of PHP, and there are many situations where it will produce segmentation faults [^apc-segfaults], cryptic `FATAL` errors on _"Line 0"_ [^apc-problems], or blank white pages in the web browser.

[^php-apc]: PHP Manual: _Alternative PHP Cache (APC)_  
	<http://php.net/manual/en/book.apc.php>
[^apc-segfaults]: Google: _Segfaults caused by APC._  
	<http://www.google.com/search?q=apc+segfault>
[^apc-problems]: Cerb5 forums: _Cryptic errors on "Line 0" found to be caused by APC._  
	<http://forum.cerb4.com/showthread.php?t=3106&highlight=xcache>

### Memcached ###

One of the major sources of latency in web applications is often database interaction.  Databases are used to save information between sessions (_persistence_), and they provide functionality for sorting, joining, and filtering large collections of information.  Databases are a natural fit for _dynamic_ content -- that which changes frequently as people use the application (e.g. profiles, voting, comments).

The use of a database may feel so convenient for novice developers that they begin to serve all content that way, including _static_ content -- that which doesn't change while the application is in use (e.g. HTML templates, CSS stylesheets, files for download).

In our experience, the worst kind of content to frequently read from a database is _immutable_ -- that which is written once and then *never* changes (e.g. email messages, file attachments, log entries).

There is nothing inherently wrong with storing this information in a database.  Issues arise when infrequently changed information is *frequently* read from the database, due to the overhead of aggregating, joining, filtering, sorting, and returning results.  At best, the database itself caches the results of these extraneous read queries.  At worst, the database expends considerable resources every request to retrieve data that hasn't changed in the past 1,000 read queries.

**Memcached** (<http://memcached.org/>) provides a shared memory cache for arbitrary information.  This allows static or immutable content to be retrieved from the database once and then requested many times without incurring database overhead.  Even better, content in a single Memcached instance can be cached from multiple databases, and read by multiple servers.

Memcached advantages:

* Shared memory allows multiple processes to share a single cache.
* Memcached instances can be shared by multiple readers.
* Cache requests can be distributed over multiple instances.
* Reading from Memcached is often faster than reading from a database; especially when content has been pre-sorted and pre-filtered, and is cached in a serialized object format (e.g. JSON) that can be quickly reconstituted by the application.  Information read from databases is usually based on rows and columns, and these need to be reassembled into objects which have a considerable overhead cost if performed frequently.

Memcached disadvantages:

* Applications need to be designed with Memcached support in mind.
* It's volatile memory and shouldn't be used to store anything that you can't repopulate from a persistent source (like the filesystem or a database).
* It's yet another process running on your server.
* It provides no means of authentication. That's up to you.

![Memcached flowchart](images/checklist/memcached_flowchart.png)

#### Installing Memcached ####

It's often best to install Memcached from your platform's package manager; however, some distributions (such as Debian Etch) will install rather old versions. You want version 1.2.2 or later.

You'll use PECL to install **PHP's Memcached extension** (<http://us3.php.net/memcached>). The similarly named **Memcache extension** (<http://us3.php.net/memcache>) will also work, although it is an older library with fewer features.

Once Memcached is installed you need to edit your Cerb5 `framework.config.php` file and change the following lines:

	//define('DEVBLOCKS_CACHE_PREFIX',''); // ONLY A-Z, a-z, 0-9 and underscore
	//define('DEVBLOCKS_MEMCACHED_SERVERS','127.0.0.1:11211');
	
To something like:

	define('DEVBLOCKS_CACHE_PREFIX','myhelpdesk'); // ONLY A-Z, a-z, 0-9 and underscore
	define('DEVBLOCKS_MEMCACHED_SERVERS','127.0.0.1:11211');
	
* `DEVBLOCKS_CACHE_PREFIX` - This prefix allows multiple applications to share a single Memcached. If no prefix is used, then only one application can use a particular key (e.g. "worker_list"). The prefix can be anything as long as it's unique. We often suggest using the database name as the prefix.

* `DEVBLOCKS_MEMCACHED_SERVERS` - This is a list of Memcached instances to distribute requests between. Usually you won't need more than a single Memcached instance, but if you want to distribute requests then add more host:port pairs delimited by commas (e.g. `127.0.0.1:11211, 127.0.0.2:11211`).

#### Security implications of Memcached ####

By default, Memcached does not have any form of authentication.  Make sure you don't bind Memcached to a publicly-accessible network interface without establishing firewall rules. It defaults to only serving local requests on the `127.0.0.1` address.

You also need to consider if other local users and scripts can connect to Memcached's port (`11211` by default). Choosing a unique prefix will help safeguard your cached data, but it shouldn't be all you rely on.

In summary:

* Don't bind Memcached to a public network interface without firewall rules.
* Consider that local users can connect to Memcached's port. Choosing a non-standard port is a decent start, but local users with shell access can easily discover running services with commands like:

~~~
netstat --tcp -l
~~~

* If you don't set a prefix for Memcached keys then anyone connecting to the port can read your cached information if they're familiar with an application's source code (and Cerb5's source code is world-readable).
