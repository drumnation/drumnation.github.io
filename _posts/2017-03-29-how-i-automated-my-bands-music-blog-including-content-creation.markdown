---
layout: post
title: "How I Automated My Band's Music Blog Including Content Creation"
date: 2017-03-29 21:45:02 -0400
---
## INTRO

![](/images/programatic-content-creation/62-band.jpg)

From 2012 to 2016 I played drums in the NYC instrumental progressive music group "Oneironaught." The name is based on the word oneironaut—an explorer of lucid dream worlds—which came from a previous metal band I had started with a friend, and seemed to describe our rather trippy style of metal well.

One of the hallmarks of being in a band is the struggle to balance the artistic side with the business side. In my case, despite having studied music in college, I always end up being the business guy. This means finding ways to promote our music, networking, and trying to push the whole project towards progress... the golden mecca being that place where band income outweighs band expenses (i.e. profit).

## FULL STACK/INTEGRATED DIGITAL MARKETING

In 2013, I began expanding my capabilities as a digital marketing manager for Saturn Business Systems. As an IBM partner located in midtown, a 10-minute walk from IBM headquarters, it felt like we were part of IBM, and we had suited IBM-ers strolling in and out daily.

![](/images/programatic-content-creation/63-ibm.jpg)

I began studying NYU's Digital Marketing master's curriculum on my own. I was also being sent to IBM headquarters for digital marketing trainings. I gobbled up dozens of courses on Udemy around Facebook marketing, Google Adwords, Music Marketing, etc...

![](/images/programatic-content-creation/61-fdm.png)

#### SANDBOX

Adding to my prior knowledge of web development and storytelling, I began implementing landing pages for lead generation, content marketing, sophisticated analytics tools, a/b testing, PPC ads, email marketing, and methods of managing and growing social media accounts. During this time, I began using the band as a sandbox for testing wacky marketing ideas that I didn't want to try first on my company.

![](/images/programatic-content-creation/58-ibminsight.jpg)

## WHAT INSPIRED ME

