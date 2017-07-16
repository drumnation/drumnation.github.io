---
layout: post
title: "Octopost Bundle Install Errors (Solved)"
date: 2017-03-14 16:55:04 -0400
comments: true
categories: Flatiron School, Octopress, Bundle
---
## An error occurred while installing yajl-ruby (1.2.1), and Bundler cannot continue'

I began the lab instructing us on how to build our own blog using Octopress to teach us how to use git more confidently since Octopress is completely hosted on github using git. I followed the instructions and cloned Octopress to my dave-blog directory, creating an _octopress_ directory.

![](/images/octopress-bundle-install-errors/image01.png)

The first time I ran bundle [Octopress](http://octopress.org/) I received an error from the [bundler](http://bundler.io/) gem. I was able to search for the latest bundler gem and install it on it's own using sudo for root. This solved the problem running the bundler, but as you can see below I have now hit a snag where the bundler is having trouble with one of the gems.

It looks like a similar issue where I have to figure out how to make a gem install properly on it's own and then run the bundler again.

![](/images/octopress-bundle-install-errors/image02.png)

**Note:** There are a bunch more errors related to building the gem in the middle here that I excluded for brevity...

![](/images/octopress-bundle-install-errors/image17.png)

The two chunks of this error that jumped out to me are this one at the beginning of the failure:

> Gem::Ext::BuildError: ERROR: Failed to build gem native extension.  
>   
> current directory:  
>   
> /Users/davidmieloch/.rvm/rubies/ruby-2.4.0/lib/ruby/gems/2.4.0/gems/yajl-ruby-1.2.1/ext/yajl  
>   
> /Users/davidmieloch/.rvm/rubies/ruby-2.4.0/bin/ruby -r  
>   
> ./siteconf20170314-81048-9p4mus.rb extconf.rb  
>   
> creating Makefile

And this chunk at the end of the error:

> An error occurred while installing yajl-ruby (1.2.1), and Bundler cannot continue.  
>   
> Make sure that `gem install yajl-ruby -v '1.2.1'` succeeds before bundling.

In my efforts to discover why this gem was not working I tried doing a search for the package on rubygems

![](/images/octopress-bundle-install-errors/image13.png)

It appears that it needs version 1.2.1, but the latest version is 1.3\. I copied the install code to the clipboard and pasted to terminal to see if this would install properly.

![](/images/octopress-bundle-install-errors/image00.png)

I tried installing it manually with sudo and it installed successfully, but when I ran the bundler again I got the same error.

I thought maybe I'd try running the bundler as sudo. As you can see above at the very top I got a warning message for doing that.

**Don't run Bundler as root.**

Bundler can ask for sudo if it is needed, and installing your bundle as root will break this application for all non-root users on this machine.

![](/images/octopress-bundle-install-errors/image05.png)

1.30 installed perfectly, let's try running the bundler again.

![](/images/octopress-bundle-install-errors/image14.png)

Same error. Now I'm thinking I need to figure out how to get that exact version of this gem installed. It gives instructions in the error code.

![](/images/octopress-bundle-install-errors/image08.png) ![](/images/octopress-bundle-install-errors/image03.png)

It says the gem files will remain installed for my inspection at the end of the error code. Maybe there's an installation conflict of some kind? In other words it's time for google.

![](/images/octopress-bundle-install-errors/image07.png)

This person is having the exact same problem I am. The only helpful thing in the thread was a link to another thread where it was being discussed.

![](/images/octopress-bundle-install-errors/image04.png)

So it looks like there is a conflict between yajl-ruby v 1.2.1 and ruby 2.4.

![](/images/octopress-bundle-install-errors/image16.png) ![](/images/octopress-bundle-install-errors/image12.png)

I'm running ruby 2.4\. In the thread they said that [yajl-ruby 1.30](https://github.com/brianmario/yajl-ruby) had been patched to work properly with 2.4, but this bundle installer seems to only want the incompatible 1.2.1 version. There's no way I'm going to downgrade my Ruby for this gem so I need to find a way to make the bundle accept 1.30 instead.

**EDIT**:: It seems I _wasn't_ supposed to install Ruby 2.4 but Ruby 2.3... which is why no one else in my class was having this issue.

![](/images/octopress-bundle-install-errors/image11.png)

So I decided that I would rollback after all. I already had the latest version of [ruby version manager](https://rvm.io/) installed so when I followed Ian's instructions I was able to rollback fairly easily.

![](/images/octopress-bundle-install-errors/image06.png)

For science and whatnot the first time through I used sudo to install the bundler gem. When I RAN the bundler install it smacked my hand for being root.

I usually default to sudo because 90% of the time it seems like the stuff I'm trying to install fails without root. This time was different and it installed without root. This could possibly be one reason for the error.

![](/images/octopress-bundle-install-errors/image09.png)

It's much more likely it was the version 2.3 rollback of ruby that solved my problem, but after I finished installing the bundler gem I went back to the Octopost directory and ran the bundle install command.

![](/images/octopress-bundle-install-errors/image01.png) 

![](/images/octopress-bundle-install-errors/image10.png) 

![](/images/octopress-bundle-install-errors/image15.png)

I agree Ian. It's very nice when programs work. THANKS!