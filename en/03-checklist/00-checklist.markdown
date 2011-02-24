\newpage

# Preparing Your Helpdesk For Business #

## Creating accounts for your workers ##

The people who answer helpdesk issues are called _workers_. We chose this term because it's much less ambiguous than referring to your staff as _users_.  You can consider anybody that interacts with your helpdesk as a _user_, including your customers and partners.  When we mention _workers_, we're referring to the staff members that log in to the helpdesk to respond to your customers as representatives of your organization.  These workers might be in customer service, billing, sales, I.T., or marketing; or they may be executives, developers, or system administrators.

While Cerb5 can be used effectively by a single worker, taking advantage of its powerful collaboration features requires you to invite your co-workers to use it.

If this is a brand new helpdesk, the only worker that you should see listed is yourself.  This is the account that was created for you at the end of the installation process.

Let's liven the place up by adding worker accounts for a few more people:

1. Click _setup_ in the top right.
1. Click the _Workers_ tab.

First, let's update your own account information.  Click the link on the first or last name of _Super User_ in the _Workers_ list.

![Configuring a worker account.](images/checklist/adding_workers.png)

* In **Contact Information** you can update a worker's name, title, and email address.  Keep in mind that if you change a worker's primary email address it will become their new helpdesk login.

* **Authentication** is used to update a worker's password, and you can also promote them to _Administrator_, which will give them the permission to do absolutely everything in the helpdesk; they are like the `root` account on Unix-based systems, or the `Administrator` account in Windows.  Only workers who are administrators can enter _Setup_.  Grant administrator access judiciously.

* Lastly, the **Memberships** box defines the worker's _membership_ to each of your groups.  You can also designate the worker as a _manager_ of a group.  Managers can perform group-level administration: adding other workers as new group _members_, creating _buckets_ for organizing the group's work, etc.

When you've finished with your changes, click the _Save Changes_ button.

You can use this knowledge to create several new workers in your helpdesk with the _Add Worker_ button.  Then we'll organize them into groups.

![A list of helpdesk workers.](images/checklist/workers_list.png)

## Creating groups ##

Workers are organized into _groups_. Groups are a flexible concept that can be based on anything: brand, product, department, timezone, language, etc. By default your helpdesk will contain three groups based on common departments: Dispatch, Support, and Sales. The defaults were chosen because they're common departments across most industries, not because they're all-encompassing. You are free to modify these groups to suit your needs.

If you stick with the defaults, this is the proposed workflow:

* **Dispatch** catches all mail that isn't explicitly routed somewhere. Some companies prefer to have a human dispatcher assign work -- to verify support eligibility and route issues based on skillsets -- and this group is an easy way to achieve that. Because Cerb5 doesn't require incoming mail to always map to a group, the Dispatch group is also the best way to spot incoming e-mail addresses (like `billing@example.com`) that you may want to route directly to a particular group.
* **Support** collects issues related to support: product support, customer service, FAQs, billing, etc.
* **Sales** collects issues related to sales: leads, new orders, refunds, resellers, etc.

The workers inside groups can be either _managers_ or _members_. Groups are designed to be autonomous; a group manager has the power to make most configuration changes related to the group without requiring help from an administrator. Managers can add new members to the group, and they can configure the group's workflow by creating buckets and inbox filters.

The common practice is to use groups for building a roster of fairly interchangeable workers; work can be given to the group from the outside with the confidence that any member knows what to do with it. Groups share an organization system based on _buckets_, but workers outside the group aren't expected to know how other groups organize their work (and it shouldn't matter to them). Instead, new work is given to groups through their _inbox_, and the group's own filters will decide how work is routed or assigned from there.

Let's add a couple new groups.  At this point you're also welcome to change the default groups.

* Click _setup_ in the top right.
* Click the _Groups_ tab.
* Click the name of a group on the left, or click _add a new group_ to create one.

![Configuring a group.](images/checklist/groups_add.png)

