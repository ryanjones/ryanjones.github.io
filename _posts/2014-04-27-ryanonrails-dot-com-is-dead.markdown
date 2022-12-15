---
layout: post
title: "RyanonRails.com is dead, long live ryanjones.io!"
date: 2014-04-27 11:26
comments: false
categories: General
---

I've decided to move away from ryanonrails.com and move over to ryanjones.io. When I first picked ryanonrails.com
it seemed like a clever play on Ruby on Rails (it was clever, right?), but throughout time it's grown old.

On that note, I wanted to make sure that none of my old links died off. I quickly threw together this sinatra app hosted on Heroku
to make sure that the urls are redirected correctly.

This small sinatra app will take anything after the url and forward it over to the new domain you've defined. I went with a 301 (permanent) to keep my SEO up,
however you could go for a 302 (temporary) if you're only setting this up temporarily.

````ruby

require 'sinatra'

get '/*' do
  redirect "http://www.ryanjones.io/#{params[:splat].first}", 301
end

run Sinatra::Application.run!

```

````ruby

source :rubygems
gem 'sinatra'

```

Drop these 2 files in a directory, bundle install and then run rackup.
