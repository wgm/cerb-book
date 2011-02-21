\newpage

# Developer Guide #

## The toolchain used by the official development team ##

Cerb5 is written in [PHP](http://www.php.net/) and stores data in a [MySQL](http://www.mysql.com/) database.

We use the following tools at WebGroup Media when developing Cerb5:

* [XAMPP](http://www.apachefriends.org/en/xampp.html) is an easy to install Apache distribution containing the latest versions of MySQL, PHP and Perl. XAMPP is really very easy to install and to use -- just download, extract and start.  It contains distributions for Mac OS X, Linux, and Windows.
* [Aptana Studio](http://www.aptana.com/products/studio2) is a web development environment based on Eclipse that combines powerful authoring tools for PHP, HTML, CSS, and JavaScript, along with thousands of additional plugins created by the community.  It is cross-platform and freely available.
* [GitHub](https://github.com/) is a web-based hosting service for software development projects that use the Git revision control system.  The site provides social networking functionality such as feeds, followers and the network graph to display how developers work on their versions of a repository.
* [GitX](https://github.com/brotherbard/gitx) (Brotherbard's fork) is a graphical Git client for Mac OS X that provides an interface for comparing diffs, staging changes, browsing history, and pushing/pulling changes. 
* [FakeSMTP](https://github.com/jstanden/fakesmtp-app) is a tool written for Mac OS X by WebGroup Media LLC that allows developers to monitor and discard mail that is being sent via SMTP by applications in testing environments.

## Forking and checking out the project files from GitHub ##

## Setting up a development environment ##

### Mac OS X ###


### Linux ###


### Windows ###


## Technical information ##

### Features vs. Plugins ###

(`/features`)
(`/storage/plugins`)

## Developer guidelines ##

### Code formatting ###

### Commit messages ###

(* is changelog-worthy)
(reference bug tracker issue numbers when they are available)

## Writing plugins ##

(SDK in `/cerb5/install/extras/sdk`)

### plugin.xml manifests ###

### Extension points ###

### Event points ###

### Namespaces ###

### Resource proxy ###

### Smarty ###

* `{devblocks_url}`
* `{$var|devblocks_date}`
* `{$var|devblocks_prettybytes}`
* `{$var|devblocks_prettytime}`
* `{$var|devblocks_translate}`

### DevblocksPlatform class ###

### CerberusApplication class ###

### Scope (jQuery/Smarty/etc) ###