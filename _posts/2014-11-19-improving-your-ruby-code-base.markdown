---
layout: post
title: "Improving your Ruby code base"
date: 2014-11-23 19:32
comments: false
categories: [Ruby, Rails, Code, Improvement]
description: Everyone has inherited a codebase that was in dire need of a re-write (at least a portion of it). If you haven't, consider yourself one of the lucky few.
---

Everyone has inherited a codebase that was in dire need of a re-write (at least a portion of it). If you haven't, consider yourself one of the lucky
few. I was at the local [yegrb](www.yegrb.com) meetup a few nights ago and there were a bunch of ideas being thrown around. I brought
up a few of the methods that we used to improve our codebase(s). It's been a long trek at AMA, but we're miles ahead of where we
were a year ago.

Developing your Ruby skill set
---------------------

I'm going to ask you a loaded question. **Do you write good Ruby code?** I used to think I did, but I was mediocre at best.
 Before you can improve your codebase, you need to improve the quality of ruby that you write. If you blindly re-write code without improving, it's
 probably going to be just as bad as the previous code. It's a harsh reality, but once you accept that you're old codebase
 is a reflection of your skill level, you can start to improve, and prevent future failures.

But how do you improve? Books. Specifically this [book](http://www.amazon.com/gp/product/0321721330/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321721330&linkCode=as2&tag=sandimetzcom-20&linkId=MEEIA2TTJVD6F5DO).
 Most of the time when I read books, nothing sticks. Or it only becomes beneficial in very specific scenarios. POODR was the first ruby book that I started to read and made my code better **the next day**.
 I started leveraging objects much more, and wrote less procedural code. It teaches you to truly embrace OOP.

Coding Rules/standards
---------------------

All teams have varying skill levels. So how do you start to improve as a team? Luckily, Sandi has given a few good base
 [rules](https://gist.github.com/simeonwillbanks/4567703) to follow (with the caveat of "You can break these rules if you can talk your pair into agreeing with you"):

1. Your class can be no longer than 100 lines of code.
2. Your methods can be no longer than five lines of code.
3. You can pass no more than four parameters and you can't just make it one big hash.
4. When a call comes into your Rails controller, you can only instantiate one object to do whatever it is that needs to be done.

In addition to those rules we've added one other.
No instance variables. We use [decent-exposure](https://github.com/hashrocket/decent_exposure) and the expose helper to dry up our code which gives us
 an easy way to stub when testing.

**You're going to struggle through those rules**. I remember lots of head scratching when we decided to follow this set of rules, but it becomes second nature
 once you get used to it. You start to think differently about how to use models and structure your code. You'll notice that you'll start to use way more
 PORO's (Plain ole Ruby Objects) instead of stuffing that active model full of code.

These will start to guide you and your team towards a higher quality codebase. But a codebase without consistency will drive you nuts.
 You need to write easy to read/well structured code, as a team. How can you do that? The first is to pick a good base
 of coding standards. We went with [github's ruby coding standards](https://github.com/styleguide/ruby). Through time, your team will
 start to hit road blocks/frustration points in the codebase. This is good! Talk about it as a team and create some additions to your
 coding standards/rules. Here's our additions (with explanation):

- Follow [Law of Demeter](http://c2.com/cgi/wiki?LawOfDemeter) (only talk to your neighbors) wherever possible.
If it makes sense to break the "law", make sure you're not changing the object you're calling i.e.,
don't do this: `user.profile.update_attribute(:foo, 'bar-baz')`

This rule to prevent craziness like this: ```object.batmans.breakfast.and.lunch.and.dinner```. The more you chain the harder
 it is to test and debug. We had massive chains that made our life hell. We rarely go past 2 chains now.

- No class methods, no ```def self.foo``` (unless you're doing a finder type method or returning a collection of a your own classes instances).

I brought this up at the [@yegrb](https://twitter.com/yegrb) meetup and everyone looked perplexed. We've found class methods are rarely ever needed. The only
 outlier would be doing some configuration or in the above scenarios.

- Always pass a hash to an initializer method: ```def initialize(args = {})``` instead of ```def initialize(bar, baz)```

We used to have to do massive refactorings when we didn't pass a hash (because the changing of the method signature everywhere). This one
 is a hot topic, because it's tough to know what to pass into the method. We make sure to fetch the keys and raise an error if the key isn't
 visible. You can view how we do this [here](https://github.com/amaabca/lita-github_pr_list/blob/master/lib/lita/github_pr_list/pull_request.rb#L14).
 As we move most of our apps over to Ruby 2.0+ We hope to start leveraging keyword args much more.

- When possible - try not to pass args to an instance method - this often leads to a procedural style.
 If you have args, pass them in the constructor instead and then operate on them in the method.

You want to throw as much stuff into the object as possible and let the methods act on the attributes. The simplest reason
 would be that all of the methods can act on the attributes and more complex reasons like object composition.

- Instead of referencing classes directly - set defaults in a constructor. Ideally set defaults in an initializer.

```ruby
  # config/application.rb
  config.after_initialize do
    config.default_provider = Car::Booking::Provider
  end

  # app/models/foo.rb
  def initialize(args = {})
    self.provider = args.fetch(:provider, ::Rails.application.config.default_provider)
  end
```

If you can set a default, do it. It'll save you grief later on when using the class elsewhere.

-  If a method is not used outside a class, put it under `private` - this limits the public "API" of the class.

Every so often we'd forget to put a method under private and it would start to get consumed (even though it shouldn't). This was
 more of a reminder for us than anything.

- No conditionals in views.

Your mind just exploded. This one is really tough. You'll start to make really good presenters with this rule in place.
 We're personally big fans of [draper](https://github.com/drapergem/draper). We still have the odd conditional slip through
 (a pair was convinced!), but not very often.

- Don't start lines with 'unless'.

```ruby
unless valid? do_stuff # rejected PR
if !valid? do_stuff    # accepted PR
do_stuff unless valid? # accepted PR
```

Your mind might be fresh at the start of the day, but eventually it becomes hard for your brain to process unless statements. If you've
 ever come across something like this: ```unless method_z && method_a && !method || method_x``` you'll appreciate this rule. We still do allow unless at the
 end of the line, but we only allow one conditional.

- Deploys can't rely on .env vars

We fell into a nasty habit of relying on .env variables for a deploy (hooks specifically). This caused us all sorts of grief so
 we put the kibosh on it.

- Only access ENV['stuff'] from the application or environment config files. These values will be pulled from the config object throughout the code.

```ruby
module AwesomeApp
  class Application < Rails::Application
    config.epic_api_url = ENV['EPIC_URL']
  end
end

# example of use
RestClient.get Rails.configuration.epic_api_url
```

This allows you to change the config var in 1 place instead of a global find/replace.

- Do not use [delegate_presenter](https://github.com/rwilcox/delegate_presenter). [draper](https://github.com/drapergem/draper) is feature-rich.

We started to use delegate_presenter as a slimmed down feature set of draper. We regretted it for 2 reasons. 1. turns out we needed those features (doh!) and
 2. The gem is really inactive and it took almost 6mo to merge in a PR. Draper is really well maintained at this point. We probably should change
 this rule to 'use Draper'.

-  Blank lines don't matter (within a method) and are not counted towards for method line length. It's a signal as to maybe that method
should be broken apart.

Sometimes we can become sticklers about method length. We tossed this in to make sure that the rule is just a guideline. Above all,
 don't write bad code just to conform to the rules.

-  Only 1 line allowed in a rake task

We used to have huge rake tasks that were almost impossible to test. Ironically we started to push those into class(self) methods which just moved the problem
 from A to B. We broke those downs into properly instantiated classes and we were golden. Our 1 liner's look like
 ```EpicImporter.new({data_url: 'http://example.com'}).import```

Tooling
---------------------

We use a couple of tools to keep the quality of our codebase up. Specifically we use:

- [Brakeman](https://github.com/presidentbeef/brakeman) for security checks (make sure to keep it up to date!).
- [Cane](https://github.com/square/cane) which helps keep the complexity low.
- [Flay](https://github.com/seattlerb/flay) to flag down duplicate code.
- [Simplecov](https://github.com/colszowka/simplecov) to make sure that our testing coverage doesn't go down.

When it comes to Brakeman/Cane/Flay the rake tasks weren't crystal clear, so we created a few [wrapper rake tasks](https://gist.github.com/ryanjones/a9295a969e1804855ae4)
 to make them a bit more clear. We also make sure they returned 0/non-zero return codes so our CI would flag it if we broke some of the thresholds. Simplecov is a great
 tool for preventing code being written without tests. It's really useful to see if you've [covered off those edge cases](https://camo.githubusercontent.com/3cb7252450587d575bca65e27f20107b1986d67e/687474703a2f2f636f6c737a6f776b612e6769746875622e636f6d2f73696d706c65636f762f6465766973655f736f757263655f66696c652d302e352e332e706e67).

Build your own Rules/Standards/Tool set
---------------------

It's important to note that we built this rule set as a team as we hit rough patches of code. It's not going to work if you just
 drop a bunch of rules on your team, or try and implement something overnight. **Do not** drop some of the tools
 in place without chatting with your team (you are doing a weekly meeting, right?).

Work together at a team and make those codebase(s) better! Good luck!

