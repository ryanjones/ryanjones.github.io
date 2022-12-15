---
title: "Avoiding Forks with Gem Extensions"
date: 2014-10-27 19:48
categories: [Ruby, Rails, Gems, Forks]
description: Gems are one of the best things of the Ruby ecosystem. There's pretty much a gem for everything. But what happens..
---

Gems are one of the best things of the Ruby ecosystem. There's pretty much a gem for everything. But what happens
when that gem needs a slight tweak to work with your app? Eventually as you create more and more Ruby/Ruby on Rails apps you're going
to run into this issue.

The easiest way to make a change to a gem that's hosted on Github is to just hit the ole [Fork](https://help.github.com/articles/fork-a-repo/)
 button. Now you have an exact copy in your Github repos. You can make all the changes you want and reference the
 repo directly in your Gemfile (you are using [Bundler](http://bundler.io/), right?).

````ruby
  gem 'doorkeeper', github: 'ryanjones/doorkeeper'
```

At this point in time you're happy. Jump ahead 10 months and you're probably going to be a grumpy developer. Why? Well,
 10 months is a lonnng time in the open source world. Tons of features and bug fixes have made it's way into the
 gem, and you're stuck way back at the version that you forked from.

So, the easiest way to fix this is to pull in master, correct? Sometimes you might get lucky, but it's pretty rare
 that you won't get merge conflicts. I've been in scenarios where the method I've changed was completely removed
 from the gem! How do you even go about fixing that?

Unfortunately, there's not really an easy way to fix that (sadface). You'll still have to re-write your patch, but you can create
 a gem extension which will allow you to manage the fallout a little bit easier. A gem extension is a just gem that overrides
 existing functionality within a gem. There's a bunch of great benefits for creating a gem extension:

- Easy to track what the actual change was, as the code only related to the change is needed. Meaning, you don't need all
of the original gems code to go along with it.
- You can bring it in from rubygems across multiple projects (with proper versioning).
- You can remove the gem and the app reverts back to the default functionality.
- Tests are isolated to the overridden code (though I suggest you test it in your project also).
- Provides a consistent way to override gem functionality across multiple projects.

We recently had to make some changes to [doorkeeper](https://github.com/doorkeeper-gem/doorkeeper) (an OAuth2 provider).
 After a user authenticates, we needed to log them out. You can look at the gem extension [here](https://github.com/amaabca/doorkeeper-logout_redirect).
 You'll notice a couple of things:

- Naming the gem properly "gemname-module_name_space_to_extend". ```Doorkeeper::LogoutRedirect``` in this case to keep the module
structure in place.
- Proper gem versioning in case we need to upgrade doorkeeper (and keep out logout after redirect in place).
- When we decide to remove the functionality we can just drop the gem from our Gemfile and doorkeeper will revert
back to it's original functionality.

It's important to note that this method won't get you away from updating your gem extension if large parts of the gem's
internals change. However, it should be less painful to apply across projects, provide some versioning, and structure
to how you do method overriding.

Sidenote: If there's a different name for what I'm doing above I'd like to hear from you. At AMA we call them
gem extensions, but after some googling I'm still unsure if this is the right lingo.
