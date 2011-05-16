\newpage

# Guide for Workers #

## Logging in ##

![Logging in](images/getting_started/logging_in.png)

Let's log in and take a look.  Enter your email address and password, then click the _Sign On_ button (or press `ENTER` after typing your password).

## Terminology ##

There are several terms that you will hear used throughout this book:

* **Cerb5** refers to this application, originally known as Cerberus Helpdesk (5th generation).  The project was started in January 2002 to help online teams efficiently distribute and handle large volumes of work: customer support questions, technical problems, tasks, sales opportunities, business partnerships, and more.

* A **database** provides long-term storage for arbitrary information (_data_).  Databases help applications maintain relationships between data (e.g. _"Which products were shipped for order #1234?"_), and they assist with performing complex analysis (e.g. "_How many orders over the past 12 months were from someone who has a surname that begins with F (AND) resides in Germany (AND) spent more than $100 USD?_").

* The **helpdesk** is web-based hub for centrally managing and archiving tickets, and routing messages between workers and recipients. This allows several workers to receive and share email without **recipients** writing to any of them individually.

* A **ticket** is a specific email conversation, including all of its messages, comments, and settings. Each ticket has a unique identifier (_ID_) for future reference by anyone involved.
	There are two kinds of IDs:
	* A _private ticket ID_ is assigned by the database.  Your first ticket is ID `1`, and a year later the most recent ticket ID may be `123500`.  These IDs are _sequential_; in other words, they add 1 for each new ticket, starting from 1.  There are many reasons you should not share these IDs:
		* They are predictable.  If someone knows that one particular ticket ID is `1025` then they are now aware of 1,024 other valid IDs.
		* They give away the volume of your helpdesk.  They can probe for the highest valid ID to determine how many tickets are in your database.
		* They give away the velocity of your helpdesk.  Your customers or competitors could create new tickets at regular intervals to gauge the traffic of your helpdesk.  	Many other software applications overlook this -- invoices are given sequential IDs, etc.
	* A _public ticket ID_ is also referred to as a _mask_, because it doesn't betray any information about the volume or velocity of your helpdesk.  Instead, ticket masks look like `XER-83472-879`.  Masks are designed to be short and verbally readable, while still providing a very large number of combinations.  There are more combinations available in three letters (17,576) than there are for 3 numbers (1,000), so you can often enter the first three letters of a ticket mask to find a match without having to type in the whole thing.  Masks are also very useful for verification outside of email (_"I need your email address and the first three letters of the ticket from the subject line"_).

* The people who log in to the helpdesk to represent your organization are called **workers**.  Other software applications frequently use the term _users_, but we find that term ambiguous; users can refer to either customers or staff members.

* A **watcher** is a worker who subscribes to notifications about new activity on specific records.  For example, a manager may assign a task to another worker, and add themselves as a watcher to the task so they can be automatically notified about progress and completion.  Being a watcher on something _does not_ imply that you are taking responsibility for it.  If you aren't sure how to answer a particular question, it can be useful to watch it so you can leave from the answer a more experienced worker gives.  Watching facilitates collaboration, observation, and training.

* A person who corresponds with the helpdesk is referred to as a **contact**.  These are your customers, partners, and prospects.  If you're part of the I.T. department for a large corporation, then contacts are likely your co-workers who ask questions and report problems.

* The **address book** is a list of all the contacts in your helpdesk.  It provides the ability to group contacts by various attributes (e.g. location, language, industry) to build customer lists and mailing lists.

* Contacts that are associated with a particular ticket are called **recipients**.  Recipients receive a copy of every new message on a ticket.

* A **bucket** is a container for storing similar tickets together. Common buckets are: Leads, Receipts, Newsletters, Refunds and Spam.  Buckets aren't the only means for grouping tickets, but they're the simplest to understand and use.

* A **group** is a collection of workers who share responsibility for the same tickets and buckets. Common groups are: Sales, Support, Development, Billing, and Corporate. These examples happen to all be departments, but groups can be established based on any attribute (product, location, etc).

* A worker in a group is called a **member**. A member with the authority to modify the group is called a **manager**. Managers can create buckets, add new group members, and establish routing rules for new work.  Groups can have any number of managers.

* Each group has an **inbox** where new tickets are delivered by default. These tickets are then dispatched automatically by workflow rules or manually by workers.

## Tour mode ##

![Tour mode](images/getting_started/tour_mode.png)

"Tour mode" is automatically enabled to provide basic training when you log in for the first time.  As you navigate through the software, the yellow box at the top of the page will highlight relevant _points of interest_ based on where you are.  Clicking on each item of interest will display a speech bubble that highlights the location of the feature and explains how to use it.  Once you are familiar with the software you can click the _hide this_ link at the top right of the yellow box to disable the tour.  You can resume the tour from your worker settings.

## Setting your basic account preferences ##

* Click your name in the top right of any page to reveal a menu.
* Select _Settings_ from the menu.

