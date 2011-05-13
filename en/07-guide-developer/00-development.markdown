\newpage

# Guide for Developers #

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


## Developer guidelines ##

### Features vs. Plugins ###

You should instruct end-users to install custom plugins in the `/storage/plugins` directory rather than the `/features` directory.  While the design of plugins in either location is identical, the official plugins in the `features/` directory have a special significance.

There are several important reasons for this:

* In high-availability scaling and clustering, a shared filesystem (e.g. GlusterFS, NFS) should be used for serving `storage/` content, but there's no need to serve all content from a share since there's more overhead and latency than the local disk.  In some cloud environments, virtual volumes accumulate I/O costs.  The files in `features/` (and the rest of the application) can be considered static and loosely replicated across webserver nodes.

* In On-Demand environments, a single copy of the core Cerb5 files should be _symlinked_ between multiple instances.  This allows opcode caching (e.g. XCache) to operate far more efficiently.  Hundreds of Cerb5 instances can be served from a single XCache of around 32MB.  Each of these sites only has a unique `framework.config.php` file and `storage/` directory.

* Filesystem backups should only need to account for a single directory of filesystem changes in `storage/`.  They can exclude our project files, which may be easily recovered from our repositories.  This is especially important in On-Demand environments with hundreds or thousands of copies of Cerb5.

* The instructions for moving or upgrading Cerb5 take into account the `storage/` directory, but changes to the core files are considered expendable.  The files outside of `storage/` can be easily replaced from our GitHub or Subversion repositories.

* During upgrades or troubleshooting, we may need to disable all third-party plugins.  It's easy to exclude the `features/` directory from this behavior, since it has been heavily quality-assurance tested prior to release.

### Code formatting ###

### Commit messages ###

(* is changelog-worthy)
(reference bug tracker issue numbers when they are available)

