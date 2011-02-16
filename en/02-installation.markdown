
# Installation #

_If you're using Cerb5 On-Demand then you don't need to worry about installing the software.  You can skip ahead._

While it is possible to successfully install Cerb5 on a shared hosting account, we highly recommend that you control the server you're installing the software on.

## Downloading the files to your webserver ##

First, change directory to your website's document directory.  The directory will usually be named something like `htdocs`, `httpdocs`, or `www`, under the appropriate website.

**On Unix-based servers:**

	$ cd /home/vhosts/example.com/httpdocs
	
**On Windows-based servers:**

Use Windows Explorer to open the website's document directory.

### Developers should download the project from GitHub using Git ###

If you are planning on contributing patches back to the project then you should [fork](http://help.github.com/forking/) the project on [GitHub](http://github.com/wgm/cerb5) first.  This will give you your own copy of the source code.  Once that is done you should use your own repository information in the command below.  If you want to checkout the project from our official repository use the following command:

**On Unix-based servers:**

	$ git clone git@github.com:wgm/cerb5.git
	
**On Windows-based servers:**

Use a graphical Git client, like [TortoiseGit](http://code.google.com/p/tortoisegit/).

### Production sites should use Subversion ###

When deploying Cerb5 on a production server you should use Subversion.  This allows you to quickly upgrade the software to benefit from future updates.  Subversion compares your files to our files and it will only download the changes.  You'll never need to download the entire project again after your initial installation.  With Subversion you won't have to worry about an update resetting the contents of your `framework.config.php` file, or erasing your other changes.

**On Unix-based servers:**

	$ svn checkout http://svn.webgroupmedia.com/cerb5/branches/stable cerb5
	
**On Windows-based servers:**

Use a graphical Subversion client, like [TortoiseSVN](http://tortoisesvn.tigris.org/).

### The unfortunate should use the ZIP Archive ###

In the event you don't have access to Git or Subversion, you can download a ZIP archive from the project website.  This archive includes the Subversion repository information in case you want to take advantage of the convenience of version control in the future.
	
You can download the latest release using `cURL`:

	$ curl -O http://www.cerberusweb.com/downloads/cerb5/cerb5-latest.zip
	
Or you can use `wget`:

	$ wget http://www.cerberusweb.com/downloads/cerb5/cerb5-latest.zip

## Setting permissions ##

Make sure the Cerb5 files are owned by the proper user and group on the webserver.

There are only two locations that need write access by the webserver during your installation:

*	`framework.config.php`
*	`storage/`

**On Unix-based servers:**

	$ cd cerb5
	$ chown -R www-data:www-data .
	$ chmod -R framework.config.php storage

**On Windows-based servers:**

Configure your site in IIS.

## Running the guided installer ##

Cerb5 comes with a guided installer that helps you initially configure the system.

To start the installer, point your browser to: `http://www.example.com/cerb5/install`

_You will need to replace `www.example.com` with your actual domain name_.

### Accepting the software license ###
### (and so on) ###

## Friendly URLs ##

## Security ##
