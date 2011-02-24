\newpage

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

## Creating the database ##

Create a new MySQL database using the console or your favorite GUI.

	mysql> CREATE DATABASE cerb5 CHARACTER SET utf8;
	mysql> GRANT ALL PRIVILEGES ON cerb5.*  \
			TO cerb5@localhost \
			IDENTIFIED BY 'secret_password';

## Running the guided installer ##

Cerb5 comes with a guided installer that helps you initially configure the system.

To start the installer, point your browser to: `http://www.example.com/cerb5/install`

_You will need to replace `www.example.com` with your actual domain name_.

### Running the server environment checker ###

![The requirements checker in the guided installer.](images/installation/installer_step1_checker.png)

The installer will determine if your server meets the requirements for installing Cerb5.  Correct any problems before proceeding, and then click the _"Next Step"_ button.

### Accepting the software license ###

Click the _"I Accept"_ button if you agree to the licensing terms.

### Configuring the database connection ###

![Adding database connection details.](images/installation/installer_step3_database.png)

Add your database connection details:

*	**Database driver:** You can leave this at the default for MySQL 5.x+
*	**Host:** This is the hostname of your database server.  If it's on the same machine as your webserver you can enter _"localhost"_ to use sockets rather than TCP/IP connections (which are faster).  Otherwise, enter a hostname or IP address.
*	**Database Name:** Enter the name of the new database you created.
*	**Username:** Enter the username from your MySQL `GRANT PRIVILEGES` command.  This user should have full privileges to the database (`SELECT`, `INSERT`, `UPDATE`, `DELETE`, `CREATE/ALTER/DROP TABLE`, etc).
*	**Password:** Enter the password from your MySQL `GRANT PRIVILEGES` command.

Once you have entered your database details, click the _"Test Settings"_ button to verify the software can connect to your database.

If successful, the installer will create your initial database structure from incremental patches for each plugin.  This may take a few moments, so don't worry if it doesn't look like anything is happening.

### Configuring general settings ###

![Personalizing your helpdesk.](images/installation/installer_step6_settings.png)

In this step you'll configure the general settings of the helpdesk.

*	**Helpdesk Title:** This is the text shown at the top of the browser, or on each browser tab, that makes it easy to distinguish your helpdesk from other web pages.
*	**Default Sender:** This is the email address that your helpdesk will use as the _"From:"_ address on outgoing mail that can't be attributed to a particular group.  It is very important that this email address delivers mail to your helpdesk.  In many cases you will want to use an email address like _"support@example.com"_.
*	**Default Sender Name:** This is the personalized name that will display next to the _"From:"_ address in your customers' inboxes. For example: 

~~~
From: "WebGroup Media LLC" <support@webgroupmedia.com>
~~~

### Configuring outgoing mail (SMTP) ###

![Configuring outgoing mail.](images/installation/installer_step7_smtp.png)

Your helpdesk won't be very useful if it can't send mail to your contacts.

*	**SMTP Server:** This is your SMTP mail server.  If your mail server is located on the same machine as your webserver then you can enter _"localhost"_.  Otherwise this should be a hostname or IP address that your webserver is authorized to relay through.  This is most often a corporate mail server or [Google Apps](https://www.google.com/a/).
*	**SMTP Port:** This is the port where your SMTP server is listening for mail.  If you aren't sure what port to use, check with your server administrator.  Here are some defaults:
	* SMTP: `25`
	* SMTP-TLS: `587`
	* SMTP-SSL: `465`
	* If you're using Google Apps, instructions can be found  at <http://mail.google.com/support/bin/answer.py?hl=en&answer=13287>.
*	**SMTP Auth User:** If your SMTP server requires authorization then enter your username here.
*	**SMTP Auth Password:** This is only required if your SMTP server is using authentication.
*	**SMTP Encryption:**
	*	TLS: Mail will be sent over a transport layer security ([TLS](http://en.wikipedia.org/wiki/Transport_Layer_Security)) connection.
	*	SSL: Mail will be sent over an secure socket layer ([SSL](http://en.wikipedia.org/wiki/SSL)) connection.
	*	None: Encryption is disabled.

Click the _"Test Outgoing Mail"_ button to verify your settings.

### Creating your account ###

![Creating your account.](images/installation/installer_step8_account.png)

Now the installer will prompt you to create an administrator account that you will use to log in to the helpdesk.  Use an email address that you check, but not something that routes back to the helpdesk.  Your helpdesk will use this email address to contact you in the event you lose your password, or if you configure notifications.

Choose a password and then click the _"Continue"_ button.

### Registering your helpdesk ###

In the final step of the installer you are given the opportunity to introduce yourself to the Cerb5 development team in exchange for a _free_ 3 seat (simultaneous workers) license to help you get started.  There are no strings attached.  This license provides full functionality with no expiration and it is valid for the version of the helpdesk you are installing, but it does not provide access to product updates.  We hope that you find the software useful, and we're looking forward to growing along with you.

If you choose not to register your copy of Cerb5 then the software will default to _"Free Mode"_ which is limited to a single seat but has no other restrictions.

Complete the short survey in exchange for your free license and then click the _"Register"_ button.

### Finishing up! ###

![The installation process is complete.](images/installation/installer_step11_finished.png)

You're almost ready to start using your new helpdesk.

If this is a production installation, you should delete the `install` directory since it is no longer necessary and it provides access to some particular sensitive information (specifically, your `phpinfo()` output that describes your webserver environment).  If you're planning to use language packs for localization of your helpdesk, be sure to copy the `install/extras/translations/*` files to another location.

If this is a development installation, then you should leave the `install` directory intact since it contains many useful scripts and examples for plugin development.

Once you're ready to log in, click the _"Take me there!"_ link.

