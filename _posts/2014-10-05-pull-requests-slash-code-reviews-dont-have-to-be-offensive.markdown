---
layout: post
title: "Pull Requests/Code Reviews don't have to be Offensive"
date: 2014-10-05 19:51
comments: false
categories: [Code Reviews, Pull Requests, Management]
description: "Code reviews are a pain. They take time away from development, disrupt workflow, and can even cause conflicts among teammates."
---

Code reviews are a pain. They take time away from development, disrupt workflow, and can even cause conflicts among teammates.
 However, I think we can all agree that they're a necessary part of life, and that if we **DIDN'T** do them, we'd
 be in a worse situation.

When I started at AMA I noticed that there wasn't a huge amount of comments on any of the pull requests that were going up
 to production. It didn't matter if the pull request had 1-2 lines of changes, or 50. That seemed odd, because
 out on the internet this isn't really a problem. Developers seemingly love to bash/1up other developers when it comes to
 tools, methodologies or the technology they're using. We see this all the time when [a new feature gets added to rails](https://github.com/rails/rails/compare/9333ca7...23aa7da)
 or [dhh states that "tdd is dead"](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html).

A local team is different. If you call the guy across from you a numbskull there's nothing stopping him from reaching
 out and bopping you one. I was able to work with the team (through 1 on 1's and team meetings) and we came up with
 the following list of problems:

1. Everyone had their own coding style and didn't want to press that on other developers
2. No one wanted to hurt other team member's feelings
3. Some didn't feel that they were on the same skill levels as others
4. No one wanted to be "the one" that was holding up code from going to production

Once we had narrowed down what was causing the issue we could start to work through each of the scenarios.

**Everyone had their own coding style and didn't want to press that on other developers.**
 This was pretty easy to address. [Github's style guide](https://github.com/styleguide/ruby) provided a great base
 to build off of. It wasn't overly complex and it laid out the rules in a clear concise fashion. We've added extra items
 to our own style guide over time to improve readability and lower complexity. We've added other guides for other languages
 such as CSS/JS/Coffeescript over time. It's very easy to point to a styleguide if any conflicts occur.

**No one wanted to hurt other team member's feelings.**
 If you don't know what's out there you'll never know what to use. "I wish there was an app that did X.", "Haven't you heard about Waffle-a-tron,
 it will do exactly that!". Sound familiar? _Knowing is half the battle._
 Code reviews allow you to learn about new ways of constructing/architecting code.
 It might be as simple as introducing a map instead of a loop, introducing a builder pattern or even a quick lesson in dependency injection.
 It's all about improving the clarity and quality of the code and never against the person who wrote the code.

**Some didn't feel that they were on the same skill levels as others.**
 Sometimes it's hard to "call out" team members of higher rank. You might not think that your ideas carry a lot of merit,
 or that you don't have enough experience in the programming language. Here's the rub. If you ask a question like
 "How does think work?" or "I was looking at X pattern, could we use this to improve this chunk of code?" you're going to learn a
 100x more than the guy who blindly accepts the PR. You're going to be 100x more valuable in the future and you're going to be 100x
 more likely to excel. Be the best by calling out the best.

**No one wanted to be "the one" that was holding up code from going to production.**
 If you push sub-par code into production. You're going to get a sub-par experience. Once I hammered that home with
 the developers and upper management, the overall issue was moot. The developers focused on creating quality code
 and the upper management enjoyed dealing with a very low amount of regression which led to happy members.
 Over time, the developers realized that I would have their back if code wasn't to par and upper management
 respected that I wouldn't release poorly written code into production (which can lead to a tarnished reputation).

Keep in mind that code reviews are a process and can take awhile to turn things around. It might take a few weeks
 before people start to get comfortable. Your code base will thank you for taking the time to improve your code reviews ;).

A great guide on code reviews can be found here on [Thoughtbots code review guide](https://github.com/thoughtbot/guides/tree/master/code-review).
 It has some really great suggestions for reviewing code and receiving code reviews.

