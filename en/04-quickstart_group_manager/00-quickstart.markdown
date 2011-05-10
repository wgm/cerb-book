\newpage

# Quick Start Guide for Group Managers #

![Navigating to the groups page.](images/checklist/menu_groups.png)

1. Click _groups_ in the top right.
2. Click the name of any group to manage it.

This page shows any worker the groups they are a member of, but only managers can make changes to a group.  Normally, if a worker isn't a member of a group then it won't show up in this list.  However, as an administrator you have access to every group as if you were a manager, even if you aren't on the roster.

![Managing a group.](images/checklist/groups_manage.png)

There are several tabs on group management:

* **Mail:** Configures the group's mail preferences.
* **Buckets:** Manages the group's organizational buckets.
* **Virtual Attendant:** Configures the group's automated Virtual Attendant for various behavior; for example, routing new mail into buckets or assigning workers to relevant conversations.
* **Members:** Manages the group's roster.
* **Custom Fields:** Each group can track their own custom fields on new tickets. For example, the Sales department may want to track the source (Google, website, ad) of leads while Support is interested in tracking the category of requests (FAQ, feature request, etc). These custom fields can be used to generate reports.

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

Since buckets are often only concerned with a single shared characteristic they aren't always the best way to organize work, but they're a useful building block for more complex workflows.

Let's add a few new buckets to the _Sales_ group:

* Manage the _Sales_ group from the _groups_ link in the top right.
* Click the _Buckets_ tab.
* Click the _Add_ button.

<!-- [TODO] Continue -->

* In the _Add Buckets_ box, enter the names of the buckets you'd like to create.  For example:
	* Leads
	* Invoices
	* Orders
	
	![Creating new buckets for the Sales group.](images/checklist/groups_buckets.png)
	
* Click the _Save Changes_ button.

You should now see your new buckets in the _Workflow_ list.  It's a good idea to move the default _Spam_ bucket to the bottom of the list so it's out of the way.  You can do this quickly by setting its order to `99` (or anything higher than the number in the last row).

For workflow purposes, buckets can be designated as _assignable_ or _non-assignable_.

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

Given these definitions, you can also unmark the checkbox in the _Assignable_ column next to the _Spam_ bucket.  This will hide your spam buckets from the _Workflow_ view, making it easier to focus on actionable work.

![Customizing group workflow.](images/checklist/groups_workflow.png)
