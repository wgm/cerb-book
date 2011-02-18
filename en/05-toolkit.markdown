
# Toolkit #

## The "Toolkit" mentality ##

Over the past 9 years we've built up a library of reusable web-based tools that can be applied to a wide variety of business requirements.  When we think about solving new problems or implementing interesting feedback, we think in terms of these tools that we have at our disposal.  The Cerb5 project has been strangely fortunate to spend a decade refining a set of business challenges that are essentially unsolvable; our improvements over time can leverage technology to make you faster, more efficient, and more informed -- but there isn't going to be a point in the future where anyone can declare that they've solved online collaboration once and for all.  Ideally, your company will continue to grow for countless years to come, and information will continue to grow at an exponential rate, while becoming increasingly interconnected.  You're always going to be looking for new tools to mine that data to better understand the needs of your community, and to improve your efforts.  There will always be room for improvement.

Cerb5 is a repository of your community contacts, a communications crossroads, and a storehouse for your past collaborative energies and accumulated team knowledge.  This makes it an ideal place for us to provide you with a platform for ongoing development and innovation.  There are more interesting things to work on than we could hope to accomplish in a hundred careers.  Recognizing this, we've focused our energy on building open source tools that allow any company to spend their valuable time directly attacking the problems that they find interesting and important, rather than reinventing the web-based wheel.  Interesting approaches can be shared with others who can help further improve them.

As a toolkit, it is not our intention to quixotically try and replace all the other applications that you currently use with a single package from a single vendor.  There are thousands of projects out there striving to be the best mailing list software, the best billing suite, the best social network, the best community forums, the best project management software, and so on.  The _"best"_ in every situation is relative to the needs of particular groups of users.  It is impossible for a single application to make everyone happy.  You should benefit from the energy of developers who are running projects that are deeply invested in improving the challenges that you face -- but you also don't want to end up managing fifteen different copies of your customer information in isolated islands of software that aren't interested in talking to each other.

If each application has to integrate with all the others, then you're going to spend an incredible amount of time and money tying lose ends together.  Your team members will have a dozen browser windows open, with a dozen tabs in each one, trying to navigate the confusion.  What you need is a piece of infrastructure in the middle that synchronizes information: what you're hearing on social networks, what customers owe in invoices, what the last thing said in email was, what each member of your team needs to work on, and what they need to know.  This infrastructure doesn't have to be your billing system, your forums, and your issue tracker -- it just needs to be very efficient at moving information around, forming relationships within and between data, and providing a single location for your team to monitor and be notified about everything else.

## Features & Plugins ##

In many web-based applications, third-party extensions are an afterthought that have limited support for changing existing behavior or adding new functionality.  Developers provide _hooks_ that allow custom code to run when a particular _event_ happens, but this code doesn't have access to the same tools, services, and conveniences as the _core_ application.  This ties the hands of plugin developers and requires them to recreate solutions for problems that have already been solved in other places throughout the project.

When architecting Devblocks as the web development platform for Cerb5 (and all our future web projects), we made **plugins** the primary building block for the entire application.  This means that every time we add new functionality and capabilities, we're creating new **extension points** and **event points** that provide the same tools, services, and conveniences to plugin developers.  We call our built-in functionality **features** to distinguish them from third-party add-ons, but these features are just a collection of plugins.

Our concept of plugins goes far beyond the ability to just add new interface elements like pages, tabs, and buttons; or providing a way to add custom behavior on built-in events.

All of the content for a plugin is in a single directory.  To install a plugin you simply copy its directory to `/storage/plugins`, and to uninstall a plugin you simply delete that directory.  A plugin should never require you to make changes to the rest of the project.

Plugins are only invoked when they are required, keeping your helpdesk fast and efficient even with a large number of plugins installed.  Plugins that are disabled become invisible to the application and have absolutely no impact on performance.

Devblocks itself is a plugin, and the essence of Cerb5 is a _"core"_ plugin that provides all the common functionality for other plugins to share and build with.  These are the only two plugins that are required and cannot be disabled.

Below is a list of the capabilities of Devblocks plugins.

### Manifests ###

Plugins provide an [XML](http://en.wikipedia.org/wiki/XML) _"manifest"_ that tells the platform what contributions it is making to the application.  Devblocks is designed to load and execute code to respond to requests in as few steps as possible.  Manifests are cached in memory, so a quick lookup can tell the platform which plugins are needed to respond to a particular request without having to load a single plugin in advance.

This strategy makes the platform incredibly efficient, because it reduces the number of files accessed, the peak amount of memory utilized, and the amount of time spent compiling and executing unnecessary code.

### Resources ###

A plugin contributes new resources -- images, scripts, stylesheets -- as a bundle.  These resources are not requested directly from the filesystem, but through a **resource proxy** that controls requests for access in conjunction with their plugin identifier (_"ID"_).  When a browser is directed to make a request for resource -- like an image -- it uses the `/resources/` controller in the URL.  All resources must reside in a `resources` subdirectory in the plugin's home directory to be accessible.

### Patches for change management ###

Plugins provide incremental patches for automatic upgrades between any two versions.  These patches can create or change the database schema, migrate data, perform maintenance tasks, or run custom code.  Patches are automatically managed by Devblocks to ensure that they run in the proper order and only when they are needed.

### Dependencies ###

Plugins can specify other plugins as **dependencies**.  This means that a plugin should not be loaded unless other plugins are also loaded (i.e. it _depends_ on them).  Devblocks will analyze these dependencies to ensure plugins are loaded or updated in the proper order.  Dependencies are useful when a plugin requires the use of functionality from another plugin and cannot function without it.  Everything depends on `cerberusweb.core`, which itself depends on `devblocks.core`.  This dependencies doesn't need to be specified, as it is declared for you automatically.

### Extensions ###

### Extension points ###

### Event listeners ###

### Event points ###

### Permissions ###

### Templates ###

A plugin provides all of its own templates for rendering interface elements.  It can also utilize common templates (known as _"internal"_) provided by Cerb5's _core_ plugin.  For example, if a plugin needs to display a search filter for a text field or a date, it can simply use the existing template.  There are many _internal_ templates: search filters, comments, displaying relationships between objects, etc.

### User-editable templates ###

A plugin can also specify if any of its templates should be available for user editing.  This allows you to make changes to a template. Devblocks will always maintain the original version of a template.  This is primarily used in situations where a plugin will be in use in disparate places, such as plugins that contribute to community portals like the Support Center.  You can customize these templates to be consistent with your brand on each community portal, without ever compromising the integrity (and sacrificing the upgradability) of your project files.

### Classloading ###

A plugin can register any of its classes in the global classloader.  Plugins aren't always loaded into memory, so classloading provides a hint to the platform telling it where the definition of particular objects comes from.

### Routing ###

### Translations ###


## Objects ##

(Objects are...)

(Records/Kinds)

## Custom Fields ##

## Tabs ##

## Views ##

## Filters ##

## Presets ##

## "Peek" Popups ##

## Workspaces ##

## Comments ##

## Choosers ##

## Links ##

## Notifications ##

## Explore Mode ##

## Feed Reader ##

## Bulk Update ##

## Decision Trees ##
(Chapter on this)



