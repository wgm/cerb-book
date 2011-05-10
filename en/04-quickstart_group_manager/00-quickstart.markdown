\newpage

# Guide for Group Managers #

If you've been given manager status in a group then you have autonomous control over its roster, configuration, and workflow.

To manage a group:

1. Click _groups_ in the top right.
2. Click the name of the group you would like to manage from the list on the left.

![Managing a group.](images/group_manager/groups_manage.png)

Group management is organized between several tabs:

* **Mail:** Configures the group's mail preferences.
* **Buckets:** Manages the group's organizational buckets.
* **Virtual Attendant:** Configures the group's automated Virtual Attendant for various behavior; for example, routing new mail into buckets, or assigning workers to relevant conversations.
* **Members:** Manages the group's roster.
* **Custom Fields:** Each group can track their own custom fields on new tickets. For example, the Sales department may want to track the source (Google, website, ad) of leads while Support is interested in tracking the category of requests (FAQ, feature request, etc). These custom fields can be used to generate reports.

## Configuring group mail preferences ##

![Setting group mail preferences.](images/group_manager/group_setup_mail.png)

You have one option in your group's mail preferences -- whether or not a reference number should be displayed in the subject line of your email responses.  There are some considerations for both users and technology.

For users, a reference number makes it easy to refer to the details of a particular conversation at a later date, in any channel.  For example, a user may decide to follow-up their original email through a live-help chat, IM, Twitter, Facebook, or phone conversation.  In public channels, a reference number like "ABC-12345-678" is far more anonymous than providing a name, company, or email address.  In private channels, like a phone call, it's probably easier to just ask a customer for their email address when retrieving their conversation history.

From a technical standpoint, the reference number was used in earlier versions of Cerb5 to associate a reply with the message it was replying to.  When someone replies to a message sent from Cerb5, the headers are checked first, and then the subject line is scanned for a reference number as a last resort.  This is not necessary with most modern email applications.  Incoming messages can be associated with the message they are replying to using the `In-Reply-To:` and `References:` headers from the RFC-2822 standard.  Unsurprisingly, Microsoft chose to implement their own proprietary headers in Exchange and Outlook (`Thread-Topic:` and `Thread-Index:`), but the proper `In-Reply-To:` header should still be added by Exchange when mail originated from outside a sender's organization.  However, it's not uncommon to encounter users with misconfigured Exchange servers.

Ultimately, the decision to display a reference number on the subject line is entirely subjective.  If you find it useful then you can enable the feature.  Otherwise, there is no penalty to having your outgoing mail look more personal with a subject line that simply reads _"Re: That stuff you asked about."_

With reference numbers enabled, your recipients will receive replies with a subject like:

> Re: [Support #ABC-98765-123]: My printer seems to vaporize paper.

When reference numbers are disabled, the subject of your replies is provided in the traditional format:

> Re: My printer seems to vaporize paper.

## Creating buckets to organize work ##

_Buckets_ are flexible containers used by groups to organize their workload. Work can queue up in buckets, making it more efficient to handle similar issues at the same time (e.g. processing orders, issuing refunds, sending out beta information). Buckets are also useful to move piles of work out of the way if they shouldn't be handled immediately (e.g. newsletters, survey responses, feature requests). 

With department-themed groups your buckets might look like:

* **Billing:** receipts, refunds
* **Corporate:** execs, partners, biz-dev
* **Development:** bugs, feature requests, feedback
* **I.T.:** logs, alerts, abuse
* **Marketing:** surveys, newsletters
* **Sales:** leads
* **Support:** technical, account issues

Or if you had product-related groups you might do the following:

* **Product X:** Orders, Refunds, Support
* **Product Y:** Orders, Refunds, Support
* **Product Z:** Orders, Refunds, Support

Since buckets are often only concerned with a single shared characteristic they aren't always the best way to organize work.  They provide a simple building block for more complex workflows.

To add new buckets, select the _Buckets_ tab, and click the _Add_ button.  Enter the following information:

![Creating a new bucket.](images/group_manager/groups_bucket_add.png)

* **Name:** This is the name of the bucket.  For example: receipts, leads, newsletters, etc.

* **Send replies as email:** This specifies an email address to use as the sender in `From:`.  This will default to the sender address on the group's inbox.

* **Send replies as name:** This allows you to provide a personal name that will display next to the sender address when your recipient reads their email.  For example, `billing@example.com` may use the personal name _"Widget Factory - Billing Dept."_.  This gives the recipient an idea of who you are; especially if they don't recognize your email address.  This will default to sender name on the group's inbox.

* **Bucket email signature:** This defines the worker signature that will be used when mail is sent from this bucket.  You can use placeholders like the current worker's name or title, which will automatically fill in the appropriate values depending on who is sending the email.  This will default to the signature on the group's inbox.  You can also use conditional logic and modifiers in this template.  Refer to the section about Snippets for more information.

* **Workflow:** If you check this box then the bucket will be hidden from the _Workflow_ tab on the Mail page.

When you're finished, click the _Save Changes_ button.

### Configuring group buckets for workflow ###

For workflow purposes, buckets can be designated as _assignable_ (visible) or _non-assignable_ (hidden).

**Assignable buckets** contain work that is actionable -- that is, the work should be assigned and handled in a relatively quick timeframe in an effort to keep the bucket empty and the recipients happy.  Some examples of assignable work include:

* New orders
* Payment receipts
* Sales inquiries
* Requests for help
* Partnership requests

**Non-assignable buckets** contain work that doesn't need immediate attention and would otherwise detract from the goal of having empty buckets and quick handling of actionable work.  Non-assignable work is usually queued up until a future point and then handled in bulk.  Some examples of non-assignable work include:

* Automatically quarantined spam
* Bounces (return to sender)
* Logged events sent to email
* Long-running promotions that will be processed at a future date
* Vendor newsletters

Given these definitions, you should hide your spam quarantine buckets from the Workflow view, making it easier to focus on actionable work.

### Organizing group buckets ###

A group's inbox is always the top bucket in the list.  You can drag custom buckets into a new desired order.  It's a good idea to move the default _Spam_ bucket to the bottom of the list so it's out of the way.

## Configuring the group Virtual Attendant ##

### Example Behavior ###

#### Sending an automated message to new conversations ####

#### Sending an automated message to closed conversations ####

#### Assigning watchers to new conversations ####

#### Quarantining incoming spam ####

## Changing the group roster ##

![Changing the group roster.](images/group_manager/group_members.png)

As a group manager you can add or remove members.  You can also promote members or demote managers.

## Tracking group-specific custom fields ##

