---
layout: post
title: "0 Downtime Infrastructure"
date: 2015-05-18 19:01
comments: false
categories: [Devops, Uptime, 0 Downtime]
description: No one should ever try and visit a web page and find that it's down for maintenance. This is how we keep AMA websites up 100% of the time.
---

At AMA we strive for a high quality user experience. Part of that user experience is making
 sure that our services are available for our members whenever they need them. If a service/site is down, we've failed the member. Imagine trying
 to book road side assistance and the site was down. That wouldn't be good, and worse yet could put our member’s safety in jeopardy.

Here's what we do to make sure that our sites are running 24/7.

Servers
---------------------

We use Amazon for our server hosting to make sure that we have servers available to spin up whenever we need them.
 In the winter time we can spike from 50 to 3000+ concurrent users on our [AMA road reports](http://amaroadreports.ca/) site so
 the ability to flex our server load is very important.

We use EC2 for the application boxes themselves; Elasticache for our in memory storage and
 RDS for our data storage. All of the boxes/services are hosted on multiple [availability zones](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) so
 if we suffer an outage in 1 data center we will automatically be flipped over to the other data center. We also leverage geo-redundant S3 for backups to ensure
 scalable and easy retrieval of our information.


Application Deploys
---------------------

We never want the member to see a downtime page and [Unicorn](http://unicorn.bogomips.org/) is our server of choice for  0 downtime application updates.
 This allows us to deploy without having to take the website down for a short period of time. This also
 allows us to deploy at all hours of the day, so our team doesn't have to do night time deploys.

Unicorn does a dance where it keeps the original version of the application in memory while you're deploying the new version
 of the application. It slowly phases out the original for the new version and this experience is 100% seamless
 to the member.

Server Patching
---------------------

It's extremely critical that we stay on top of security patches in both the Ruby/Rails world, and on the infrastructure server side.
 Generally server patching needs to take down the server for a short period of time, however, due to the technologies we use
 (RDS/Elasticache) our database and in memory storage is covered for us (by Amazon). For the application servers we pull 1 server out
 of the load balancer, apply the patches and then bring the server back into the load balancer once it's been rebooted (if it needs to).

This fully automated process using Ansible enables us to execute in parallel, patching all our sites at the same time.

Notifications
---------------------

We run a 24/7 on call crew that allows us to address any issues that might happen outside usual work hours (you'd be surprised at how many people are
 renewing their memberships in the middle of the night!). The tough part of monitoring is that you need it at a few levels.

The first is the server level. Is the RAM/CPU/HD/Network performing correctly? No? Send an email/SMS! At the next level is the application;
 we need to make sure that the application itself is working correctly. If the web page isn't loading but the server is up
 something is wrong and someone needs to be notified. Finally is the 3rd party integration. We're hooked into
 multiple webservices and they might go down. We need a way to make sure we are alerted so we’ll get a notification and follow up with the 3rd party.

We're able to cover off these scenarios with [New Relic](http://newrelic.com) and [Rollbar](http://rollbar.com).

Armageddon
---------------------

What happens if your Amazon zones get wiped off the face of the earth? We store offsite backups of our data for exactly this reason.
 Our server build and deploys are all fully automated allowing us to bring up our hardware infrastructure extremely fast.
  It would probably take longer for DNS to propagate than it does for us to rebuild in a new Amazon Region.

We have had the odd outage here and there with Amazon (which is to be expected with cloud providers) and we've
 failed over without problems.


Summary
---------------------

If you're a member of AMA, you should expect the services to be available at all times. And we'll make sure that happens.
