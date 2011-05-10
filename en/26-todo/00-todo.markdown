\newpage

# [TODO]: Topics to cover #

## Updating the helpdesk to a new version ##

## Backing up your helpdesk data ##

## Scheduler `/cron` ##

special options (pop3 max, parse max, ignore wait, loglevel, etc)

## Ticket statuses ##

* Open
* Waiting for reply
* Closed
* Deleted

## Anti-spam ##

If a conversation is junk mail, and you want to avoid wasting time with mail like it in the future, be sure to mark it as spam rather than simply deleting it.  You also shouldn't mark these conversations as _closed_ because they will be preserved forever, causing clutter in your database, wasting space in backups, reducing search performance, etc.

How it works...

## Cerb5 Software License ##

Dual license?

## Version numbers ##

Cerb5's version numbers are in the format: `<generation>.<release>.<update>`

* **Application generations** represent software evolution and innovation in the purest form. A new generation embodies everything learned from earlier work without the requirement to be conceptually or technologically compatible with the past.  Generations in our project typically occur once every 2-3 years.
* **Major releases** represent significant improvements in functionality, usability, or style.  Releases are focused on delivering new value, and they are required to be consistent with the current generation's concepts and technology.  Major releases occur every 2-3 months.
* **Maintenance updates** implement usability feedback and fix reported bugs.  Maintenance updates occur every 1-3 weeks.

Therefore, a version of `5.4.2` means:

* 5th generation (`5.x`)
* 4th major release (`5.4.x`)
* 2nd maintenance update (`5.4.2`)

The next maintenance update is: `5.4.3`

The next major release is: `5.5.0`

The next generation is: `6.0.0`

## Clustering ##

Two web server nodes; one database node

* Install Cerb5 completely on one web node first (or outside of the cluster and move the database in)

* Grant DB access to the other nodes
* Run the installer requirements checker on the other nodes
* Copy the framework.config.php to the other nodes
* Load balancer share sessions (cookies)

* Memcached (share cache) -- edit /etc/memcached.conf and make sure bind to internal IP -- enable + copy MEMCACHED node list to all framework.config.php -- make sure php5-memcache package (ext) is installed

### Replicate /storage filesystem ###
* NFS:
* Firewall ports (`rpcinfo -p`: portmapper, nfs, nlockmgr, lockd)
* Move `storage` on slaves
* `mount -o"rw,hard,intr,async,nosuid,nodev" host:/path/to/master/storage storage`