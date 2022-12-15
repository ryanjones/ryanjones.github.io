---
layout: post
title: "Converting a Monolithic Rails Application to a Service Oriented Architecture - Theory"
date: 2014-09-26 21:02
comments: false
categories: Architecture SOA
published: false
---

Over the past year at AMA we've done some amazing things. One of them is convert over a huge Rails (3.0+)
application into multiple sites which are all linked together with OAuth2. This was a huge undertaking that took over a year,
and there's still some cleanup we have to do.

There's a few reasons that we wanted to get this in place:

- Technical debt was at an all time high, we needed better test coverage
- As the team grew, working on 1 app was painful
- We needed a common API to communicate in and out with external services
- 1 login for every site, including mobile (native and web)

For the purpose of this blog post I'm going to pretend we're a membership organization (such as Costco). I'm also only
going to cover the general theory of how to break everything apart.
I'll be creating more technical blog posts to supplement this one (gems, server setup, etc.).

So here's where you start. You have a monolithic rails app that needs to be tamed:

<picture of single app>

Breaking out app into functional sites.

We found that breaking out the authentication was the way to go. OAuth - doorkeeper blah blah Consumer app leverages sorcery
(could use anything such as devise) etc. Implicit grant.

<picture of OAuth server>

We kept our API beside our oauth app for the short term - basic rails classes.

<picture of oauth server with API server (50 50)>

Then we broke out the API into a stand alone app - grape + grape swagger. versioning + 3rd party interface. Kept the packages the same.

<picture of api moved from the oauth server>

Add in the oauth consumer gem to the other apps.

<all apps oauth>

Mobile app logins, advantages of oauth







