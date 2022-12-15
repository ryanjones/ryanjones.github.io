---
title: "AWS AppSync Calculator"
date: 2019-03-09
categories: [General]
---

We’ve been leveraging AWS AppSync more and more recently and it really bugged me that AWS doesn’t have a calculator for it. As you start using more features such as websockets (which they refer to as “real-time data access and updates”) it starts to get really complicated.

I was doing it through an excel spreadsheet but recently I created a small react.js app deployed on AWS Amplify Console.

Feel free to check it out here: [https://micro-serverless.com/appsync-calc](https://micro-serverless.com/appsync-calc)

![Getting started](/assets/img/medium/c_1.png)

If you want a quick preview scroll down and click on ‘Populate Sample Data’ in the Sample Scenarios section. I’ve mirrored the scenario that AWS uses [here](https://aws.amazon.com/appsync/pricing/).

![Calc](/assets/img/medium/c_2.png)

I have the code hosted on AWS CodeCommit but I’ll flip it over to github and open source it over the next few weeks. If you want it sooner, shoot me a message.

Update: I've open sourced the calculator. You can download it [here](https://github.com/ryanjones/aws_appsync_calc)
