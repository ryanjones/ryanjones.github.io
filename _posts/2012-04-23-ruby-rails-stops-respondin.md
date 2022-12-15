---
title: Ruby + Rails Stops Responding on Windows
categories:
  - Ruby on Rails
---


A friend ran into a bug that I had already conquered. I figure that if we both have the issue, other people might also.

Randomly, Rails (Ruby) will just crash. Most of the time it happened while the assets are being served. After some research I was able to figure out that there was a ton of lines being written to the log file. It turns out that there’s some weird bug, either with WEBrick or Ruby itself that ends up crashing the server when writing information to the log file.

You can disable logging, but that’s a pretty big hindrance. My solution was to install thin server (which is very similar to WEBrick but lightweight.

Here’s what you’re going to have to do:

Update your gemfile:
```ruby
group :development, :test do
  gem 'eventmachine', '1.0.0.beta.4.1', :platforms => [:mswin, :mingw]
  gem 'thin'
end
```

There’s tons of issues with event machine installing on windows, so make sure you’re using that exact version above. Run **bundle install**. And instead of running “rails s”, run “**thin start**” to get your server running.

The worst part about this bug is that it can easily take you down a rabbit hole. Your error might be on a jquery line, or a css line (whatever asset is being served up). Some of the key things that I do know:

This happens with Ruby 1.9.2
This happens with Rails > 3.0 (which makes me think it’s more a Ruby issue)

Windows will produce these errors (hard crash of Ruby):
Ruby Interpreter (CUI) 1.9.2p290 [i386-mingw32] has stopped working
Ruby Interpreter (CUI) 1.9.2p320 [i386-mingw32] has stopped working

**Update:** I’m running Ruby 1.9.3p125 and haven’t hit this issue yet using WEBrick. This might be an alternative solution instead of using thin server.

**Update:** If you’re working with people using Linux/Mac, something like this might be more suitable. It will ignore these if you’re not on Windows:

```ruby
# le windows
platforms :mswin, :mingw do
  gem 'eventmachine', '1.0.0.beta.4.1'
  gem 'thin'
end
```
