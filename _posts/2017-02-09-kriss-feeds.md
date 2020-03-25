---
title: Slick fast feed aggregation
location: Rastatt
---
Personally, I read a few blogs and feeds that I like to keep an eye on. The Debian Security and Debian News feeds, for example, since sometimes I'd like to know if any vulnerabilities are coming my way. (Though they rarely affect me.) The cooking blog of a close friend, [Cuisine Carolin](http://www.cuisine-carolin.com/). George R.R. Martin's "[Not a Blog](http://grrm.livejournal.com/)". When I have a bit of spare time for reading stuff on the web, I don't want to visit each of those sites to look for new posts. So, the natural decision is to have a feed aggregator.

Ages ago I used Google Reader for this purpose, and it was quite an alarming experience when it suddenly shut down (although they did give their users enough time to take out their data, which was nice). After that I switched to Feedly, where you could import your Google Reader data and continue where you stopped. But the experience of losing a service that I very much liked because the owner of that service decided that it was no longer worth the effort stuck with me.

So when I recently picked up blog reading again, I thought about a self-hosted alternative to Feedly and all the other aggregators out there. I had used Thunderbird's feed capabilities in the past, but the hassle of keeping those feeds synchronized across two computers and a smartphone was a bit too much. Since I already run a little Raspberry Pi, I looked for a simple and fast aggregator preferably written in PHP (since that's what I'm used to and what can run with no additional packages on my Pi).

I just want to give a quick shoutout here to [Kriss Feed](https://github.com/tontof/kriss_feed), "A simple and smart (or stupid) feed reader." In its compiled form, it's just a single PHP file that you place on your server. Its source is a bit hacky, but I like that. When you install it (or rather put it in your docroot), it creates some files to store feeds etc. for itself. The built-in help is concise and helps you to establish a cron job for fetching new posts, among other things.

The mobile front-end could use some refurbishing, but it works well enough. I contributed a German interface translation since the source for those translations is GNU Gettext and I know my way around that.

So, if you're looking for a feed aggregator that nobody can take away from you, I'd recommend Kriss. If you know of similarly simple or even better PHP software, why not leave a comment below.
