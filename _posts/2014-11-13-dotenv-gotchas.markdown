---
layout: post
title: "Dotenv Gotchas"
date: 2014-11-13 20:40
comments: false
categories: [dotenv, Rails, bash, Gems]
description: Every so often you get your ass kicked by something and you just need to write down all of the quirks.
---

Every so often you get your ass kicked by something and you just need to write down all of the quirks. [Dotenv](https://github.com/bkeepers/dotenv)
 was today's culprit.

At AMA we start up our blessing of unicorns using foreman (which loads in our .env files). When we do a 0 downtime (USR2) restart we
manually call ```Dotenv.load``` to reload in the environmental vars(.env). Here's the code in our ```unicorn.rb```

````ruby
before_fork do |server, worker|
  Dotenv.load
end
```

Through time we've learned a few valuable lessons when upgrading:

- < 0.7 you can't have comments in your .env's
- < 0.8 you can't have blank variables (which was a bummer for .env.development)
- 0.9 and above you'll need to escape all of your dollar signs as variable expansion is now enabled (or use single quotes - not double)

Recently we noticed our unicorns weren't properly reloading their environment which led to us having to do a hard
 restart (TERM) on the unicorns. Luckily we weren't taking on a huge load and our caching offloaded most of the hits,
 but this can mean that some users could get an error if the unicorns don't come up fast enough.

We dug in a bit deeper and we noticed that calling ```Dotenv.load``` just loads in any new variables (it won't override any variables).
 This led to the inevitable "How did that even work in the first place?".

At this point, we're not sure if it ever did (seriously, it did :( ), but
 what we did find was that we should have been using ```Dotenv.overload```. This will override all of the variables that were
 in your .env file.

We noticed that it wasn't being loaded up into rails so [mvandenbeuken](https://github.com/mvandenbeuken) created a [pull request](https://github.com/bkeepers/dotenv/pull/149/files)
 to allow us to make the call without directly requiring the library in ```unicorn.rb```

Hopefully this will save you a few **'wtfs'** when your app is acting up when you upgrade dotenv. I think we've burned enough
 universal **'wtfs'** on our own.

