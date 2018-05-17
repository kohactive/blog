---
title: Native vs. Hybrid Apps
subtitle: How to determine what mobile experience is best for your startup.
date: 2016-03-09 23:06 +0000
tags: Startups, Lean Startup, Resources
category: Programming
author_id: j_mcnally
image: https://s3.amazonaws.com/mediocre-production/uploads/image/filename/66/Native_v_Hybrid-01.jpg
banner: true
featured: true
---

One question that has come up since the first release of PhoneGap is, "Should we build a native app or a hybrid app?"

First, let’s break down what we mean when we speak of each.

#### What is a native app?

A native app commonly refers to an app created with Apple or Androids tools and languages. They run "natively" on devices without the use of a web browser. At a deeper level, a native app runs directly on a processor and has direct access to all of a device's hardware.
​
#### What is a hybrid app?

A hybrid app commonly refers to an app built using HTML, CSS, and Javascript--or a combination of the three--and runs on a web browser, and packaged in a custom application. Imagine you are using Safari, except it is branded as your app and it only loads your website.
​
## What is the difference between Native and Hybrid Apps?

At this point, a native app probably sounds more like an “app” in the sense that it’s something you download and install, and a hybrid app probably sounds more like a website. While this is basically true, hybrid apps are often purposely built as single page web applications.
​
Many hybrid app experiences are actually indistinguishable from native applications. While performance was previously a major problem in hybrid apps, modern phones have much better memory and processors, so this doesn't come into play nearly as much.

## Advantages of a Hybrid App

Hybrid apps also have some huge advantages. For instance, if you are already familiar with Javascript frameworks like AngularJS or EmberJS, you will instantly be able to understand how hybrid apps are made. This can be a huge win if you are outsourcing your mobile app to let your internal team focus on your web app. It will be easy for you to maintain without adding staff in the future.
​
If you don't have a team today but plan to hire one in the future, you can benefit from hybrid apps too. A team of Javascript developers will be able to support your website and your mobile app. Compare that to native apps, and you might need JS people, iOS people, Android people, and Windows people.
​
One of the most compelling arguments for hybrid apps that I give to startups is the reduction of cost involved in deploying to multiple platforms. Hybrid apps, for the most part, you write once and deploy anywhere. Since they basically use Webkit as a virtual machine, the code, UI, and APIs are 100% portable. In native apps, you would end up writing completely separate apps for iOS, Android, and Windows. Not only will that incur a ton of additional upfront cost, but maintenance will be more expensive too. New features will have to be developed 3 times.


## Advantage of a Native App

One of the big advantages of native code is being able to access hardware. As an example, accessing the camera is pretty awkward from a website. However, Cordova, one of the most popular hybrid app containers, provides a number of libraries to make low level access possible and pleasant from your hybrid app. You can see a list of some of the most popular <a href="http://ngcordova.com/docs/plugins/" target="_blank">hybrid app plugins here</a>. It is also popular to write your own native plugins, making your hybrid apps truly hybrid. This allows you to make 90% of your app reusable, which means you only have to maintain multiple platforms on the 10% that really need that native boost.
​
Native is, however, very good at a number of things that just don't feel right when done in the webview. I believe animation and drawing is still smoother in native. Very few games, for instance, would be well-suited to be hybrid apps. Also, given my previous examples, apps that create or edit media might be more well-suited for native, and the experience will be smoother. As an example, building Instagram with their filters or Vine/Snapchat with their video editing features would both work better as native applications.


## Final verdict: It depends on what you’re looking for.

If you are a startup that wants to be in both app stores, and your mobile app is a pocket version of your desktop application, seriously consider a hybrid application. You will cut your costs in at least half, have a huge talent pool, and be able to cross-train people to work on both. This is a great way to validate a mobile MVP, iterate quickly without scaling budgets and teams, and get valuable user feedback before investing in a native application.
​
If you are looking to build a very custom user experience, provide an app that is rich with editable media, or create a game, native is probably your best bet. Regardless, it might be interesting to explore how you could mix a hybrid app with custom native code.

If you need help determining whether you need a native or hybrid app, please let us know. <a href="https://www.kohactive.com/contact/">Fill out our Project Planner</a>





