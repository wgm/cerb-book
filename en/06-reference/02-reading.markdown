
## Display Conversation ##

At the top of the conversation display page, a toolbar provides shortcuts for common actions:

![The conversation toolbar.](images/06-display_toolbar.png)

* **Edit** displays a popup to edit the conversation properties.

* **Close** closes the conversation.  Closed conversations are considered to be resolved.  If you want to hide a conversation that is dependent on a future date (e.g. _"We'll cancel your site at the end of the month."_) then use _Waiting for reply_ instead. A closed conversation will be reopened automatically if a recipient replies.

* **Spam** marks the current conversation as spam and delete it.  This process trains your helpdesk to become more proficient at automatically detecting spam in the future.  If a conversation is junk mail, and you want to avoid wasting time with mail like it in the future, be sure to mark it as spam rather than simply deleting it.  You also shouldn't mark these conversations as _closed_ because they will be preserved forever, causing clutter in your database, wasting space in backups, reducing search performance, etc.

* **Delete** deletes the current conversation.  After a few days of inactivity all deleted conversations are permanently purged from the helpdesk and they will be unrecoverable without database backups.  When you delete a conversation, it is the same as moving a file into the _trashcan_ on your computer.  You want to be absolutely sure that the conversation is not something you will need in the future.  If it is something you will need later, then a better option is to mark the conversation as _Closed_ or _Waiting for reply_.

* **Take** is a shortcut to assign the conversation to yourself.

* **Print** displays the conversation in a print-friendly format.

* **Refresh** refreshes the conversation.  It is exactly the same thing as refreshing a page in your browser, except that it always uses the full permalink URL for the conversation.

* **Merge** shows a popup for merging multiple conversations together.

* **more>>** displays additional actions that are less frequently used.

* **Move to** is a shortcut for moving a conversation into a new bucket.

### Conversation ###

![The Conversation tab.](images/06-conversation_tab.png)

The _Conversation_ tab displays a timeline of messages and comments.  This timeline uses Coordinated Universal Time (UTC  [^wikipedia-utc]).  If a conversation is taking place between people in Los Angeles, London, and Tokyo, all that matters is the _relative_ time.  In other words, something that happened _5 minutes ago_ in Tokyo also happened _5 minutes ago_ from the perspective of London.

[^wikipedia-utc]: Wikipedia: _Coordinated Universal Time_ <<http://en.wikipedia.org/wiki/Coordinated_Universal_Time>>

There is a toolbar at the top of the timeline:

* **Comment** displays a popup for adding a comment to the conversation history.  
* **Read All** displays all the messages in the conversation history in ascending chronological order (i.e. the oldest messages are shown first).  This is useful when you're new to a conversation and you want to catch up from the beginning.

There are three types of messages in the timeline:

* **Received** messages that have been sent _to_ the helpdesk _from_ recipients.
* **Sent** messages that have been sent _from_ the helpdesk _to_ recipients.
* **Drafts** that are saved as a _work in progress_ but haven't been _sent_ yet.
	
Messages can be **minimized** or **maximized**.  A minimized message condenses the sender information and hides the message content.  Conversely, a maximized message displays everything.  Older messages are minimized to reduce _information overload_.  The latest message is usually the most important one.

![A message.](images/06-conversation_message.png)

When you send a physical letter through the mail you have to write an address on the outside center of the envelope for it to be deliverable (i.e. a _destination address_).  You also write the name of a person or organization at that destination whom you authorize to open the letter (i.e. _recipients_).  If the destination address is invalid, or your authorized recipients can't be found there, then it is useful to include your own address on the outside top-left of the envelope so the letter can be sent back (i.e. a _return address_).

The delivery of email works in a similar way.  With email, the writing on the envelope is replaced with information called _headers_.  The most useful headers are displayed at the top of each message:

* `From:` is the email address of the sender, and optionally their name.
* `To:` is a list of the primary recipients' email addresses.
* `Cc:` is a list of _carbon copy_ email addresses.  These recipients are publicly declared to have received a copy of the message.  Recipients who received a copy of the message in private would be included in the `Bcc:`, or _blind carbon copy_, list, but this header is not sent to each recipient for obvious reasons.
* `Subject:` is the current topic of conversation.
* `Date:` is the date and time that the message was sent.  This is usually displayed in the _local timezone_ of the sender since this information is helpful.

You can also click the _full headers_ link to view any additional headers associated with the message.  This will often include information like spam scoring (`X-Spam-Status:`, `X-Spam-Level:`), routing and relay information (`Received:`), tools used to create the message (`X-Mailer:`), character encoding (`Content-Type:`), unique identifier (`Message-ID:`), and so on.  These details are useful for troubleshooting and forensics, but they probably aren't something you'll need to look at for every message.  

**sender name**  
**set organization**  
_full headers_  
_skip to bottom_  

* **Reply**
* **Forward**
* **Sticky Note**
	**Sticky notes** are used to keep a message maximized regardless of how far back it is in the timeline.  This is useful when you want to call attention to a specific message.  Compared to comments, sticky notes are short-lived; it is expected that notes will be deleted after they are read by their recipient, and the desired action has been taken.
* **Print**
* **Delete**

**Comments** are private messages that only other workers can see.

### Links ###

### Recipient History ###

