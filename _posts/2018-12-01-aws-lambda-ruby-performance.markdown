---
title: "AWS Lambda — Ruby Performance"
date: 2018-12-01
categories: [General]
---

AWS recently [released Ruby as a language](https://aws.amazon.com/blogs/compute/announcing-ruby-support-for-aws-lambda/) for their lambda (serverless) platform. This is quite exciting as it’s one of my favourite languages to develop in. Of course, there’s always this whole ‘Ruby is slow’ comment that’s constantly made. In my experience I’ve never hit a performance wall with Ruby. In scenarios where I thought I was hitting a wall, it was generally down to poor architecture.

I wanted to do a quick test to see how it compared to [another blog post that measures lambdas](https://read.acloud.guru/comparing-aws-lambda-performance-of-node-js-python-java-c-and-go-29c1163c2581). I forked this [repo](https://github.com/aykutaras/lambda-platform-perf-comparison) which is a few forks down from the original repo. I added a Ruby lambda to the repo, deployed with serverless framework, installed [artillery](https://artillery.io/) and ran the test -> artillery quick — duration 3600 — rate 10 — num 1 [https://xyz123.execute-api.us-east-1.amazonaws.com/dev/ruby25](https://37yg9txa0e.execute-api.us-east-1.amazonaws.com/dev/ruby25)

> Repo + branch [here](https://github.com/ryanjones/lambda-platform-perf-comparison/tree/ruby) on my Github

I also did some quick math to make sure I wasn’t going to go over my total lambda counts for the month as this was on my personal account ;)

I was bracing myself for dire news……. I popped open the console, and….

Victory!

Here’s what it looks like. I’ve done my best to match the performance/line colors to the other blog post.

CloudWatch Line GraphCloudWatch Number GraphCloudWatch Insights DataCloudWatch Insights Visualization

Alright, I relent. **It’s average is quite a bit slower that other languages**. But with that being said, I’m still happy with the outcome. It’ll be worth running again when Ruby hits 2.6.

I’ve always had a passion for Ruby and it’s fantastic stdlib. Turns out I’ll be able to continue to hack away in serverless land w/ Ruby on AWS without any major performance/cost concerns.

There’s another framework out there called [Jets](http://rubyonjets.com/) that runs Ruby on AWS, but I’ve always been leery as Ruby wasn’t natively supported by AWS. If Jets starts to target native Ruby lambdas I might have to give it a shot.

My team and I have been joking about running Rails in a lambda… Let’s see who unleashes the insanity first. Happy hacking.

Notes:

*   If you have any thoughts/critique about the approach, please let me know. I did my best to try and mirror the tests.
*   Artillery output after completing all of the tests:

Artillery Output