Set _Timezone_ to your current location.  The software will use this timezone when displaying dates and timestamps, making it easier for you to do time-based comparisons.

If you are in a multilingual environment, set _Language_ to your preferred language.  This will change all the text in the software.  Any text that does not have an available translation will be displayed in English by default.  This option won't translate individual email messages; they will still be displayed in the language they were written in.

If you have additional email addresses that you would like to associate with your worker account you can add them in the _Email Addresses_ section.  This is useful if you want to be able to relay mail to a mobile email account.  Keep in mind that these addresses will all represent your worker account, so you don't need to add all your personal email addresses unless you intend to use them for this purpose.

Once you have established your preferences, click the _Save Changes_ button at the bottom of the page.

## A walkthrough demonstration ##

![The navigation menu.](images/getting_started/global_navigation.png)

The navigation menu at the top of each page divides the application into sections based on the type of information being presented.

We'll now explore each of these sections and explain their functionality.  Keep in mind that some of the sections and features listed below may not be visible if your account does not have access to them.  If this happens, skip ahead to the next section.  You may also see additional sections if various optional plugins are enabled.

### Mail ###

![The mail section.](images/getting_started/section_mail.png)

The **mail** section is where you read, respond to, and initiate email conversations.  These conversations are shared with other workers in the groups you are a member of.

There is a toolbar at the top left of the mail section with several buttons:

* _Send Mail_ composes a new message to any number of contacts.  Each contact will receive a copy of all future worker messages.  The opening message will be outgoing and attributed to yourself.  This should be how you start new conversations most of the time.
* _Open Ticket_ offers similar functionality to _Send Mail_ with an important distinction: the conversation will be started as an incoming message attributed to the recipients.  This is useful when you are creating a new conversation as a convenience for one of your contacts after discussing something on a phone call.
* _Auto-Refresh_ will automatically reload the page at the given interval.  This is useful if you have multiple monitors and you want to be able to see new messages at a glance without manually reloading the page.

At the top right of the section there is a quick search text box.  This time-saving feature allows you to perform a search from most pages in the mail section without having to navigate to the search page first.

The mail section is further divided by a series of tabs that provide specialized functionality.

#### Workflow ####

The **Workflow** tab is where you will spend most of your time in the mail section.  It is optimized for working with a large volume of mail by highlighting conversations that are immediately actionable.

In many environments there are conversations that sit idle for long periods of time because they're waiting for something: a reply from a customer, a worker task to be completed, or a specific date and time.  Additionally, there are several kinds of mail that may never need a response but can't be closed without being reviewed: vendor newsletters, receipts, or automated reports.  This work should be moved to a bucket that has been flagged by a group manager as being hidden from Workflow.

Workflow focuses on conversations that you can do something about by enforcing the following criteria:

* It must be open (not closed, or waiting for a contact to reply).
* It must be located in an inbox, or a bucket that is not hidden from Workflow.

The key to the efficient use of Workflow is to have groups and buckets that are organized by the process required to complete the work contained within them.  For example, if you have buckets like _Receipts_, _New Orders_, and _Leads_, you can repeat the same process for each message in a bucket, while also reusing any resources you need in order to complete them.

You can also use the subtotals feature to group conversations based on criterion other than bucket.

The last thing you want to be doing is competing with a dozen workers who are randomly grabbing work.  Cerb5 is not meant to be used in a social vacuum.  Find a co-worker and tell them "I'll handle the new orders if you want to go through the leads".  If your team is too large to organize with an informal process, you should nominate a group _dispatcher_ to make assignments, or you should use a real-time, group-wide communication tool like Campfire by 37signals (<http://www.campfirenow.com/>).

#### Search Tickets ####

The **Search Tickets** tab searches through the mail archive and provides results as tickets.  This means that even if multiple messages from a conversation match they will be represented by a single result.

This is the best place to display all the conversations in the system, regardless of their status or other factors that prevent them from being displayed on the Workflow tab.

#### Search Messages ####

The **Search Messages** tab searches through the mail archive and provides results as messages.  This is useful when you want to be able to match individual messages on a conversation as distinct results.

This is the best place to display a list of outgoing messages that behaves like a 'Sent' folder in a traditional email reader.

#### Drafts ####

The **Drafts** tab displays a worklist of messages that you have saved but not yet sent.  You can click on a draft to resume it.

#### Snippets ####

The **Snippets** tab manages fragments of text that can be reused in various conversations.  For example, when a customer asks for your price list you can simply insert the 'Price Sheet' snippet rather than monotonously typing it by hand or copying and pasting it from your website.  Snippets can also contain placeholder text that will change depending on where they are used; for example, a `{{first_name}}` placeholder would be replaced by the first name of the contact you are talking to.

### Activity ###

![The activity section.](images/getting_started/section_activity.png)

### Address Book ###

### Knowledgebase ###

### Reports ###

### Profiles ###

### Groups ###

## Watching ##

## Your worker profile ##

## Reading notifications ##

## Creating custom workspaces ##