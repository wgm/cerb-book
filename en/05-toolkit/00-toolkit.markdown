\newpage

# Toolkit #

## The "Toolkit" mentality ##

The Cerb5 development team has been ironically fortunate in choosing our area of expertise.  When we started the project in 2002, we possessed the naive belief that a web-based shared inbox _"couldn't take more than a couple of weeks to build"_. We also couldn't have imagined where that simple idea would take us.  Nearly 10 years later, we've been pleasantly surprised.  Online collaboration is a business challenge that is inherently unsolvable.  We've had more than 20,000 organizations express interest in what we're working on -- including a countless number of companies, institutions, brands, and causes that we have always highly respected.  For every task that we complete, the community spawns a dozen more great ideas.  Every time we make a major improvement in some part of the application, we begin to see how that same approach could solve a wide variety of other feature requests or development obstacles.

Over the past 9 years, we've used this cumulative experience to refine a library of reusable web-based tools that can be applied to an increasingly wide variety of business requirements.  When you hear us talk about **Devblocks**, we're talking about these tools and the philosophy that guided their development.  As we're designing improvements to Cerb5 from interesting feedback, we assemble these tools like well-crafted _blocks_ that can be combined in new and interesting ways.  Every compelling idea that has come up so far can be reduced to a particular set of these blocks.  Any time we sense there's a missing piece, it leads us toward a new block that we can add to the toolkit.  It has become part of our DNA -- to the point that when we look at other applications, we start to instinctively break them down into the same compositional blocks.

Our ongoing improvements can leverage technology to make you faster, more efficient, and better informed -- but there won't be a point in the future where we can put down the tools and declare that we've _solved_ online collaboration once and for all.  In a perfect world, your team will continue to grow for countless years to come, and the information you gather will continue to expand at an exponential rate and become increasingly interconnected.  There will always be room for improvement.

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

A plugin can contribute new resources -- images, scripts, stylesheets -- as a bundle.  These resources are not requested directly from the filesystem, but through a **resource proxy** that controls requests for access in conjunction with their plugin identifier (_"ID"_).  When a browser is directed to make a request for resource -- like an image -- it uses the `/resources/` controller in the URL.  All resources must reside in a `resources` subdirectory in the plugin's home directory to be accessible.

### Patches for change management ###

A plugin can provide incremental **patches** for automatic upgrades between any two versions.  These patches can create or change the database schema, migrate data, perform maintenance tasks, or run custom code.  Patches are automatically managed by Devblocks to ensure that they run in the proper order and only when they are needed.

### Dependencies ###

A plugin can specify other plugins as **dependencies**.  This means that a plugin should not be loaded unless other plugins are also loaded (i.e. it _depends_ on them).  Devblocks will analyze these dependencies to ensure plugins are loaded or updated in the proper order.  Dependencies are useful when a plugin requires the use of functionality from another plugin and cannot function without it.  Everything depends on `cerberusweb.core`, which itself depends on `devblocks.core`.  These two dependencies don't need to be specified in custom plugins, since they are declared for you automatically.

### Extensions ###

The primary purpose of plugins is to contribute new functionality, collectively referred to as **extensions**. Extensions are registered on **extension points** to notify the platform of their existence.  With this strategy, plugins become a _native_ part of the application, with the same _rights_ and _aesthetic_ as built-in features.

### Extension points ###

A plugin can also declare new extension points that may be extended by other plugins.

### Event listeners ###

A plugin can register **listeners** that _listen_ to **event points** and execute custom functionality when particular **events** occur.

### Event points ###

A plugin can declare new events that may be observed by other plugins.

### Permissions ###

A plugin can contribute its own _access control lists_ (**ACL** [^wikipedia-acl]) to the global list of worker permissions.  This allows seamless administration of the rights workers have when using plugins. 

[^wikipedia-acl]: Wikipedia: _Access Control List_ <<http://en.wikipedia.org/wiki/Access_control_list>>

### Translations ###

A plugin can contribute all of its interface text to the application's translation system, which then becomes translatable with the Translation Editor just like the rest of the text in the application.  No distinction is made between the text coming from plugins and that of built-in features, so popular plugins will become translated into new languages as a natural byproduct of their use.  Plugins should also make judicious use of the shared _internal_ translations for common words and phrases (e.g. _save changes_, _delete_, _worker_, _email_, etc), which reduces duplicate text and eases the work of translators.

### Templates ###

A plugin can provide all of its own templates for rendering interface elements.  It can also utilize common templates (known as _"internal"_) provided by Cerb5's _core_ plugin.  For example, if a plugin needs to display a search filter for a text field or a date, it can simply use the existing template.  There are many _internal_ templates: search filters, comments, displaying relationships between objects, etc.

### User-editable templates ###

A plugin can also specify if any of its templates should be available for user editing.  This allows you to make changes to a template. Devblocks will always maintain the original version of a template.  This is primarily used in situations where a plugin will be in use in disparate places, such as plugins that contribute to community portals like the Support Center.  You can customize these templates to be consistent with your brand on each community portal, without ever compromising the integrity (and sacrificing the upgradability) of your project files.

### Classloading ###

A plugin can register any of its classes in the global classloader.  Plugins aren't always loaded into memory, so classloading provides a hint to the platform telling it where the definition of particular objects comes from.

### Routing ###

A plugin can take advantage of the platform's built-in request **routing** by registering a unique _uniform resource identifier_ (**URI** [^wikipedia-uri]) and associating it with one of its **controllers**.  For example, the `files` URI routes a request to a built-in controller that authenticates requesters before serving file attachments from storage.

[^wikipedia-uri]: Wikipedia: _Uniform Resource Identifier_ <<http://en.wikipedia.org/wiki/Uniform_Resource_Identifier>>

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



