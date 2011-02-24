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
