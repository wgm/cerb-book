
## Maintenance ##

### How Cerb5 Fights Spam ###

**Important Note:** Until you've handled a few hundred messages the anti-spam system will probably come to some seemingly bizarre conclusions.  This is part of the training process, and why it's important to spend a few minutes a day to review the spam filtering decisions that were made for you.  By making corrections Cerb5 will learn your preferences.

#### Executive Summary ####
Cerb5 employs a [Bayesian filtering algorithm](http://en.wikipedia.org/wiki/Bayesian_spam_filtering) to combat spam.

This filtering tactic is useful because it's highly adaptable and learns based on the type of mail you receive.  For example: a pharmacy's helpdesk may receive mail that mentions "pills" and "prescription", and it won't be considered spam-related since those words come up often in the health care industry.  To the rest of us they're very suspicious words in our inbox.  Messages that mention "outsourcing" and "programming" likely have a much higher probability of being junk mail to a pharmacy than, say, a software company.

The above example is simplistic, but it should give you a good idea of how it works.

#### Technical Notes ####
(These notes only affect how Cerb5 makes spam decisions, and not how e-mail is handled in the system otherwise.)

* Anti-spam is only triggered for the first message on a new ticket.
* All text is converted to lowercase.
* Accent marks are removed from characters (e.g. 'ü' to 'u', 'é' to 'e').  Even though Cerb5 supports European languages, accent marks are found in many spam messages attempting to disguise English words.  Accented letters also dramatically increase the number of possible word combinations, which slows down e-mail parsing on busy helpdesks.
* Punctuation is removed and whitespace between words is reduced to a single space.
* Duplicate words are removed.
* Common words are removed from the text (e.g. 'the', 'that', 'which').  These are present in almost every e-mail and aren't useful for making filtering decisions.
* The remaining words are filtered to be 3 or more characters short, or 24 or less long.  This is a performance tweak that helps keep the database manageable.

Once Cerb5 has reduced an e-mail to a list of unique, less-than-common words it sorts the words by "interestingness".  Words are considered interesting by being either very innocent ("Cerberus") or very suspicious ("pills") on a scale of 0.0001 to 0.9999.  The most interesting words are used to calculate a spam score, based on how the content of those messages was perceived by the helpdesk workers.  Words are more interesting as they deviate in either direction from a 0.5000 median (the median being where a word shows up as often in spam as non-spam and cancels itself out).

### Training ###

![Workers should always use the 'Report Spam' button](images/01-maintenance_spam.png)

* We've designed training to happen naturally based on a worker's actions in the helpdesk.
* It's important for workers to use the 'Report Spam' button rather than just deleting junk mail.  Deleting mail doesn't teach the system to be suspicious of similar mail in the future.
* Workers should never mark spam as 'Closed' or it will accumulate as clutter in the database.  Ultimately this impacts performance, bloats backups, makes searches take longer, wastes system resources, etc.
* A ticket is automatically trained as 'Not Spam' when it's replied to by a worker.  This means in most cases a worker should never have to instruct the system when a message isn't junk.  It happens automatically.
* Tickets that match explicit group filtering rules are automatically trained as 'Not Spam'.  This helps catch newsletters, receipts, and other read-only tickets your workers may never need to reply to.

#### A note on whitelisting ####
We're often asked why Cerb5 doesn't just whitelist contacts from the address book and never consider their messages as spam.  The problem with that approach is how easy it is for spammers to forge ("spoof") the sender on e-mail they're sending you. It's not uncommon to receive spam *from* your own e-mail addresses.

By not automatically whitelisting address book senders, Cerb5 has a chance to filter out spoofed mail as well -- mail that would create thousands of junk messages otherwise.

### Tips for efficiently managing spam in Cerb5 ###
The main goal of Cerb5's anti-spam functionality is to quarantine suspicious messages so they don't show up as active tickets in your work lists.  Companies vary in their anti-spam processes -- from deleting suspicious mail immediately, to quarantining and reviewing -- and we don't want to force people to adopt an arbitrary official process.  The downside of this philosophy is that it's not obvious how to best configure the anti-spam system.

While the ideal process will depend on your environment, here are the tips that we recommend to all new community members:

#### Groups ####
The first step in filtering spam is configuring the behavior of your groups:

![Configuring a group to quarantine probable spam.](images/01-maintenance_spam_group.png)

* Configure each group to filter probable spam (>=85%) into a bucket called 'Spam'.
* While it may seem efficient to have a single 'Spam' bucket shared between all groups, you should create a bucket for each group.  The advantages:
	* A 'Spam' bucket per group allows you to quickly tell where messages lived before they were considered spam.
	* It's easier to delegate spam review to workers for their respective groups.
	* A bucket per group still respects your group rosters.  If you have a private group, you don't want any false-positives (legitimate mail accidentally marked as spam) to become readable by everyone because it lands in a global 'Spam' bucket.



#### Mail Workflow ####
Because the goal of spam filtering is to prevent workers from wasting their valuable time looking at junk mail, it's not enough to just flag mail as spam.  You also need to hide spam from work lists.

On the 'group setup', 'Workflow' tab, workers should uncheck the 'Assignable' option for all 'Spam' buckets. This will hide them from the totals in Mail Workflow.

Hiding the spam buckets is important because it allows workers to actually accomplish zero active tickets.  Otherwise, a 'Spam' bucket in the 'Sales' group with 20 messages will always show a 'Sales' group total of more than 20.  The spam buckets should only be cleared once a day.  It's really inefficient for workers to have to deal with spam buckets every time they quarantine something.

#### Purging quarantined spam ####
Once a day, a group should elect a member to review and empty the spam bucket.  This helps make sure that a legitimate message that has been mistakenly flagged as spam (rare as that may be) isn't ignored for too long -- such messages may relate to a valuable and time-sensitive opportunity.

![There are usually way too many spam messages in a list to check them all individually.](images/01-maintenance_spam_list.png)
	
We don't expect your workers to read every single quarantined message in the spam bucket, since most of the time everything is going to be junk.  Here's our process for rescuing legitimate mail from an overflowing spam bucket and purging the rest:

* Create a new workspace with a work list that shows only the spam buckets of groups you're responsible for.
* Click 'customize'
	* Add the '# Nonspam' column to the work list.
	* Set the number of rows to 100 (or whatever you find comfortable).
* Click 'piles', choose 'senders' and click the red 'X' to delete anything that's obviously forged (e.g. junk mail sent to the helpdesk from your own email addresses).  This goes quicker if you can delete mail sent from an entire domain (in our case, @webgroupmedia.com).  You don't have to spend too long on this step, it just helps quickly reduce the amount of mail you're double-checking.
* Sort the '# Nonspam' column in descending order (by clicking on the heading until the icon next to it shows large rows above small rows).
* Quickly scan through the remaining subjects in the list until the '# Nonspam' count is 0.  Use 'peek' on any ambiguous tickets.  Rescue anything legitimate (which should usually be nothing) by selecting the row and using 'Bulk Update' ('only checked'). Don't worry about confirming anything that's junk. 

* Once you're done, click 'Bulk Update' at the bottom of the list:
	* Select 'Whole list'
	* Status: 'Deleted'
	* Spam: 'Report Spam'
	* Save Changes (this may take a while)
	
![Use piles to automate a big chunk of the work in a few clicks.](images/01-maintenance_spam_piles.png)

![Sorting the '# Nonspam' column in descending order.](images/01-maintenance_spam_sort.png)	

![Using Bulk Update to rescue false positives.](images/01-maintenance_spam_bulk_update.png)

![Using Bulk Update to purge the rest of the quarantined spam.](images/01-maintenance_spam_bulk_update_purge.png)
