---
layout: post
title: Jobber's React Native migration - Success!
date: 2023-06-19 15:05 -0600
category: ['React Native', 'Jobber' ]
---

![Jobber](/assets/img/2023/react-native-migration/jobber_logo.png)

January ‘23 was an exciting month. Not only did we raise a [Series D](https://betakit.com/jobber-closes-100-million-usd-series-d-amid-strong-demand-for-home-services/) but we launched our new version of the Jobber app which is now powered by React Native. This post is mainly coming from the engineering lens. If that’s your jam, continue reading!

I’m going to try and provide as much insight as I can to what happens ‘behind the curtains’ on one of these types of projects.

Don’t care about the story? [Jump to the results](#the-results).

## The decision

Jobber has been growing fast. With that growth has come with it a set of expectations on both of our online and mobile offerings. Our mobile offering in particular needed some TLC (Tender Loving Care). We’d been running on the ember.js framework (wrapped webview) which we had launched back in Aug 2015.

The reality is that ember.js wasn’t built for mobile apps and it was starting to show. Basic native functionality that customers expect (pull down to refresh, swipe for nav) either took a long time to build or just wasn’t giving us that ‘native mobile app’ feel. As the mobile app volume started to increase we started to hit odd edge cases/errors and it was becoming really tough to debug the issues. In addition to all of that it was starting to get really hard to have multiple teams working in the app’s codebase.

The state of the app was normal for a startup that was trying to find product-market fit but we were past that stage and our customers deserved better. In October ‘21 we decided to do an ‘all stop’ and focus 80% of our effort to accelerate the rebuild of our mobile app.

## History

![Jobber React](/assets/img/2023/react-native-migration/jobber_react.png)

Alright, I know what you’re thinking. ‘A rebuild!? Are you nuts?’. I agree with you. I absolutely, to the core of my being, hate re-writes. I’ve seen more than enough go sideways in my career. Everything from under estimating to scope increases to leadership losing their appetite for a hold on feature work.

But, let’s rewind a bit. Context matters. We didn’t go into this one blind. We did a lot of exploration before we pulled the pin, here’s what we did before we made the decision to accelerate the build:

* **2020 - March** - Decision to move forward with React Native as our mobile framework
* **2020 - April** - Explore how we can migrate to React Native without a big bang. Pulled React Native into the app ‘behind the scenes’
* **2020 - August** - Decision to adopt GraphQL as our API layer (to power mobile, web and our public API)
* **2020 - September** - First 2 features launched in React Native
* **2021 - January** - A few more features launched in React Native
* **2021 - March** - Replaced the Navigation
* **2021 - July** - A few more features rebuilt in React Native
* **2021 - October** - Decision to Accelerate build

We had done a lot of way finding along the way and even started to migrate the app without having to do a ‘big bang’. This wasn’t a full YOLO moment, but we definitely had some hurdles to overcome.


## The scope

Alright, so we’re going to accelerate the build. What does this mean? Well, it means a lot of other things are going to need to be accelerated. Here’s the list of known hurdles that we had in front of us:

* Train 50+ software engineers in React Native
* Train 50+ software engineers in GraphQL
* We need to design the new experience (we opted to improve a few key experiences)
* Build out our mobile design system
* Hire 30 more software engineers in the middle of the build and ramp them up 😱

I remember laying in bed the night after we (myself, CTO and VP, Product)  made the call to accelerate the build, staring at the ceiling with a mix of excitement and dread. On one hand, this would be a huge win for the mobile app, on the other hand we had massive hurdles to overcome. Any one of the hurdles by itself would be tough to overcome. 

## The goals

These types of projects are tough because you’re trying to accomplish multiple things at once and you end up with competing priorities. We landed on these prioritized goals for the project:

1. The app is easier and more productive to develop consumer-grade experiences in
2. Our app is more reliable – it has fewer bugs, fewer crashes, fewer white screens (unhandled exceptions)
3. We have improved the user experience for our SPs
4. Our calendar is more powerful
5. Our app meets a minimum bar for performance
6. All of the above is live by Aug 30/2022
7. There is no ember.js left in the app by Feb 15/2023

We landed on Aug 30 based on a few high level estimates from a few teams who had the most experience with React Native and GraphQL. Since we had made the decision to accelerate, it was better to start executing and refine those dates as we progressed.

Every week we would meet to review each of the above goals and any associated metrics as a 🔴/🟡/🟢 projection. Some of them are tough to measure (e.g. How do you measure engineering productivity? Lack of observability in the old app ruled out many direct comparisons on performance and stability).

## The build

We’re a big fan of the triad model at Jobber where we have equal discipline (eng/product/design) representation at the table as we build. We had a triad overseeing the whole project. Within the project we had multiple teams shipping within their areas (e.g. Our fintech teams would re-write the fintech functionality). 

I absolutely hate ‘like for like’ conversions so we opted to add some improved functionality in this project. Specifically a few areas that we couldn’t have made better in the old ember.js framework. They were an improved schedule/calendar, revamped settings drawer, revamped file upload control and much better form UX. We accepted the scope increase for the benefit of our customers.

From November 2022 to Feb 2023 you can imagine a flurry of activity of designing, building, collaborating and shipping to the app. Here’s the noteworthy milestones.

* **April 2022** - Alpha ships 
* **July 2022** - Beta ships
* **August 2022** - Code freeze for GA (‘Opt in to new version’ for users)
* **Sept 2022** - Rolled out new functionality to new users
* **Oct 2022** - First wave of forced rollout
* **Nov 2022** - Second wave of forced rollout
* **Nov 2022** - Oh 💩- Low perf device issues
* **Dec 2022** - Third wave of forced rollout
* **Jan 2023** - Final wave of forced rollout to low perf devices
* **Feb 2023** - Ember.js fully removed

Ultimately we managed to ship the new React Native functionality by August but it took a few more months to fully roll it out and remove ember.js. This was great as we had the better version of the app in our customers hands as soon as possible.

### Things that went well

#### Roll out React Native beside ember.js
We rolled out the base framework underneath ember.js to help prove out the build and migrated some core components such as the navigation bar, home screen and communication center. 

This gave us enough confidence in the tech to push forward and make the full conversion.

#### Tech lead syncs
Every week the tech leads from the teams would sync up and either highlight upcoming changes or raise a hand to help get support from other teams.

#### Strike teams on common components
As common components were identified, having an individual contributor from each of the teams work on the shared components as a short-term strike team worked well.

#### Mobile components team
We spun up a new team to help support the mobile iOS/Android work and added to our components team which helped accelerate all of the other teams. This approach was so valuable that we’ve mirrored a similar approach as we migrate our online interface to React.js

#### User testing
Testing functionality with users and non-users gave us early feedback that allowed us to iterate before we invested in the build. 

Through the project, our entire processes around design review and user testing leveled up. We continue to benefit from our newly established best practices around early user engagement feedback.

#### Observability
Purchased and implemented [sentry](https://sentry.io/welcome/) to provide crash/performance metrics and make finding, assigning and fixing issues easier for teams. 

#### Team health
Engage teams with qualitative surveys to understand if our code health, build speed and release process was improving.

#### Migration metrics
We kept a keen eye on our technical metrics to make sure that we were progressing to plan. A few of these were:
* Ember.js vs. React Native routes converted
* GraphQL adoption metrics

#### Bitrise
Unified both iOS and Android building to [bitrise](https://bitrise.io/) for our mobile build pipeline. Much better than the older Buddy Build system which was purchased by Apple, put into hibernation, and then shut down by Apple.

#### Runway
Implemented [runway](https://www.runway.team/) to help with our mobile release process.

#### React native upgrade
We started to hit a few snags along the way due to being behind a few releases of React Native. Popping off the upgrade helped us get over those hurdles.

#### Split
We implemented [split](https://www.split.io/) on mobile which helped us control the rollout and alpha/betas with feature flags.

### Things that didn’t go well

Every project of this size will have bumps and bruises along the way. 

#### Fine grained iterative deploys
We originally wanted to deploy each screen behind feature flags to allow the most granular level of deploys, but when you mixed in different SaaS plan types it became really hard to manage. We also ran into complex navigation issues between ember.js nav stack and React Native’s nav stack. We did use feature flags but we ended up leaving them at the SaaS plan level.

We took 3 runs at this before bailing. We just couldn’t manage the complexity of multiple frameworks effectively.

#### 1 dedicated principal IC
We should have assigned 1 principal engineer to the project to help coordinate and overcome some of the engineering challenges. We had 3 key individual contributors take on this mantle 1/3rd of the way through the project, but it would have been better to assign this early on.

#### Oh 💩- Low performance device issues
Woof. We took a few app review hits on this one. We started to see a few bad reviews come in regarding performance and we realized that we didn’t have a wide enough set of test devices on Android. We didn’t see this in our alpha/beta group as we learned that they didn’t reflect the device spread for the general release. The other issue was that our simulators didn’t reflect the realities of the slower devices like we had assumed. 

We rectified this quickly by: 
* Implementing a bunch of optimizations via memoization/use callback (using Shopify’s lovely [perf libs](https://github.com/Shopify/react-native-performance) ❤️🇨🇦) to improve performance.
* Purchasing and distributing the lowest performing Android phones (the slowest android phones on the market) to every team to be used for all testing going forward.
* Build out a test framework on AWS’s Mobile device farm w/ Appium to smoke test across many devices.

#### Datadog migration 
We needed to migrate from New Relic to Datadog through Oct/Dec due to New Relic not willing to budge on user licenses. This was expected but really hampered a few of our teams during the project.

#### Features slipped in
We had 1-2 teams that opted to hold off on converting their section of the app to pop off a few things they had on their plate. This squished them at the end of the project. We ended up adding 3-4 more experienced React Native engineers to the teams to hit the deadline.

## The results

### The data

It’s been a few months since we’ve launched and now it’s time to present the outcomes! Let’s cycle back to the goals:

1. The app is easier and more productive to develop consumer-grade experiences in. We fast followed with a slew of new features such as our [new map view, offline mode, arrival windows and more](https://productupdates.getjobber.com/)! ✅
* Our qualitative data from the teams saw our health of codebase, speed (team), and ease of release hit some all time highs. We saw a slight dip in the health of codebase category post-launch due to moving into different codebases unrelated to this project.
![Health Checks](/assets/img/2023/react-native-migration/health_checks.png)
2. Our app is more reliable – it has fewer bugs, fewer crashes, fewer white screens (unhandled exceptions) ✅
* Holding well above 99.9% crash free sessions - a lack of observability between the previous app confounds direct comparison, but we are confident we were well below this threshold.
* Since launch, this has further improved from ~99.95 to ~99.98.
3. We have improved the user experience for our SPs ✅
TODO
* App store
* All of our app store ratings are at, and remain at, an all time high
  * Android (Google Play) 🇺🇸 4.4 -> 4.8 
  * Android (Google Play) 🇨🇦 4.1 -> 4.3
  * iOS (App Store) 🇺🇸 4.6 -> 4.7
  * iOS (App Store) 🇨🇦4.5 -> 4.6
* [AARRR metrics](https://www.productplan.com/glossary/aarrr-framework/)
  * Approved across the board (very large improvements in activation and conversion metrics)
4. Our calendar is more powerful ✅  
Old calendar screen  
![Old Calendar](/assets/img/2023/react-native-migration/old_calendar.gif){: w="300" h="400" .normal }  
New calendar screen  
![New Calendar](/assets/img/2023/react-native-migration/new_calendar.gif){: w="300" h="400" .normal }
5. Our app meets a minimum bar for performance ✅
* Cold starts - We’re coming for you android. We’re working to grind that down even more. Median start times from ~15s to ~5s.  
![Mobile Performance](/assets/img/2023/react-native-migration/mobile_perf.png)
6. All of the above is live by Aug 30/2022 ✅
7. There is no ember.js left in the app by Feb 15/2022 ✅  
![React Native Routes](/assets/img/2023/react-native-migration/rn_routes.png)

### The beauty

Before I close here’s a few more comparisons 😍. The new style and UX has received rave reviews from our customers.

#### Quote Screen  
Old quote screen  
![Old Quote Screen](/assets/img/2023/react-native-migration/old_quote_screen.gif){: w="300" h="300" .normal }  
New quote screen  
![New Quote Screen](/assets/img/2023/react-native-migration/new_quote_screen.gif){: w="300" h="400" .normal }

#### Client Screen  
Old client screen  
![Old Client Screen](/assets/img/2023/react-native-migration/old_client_screen.gif){: w="300" h="400" .normal }  
New client screen  
![New Client Screen](/assets/img/2023/react-native-migration/new_client_screen.gif){: w="300" h="400" .normal }

#### Visit Screen  
Old visit screen  
![Old Visit Screen](/assets/img/2023/react-native-migration/old_visit_screen.gif){: w="300" h="400" .normal }  
New visit screen  
![New Visit Screen](/assets/img/2023/react-native-migration/new_visit_screen.gif){: w="300" h="400" .normal }

## Other interesting metrics

### GraphQL adoption

GraphQL API usage/surface area increased dramatically. The data stopped when we migrated over to Datadog in Nov but we’re at almost ~80% GraphQL traffic now with our goal to deprecate the REST API by EOY. [Check out our public GraphQL API!](https://developer.getjobber.com/apps/).

![REST Deprecation](/assets/img/2023/react-native-migration/gql_rest.png)

Our GraphQL API covers almost all of the functionality available in Jobber’s online and mobile offering.

![GraphQL Types and Field Growth](/assets/img/2023/react-native-migration/gql_fields.png)

### Mobile build time
Our mobile build times were cut in half 😱

![GraphQL Types and Field Growth](/assets/img/2023/react-native-migration/mobile_build_times.png)

And, we went from deploying our app every few weeks to shipping a new version every week.

## Closing
I’m incredibly proud of the team and what we’ve delivered. Our customers love the new mobile app and we’ve already shipped a ton of new features in record time! Check out our [changelog](https://productupdates.getjobber.com/) (Mobile App).

We have a lot of future plans for Jobber’s mobile and online offering. Enjoy React? React Native? GraphQL? Rails? Helping small businesses? [Come join us at Jobber](https://getjobber.com/about/careers/#open-positions)! We just [raised a Series D](https://betakit.com/jobber-closes-100-million-usd-series-d-amid-strong-demand-for-home-services/) to grow the business and become the #1 market leader in the home services space.

Thanks for reading! Want to chat? Click on the links in the sidebar and drop me a line on your platform of choice.