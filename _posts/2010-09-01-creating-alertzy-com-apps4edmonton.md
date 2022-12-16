---
title: 'Creating Alertzy.com - #Apps4Edmonton'
categories: [Hackathons]
tags: ['Ruby on Rails']
---


This is abit of a read, but I tried to cover everything. I’m going to create this is a timeline format. Hopefully I don’t fail.

*July 22/2010*  
**The Meetup**  
At the beginning of July I received a message over Steam (while dominating as a Sniper in TF2 \*flex\*) asking if I wanted to take part in an app making contest for the City of Edmonton via [Greg][1]. Since most of my other friends were either to busy doing other things, or still in school I leapt at the chance to build \*something\*.

 [1]: http://twitter.com/gabain

I had looked into the deadline for the application at the City of Edmonton and saw that it was August 27, 2010. That would give us roughly a month to build an app, test it, and release it out to the public.

At this point we hung out and decided our roles, Ben and myself as developers, and Greg and the graphics designer. We were to break out, come up with some ideas and meet in a few days.

*July 25/2010*  
**Decisions, Decisions**  
First things first. We started throwing out ideas over the table. We wanted to use *something* from the [open data catalog][2] that the City of Edmonton has graciously provided.

 [2]: http://data.edmonton.ca/

[Ben][3] had built a prototype of an ETS map that would’ve allowed us to do many things with the ETS data. There was *tons* of data on ETS, but we new that market was going to be saturated and wanted to try and think outside the box. We also wanted something that everyone could use (not platform specific ie. iPhone, Android). We decided pretty much everyone has a cellphone now (prove me wrong!).

 [3]: http://twitter.com/benzittlau

I had taken inventory of everything that was on the site (that I found might pertain to our app) and came up with the idea of alerting people of garbage day via text. With the team building off of this idea, it slowly morphed into a full, real-time alerting system with many features (not just garbage day anymore). We had loads of data to provide alerts on. Garbage days, soccer field closures and many others.

![Data thoughts][4]
Analyzing City of Edmonton

The room was alive with ideas and how we could implement it. But it all came to a full stop once we realized the amount of $$$ it costs for a [short code][5]. Upwards of 500$ a **month** (short codes must be used for business, you couldn’t buy a phone from a cellphone company and use that phone number. If i’m not mistaken it’s against the TOS).

 [4]: /assets/img/old/Ryan_on_Rails_book2.png "Data thoughts"
 [5]: http://en.wikipedia.org/wiki/Short_code

We thought the idea was dead right there.. But Ben had a plan! He has a friend who owns [Direct Text Marketing][6]. And thought we might be able to get some sponsored texts… We put this thought on hold for now (until Ben heard word from his friend) and moved on to the next topic…

 [6]: http://www.directtextmarketing.com/

**Watch your Language!**  
Ben being a pHp developer, and myself being a .NET/COBOL developer didn’t help things. I’ve always declared that I hate Ruby on Rails. I just didn’t get [MVC][7], or the magical ninja helpers that Ruby on Rails provides, on my first attempt with RoR. But, we needed a language that would be common to both of us. Ruby on Rails it was! For the next week, we’d be learning Ruby on Rails.

 [7]: http://en.wikipedia.org/wiki/Model–View–Controller

It was a little crazy, we had a month to learn a new framework, and a new language. But we were down for a challenge.

*July 27/2010*  
**Good news everyone…**  
Ben spread the news to Greg and myself that the text messaging with [Direct Text Marketing][6] was a “go!” and we had a sponsor.

*August 1/2010*  
**The Tools**  
We knew we needed some tools for this, so we setup [Dropbox][8] for our non source code files (images, word docs, etc.), [github][9] for our source code (12ish $ a month for an awesome remote repository) and we used [Cacoo][10] for our design documents, web layouts, and over all workflow.

 [8]: https://www.dropbox.com/home#
 [9]: http://github.com/
 [10]: http://cacoo.com/

We both started with [Rad Rails 2][11] which was the worst mistake of our lives. After banging our heads against the walls, we changed up our development tools. Ben moved over to [Textmate][12] for his Mac and I moved over to [Aptana Studio 3 Beta][13] which basically copied Textmate (but it’s for Windows!) and has the bash console in the program. I also tried out [ E Text Editor][14], but I found it to be a terrible clone of Textmate (the tabbing system didn’t even work properly for me).

 [11]: http://www.aptana.com/products/radrails
 [12]: http://macromates.com/
 [13]: http://www.aptana.com/products/studio3
 [14]: http://www.e-texteditor.com/

![E Text Editor Fails][15]
E Text Editor Fails. Half Tabs!?

