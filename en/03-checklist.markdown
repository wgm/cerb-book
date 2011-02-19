
# Preparing Your Helpdesk For Business #

## Friendly URLs ##

![This URL isn't very pretty.](images/02_friendly_urls.png)

You may notice that your URLs look a bit ugly with the omnipresent `/index.php/` in the path.  Cerb5 can use _URL rewriting_ to make URLs shorter and more user-friendly, but this requires webserver support. 

![This looks much nicer with friendly URLs enabled!](images/02_friendly_urls_done.png)

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
*	**ajax.php** returns _responses_ for [Asynchronous Javascript And XML](http://en.wikipedia.org/wiki/Ajax_(programming)) (_"AJAX"_) _requests_.  _"Asynchronous"_ refers to the fact that these requests happen silently in the background, and your browser won't clear the existing page contents as it would during a full-cycle request.  Ajax requests are used to provide functionality aimed at making web applications feel more responsive and interactive (in a way that only desktop applications used to be).  These requests generally only affect one part of the greater whole.

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

![XCache flowchart](images/03_xcache_flowchart.png)

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

The [Alternative PHP Cache](http://php.net/manual/en/book.apc.php) (APC) is a popular choice for opcode caching because it's part of PEAR, which makes installing it really simple.  However, APC suffers from some major drawbacks.  It has limited support for recent versions of PHP, and there are many situations where it will produce segmentation faults [^apc-segfaults], cryptic `FATAL` errors on _"Line 0"_ [^apc-problems], or blank white pages in the web browser.

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

![Memcached flowchart](images/03_memcached_flowchart.png)

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
