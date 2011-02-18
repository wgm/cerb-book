
# Preparing Your Helpdesk For Business #

## Friendly URLs ##

![This URL isn't very pretty.](images/02_friendly_urls.png)

You may notice that your URLs look a bit ugly with the omnipresent `/index.php/` in the path.  If you're using the Apache web server you can enable _"friendly URLs"_ with the following commands:

	$ cd /path/to/cerb5
	$ cp .htaccess-dist .htaccess
	
For this to work you will need `mod_rewrite` to be enabled in your Apache configuration.  This is usually the case, unless you have just installed it.

If `mod_rewrite` isn't enabled, you can enable it with the following command on many Linux-based servers:

	# a2enmod rewrite
	
Your URLs should now look more presentable:

![Much nicer!](images/02_friendly_urls_done.png)

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

### Considerations for HTTP authentication and IP-based security ###

#### Helpdesk users ####

Your organization may have a requirement that all helpdesk access should be password protected or restricted to requests coming from inside your corporate network.  That's a fine policy, but there are a few things you need to consider when implementing a firewall, HTTP authentication, or IP-based security restrictions.

#### Scheduled tasks ####

All scheduled tasks that are configured in the helpdesk are triggered by automated requests to the `/cerb5/cron` URL.  If these requests are coming from a cronjob on the same webserver then this usually doesn't present a problem.  However, once you've established IP-based restrictions you should test the scheduled automated access to that URL to make sure it can get through.  These scripts will also need to provide HTTP authentication details if you're enforcing them.

#### Web-API ####

If you have applications that use the [Web-API](http://wiki.cerb5.com/wiki/Web-API) to integrate with your helpdesk then you'll need to make sure they can make requests to the `/cerb5/rest/*` path.  You'll need to provide some extra code to handle HTTP authentication.

#### Community portals ####

Community portals also make requests to your helpdesk.  If you install the Support Center, or another community portal, on an external webserver then you need to make sure that machine can make requests to the `/cerb5/portal/*` path.  The default community portal script doesn't provide a mechanism for HTTP authentication, but you could provide an override by IP address.  [This feature is on the project wishlist](http://wgmdev.com/jira/browse/CHD-679).

## Performance ##

<http://wiki.cerb5.com/wiki/Performance>

### PHP opcode caching with XCache ###

When a webserver receives an HTTP request for a PHP script, the human-readable source code is compiled into a machine-friendly format called _["opcodes"](http://en.wikipedia.org/wiki/Opcode)_. By default, these opcodes are only kept around long enough to serve an individual request and then they're disposed of to free up resources.
The default behavior makes sense in a shared hosting environment where dozens of scripts and applications may be sharing the same system resources. However, as you can imagine, when a single PHP web-app receives a disproportionately large amount of the traffic then the process of constantly recompiling its source code can become a performance bottleneck.

[XCache](http://xcache.lighttpd.net/) is an extension for PHP which caches the most frequently-accessed scripts in the machine-friendly opcode format. Typically, XCache is a moderate, turnkey performance-boost that doesn't require any application-level changes to benefit from the cache.

![XCache flowchart](images/03_xcache_flowchart.png)

#### Installing XCache ####

Installing XCache is an advanced topic. If you don't have administrator-level control of your server then it's likely not something you're going to be able to do.

XCache is not found in the 

Like most PHP extensions, XCache is installed through the PECL repository. Installation instructions will vary according to your operating system and distribution. Check your package manager for the appropriate package for PHP's PEAR commands. For example, with Debian Etch Linux you want the php-pear package.

You can search for platform-specific instructions.

Once PECL is installed, you can issue the following console command (you may need to use sudo or your root account):

<http://en.wikipedia.org/wiki/Opcode>

<http://xcache.lighttpd.net/>

#### Don't use Alternative PHP Cache (APC) ####

The [Alternative PHP Cache](http://php.net/manual/en/book.apc.php) (APC) is a popular choice for opcode caching because it's part of the [PHP Extension and Application Repository](http://pear.php.net/) (PEAR), which makes installing it really simple.  However, APC suffers from some major drawbacks.  It has limited support for recent versions of PHP, and there are many situations where it will produce segmentation faults [^apc-segfaults], cryptic `FATAL` errors on _"Line 0"_ [^apc-problems], or blank white pages in the web browser.

[^apc-segfaults]: Segfaults caused by APC. <http://www.google.com/search?q=apc+segfault>
[^apc-problems]: Cryptic errors on "Line 0" found to be caused by APC.  <http://forum.cerb4.com/showthread.php?t=3106&highlight=xcache>

### Memcached ###

![Memcached flowchart](images/03_memcached_flowchart.png)

<http://en.wikipedia.org/wiki/Memcached>

<http://memcached.org/>
