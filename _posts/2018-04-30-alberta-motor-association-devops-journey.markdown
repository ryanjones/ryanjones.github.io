---
title: "Alberta Motor Association’s DevOps Journey"
date: 2018-04-30
categories: [General]
---
We’ve been doing DevOps at AMA for about 4–5 years now. Some teams are farther along than others, however, we’re all pointed in the same direction. If you’re unsure as to what DevOps is take a quick peek here: [https://www.visualstudio.com/learn/what-is-devops/](https://www.visualstudio.com/learn/what-is-devops/)

Now, we’re a relatively polyglot organization when it comes to programming languages. My teams mostly focus on Ruby on Rails, .NET (Forms, MVC, Core) and we’ve been trekking towards ReactJS on our front end as we’ve moved towards microservices. We’ve also just recently started using ReactNative (ES6 Javascript) to start building out our mobile capabilities in-house.

Before we get into it, it’s impossible to go from 0 -> DevOps. It takes time to get test coverage, continuous integration, continuous deployment, scriptable infrastructure, etc.

**DevOps is a journey.**

![](/assets/img/medium/a_1.png)

Where does the journey start? What is the order of events to start to become DevOps? As I’ve taken over teams that don’t have many of the above DevOps capabilities, it’s pretty simple. It starts with the culture.

**Culture**

One of the big reasons I’ve always been a big fan of Ruby on Rails, is that right at the very core, it’s focused on tested code. Over the last 10 years of software projects I’ve never once seen unit testing on a .NET project, a MS Dynamics project, or hell, even a COBOL project.

I feel the industry is changing for the better, but there’s still a hesitation against unit testing because it ‘costs too much money’.

At AMA we’ve engrained the value of unit testing and it’s become part of what we do. To the point where our development teams have become uncomfortable without tests.

**Writing Tests**

![](/assets/img/medium/a_2.png)

It’s not to say there aren’t challenges. Some of our monolithic .NET apps have taken time to get the proper frameworks in place. Our general approach is that but we keep taking small slices of unit tests for each new feature and we’ve grown our test suites over time.

For some teams we still need manual testing/QA to help supplement the automated tests. Over time this has become less and less frequent.

We’ve found that manual/QA testing has been instrumental for outsourced projects that we don’t have the capacity for, in house. When we don’t have full control of the stack, the QA tester has insights into the product quality that we don’t necessarily have.

**Continuous Integration**

We use various SAAS to run our automated tests. Our test suites vary from integration tests (full end-to-end) to unit tests. Because we’re a polyglot organization, we’re not locked down to 1 specific tool.

For .NET/Dynamics we use Visual Studio Team Studio (VSTS) and their build services. They’re really geared towards supporting the .NET eco-system, so it easily supports what we’re trying to do. It also has some great integration(s) into Azure.

![](/assets/img/medium/a_3.png)

VSTS Build

For Ruby on Rails/NodeJS we use travis-ci. It integrates well with the unix environment that we use to run Ruby on Rails. It’s also had some great hooks into github.

![](/assets/img/medium/a_4.png)

Travis-CITest output from Travis CI

![](/assets/img/medium/a_5.png)

For React Native we use Visual Studio App Centre (formerly HockeyApp). It gives us some great functionality for running tests, replaying them, and troubleshooting build issues.

![](/assets/img/medium/a_6.png)

VS App Centre

One thing to note, we’ve never had good luck with any automated testing tools such as Ranorex/HP Functional Testing/Telerik Test Studio. We’ve moved completely away from those types of tools. Without the ability to mock endpoints and rollback databases we’ve found that those tools don’t work for the automated testing we’re looking for.

That being said, we’ve spent more testing those tools on COTS applications to see if we can automate some of the simple smoke tests that don’t do create/update/delete functionality in the DB.

**Continuous Deployment**

Code sitting to be deployed to production is a liability. Once it’s been signed off, the goal is to get it into production as soon as possible. Some of our teams deploy an upwards of 5 times a day, during the middle of the day, without business interruption.

For .NET/Dynamics we use VSTS to deploy to Azure. That being said, we still have some .NET applications on premise. In general we’re working these applications to the cloud, however, in some cases we run a hybrid where we do the build on VSTS and then deploy manually on premise. The goal here is to have the deployments fully scripted so once we get to Azure we can integrate those scripts (such as CRM solution deployment) into the build process.

![](/assets/img/medium/a_7.png)

VSTS Release — Disregard the ‘no tests’, VSTS doesn’t support .NET Core test integration at the time of writing this. We run the tests via command line in the build phase.

For Ruby on Rails/NodeJS we’ve been debating be using [Github hooks](https://developer.github.com/v3/guides/delivering-deployments/) to deploy our repos once a pull request has been reviewed. Our Ruby on Rails applications have the best test coverage out of any applications, but we had some bad experiences with Jenkins that caused us to be hesitant about automated deploys. We expect to be deploying automatically on PR merge by the end of 2018.

For React Native we use Visual Studio App Centre (formerly HockeyApp). Microsoft had a tool called Code Push that more or less has become the defacto deployment solution for Native JS mobile apps. They’ve integrated it into Visual Studio App Centre. This allows us to do a build, and then deploy the application out. This allows us to continuously deliver applications without Google/Apple app store approval.

![](/assets/img/medium/a_8.png)

**Automated Infrastructure**

In order to achieve peak operational efficiency, your architecture/app stacks needs to be automatically built. The end goal is that you can spin up a dev/test/prod environment extremely quick. This is a massive advantage as we start to go down the microservices path.

For the work we do on AWS, we’ve automated the majority of our architecture through using [Ansible](https://www.ansible.com/). We had tried out doing our own commandline/bash deployment frameworks and we tried out chef, but Ansible was a clear winner in terms of ease of use and support from the community (via playbooks).

Azure is mostly done by hand, no automation :(. Our Windows management has traditionally been on premise and hasn’t been automated. As we’ve moved to the cloud the same philosophy has carried over, however, we hope to start automating Azure via Powershell and eventually move on to [Azure Resource Manager Templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) as we move towards serverless infrastructure.

For our mobile solution we also chose AWS, however, for this we used [AWS Cloudformation templates](https://aws.amazon.com/cloudformation/). This is all serverless. As we move towards more serverless infrastructure I have a feeling we’ll be moving away from Ansible to using Cloudformation templates as we won’t need to be installing and managing packages on the server(s).

**Microservices/Serverless**

In order for us to be able to deploy smaller feature sets, more often, we needed to start moving down the microservices path. This has allowed multiple teams to work across a group of repositories, with very little conflicts. Using microservices has also allowed us to target specific subsystems in our monoliths and extract them without a big ‘lift and shift’.

On the same note, since you’re spinning up new services all of the time, you need the ability to spin up a service quickly. Serverless allows us to deploy code on the cloud (using something like AWS Lambda) and not have any infrastructure overhead. Server monitoring, patching, maintenance, etc. disappears.

**Monitoring, Alerting and Logging**

Monitoring, alerting and logging are some of the most critical things that your team(s) need to help support your applications. For logging/alert triggers we use [Loggly](https://www.loggly.com/) and for on-call management/escalation we use [OpsGenie](https://www.opsgenie.com/).

Application error catching, we use [Rollbar](https://rollbar.com/) for Ruby on Rails, [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) for .NET and [VS App Centre’s Diagnostics](https://docs.microsoft.com/en-us/appcenter/crashes/) for Mobile.

**The Journey Never Ends**

![](/assets/img/medium/a_9.png)

You will not be DevOps over night. In some cases it will take years in to move your systems (or pick new ones) that support DevOps. The goal is to slowly slice off high value pieces, write tests, and hook into a build/deployment pipeline.

Feel free to shoot me a message if you have any questions/comments or would like to chat about DevOps!
