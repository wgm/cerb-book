
## Upgrading to Newer Versions of Cerb5 ##

### Introduction ###

The officially supported way of upgrading Cerb5 is by using Subversion[^subversion]. Subversion is a version control system which will automatically update your helpdesk files to the latest version.

[^subversion]: Wikipedia: _Subversion_  
	<http://en.wikipedia.org/wiki/Subversion_(software)>

The major advantage of Subversion is that it will attempt to automatically merge official code changes with any customization you have done. It also gives you the ability to list all your local changes to any project files, and to revert to an official version when desirable.

On Unix-based servers you can check if Subversion is installed by typing:

    svn --version

On Windows-based servers [TortoiseSVN](http://tortoisesvn.tigris.org/) is your best option.

_If SVN is not an option you can fall back on using the zipped build of Cerb5. [How do you upgrade Cerb5 without SVN?](http://wiki.cerb5.com/wiki/Upgrade_Cerb5_without_SVN)_

### Preparation ###

* **Always make a backup** of your helpdesk database prior to
    upgrading. MySQL backups are best done with mysqlhotcopy or
    mysqldump.

* Copy your _framework.config.php_ file to _backup.config.php_

* Download the Server Environment Checker and ensure your server meets
    the current minimum requirements for Cerb5:
    [http://www.cerberusweb.com/downloads/cerb5-servercheck.php.txt](http://www.cerberusweb.com/downloads/cerb5-servercheck.php.txt)


#### On Unix-based Servers ####

Change to your cerb5/ directory and type:

    svn update

##### Changing between stable, a specific version, and development (optional) #####

**To switch your files to the latest stable version**

    svn switch http://svn.webgroupmedia.com/cerb5/branches/stable/cerb5

**To switch your files to a specific version**
_This example assumes you want to switch to version 5.3.2. Replace `5_3_2` with the version you wish to switch to._

    svn switch http://svn.webgroupmedia.com/cerb5/tags/5_3_2/

**To switch your files to the latest development version**

    svn switch http://svn.webgroupmedia.com/cerb5/trunk/cerb5

##### Viewing the changelog #####

You can view the Subversion changelog by typing the following command.

    svn log | more

#### On Windows-based Servers ####

**Using TortoiseSVN to update Cerberus Helpdesk**

![](images/maintenance/maintenance_updating_svn_win.png)

TortoiseSVN integrates with your Windows GUI. To update Cerberus Helpdesk:

* Open the folder containing your Cerb5 files.
* Right-click in the empty whitespace of the folder (as if you were going to create a new file).
* Choose "SVN Update" from the pop-up menu.

You should be shown a list of updated files and the new build versions for the project. Press the "OK" button when finished reading.

### Finishing the Upgrade ###

#### Permissions ####

You should set file ownership and permissions again after updating your files using Subversion.

**Unix-based servers:**

From your cerb5/ directory at the console (replace **www-data** with
your appropriate Apache user and group):

    chown -R www-data:www-data .
    chmod -R 0774 storage/

_Permissions, especially on php files are a common upgrade issue. See **Troubleshooting** (below) for more details._

**Windows-based servers:**

Use Windows Explorer to set the appropriate write permissions on the `/cerb5/storage` directory for your IIS user.

#### Database Patches ####

Some Cerberus Helpdesk updates contain database changes which require a helpdesk administrator to finalize. This will prohibit all helpdesk activity (e.g., logins, scheduled tasks, mail parsing) to prevent any database corruption while you're between versions.

After your files are updated, attempt to log into your Cerb5 helpdesk instance as you normally would. If a database update is required Cerberus will automatically prompt you. Upon finalizing you should be able to log in and continue working.

#### Community Tools ####

Very rarely, the `index.php` file which drives your Community Tools may change during an upgrade.

How to tell if you need to update your Community Tool file:

* Log into your Cerb5 helpdesk.
* Click 'Helpdesk Setup' from the top navigation menu.
* Click the 'Community Portals' tab.
* Select any portal to edit it.
* Click the 'Installation' tab.
* Compare the following line from the `index.php` output with your deployed `index.php`:
	
	`define('SCRIPT_LAST_MODIFY', 2009070901); // last change`

* If the number is different you should replace the `index.php` file for your Community Tool with the new version from the helpdesk.

We're working on a way to make this check happen automatically.

#### Install Directory ####

A Subversion update will recover your `cerb5/install` directory. You need to delete this directory again, since it poses a security risk if left available.

#### Troubleshooting ####

If you opted for a "safe" upgrade by making a backup and moving your install to a different location, you may see a blank page or cache-related error in your browser when loading the Helpdesk. Try [clearing the cache](http://wiki.cerb5.com/wiki/Clearing_the_cache) to fix this.

Occasionally you may want to force your plugins to reload after a simple update that doesn't patch the database and clear out the cache. Click 'Helpdesk Setup' and select the plugins tab to automatically reload them.

##### Linux Errors Caused by PHP File Permissions #####

When upgrading, SVN can change file permissions. When this happens, you may see several symptoms

* Cerberus fails to start and the browser displays an *Internal Server Error* page
* Cerberus displays in a browser, but nothing happens when you click some of the buttons or links
* If you have access to your server logs, look for errors similar to *SoftException in Application* or other errors indicating a specific php file failed.

If you are receiving the *Internal Server Error* page, check the permissions on `index.php` in your main Cerberus directory.

If you are receiving the *Internal Server Error* page or Cerberus fails to execute when links or buttons are clicked, check the file permissions on the php files (`index.php` in a Cerberus sub-directory, or other php file which starts the function).

**PHP Permissions**

**File permissions are dependent on server setup and vary widely. Before proceding, if you have any questions about correct php file permissions, check the permissions on other browser accessible php files that are currently working or check with a server admin.**

If PHP permissions are causing these errors, the permissions may be set to allow group or public writing to these files. Disallow writing to *public* and *group*. Then test your installation.

You can use `CHMOD` or an FTP client to set the permissions. With the exceptions noted in the upgrade procedure, all files within the Cerberus directory and sub-directories usually have the same permissions.