* **Name** is what the group will be referred to as throughout the interface.
* **Members** is the list of workers that you would like to designate as members or managers.

If the configuration of a group seems too simple, that's because it is -- from _Setup_.  The rest of group configuration is handled by managers:

* Click _groups_ in the top right.

	![Navigating to the groups page.](images/checklist/menu_groups.png)

* Click the name of any group to manage it.

This page shows any worker the groups they are a member of, but only managers can make changes to a group.  Normally, if a worker isn't a member of a group then it won't show up in this list.  However, as an administrator you have access to every group as if you were a manager, even if you aren't on the roster.

![Managing a group.](images/checklist/groups_manage.png)

There are several tabs on group management:

* **Workflow:** This is where new buckets are created and optionally flagged as assignable. The contents of assignable buckets will be shown as _Available_ when workers are looking for things to do in the _Workflow_ tab of the _mail_ page.
* **Mail Preferences:** Each group can define their own `From:` address, personal sender name, shared e-mail signature, and auto-responses for new tickets or closed tickets. Each group can also define a different spam filtering policy, as different workflows are more sensitive or forgiving of junk mail.
* **Inbox Routing:** Group managers can define a number of inbox filters that will be applied to any new mail received by the group. This automates most of the work of putting things in their proper place.
* **Members:** This is the group's roster.
* **Ticket Fields:** Each group can track their own custom fields on new tickets. For example, the Sales department may want to track the source (Google, website, ad) of leads while Support is interested in tracking the category of requests (FAQ, feature request, etc). These custom fields can be used to generate reports.

### Creating buckets to organize work ###

_Buckets_ are flexible containers used by groups to organize their workload. Work can queue up in buckets, making it more efficient to handle similar issues at the same time (e.g. processing orders, issuing refunds, sending out beta information). Buckets are also useful to move piles of work out of the way if they shouldn't be handled immediately (e.g. newsletters, survey responses, feature requests). 

With department-themed groups your buckets might look like:

* **Billing:** receipts, refunds
* **Corporate:** execs, partners, biz-dev
* **Development:** bugs, feature requests, feedback
* **I.T.:** logs, alerts, abuse
* **Marketing:** surveys, newsletters
* **Sales:** leads
* **Support:** technical, account issues

Or if you had product-related groups you might do the following:

* **Product X:** Orders, Refunds, Support
* **Product Y:** Orders, Refunds, Support
* **Product Z:** Orders, Refunds, Support

Since buckets are often only concerned with a single shared characteristic, they aren't always the best way to organize work; but they're a useful building block for more complex workflows.

Let's add a few new buckets to the _Sales_ group:

* Manage the _Sales_ group from the _groups_ link in the top right.
* Click the _Workflow_ tab.
* In the _Add Buckets_ box, enter the names of the buckets you'd like to create.  For example:
	* Leads
	* Invoices
	* Orders
	
	![Creating new buckets for the Sales group.](images/checklist/groups_buckets.png)
	
* Click the _Save Changes_ button.

You should now see your new buckets in the _Workflow_ list.  It's a good idea to move the default _Spam_ bucket to the bottom of the list so it's out of the way.  You can do this quickly by setting its order to `99` (or anything higher than the number in the last row).

For workflow purposes, buckets can be designated as _assignable_ or _non-assignable_.

**Assignable buckets** contain work that is actionable -- that is, the work should be assigned and handled in a relatively quick timeframe in an effort to keep the bucket empty and the recipients happy.  Some examples of assignable work include:

* New orders
* Payment receipts
* Sales inquiries
* Requests for help
* Partnership requests

**Non-assignable buckets** contain work that doesn't need immediate attention and would otherwise detract from the goal of having empty buckets and quick handling of actionable work.  Non-assignable work is usually queued up until a future point and then handled in bulk.  Some examples of non-assignable work include:

* Automatically quarantined spam
* Bounces (return to sender)
* Logged events sent to email
* Long-running promotions that will be processed at a future date
* Vendor newsletters

