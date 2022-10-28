---
layout: post
title:  "10 Basic Tips to Build a WebAPI for Mobile Apps"
author: "Liliana Sousa"
date:   2016-04-05
thumbnail: /img/posts/retro-thumb.jpg
categories: [mobile]
tags: [api, ios, android]
permalink: /10-basic-tips-to-build-a-webapi-for-mobile-apps/
---

# 1. Build an API for Mobile!

If you think developing an API is just knowing PHP or whatever language you use, you are very wrong.

Please search for a good book or documentation about APIs and get to know how architecture / methodology should be.

Think that data will be consumed by mobile devices. And if you live in a perfect world with WiFi and good connection everywhere… think that your users might be in a remote place in Africa with a sh*tt* phone and they still want to be able to use your app.


# 2. Keep logic on the API, App should be stupid!

Yes, app should be stupid, app should know NO business logic at all!

Your business is not static, it changes often. Think of your app as a stupid display and have as much as possible all logic on your API. Changes should be as much transparent as possible for the users, with no need to update the app.

API deploys are instant while on apps might never happen.

# 3. Less is More < = >

Don’t just dump a DB table into a JSON and send it to the apps.

When designing a JSON response look into designs and talk with apps dev team in order to just send data that will actually be used.

Again: less data = more speed.

# 4. Number of requests vs amount of data

This one is tricky.

On one side you want data transfer to be fast so you tend to have partial requests with less data.

On another side, if you can have some more data on one request you could reuse it in another screen without having to contact the server again.

So, here, you need to study case by case and decide wisely. If we’re talking about secondary data, on a section of the screen that user needs to scroll to see… maybe you can have it in another request that is being done in background and screen will update on scroll.

If we’re talking about a small amount of data that is used in the main section of the screen then maybe worth to get it all at once.

This is a critical point for user retention, if apps load forever user will just quit and go to another app.

# 5. Dynamic layout – Backend configurations FTW

Use and abuse from backend configurations.

Effort is not big to have them and they can save you later.

Use configurations to activate / deactivate features. Use them to hide / show sections of data on screens. Use them for things that are always changing or for things you would like to easily A/B test.

Real example: FB login is a feature for which we have a configuration to activate / deactivate. Login screen adapts accordingly, with no app update needed.

<img alt="fb FB_comparison" src="/img/posts/FB_comparison-1024x931.png" style="width: 100%;" />

# 6. Cache it but be smart

Cache on app side, yes. User still wants to be able to see your products when he’s in the elevator. If he has seen it before, he should be able to see it when offline.

But have a smart system for critical data as, for example, product prices or stock.

Don’t cache based on time, that’s stupid… i might be requesting data that didn’t change or not updating data that changed and it’s critical to show it to the user (like a time limited promotion).

Have some kind of way to be notified on apps by API that data changed or have your app pinging API to ask if data changed (in background).

# 7. Keep user logged in

Once user registers or logs in an app he should never be requested again to login.

But be careful about security.

Save a session on server side but not forever.

Build an auto login system, preferably based on tokenization, don’t store user credentials on app side!

You can use the token to auto login user every time your server session expires or, in background, every time he re-opens the app. Renew the token constantly, you can have your API sending a new token on each auto login response or find another way to exchange these tokens.

# 8. Apps updates

<img alt="Installs by device" src="/img/posts/installs_by_device.png" style="width: 100%;" />


Most users have automatic updates on, but some others don’t.

Encourage users to update every time you make an app release (wait 2 to 3 days until you’re sure there’s no critical bug in it).

You can build a mechanism that detects if user has the latest version of the app and suggest the update on app launch. And you can use an API request for it.

But don’t do it forever, activate it around 1 week after your release.

Why?

There are several reasons why users don’t update apps: they don’t have WiFi and don’t want to pay for it, the device does not support the update, the device has no space for the update, etc…

You may also have a mechanism to force the update, in case a big bug arises… but please avoid doing it and needing it. If suggest update might be annoying, force update might be a conversion killer.

# 9. Retrocompatibility – Versioning

<img alt="Installs by device" src="/img/posts/retro-940x272.jpg" style="width: 100%;" />

As said before, your business is changing all the time. Business logic change, new feature comes up.. API needs to adapt.

Plan very well changes made in API and evaluate if you need a new version.

Main rule is: if we’re adding data, no need for a new version; if we’re removing data or changing fields data type, we need a new version.

# 10. Monitor and improve, improve, improve

Yes, evolution never stops and our API is no exception.

Monitor performance, usage and everything possible. You can mainly use New Relic “Mobile” section to collect data.

Analyze data collected and change accordingly.

Cleanup data that is no longer needed, adapt requests / data retrieved based on results… just don’t stop!
