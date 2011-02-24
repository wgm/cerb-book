
## Setup ##

![Navigating to the setup page.](images/06-menu_setup.png)

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

![The storage tab in Setup.](images/06-storage_tab.png)

#### The downside of database-backed storage ####

One major benefit of working on Cerb5 for over 9 years is that we've been able to observe the long-term outcome of our technology decisions.  It's not uncommon for a popular helpdesk to have millions of tickets in the database after a few years.  Most tickets have a conversation with several email messages, and occasionally those messages have file attachments.

Relational databases just aren't designed for efficient document storage.  If your email conversations average 10,000 characters in length (10KB) then you will require 1GB of storage for every 100,000 tickets in your database.  Your email contents will take up around 10GB after you hit 1 million tickets.  While it's important to have fast access to historical ticket _meta_ information for building worklists and running reports -- such as sender, subject, status, group, bucket, owner, and last activity date -- you rarely need fast access to message contents or attachments for tickets that have been closed for a considerable amount of time.

Even though the technology curve has storage capacity increasing while prices are decreasing, this type of usage would have a negative impact on the performance of your helpdesk:

* The database engine still has to read/write a massive file to add, update, and remove records in a bloated table.
* If corruption occurs on the table then it may take several hours to rebuild it.
* When running backups, you'll be copying the same _immutable_ data over and over again.  The meta information on tickets changes frequently (the status and bucket may change multiple times per day), but the content of an email message never changes.  The same is true for attachments; their content is written once, and it's far more efficient to do _incremental_ backups of only changed content -- especially when you're talking about 100,000s or millions of objects.

There are several reasons why poorly-designed applications continue to bloat the database with heavy, immutable content:

* It's easier to back-up a single database than multiple components (database + filesystem, etc).
	* Our response: It might be easier to back-up a single database, but it's far quicker and more efficient to incrementally backup immutable database content.  While you're copying the same 500,000 file attachments in a large database table every couple hours, I'm adding incrementally adding the newest 100 file attachments to the existing 499,900 attachments that have already been backed up.
* Full-text searching is easier to implement when it's enabled directly on a database table with lots of content.
	* Our response: We pre-digest content to remove stop words (common phrases like: the, when, and) and strip punctuation and whitespace before inserting it into a database table specifically designed for our full-text indexes.  This heavily reduces the amount of space needed to store the originals for full-text indexed content, since only the interesting words are being indexed.  MySQL ignores the stop words and punctuation anyway.
* When a cluster of webservers are serving the same content, it's usually easier (but not desirable) to share content through the database than network-based filesystems.
	* Our response: Writing large BLOBs to multiple database slaves is also inefficient, which could block other queries in the meantime, and pollutes the binary logs.  It's worth investing some time in learning how a distributed filesystem or API-based network storage work.
* With enough memory, a database (or at least its indexes) can be stored entirely in RAM without suffering from filesystem I/O bottlenecks.
	* Our response: There's no benefit to loading up 100,000s of infrequently accessed rows in memory, no matter how fast it is. Distributed filesystems have similar, if not superior, caching and concurrency compared to a relational database.  Especially when you're talking about using simple keys for lookups.

In previous generations of Cerberus Helpdesk we also stored file attachments in the database.  Based on our experience with scaling the project throughout the years, we now store attachments in the local `storage/` filesystem where they can be handled more efficiently (note: message content is still stored in the database).  Despite the advantages of this approach, there are still valid situations (like clustering) where the local filesystem is a source of constant inconveniences.  System administrators have to `rsync` the `storage/` filesystem around a cluster, or _symlink_ the `storage/` filesystem to a shared filesystem.  In effect, we trade the inefficiencies of the database with new inefficiencies in scaling beyond a single webserver.

To provide a truly scalable and flexible solution, we designed the _Devblocks Storage Service_.

The Devblocks Storage Service automatically _archives_ heavy content to long-term storage when it is deemed _inactive_, and it quickly _unarchives_ inactive content if it becomes active again.  The Storage Service can manage content from any functionality in the helpdesk, including third-party plugins.  For example, while it's great for efficiently managing a large number of file attachments or messages contents, it could also be used to manage driver downloads, a PDF document collection, or an image gallery.

Inactive content can be archived to one of three _storage engines_ by default.  A _storage profile_ brings objects (e.g.  attachments, message content) under Storage Engine management.  Each storage object is given a unique _storage key_ based on the storage engine.  Additional storage engines and storage profiles can be added through plugins.

* **Disk:** Content is stored in the local `storage/` filesystem.  Each storage profile receives its own subdirectory.  Stored objects are hashed and distributed between 1,024 buckets -- 32 directories with 32 subdirectories each.  The 32 options represent the letters A through V, and the numbers 0 through 9.  Each directory is a single character.  Hash buckets look like `storage/attachments/5/f` and `storage/messages/b/d`.  The storage key is the hash bucket and file record ID (e.g. `5/f/1000`).  There are no file extensions.  This layout makes it easy to correlate filesystem files with database records irrespective of the hash buckets involved, simplifying maintenance and integrity checks.

* **Database:** Content is stored in the database for situations where it makes sense.  Each storage profile has its own database table.  The storage key is the file record ID.

* **Amazon S3:** Content is stored in Amazon's Simple Storage Service (S3) through Amazon Web Services (AWS), which is distributed, redundant, scalable, secure, and durable.  Each storage profile has its own virtual directory inside the designated _S3 bucket_.  We chose to use a single S3 bucket per helpdesk so multiple storage objects for multiple helpdesks under the same management could share a single AWS account.  Storage objects in S3 are stored with the same hash bucket approach as the _Disk_ storage engine (1,024 hash buckets, 32 directories with 32 subdirectories each).  The storage key is the hash bucket and file record ID.

The Storage System is also designed to be fast and fault-tolerant.  You are free to configure the Storage System to save all attachments to Amazon S3 immediately, while saving all message content directly to the filesystem.  All fresh content is written to either the disk or the database, since if the application is working they will always be available.  This is more efficient than blocking the email parser while waiting for attachments to upload to a storage engine like Amazon S3.  Once content is saved, it may then enter the queue to be moved to a remote storage engine.

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