IBM and Arrow Electronics [hired a consultant](https://www.linkedin.com/in/valuecreationprofessional/) to come and help our 30-year-old company [shift toward a newer business model](http://www.cpinnovators.com/)m, which would shift us from a sales led organization to one led by digital marketing. The strategy was centered around recruiting data governance experts to promote in our new company blog, gaining mindshare and demonstrating thought leadership through association.

#### HUBSPOT

In 2014, We purchased [Hubspot](https://www.hubspot.com/), which is a Marketing Automation suite that consists of a CMS, Email Marketing System, Social Media Management, Landing Pages, and a Sales Database (CRM) all loaded into one. Data from any of those modules is shared with all the other modules for better analytics and the ability to score each individual customer's interaction with the site. While I was learning and setting it up I made note of all the features and was excited to play with them all.

It wasn't long before I wanted something like Hubspot to run experiments promoting my band but it was too expensive.

## THE EXPERIMENT BUILDS MOMENTUM

This whole time, I had been running my band's website on Wordpress and had begun to experiment with content marketing to promote my band. Essentially, where I strayed from the norm in music marketing, was that I was writing articles about OTHER bands on OUR website in order to get search and social traffic. And it was working.

Despite the fact that it was working, it was an enormous amount of work, and I was burning out fast. Keeping a steady stream of blog posts and social media content going had me hiring bloggers and staying up all night working, on top of my day job. I begged my bandmates to help, and while they eventually complied, reluctantly, it was too late. I was burnt out.

#### TIME TO AUTOMATE

I said to myself _The only way I can keep doing this is if I find a way to automate 100 percent of this because I can't keep this up consistently alone._

I started building a new site that aimed to replicate the functionality of Hubspot using Wordpress plugins. My only rule was that whatever I was building would not be considered complete if I had to do anything at all to maintain it. It would need to allow me to be able to disappear for 3 months with zero inconsistency in posting frequency or timing. This was when I discovered a service called IFTTT, and instantly fell in love with the possibilities.

## IFTTT a.k.a IF THIS THEN THAT

![](/images/programatic-content-creation/37-ifttt.png)

If you’re unfamiliar, IFTTT is a service that lets you automate all kinds of things. When I first discovered that use it with the Spotify API, I ran all kinds of experiments, like using it to tweet songs anytime I saved them to my library and so forth.

Then I noticed you could connect to the Reddit API. I started connecting the dots…it hit me that I could use Reddit as a curation tool, if you will, to automate selecting songs for my Spotify playlist, which was a huge breakthrough. Since posts including songs are upvoted and downvoted on Reddit, in real time, by an incredibly engaged community, the songs that make it to the top of any Subreddit genre would be even more relevant and fresh choices for a playlist than if I were curating one by myself. And since it’s not a human like me, and doesn’t need sleep, that means the volume of high-quality song recommendations I could extract would be exponential. Talk about harnessing UGC (user generated content).

#### Here's what the /r/progmetal Subreddit looks like:

![](/images/programatic-content-creation/55-reddit.png)

Before long, I had figured out that I could use Reddit to create posts on my Wordpress site using recipes that made use of the Spotify API. The margin of error for posting something off-genre was very low since only a portion of the top posts titles would actually be recognized as songs by Spotify. Occasionally, a song title is upvoted as a joke and the site ends up posting 80’s dance music with the hashtag #deathmetal. The first few times this happened I was mortified… after a while my followers just started assuming it was a joke and commented about how funny it was that I was posting classical metal with #doom hash tags on it.

![](/images/programatic-content-creation/33-ifttt.png) ![](/images/programatic-content-creation/34-ifttt.png)

#### Here's what the Reddit recipe for one of my fetches looks like:

![](/images/programatic-content-creation/35-ifttt.png)

#### Here's what my Spotify to Wordpress recipe looks like along with the HTML template and data fields from the API I'm using:

Notice how I was able to insert the artist and album data to the html's alt tag. The alt tag is what allows you to tell search engines what your images are. It's a lot of work to make sure everything is labeled correctly so your site is seen as relevant. This simple dynamic insertion automated the SEO tagging process for thousands of images on my site.

![](/images/programatic-content-creation/36-ifttt.png)

## WORDPRESS

Once the posts hit my website they have left the land of IFTTT and I needed to figure out a way to complete the remaining 50 percent of the automation by other means.

The posts would get sent to my site but if I set them to publish immediately then they would post erratically 5 at a time when IFTTT ran and then nothing and then 4.

I needed a way to spread the posting out evenly no matter how fast the new posts came in from IFTTT.

#### WHAT MY IFTTT GENERATED POST LOOKS LIKE IN WORDPRESS

![](/images/programatic-content-creation/39-wp-postcode.png)

#### Which generates pages that look like this:

![](/images/programatic-content-creation/42-songpage.png)

## CONTENT MARKETING & SEO

I did a bunch of experiments creating blog posts to generate targeted organic traffic to my site. With this particular example I did some keyword research into topics to write about and saw an opportunity for the **keyword "Bands like Tool."**

I know a lot about Tool so I assembled a **listicle style blog post** and optimized it for search.

[![](/images/programatic-content-creation/04-tool-blog.png)](http://oneironaught.com/9-bands-like-tool/)

## #1 ON GOOGLE

As a result you can see that I rank #1 on google for "Bands like Tool."

![](/images/programatic-content-creation/56-tool-frontgoog.png)

## ORGANIC WEBSITE TRAFFIC

I want to come back to the site analytics traffic overview, but check out how much of my traffic comes from that one article. Organic traffic is free and it's a very valuable thing to build because it continues to work indefinitely.

You can clearly tell when I launched this automated version of the website. I launched it around this time last year and have barely touched it since.

![](/images/programatic-content-creation/59-goog.png)

## LAYOUT

Above the fold I have this funky square and rectangle based design that always looks interesting because there is an endless supply of new images flowing to my website.

#### ABOVE THE FOLD

The home page displays access to these awesome image files making great use of Spotify's album art API:

![](/images/programatic-content-creation/01-home.png)

#### MIDDLE BODY

Under that I have an image carousel that displays the SEO-targeted blog posts so users get more in-depth, human-produced content, as well.

You can also see the right rail sidebar now. At the top is a newsletter sign-up module. Below that is a list of songs ranked by weekly view count (sort of like a “trending now” module).

![](/images/programatic-content-creation/02-home.png)

#### BODY BOTTOM

I decided I preferred the footer minimal

![](/images/programatic-content-creation/03-home.png) Now to dive deeper...

## WORDPRESS SITE POST QUEUE + POSTS ONCE AN HOUR

As you can see in the screenshot, I've already published 6,959 blog posts over the past year since I developed this new automated version of my site.

With all the content coming in at random intervals it needs to be scheduled properly or 24 posts could publish at the same time.

They needed to be programmatically scheduled so I wouldn't need to touch it.

![](/images/programatic-content-creation/38-wp-postcount.png)

## PROGRAMMATICALLY CREATE SOCIAL MEDIA POSTS + PUBLISH AT APPROPRIATE TIME INTERVALS

Here's how I'm taking care of this: I installed a plugin called auto-post-scheduler that would let me publish a post that was set to draft on a timed interval. I did some research and found that I could tell IFTTT to set all new posts to draft instead of publishing them immediately.

![](/images/programatic-content-creation/43-autopostsched.png)

It's appropriate to post at different frequencies that suit the style and use cases of different social networks.

My biggest following is on Twitter and since my account has a large international following, posting 24 times a day so posts hit newsfeeds in all the right time zones is necessary.

Posting that often, EVERY DAY, for one person is, well, almost impossible.

#### BUFFER

You can see the times I've chosen to queue the 24 posts each day using a social media scheduling service called BUFFER that I've connected to my site via a plugin.

![](/images/programatic-content-creation/52-buffertime.png)

#### GENERATES SOCIAL MEDIA POST WHEN WP-PUBLISHES

Whenever a post on the site publishes it generates a new social media post in the buffer queue for each social media network connected.

##### EACH SOCIAL MEDIA SITE HAS DIFFERENT RULES

You'll see below that I only post 4 times a day on Instagram while I'm posting 24 times a day on Twitter. That's cool.

Here's what the Instagram post scheduling looks like:

![](/images/programatic-content-creation/53-buffertimepin.png)

Here's our actual Instagram account. You'll notice how nice the album art translates into social media. It's beautiful and attracts the eye without me having to do anything.

![](/images/programatic-content-creation/60-insta.png)

I'm posting 24 times a day on Pinterest as well because it's totally fine to pin a whole bunch of stuff. Keep in mind all these links I'm posting lead straight back to my website.

I'm seeding the internet with links back to my site.

![](/images/programatic-content-creation/31-pinterest.png)

## PINTEREST STATS

Here are some stats from the Pinterest account. It's very new and I didn't invest much time into it.

I wrote and ran a custom iMacros script for several weeks to gain the initial following and then did nothing with it.

Regardless, with the following I have my **posts hit 22,463 pin newsfeeds** for free every month.

![](/images/programatic-content-creation/32-pt-stats.png)

#### GOOGLE+

I got in on botting Google+ before they made it harder and built 1,909 followers there:

![](/images/programatic-content-creation/25-gp.png)

#### TUMBLR TOO

While I was at it, I programmed IFTTT to repost all my posts to Tumblr too!

![](/images/programatic-content-creation/30-tumblr.png)

#### BUFFER TWITTER QUEUE

Here's what my Twitter queue looks like right now:

![](/images/programatic-content-creation/54-bufferqueue.png)

And here’s my Twitter account, where I've managed to grow the follower count to 14,600 using iMacros bot scripts that I wrote:

![](/images/programatic-content-creation/09-twitter.png)

## WORDPRESS TO BUFFER LOG

After a post publishes from Wordpress, this report fires from Buffer on which posts were generated.

#### LINKEDIN NOT RIGHT FOR MY NICHE

You can that I'm getting an error for LinkedIn. I was sending posts there for a while but saw no real way to build a following for the site's “company page” with botting or other methods that weren't expensive.

At Saturn I had experimented with targeted ads via LinkedIn and the results were bad and expensive. I decided to ditch it.

![](/images/programatic-content-creation/40-wp-bufferstatus.png)

## LANDING PAGE WITH OFFER TO GROW MAILING LIST

What's the point of doing all this?

Everything that just happened was designed to generate traffic to the website in a variety of different ways. Once the users get to the website I'm hoping they will sign up for the mailing list, discover other good content, or even better yet, discover my band and buy an album or something!

Usually you have to offer up a piece of bait to get someone to give you their contact info. In this case I'm offering the benefit of a music digest to their email twice a week as well as an email response with a link to download 2 of my band’s albums for free just for signing up.

#### MUSIC IS A GIVE AND TAKE RELATIONSHIP

This is a great way to **AT LEAST GET SOMETHING** of value that the all too typical free streaming transaction does not provide. The email address is still rated the best way to contact someone among marketers.

That's why a really good email list can be worth a lot of money. Even just 1000 niche followers on a list can generate a big chunk of change if one has products to offer (other than music though usually).

![](/images/programatic-content-creation/05-landing.png) ![](/images/programatic-content-creation/06-landing.png) ![](/images/programatic-content-creation/07-landing.png)

But why stop there? Once they sign up to the mailing list I need to continue engaging them somehow or they'll forget about my site. Repetition is the key to retention marketing and that's why a newsletter is the last stage of an integrated digital marketing strategy. The hope is that if the visitor returns to the site there is a chance that they will take the next step on one of my offers, in my case it would be checking out my band, and I can also send them an occasional offer to check out purchasable Merch like t-shirts and records.

## AUTOMATED BI-WEEKLY EMAIL NEWSLETTER FOR MAILING LIST SUBSCRIBERS

I installed a plugin called Mailpoet that's able to take Wordpress posts and generate newsletters out of the latest content posted to the site.

![](/images/programatic-content-creation/44-mailpoet.png)

I made three different emails: The Monday metal and the metal Friday emails and the email that's automatically sent with the link to Oneironaught's 2 free albums.

![](/images/programatic-content-creation/48-newsletter.png)

When someone signs up I get a happy email in my inbox. Cool!

![](/images/programatic-content-creation/49-newsubscriber.png)

This is first email with the links to Oneironaught's album:

![](/images/programatic-content-creation/50-autoresponse.png)

This is what a standard weekly email looks like:

![](/images/programatic-content-creation/51-newsletter.png)

## PLUGINS CHAINED

To get Mailpoet to work properly, I had to figure out how I could programmatically copy the Spotify album art image from the foreign Spotify image url posted from IFTTT to my site's media folder, and then automatically set that image to Wordpress's "Featured Image" field, which is what Mail Poet requires in order to include pictures in its autogenerated newsletters.

![](/images/programatic-content-creation/41-featuredimage.png)

I found a plugin called Quick Featured Images Pro and was able to get it to do just that.

![](/images/programatic-content-creation/45-quickfeatured.png)

Some other plugin shennanigans going on on the page include a plugin that scans the Wordpress post's title and searches for it on YouTube, returning a thumbnail image that lazy loads the video upon user click (to save precious page load time.)

![](/images/programatic-content-creation/46-relatedyoutube.png)

To get the Spotify urls to easily embed in the site I installed the Spotify Embed plugin. I'm also caching the site to save speed and have it connected to a content delivery network (CDN).

Tracking Code Manager Pro allows me to place Facebook, Google...etc... tracking pixels on each page of my site while only loading them in one place.

![](/images/programatic-content-creation/47-spotifyembed.png)

## YOUTUBE

Video marketing on YouTube is awesome. I ran a series of experiments using very targeted GoogleAdwords ads and was able to generate approximately 16,259 views on this video for about $50.

![](/images/programatic-content-creation/28-yt-dennis.png)

Here's the actual video if you wanna listen:

<iframe width="789" height="444" src="https://www.youtube.com/embed/PSdwU6fIr_A?rel=0" frameborder="0" allowfullscreen=""></iframe>

Here's the YouTube Channel page where you can see I've grabbed another 1,318 subscribers.

![](/images/programatic-content-creation/29-yt-home.png)

I tried some similar experiments with Facebook ads and was able to get a native Facebook video version of the same song to over 28,000 views for about $50.

![](/images/programatic-content-creation/27-fb-vid.png)

## MY CURRENT EXPERIMENT

I love going to see live music and noticed people are always filming parts of the concert with their phones and security doesn't care. I had been using Zoom products for a while and when I discovered their GoPro sized concert recording video camera I had to put it on my Christmas list.

## SITE BRANDED HD AUDIO + VIDEO CONCERT BOOTLEGS

Dear old Dad hooked me up and so I began my quest to capture some supreme footage at awesome concerts. Here's one of the videos from a recent concert:<iframe width="789" height="444" src="https://www.youtube.com/embed/arv9l7rYaMQ" frameborder="0" allowfullscreen=""></iframe>

A friend of mine who works in video security recommended I try using a magnetic tripod mount to non-chaulantly stick the camera somewhere stable so I wouldn’t have to hold the camera. I bought a magnetic mount, had no trouble for security on the way in with these small items in my pockets, and successfully recorded a whole RJD2 set at Brooklyn Bowl with the camera attached to metal piping above a door.

<iframe width="789" height="444" src="https://www.youtube.com/embed/y5OS65TWpcM" frameborder="0" allowfullscreen=""></iframe>

Anyway so with all this stuff what kind of results am I getting?

Let's see:

## ANALYTICS

I'll let you guess when I turned the automated website on. You can see the traffic begins to ramp very quickly from March 2016 up until now.

The traffic is dropping off most recently because the site got hacked in the beginning of January with a ad injection attack that changes all the site links to links to some hacker’s ads. I didn't notice for a while because umm... I didn't want to have to do anything. So I wasn’t paying much attention to be honest. Lesson learned.

Whoops. Well I fixed it and installed much better security tools.

![](/images/programatic-content-creation/58-goog.png)

## 1 YEAR OF TWITTER ANALYTICS

To go along with the site traffic stats I thought what was happening on Twitter was pretty fascinating. Since my largest follower count is there and I'm also posting the most times a day there it produced some fascinating results.

They may look paltry in these recent months, but that is again because of the hack. Twitter stopped accepting new posts from Buffer knowing I was hacked and I lost a lot of traffic momentum because of it.

Look what happened from Feb 2016 to when I launched mid March 2016 and everything leading up to January 2017.

#### PRE-NEW-SITE LAUNCH

![](/images/programatic-content-creation/24-tw-feb16.png)

#### SITE LAUNCHES MARCH 8TH 2016

WOAH! I went from 49 tweets in a month to 464 and from 37.4K Tweet Impressions to 385K Tweet Impressions. 

![](/images/programatic-content-creation/23-tw-mar16.png)

And it keeps growing...

![](/images/programatic-content-creation/22-tw-apr16.png) 

AND GROWING! 

![](/images/programatic-content-creation/21-tw-may16.png) 

Settling down a bit. 

![](/images/programatic-content-creation/20-tw-jun16.png) 

Bouncing back up 100K. 

![](/images/programatic-content-creation/19-tw-jul16.png) 

Dropping. 

![](/images/programatic-content-creation/18-tw-aug16.png)

And the growth begins slowing down as I do actually have to pay attention to high level site things... I just didn't know where else to go with it without coding a totally custom site with no plugins.

![](/images/programatic-content-creation/17-tw-sep16.png)

![](/images/programatic-content-creation/16-tw-oct16.png) 

![](/images/programatic-content-creation/15-tw-nov16.png) 

![](/images/programatic-content-creation/14-tw-dec16.png)

![](/images/programatic-content-creation/13-tw-jan17.png) 

![](/images/programatic-content-creation/12-tw-feb17.png)

Got hammered bad with 63.4K impressions (meaning number of times it displayed in a Twitter users' newsfeed)

![](/images/programatic-content-creation/11-tw-mar17.png)

Yeah. I have some work to do to recover from that hack, but I'm more interested in building a new more custom version of this.

Regardless, the sustained increase in visibility, brand awareness, social media followers, email subscribers, and site traffic is fairly clear. Even the worst month is double what I was receiving before the new site went live.

There are many marketing things I wanted to do that I couldn't just using other people's Wordpress plugins.

## CONCLUSION

This project in particular is what inspired me to apply to coding schools and eventually join Flatiron. I’m really proud of it, but I know it could be so much better. Should I start from scratch and code it all myself, so I can do anything I imagine with it? That’s my next question.