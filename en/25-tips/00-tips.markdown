\newpage

# Tips and Tricks #

## Entering dates ##

Cerb5 accepts date entry in multiple formats:

*	-2 days
*	+1 year, 3 months
*	April 27 2001
*	9 Jan 2002 8am
*	yesterday / today / now / tomorrow
*	next Monday
*	last Thursday 5pm
*	2011-01-01 20:30:15

If you don't specify a time then the default is 00:00:00 (midnight at the start of the day).  Dates are relative to your current timezone (defined in My Account), but they are always stored in the database as UTC/GMT.

When you're doing a search with dates you should be sure to enter an explicit end time.  The search _"from today to tomorrow"_ will include anything that happened today (from 00:00:00 to 23:59:59).  However, the search _"from today to today"_ will likely return zero results because it's looking for activity within the first second of today (from 00:00:00 to 00:00:00 inclusive).

It is often useful to use relative dates in mail filters and business rules, because setting a date (e.g. a due date) of _"+2 hours"_ will always be 2 hours in the future regardless of the current time.