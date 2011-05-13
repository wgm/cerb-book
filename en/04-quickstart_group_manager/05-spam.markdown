
## Anti-Spam Filtering ##

**Important Note:** Until you've handled a few hundred messages, the anti-spam system will probably come to some seemingly bizarre conclusions based on little evidence that messages are _"spammy"_.  This is part of the training process.  It's important to spend a few minutes each day reviewing the spam filtering decisions that were automatically made for you.  Cerb5 will learn when you make corrections, and it will be far less likely to make the same mistakes in the future.  Our helpdesk at WebGroup Media has been trained for several years, and it's rare to see a single _false positive_ in 10,000 messages quarantined as spam.

### How it works: in plain English ###

Cerb5 employs a **Bayesian filtering algorithm** [^wikipedia-bayesian] to combat spam.

[^wikipedia-bayesian]: Wikipedia: _Bayesian Spam Filtering_  
	<http://en.wikipedia.org/wiki/Bayesian_spam_filtering>

This filtering tactic is useful because it's highly adaptable; it learns to recognize spam based on a studious analysis of the patterns of content found in your typical daily mail.  For example, a pharmacy's helpdesk may receive mail that mentions words like _"pills"_ and _"prescription"_ on a daily basis, and Cerb5 won't considered these messages to be spam because those terms are familiar to the health care industry.  However, to the rest of us they're very suspicious words to find in our inbox.  Likewise, messages from unknown contacts that mention both _"Java"_ and _"programming"_ have a much higher probability of being junk mail when sent to a pharmaceutical company compared to a software development company.

### How it works: a technical explanation ###

* Anti-spam is only triggered for the _first_ message on a new ticket.
* All text is converted to lowercase.
* Accent marks are removed from characters (e.g. _ü_ to _u_, _é_ to _e_).  Even though Cerb5 supports European languages, accent marks are found in many spam messages attempting to disguise English words.  Accented letters also dramatically increase the number of possible word combinations, which slows down the process on busy helpdesks.
* Punctuation is removed and repeated whitespace between words is reduced to a single space.
* Duplicate words are removed.
* Common _stop words_ are removed from the text (e.g. _the_, _that_, _which_).  These words are present in almost every email and therefore aren't very useful for making filtering decisions.
* The remaining words are filtered to be between `3` and `24` characters in length.  This is a performance tweak that helps keep the database manageable.

Once Cerb5 has reduced an email to a list of unique, _less-than-common_ words it sorts the words by _interestingness_.  Words are considered interesting by being either very innocent (_"Cerberus"_) or very suspicious (_"pills"_) on a scale of `0.0001` to `0.9999`.  The most interesting words are used to calculate a spam score, based on how the content of those messages was perceived by helpdesk workers in the past.  Words are more interesting as they deviate in either direction from a `0.5000` median (the median being where a word shows up as often in spam as in non-spam and cancels itself out).

### Configuring anti-spam functionality for groups ###

The main goal of Cerb5's anti-spam functionality is to quarantine suspicious messages so they don't show up as active tickets in your worklists.  Companies vary in their anti-spam processes -- from deleting suspicious mail immediately, to quarantining and reviewing -- and we don't want to force people to adopt an arbitrary _"the one true way"_ process.  The downside of this philosophy is that it's not obvious at first how to best configure anti-spam functionality.

While the ideal process will depend on your own particular environment and preferences, the following steps are recommended to all new helpdesk managers.

The first step in filtering spam is configuring the behavior of your groups:

![Configuring a group to quarantine probable spam.](images/maintenance/maintenance_spam_group.png)

* Configure each group to filter probable spam (``>=85%``) into a bucket called _Spam_.
* While it may seem more efficient to have a single _Spam_ bucket shared between all your groups, we've found that most people should create a bucket for each group for these reasons:
	* A _Spam_ bucket per group allows you to quickly tell where messages lived before they were considered spam.
	* It's easier to delegate spam review to workers for their own respective groups.
	* A bucket per group still respects your group rosters.  If you have a private group, you don't want any false positives (legitimate mail accidentally marked as spam) to become readable by everyone simply because it lands in a global _Spam_ bucket.

### Best practices for spam prevention in Cerb5 ###

![Workers should always use the 'Report Spam' button](images/maintenance/maintenance_spam.png)

We've designed training to happen naturally based on a worker's actions throughout the helpdesk:

