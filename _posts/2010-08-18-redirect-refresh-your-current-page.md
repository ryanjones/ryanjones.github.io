---
title: Redirect (Refresh) your current page
categories:
  - Ruby on Rails
---


I’m not sure why I was unable to find this out on the internet, but here’s the simple piece of code you need in your controller to refresh your page:

```ruby
current_user.verified = true  
current_user.save  
flash[:notice] = "You’re verified!"  

#Redirect/Refresh  
redirect_to :back
```

I call this a refresh, but technically we’re just going back (to where we came from).

Shout out to Ben for finding it!