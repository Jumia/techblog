---
layout: post
title:  "Not your kind of automation"
author: "Nuno Santos"
date:   2018-09-17
thumbnail: /img/posts/not-your-kind-of-automation/manual-vs-automated.png
categories: [engineering-quality]
tags: [automation,teams]
permalink: /not-your-kind-of-automation/
---

<center>
	<img alt="Manual vs Automated" src="/img/posts/not-your-kind-of-automation/manual-vs-automated.png" style="width: 90%; margin-bottom: 10px;" />
</center>

## Introduction

Oh no, another post about automation? Well, kinda.

When we talk about automation on our industry we first think about deploys, continuous integration, delivery and so on. But, I won’t be talking about that today, it’s, let’s say, another level, important part of it. How many times you’ve done a very boring repetitive task that takes your attention for quite some time and you get frustrated by not having time for doing the cool stuff?

…

Yeah, that’s right, I know the feeling.

## Background

Inside Jumia, everyone knew that I had a huge set of scripts that did a lot of different things. Although that is good, the scripts were for my usage “only”. So the idea of sharing them was born. To make it happen, we thought of implementing a basic but powerful UI, which allows, execution of such scripts easily and be very user friendly. And so, we created JDash.

(At the end I share JDash stack for the curious ones.)

## Why does it matter

You can improve everyone’s work by doing so, from Developers to Top Management. The sky’s the limit. Also, you motivate your team by doing something different, you empower them, you make them more noticeable, and everyone can be involved in the tool / product evolution. So, how do I say this, our industry is full of talent, but, we still do not automate our most common tasks. And, honestly, we all are the one’s to blame. We do tech, so why don’t we take the time to improve our workflow by creating something that eases your and everyone’s life?

## Real example

Let’s show exactly how JDash began.

As you all know, we build e-commerce for several countries in Africa. For each, we have a backend and it’s own database, thus it’s specificity, like configurations. The first real scenario and feature of our tool was a simple Configuration Search. Imagine, having 12 countries, so, 12 databases / backends, where you must log into, search for the configuration, and get the value, versus, selecting a configuration for a given set of countries at a click of a button in which within less than 5 seconds you have your results. All at once.

Ridiculous!!

<center>
	<img alt="Manual vs Automated" src="/img/posts/not-your-kind-of-automation/configuration-search.jpg" style="width: 90%; margin-bottom: 10px;" />
</center>

Just think of how much time everyone saves with just this one feature.

Currently it also has some basic statistics, automate a hell load of features, it’s performant, has some easter eggs to be found and brings new technologies that can serve for something else, and, it won’t stop growing.

Below I share current major features:

- Basic statistics;
- Amount of new customers, new orders, sum of value of orders;
- Average orders per minute;
- Orders placed by device and payment method;
- Configuration search and audit (like a history of changes);
- Moving order status on test environments;
- Several syncs and resyncs between systems, like items, orders, stock, suppliers and others;
- Export of vouchers, customers, vendors and others;
- Overview of what is deployed on each test environment with several details like, when, git revision and others;
- Debugger for Android/iOS apps requests;
- And much more...

## Impact and how we made it possible

With such initiatives you get the attention of everyone, all the teams participate in order to improve what you’ve built, give feedback and suggestions. These kind of tools, empower people and make their life’s easier since every task is performed with efficiency and speed. On my team I do promote a simple Hackathon on every first day of a sprint. It’s here where new ideas come to daylight, just like this one did.
So, I do encourage you to do the same. Even with multidisciplinary teams, keeping high performance, without losing productivity and not abandoning your roadmap, it’s possible to do it, just plan it, organize your days, delegate and lead, trust me, everyone can do it.

## Conclusion

Apart from the obvious benefits of automating your daily, repetitive tasks, it has a huge impact on your team and your peers, and below are some key points:

- Involvement;
- Empowering;
- Peer recognition;
- Motivation;
- Inspiration;

With it, other benefits also arise. The team is able to work on something new, imagine, eating the same thing every day, yeah, you will be tired of it and may never eat it again. The same happens with technologies, and, developers are thirsty for new things. But that’s not all, in this blog post, as Team Leader, I do want you, that have a leader role to involve your team members in the creation of tools that ease everyone’s job, do regular small Hackathons with them, use this opportunity to use new technologies, motivate along the way, let them grow, give them space to experiment, take risks and do mistakes. You’ll see that only good things will show up.

## About Me

I’m Nuno Santos, started as developer at Jumia in 2012 and have been invited to be a Team Leader in 2015 and founded Ninja Bytes along with it’s members. A team that i’m proud of, being bold and innovative, brings innovation all the times and is always one step ahead leading Jumia to success.

Follow us:
https://ninjabytesteam.com/
https://www.facebook.com/NinjaBytesTeam/

## JDash stack

As promised here is the tech stack of JDash:

- PHP
- Laravel
- Bootstrap 3
- Vue.js
- MariaDB
- Redis (for queuing as well)
- Algolia
- NodeJS
- Socket.io
