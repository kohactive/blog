WordPress is a big part of our front-end development flow here at kohactive, but its base tooling doesn't suit us super well. With the help of some solid open source tools, we've been able to mold a modern workflow in-sync with other technologies we use.

##Startup
We start with our [sage-but-better](https://github.com/kohactive/sage-but-better) repo as a boilerplate, which contains all the files needed for a complete WordPress install. This allows us to keep a central starting point for managing core version updates, plugin updates, etc. The theme is really [Sage](https://roots.io/sage/) with a few minor updates. Sage sets up a couple things that we want:

* A boilerplate theme setup
* NPM & Bower
* Gulp for compiling Sass & JS(+ CoffeeScript currently, but that will probably go away)

The "but-better" part includes a couple other things we want:

* Shell scripts for installation, development, & deployment
* wp-config setup for local & Pantheon(our WordPress host of choice) environments

##Plugins
The repo also contains some plugins that we use in just about every WordPress project we do:

* [Advanced Custom Fields Pro](https://www.advancedcustomfields.com/pro/): I don't know where I'd be without ACF. Probably nowhere near WordPress. ACF is free, but the paid version is totally worth it. $76(USD) for unlimited use(which is extremely underpriced, imo). We basically build admin pages with this plugin. It makes the admin side of things much more user friendly and allows us to keep restrictions in place to fend off user error.
* [WP Migrate DB Pro](https://deliciousbrains.com/wp-migrate-db-pro/): Migrate DB Pro is one of those things I didn't bother with for a while(if only they'd consider sponsoring a podcast or something). Dumping and importing SQL files is easy enough, etc...until you use this plugin. Don't deal with dumps. Don't deal with find-and-replace. Migrate DB Pro is another plugin that is far more worthy than its price(especially if you catch it on sale) when dealing with multiple environments & devs.
* [Contact Form 7](http://contactform7.com/): CF7 is pretty standard and does a lot of the junk you probably don't want to deal with for forms. It can feel a little archaic to set up and use on the back-end, but it's easily customizable and there's a respose out there for just about every problem you'll run into.
* [Yoast SEO](https://yoast.com/wordpress/plugins/seo/): Not much to say here. This plugin does a pretty good job of "just working" and giving some standard SEO out of the box. There's a lot more to it if you want to dig into it, but it'll get you a sitemap and some decent SEO with little poking & prodding.

##Hosting
We've gone through a few different hosting options and landed wholly on [Pantheon](https://pantheon.io/). It does everything you'd expect from a WordPress hosting service, plus a couple other things that are quite nice.

* wp-config Setup. The wp-config setup is hassle-free. The Pantheon wp-config file uses ENV variables, and each dev has a wp-config-local that contains their connection info. Done.
* Git Push. This one's not super unique, but it works better/more easily than other services we've used. Basically there's a Git repo hosted on Pantheon and it automatically updates the site when pushed to.
* Dev/Test/Live Environments: Each site created in Pantheon starts in the Dev environment. Dev code syncs with the master branch of the Pantheon-hosted Git repo. This tripped me up a bit initially when thinking that branches represent the different environments(which they do in my next point, but not here). This creates a [specific workflow](https://pantheon.io/docs/pantheon-workflow/) that actually ends up being quite nice. To that same point, Pantheon makes it really easy to move uploads & databases down to keep things in sync.
* Multidev: Want feature review? Of course you do. Don't merge it in just to get a review, get it its own url. Create a new Multidev environment, push up the code to the remote repo branch of the same name, there ya go.
* Downside: Pantheon doesn't really do multisite(they do, but only on an enterprise level). Can't say I blame them, but if needed, head over to [WP Engine](https://wpengine.com/).

All in all, it's a good setup. Is it perfect? Not quite. I need to spend some time playing with [Bedrock](https://roots.io/bedrock/) and/or finding a good way to manage plugins a little better(committing them to the repo makes me sad). I'm also not crazy about [force-committing the dist/ directory](https://github.com/kohactive/sage-but-better/blob/master/deploy.sh) to deploy to the Pantheon remote. It stays out of the review/"main" repo per-se, but it could be better.