Given these definitions, you can also unmark the checkbox in the _Assignable_ column next to the _Spam_ bucket.  This will hide your spam buckets from the _Workflow_ view, making it easier to focus on actionable work.

![Customizing group workflow.](images/checklist/groups_workflow.png)

## Friendly URLs ##

![This URL isn't very pretty.](images/checklist/friendly_urls.png)

You may notice that your URLs look a bit ugly with the omnipresent `/index.php/` in the path.  Cerb5 can use _URL rewriting_ to make URLs shorter and more user-friendly, but this requires webserver support. 

![This looks much nicer with friendly URLs enabled!](images/checklist/friendly_urls_done.png)

### Enabling friendly URLs with Apache ###

If you're using the Apache web server you can enable _"friendly URLs"_ with the following commands:

	$ cd /path/to/cerb5
	$ cp .htaccess-dist .htaccess
	
For this to work you will need `mod_rewrite` to be enabled in your Apache configuration.  This is usually the case, unless you have just installed it.

If `mod_rewrite` isn't enabled, you can enable it with the following command on many Linux-based servers:

	# a2enmod rewrite
	
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

Traditionally, when you access a URL like `http://www.example.com/help.html` from your browser there is corresponding file on the webserver with the name `help.html`.  This is true for HTML, images, Javascript, CSS, and other files available for download.

Cerb5 uses a different approach for serving content, which is known to web application developers as the [Model-View-Controller](http://en.wikipedia.org/wiki/Model-View-Controller) (MVC) architectural pattern.  As a consequence of this, all the public interaction with your helpdesk occurs with two main files in the root directory of your Cerb5 installation. The pages that your helpdesk users interact with are _virtual_.  This means that there isn't a file on your webserver that corresponds with each URL.

*	**index.php** returns _responses_ for "full-cycle" HTTP _requests_.  These requests occur when you type a URL into your browser, click a link, or request a resource (e.g. image, script, stylesheet, file download).  The typical response is to render a new page of output, often including a header, top-level menu, body content, and footer.
*	**ajax.php** returns _responses_ for [Asynchronous Javascript And XML](http://en.wikipedia.org/wiki/Ajax_(programming)) (**Ajax**) _requests_.  _"Asynchronous"_ refers to the fact that these requests happen silently in the background, and your browser won't clear the existing page contents as it would during a full-cycle request.  Ajax requests are used to provide functionality aimed at making web applications feel more responsive and interactive (in a way that only desktop applications used to be).  These requests generally only affect one part of the greater whole.

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

#### Helpdesk users ####

Your organization may have a requirement that all helpdesk access should be password protected or restricted to requests coming from inside your corporate network.  That's a fine policy, but there are a few things you need to consider when implementing a firewall, HTTP authentication, or IP-based security restrictions.

#### Scheduled tasks ####

All scheduled tasks that are configured in the helpdesk are triggered by automated requests to the `/cerb5/cron` URL.  If these requests are coming from a cronjob on the same webserver then this usually doesn't present a problem.  However, once you've established IP-based restrictions you should test the scheduled automated access to that URL to make sure it can get through.  These scripts will also need to provide HTTP authentication details if you're enforcing them.

#### Web-API ####

If you have applications that use the [Web-API](http://wiki.cerb5.com/wiki/Web-API) to integrate with your helpdesk then you'll need to make sure they can make requests to the `/cerb5/rest/*` path.  You'll need to provide some extra code to handle HTTP authentication.

#### Community portals ####

Community portals also make requests to your helpdesk.  If you install the Support Center, or another community portal, on an external webserver then you need to make sure that machine can make requests to the `/cerb5/portal/*` path.  The default community portal script doesn't provide a mechanism for HTTP authentication, but you could provide an override by IP address.  This feature is on the project wishlist [^chd679].

[^chd679]: Feature request: _Community Tools should support HTTP authentication if the parent helpdesk is password protected._  <<http://wgmdev.com/jira/browse/CHD-679>>

## Performance ##

### PHP opcode caching with XCache ###

When a webserver receives an HTTP request for a PHP script, the human-readable source code is compiled into a machine-friendly format called ["opcodes"](http://en.wikipedia.org/wiki/Opcode). By default, opcodes are only kept around long enough to serve an individual request and then they're disposed of to free up resources.
The default behavior makes sense in a shared hosting environment where dozens of scripts and applications may be sharing the same system resources. However, as you can imagine, when a single PHP web-app receives a disproportionately large amount of the traffic then the process of constantly recompiling its source code can become a performance bottleneck.

[XCache](http://xcache.lighttpd.net/) is an extension for PHP which caches the most frequently-accessed scripts in the machine-friendly opcode format. Typically, XCache is a moderate, turnkey performance-boost that doesn't require any application-level changes to benefit from the cache.

![XCache flowchart](images/checklist/xcache_flowchart.png)

#### Installing XCache ####

Installing XCache is an advanced topic. If you don't have administrator-level control of your server then it's likely not something you're going to be able to do.

XCache is not found in the [PHP Extension and Application Repository](http://pear.php.net/) (PEAR), so you'll need to install it through a package manager or compile it manually.

On Debian and Ubuntu systems you can install XCache with `apt-get`:

	apt-get install php5-xcache

Depending on your installation method the XCache configuration will be at the end of your `php.ini` file, or in a `conf.d/xcache.ini` file.  

If you have other applications on your webserver that you don't want to be managed by XCache, you can disable caching by default with the following settings in your `php.ini` or `conf.d/xcache.ini` file:

	xcache.cacher = Off
	
Then you can add the following option in your helpdesk's vhost configuration or an `.htaccess` file:

	php_flag xcache.cacher On
	
#### Don't use Alternative PHP Cache (APC) ####

The [Alternative PHP Cache](http://php.net/manual/en/book.apc.php) (**APC**) is a popular choice for opcode caching because it's part of PEAR, which makes installing it really simple.  However, APC suffers from some major drawbacks.  It has limited support for recent versions of PHP, and there are many situations where it will produce segmentation faults [^apc-segfaults], cryptic `FATAL` errors on _"Line 0"_ [^apc-problems], or blank white pages in the web browser.

[^apc-segfaults]: Google: _Segfaults caused by APC._ <<http://www.google.com/search?q=apc+segfault>>
[^apc-problems]: Cerb5 forums: _Cryptic errors on "Line 0" found to be caused by APC._  <<http://forum.cerb4.com/showthread.php?t=3106&highlight=xcache>>

### Memcached ###

In web applications the majority of the latency in serving a request is caused by interactions with a database. Databases are often necessary for managing persistence and to providing a way to sort and filter large collections of information. However, databases are also overused to frequently read and write infrequently-changed information -- or worse, information that never changes.

[Memcached](http://en.wikipedia.org/wiki/Memcached) provides a shared memory cache where arbitrary information can be read frequently without incurring the overhead of a database.

Memcached advantages:

* Shared memory allows multiple processes to share a single cache.
* Cache requests can be distributed over multiple instances.
* It's volatile memory and shouldn't be used to store anything that you can't repopulate from a persistent source (like the filesystem or a database).

Memcached disadvantages:

* Applications need to be designed with memcached support in mind.
* It's yet another process running on your server.
* It provides no means of authentication. That's up to you.

![Memcached flowchart](images/checklist/memcached_flowchart.png)

#### Installing Memcached ####

It's often best to install Memcached from your platform's package manager; however, some distributions (such as Debian Etch) will install rather old versions. You want version 1.2.2 or later.

You'll use PECL to install [PHP's Memcached extension](http://us3.php.net/memcached). The similarly named [Memcache extension](http://us3.php.net/memcache) will also work, although it is an older library with fewer features.

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
