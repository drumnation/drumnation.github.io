---
layout: post
title: "Octopost Bundle Install Errors (Solved)"
date: 2017-03-14 16:55:04 -0400
comments: true
categories: Flatiron School, Octopress, Bundle
---
<h2>
  An error occurred while installing yajl-ruby (1.2.1), and Bundler cannot continue'
</h2>
<p>
  I began the lab instructing us on how to build our own blog using
  Octopress to teach us how to use git more confidently since Octopress is
  completely hosted on github using git. I followed the instructions and cloned
  Octopress to my dave-blog directory, creating an <i>octopress</i> directory.
</p>
<img src="/images/octopress-bundle-install-errors/image01.png">
<p>
  The first time I ran bundle <a href="http://octopress.org/" target="_blank" >Octopress</a>
  I received an error from the <a href="http://bundler.io/" target="_blank">bundler</a> gem.
  I was able to search for the latest bundler gem and install it on it's own
  using sudo for root. This solved the problem running the bundler, but as you
  can see below I have now hit a snag where the bundler is having trouble with
  one of the gems.
</p>
<p>
  It looks like a similar issue where I have to figure out how to make a gem
  install properly on it's own and then run the bundler again.
</p>
<img src="/images/octopress-bundle-install-errors/image02.png">
<p>
  <strong>Note:</strong> There are a bunch more errors related to building the gem in the
  middle here that I excluded for brevity...
</p>
<img src="/images/octopress-bundle-install-errors/image17.png" >
<p>
  The two chunks of this error that jumped out to me are this one at the beginning of the failure:
</p>
<blockquote>
  Gem::Ext::BuildError: ERROR: Failed to build gem native extension. <br />
  <br />
  current directory:<br />
  <br />
  /Users/davidmieloch/.rvm/rubies/ruby-2.4.0/lib/ruby/gems/2.4.0/gems/yajl-ruby-1.2.1/ext/yajl <br />
  <br />
  /Users/davidmieloch/.rvm/rubies/ruby-2.4.0/bin/ruby -r <br />
  <br />
  ./siteconf20170314-81048-9p4mus.rb extconf.rb <br />
  <br />
  creating Makefile
</blockquote>
<p>
  And this chunk at the end of the error:
</p>
<blockquote>
  An error occurred while installing yajl-ruby (1.2.1), and Bundler cannot continue.<br />
  <br />
  Make sure that `gem install yajl-ruby -v &#39;1.2.1&#39;` succeeds before bundling.
</blockquote>
<p>
  In my efforts to discover why this gem was not working I tried doing a search for the package on rubygems
</p>
<img src="/images/octopress-bundle-install-errors/image13.png">
<p>It appears that it needs version 1.2.1, but the latest version is 1.3. I copied the install code to the clipboard and pasted to terminal to see if this would install properly.</p>
<img src="/images/octopress-bundle-install-errors/image00.png" class="left">
<p>I tried installing it manually with sudo and it installed successfully, but when I ran the bundler again I got the same error.</p>
<p>I thought maybe I'd try running the bundler as sudo. As you can see above at the very top I got a warning message for doing that.</p>
<p><strong>Don't run Bundler as root.</strong></p>
<p>
  Bundler can ask for sudo if it is needed, and
  installing your bundle as root will break this application for all non-root users
  on this machine.
</p>
<img src="/images/octopress-bundle-install-errors/image05.png">
<p>1.30 installed perfectly, let's try running the bundler again.</p>
<img src="/images/octopress-bundle-install-errors/image14.png">
<p>
  Same error. Now I'm thinking I need to figure out how to get that exact version
  of this gem installed. It gives instructions in the error code.
</p>
<img src="/images/octopress-bundle-install-errors/image08.png">
<img src="/images/octopress-bundle-install-errors/image03.png">
<p>It says the gem files will remain installed for my inspection at the end of the error code. Maybe there's an installation conflict of some kind? In other words it's time for google.</p>
<img src="/images/octopress-bundle-install-errors/image07.png">
<p>This person is having the exact same problem I am. The only helpful thing in the thread was a link to another thread where it was being discussed.</p>
<img src="/images/octopress-bundle-install-errors/image04.png">
<p>So it looks like there is a conflict between yajl-ruby v 1.2.1 and ruby 2.4.</p>
<img src="/images/octopress-bundle-install-errors/image16.png">
<img src="/images/octopress-bundle-install-errors/image12.png">
<p>
  I'm running ruby 2.4. In the thread they said that <a href="https://github.com/brianmario/yajl-ruby" target="_blank">yajl-ruby 1.30</a> had been patched to work properly with 2.4, but this bundle installer seems to only want the incompatible 1.2.1 version. There's no way I'm going to downgrade my Ruby for this gem so I need to find a way to make the bundle accept 1.30 instead.
</p>
<p>
  <strong>EDIT</strong>:: It seems I <i>wasn't</i> supposed to install Ruby 2.4 but Ruby 2.3... which is why no one else in my class was having this issue.
</p>
<img src="/images/octopress-bundle-install-errors/image11.png">
<p>
  So I decided that I would rollback after all. I already had the latest version of <a href="https://rvm.io/" target="_blank">ruby version manager</a> installed so when I followed Ian's instructions I was able to rollback fairly easily.
</p>
<img src="/images/octopress-bundle-install-errors/image06.png">
<p>
  For science and whatnot the first time through I used sudo to install the bundler gem. When I RAN the bundler install it smacked my hand for being root.
</p>
<p>I usually default to sudo because 90% of the time it seems like the stuff I'm trying to install fails without root. This time was different and it installed without root. This could possibly be one reason for the error.</p>
<img src="/images/octopress-bundle-install-errors/image09.png">
<p>It's much more likely it was the version 2.3 rollback of ruby that solved my problem, but after I finished installing the bundler gem I went back to the Octopost directory and ran the bundle install command.</p>
<img src="/images/octopress-bundle-install-errors/image01.png">
<img src="/images/octopress-bundle-install-errors/image10.png">
<img src="/images/octopress-bundle-install-errors/image15.png">
<p>I agree Ian. It's very nice when programs work. THANKS!</p>