* It's important for workers to use the _Report Spam_ button rather than just deleting junk mail.  Deleting mail doesn't teach the system to be suspicious of similar mail in the future.
* Workers should never mark spam as _Closed_ or it will accumulate as clutter in the database.  Ultimately this impacts performance, bloats backups, makes searches take longer, wastes system resources, etc.
* A ticket is automatically trained as _Not Spam_ when it's replied to by a worker.  This means in most cases a worker should never have to instruct the system when a message isn't junk; it happens automatically.
* Tickets that match explicit group filtering rules are automatically trained as _Not Spam_.  This helps catch newsletters, receipts, and other read-only tickets your workers may never need to reply to.

Because the goal of spam filtering is to prevent workers from wasting their valuable time looking at junk mail, it's not enough to just report mail as spam.  You should also hide spam from worklists.  To do so, navigate to the _Workflow_ tab for each group and uncheck the _Assignable_ checkbox for each _Spam_ bucket. This will hide their contents from the totals on the _Workflow_ list on the _Mail_ page.

Hiding the spam buckets is also important because it allows workers to accomplish the goal of having zero active tickets.  Otherwise, a _Spam_ bucket in the _Sales_ group with 20 messages will always show a _Sales_ group total of more than 20.

#### Why doesn't Cerb5 just "whitelist" address book contacts? ####

**Whitelisting** is the process of ignoring a particular set of rules for trusted entities. 

We're often asked why Cerb5 doesn't whitelist contacts from the address book and never consider their messages as spam.  The problem with that approach is how easy it is for spammers to forge (_"spoof"_) the sender on email they're sending you. It's not uncommon to receive a lot of spam _from_ your own email addresses.

By not automatically whitelisting address book senders, Cerb5 has a chance to filter out spoofed mail as well -- and that's mail that would otherwise create thousands of junk messages that you would have to manually clean up.

### Purging quarantined spam ###

The spam buckets should only be cleared once per day.  It's inefficient for workers to have to manage the spam buckets every time they quarantine something.  Each group should elect members to review and empty the spam bucket.  This helps make sure that a legitimate message that has been mistakenly flagged as spam (as rare as that may be) isn't ignored for too long.

![There are usually too many messages in a Spam bucket to audit them all individually.](images/maintenance/maintenance_spam_list.png)
	
We don't expect your workers to read every single quarantined message in the _Spam_ bucket, since most of the time everything is going to be junk.  Here's our process for rescuing legitimate mail from an overflowing spam bucket while quickly purging the rest:

* Create a new workspace with a worklist that shows only the _Spam_ buckets of the groups you're responsible for.

* Click the _customize_ link on the worklist.
	* Add the _# Nonspam_ filter to the worklist.
	* Set the number of rows to `100` (or whatever you find comfortable to read).

* Click the _piles_ link on the worklist, select _senders_ and click the red _X_ icon to delete anything that's obviously forged (e.g. junk mail sent to the helpdesk from your own email addresses).  This process is quicker if you can delete mail sent from an entire domain (in our case: `webgroupmedia.com`).  You don't have to spend too long on this step, it just helps quickly reduce the amount of mail you're auditing.

	![Use piles to automate a big chunk of the work in a few clicks.](images/maintenance/maintenance_spam_piles.png)
	
* Sort the _# Nonspam_ column in the worklist in descending order by clicking on the column header until the icon is an arrow pointing downward.  
	
	![Sorting the '# Nonspam' column in descending order.](images/maintenance/maintenance_spam_sort.png)
	
* Quickly scan through the remaining subjects in the list until the _# Nonspam_ count is `0`.  Use _(peek)_ on any ambiguous tickets to check their content.  Rescue anything legitimate (which should usually be nothing) by selecting the row in the worklist and clicking the _Bulk Update_ button.

	![Using Bulk Update to rescue false positives.](images/maintenance/maintenance_spam_bulk_update.png)
	
* At this point, don't worry about confirming that anything is junk mail.

Once you're done auditing for false positions, click the _Bulk Update_ button at the bottom of the list:

![Using Bulk Update to purge the rest of the quarantined spam.](images/maintenance/maintenance_spam_bulk_update_purge.png)

* **With:** _Whole list_
* **Status:** _Deleted_
* **Spam:** _Report Spam_
* Click the _Save Changes_ button
* You may have to wait a couple minutes depending on the size of your list.
	