*August 4/2010*  
**What’s in a name?**  
2 hours. 2… long… hours. That’s how long we sat around thinking of names, checking them against a who is database, and *face palming* whenever we had a good name, and the .com was taken. We came up with a few names and wrote them on our white board and got up to go grab some donairs. Greg went to go get ready, and Ben and I were still coming up with ideas. I was determined to get a Z in our name (Z’s are awesome. I’m gonna name my kid Zen, I swear. Boy OR girl). Anyhow, I blurted out “Alertzy!” In a greek kinda voice, and we added it to the board. We chowed down on our hard earned donairs and picked the name Alertzy an hour later.

 [15]: /assets/img/old/Ryan_on_Rails_EText.png "E Text Editor Fails"

Alertzy was added to facebook and [twitter][16] on this day also.

 [16]: http://twitter.com/Alertzy

*August 9/2010*  
**Garbage data madness**  
At this point Ben and I were pretty much through the [rails tutorial][17] that we were using to learn Ruby on Rails (with the addition to some other books I had).

 [17]: http://railstutorial.org/chapters/beginning?version=2.3

![Ryan on Rails Books][18]
Ruby on Rails Books

We noticed that the open data was kind of weird and didn’t match up, we ended up figuring out what was wrong and synced the data up. We fired an email off to the open data at OpenData@edmonton.ca and they fixed it up for anyone else that would be using this. After the competition closed, we noticed that [ @MarkBennett][19] also had the same problem.

 [18]: /assets/img/old/Ryan_on_Rails_Books.png
 [19]: http://twitter.com/MarkBennett/status/21879487660

We had been coding for a few days now, and everything was starting to come together.

*August 13/2010*  
**It works!**  
Ben sent out an email with a full test of the Alertzy backend. It worked! Hurrah! Greg also had some great images setup for us to build a layout off of.

Many many lines of code and photoshopping was done during this time. I won’t paste the technical details here, as Ben and I plan to post some technical details of the project at a later date. Ben currently has a few articles at [of mice and pen][20].

 [20]: http://www.zittlau.ca/

*August 20/2010*  
**Hosting Gongshow**  
With all of the graphics applied and Alertzy version 1.0 being done we weren’t quite sure who was the best host, or who to go with. We ended up going with hostingrails.com, but then after a [horrible hosting experience][21], we ended up moving over to slicehost.

 [21]: http://www.zittlau.ca/rails-host-review-the-bad-hostingrails

Ben spent many of many hours getting the server up and running. We currently use [deprec][22] [Capistrano][23] to deploy our application. And for the record, the documentation on the internet was quite poor. I’m sure Ben will blog about this sooner or later.

 [22]: http://deprec.org/
 [23]: http://www.capify.org/index.php/Capistrano

*August 25/2010*  
**We’re Live!**  
The site went live on slicehost providing garbage day text message reminders, along with field closure text messages.

![Alertzy goes Live!][24]

*Current day, August 31,2010*  
**Thoughts on Apps 4 Edmonton**  
I think the City of Edmonton was smart to have a competition like this. They provided incentive, data for us to work with, support on that data, and a platform for users to submit ideas, what more could a developer want?

 [24]: /assets/img/old/Alertzy1.png

The only concern I would have as a developer would be that the datasets stay up to date, and in the same format. That goes for all cities using the open data catalog.

**Thoughts on Alertzy’s business viability**  
Can Alertzy survive in the market? Absolutely.  
We’ve built Alertzy with the ability to scale, use other cities open data catalogs, have a business plan on how to create revenue, and currently spend a very minor amount to host the website thanks to our [sponsor][6].

As the cellphone world expands, we plan to be the #1 alerting system in Canada due to cross platform capability.

Did I mention we plan to roll out email alerts in the future? No more adding items to GMail or Outlook, the alert will be emailed to you directly.

**The Team**  
This wouldn’t have been possible without teamwork and dedication. Greg was off working in Toronto putting in time designing buttons (which we later scraped, sorry Greg :( ), Ben spent his whole weekend fixing the slicehost VPS to allow us to deploy easily and built an awesome garbage map zone pinpointing system while I worked on the verification system.

Whenever problems came up we’d band together and “war room” as Ben would call it. Figure out a strategy and execute the plan. We ran into many problems, but we managed to put our heads together and figure them out.

**Shameful Plug**  
At this point I’ll throw out a shameful plug. If you want to be alerted of Edmonton’s field closures, or are tired of missing garbage day, sign up at [Alertzy][25]. If you’d like to support our development, show us some love and vote for us at [Apps 4 Edmonton][26].

 [25]: http://www.Alertzy.com
 [26]: http://contest.apps4edmonton.ca/apps/17