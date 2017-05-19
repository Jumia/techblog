---
layout: post
title:  "Mobile first?"
author: "Liliana Sousa"
date:   2016-03-17 
thumbnail: /img/posts/mobile-first-300x187.jpg
categories: [product-ux-design, mobile]
tags: [ios, apple, android]
permalink: /mobile-first/
---


<img alt="Mobile first" title="Mobile first" src="/img/posts/mobile-first-300x187.jpg" />


For the past couple of years we've been talking about mobile first.   
Are we mobile first?   
The real question is: **Should we be mobile first?**   



# Let's look into the market a bit

Beyond mobile first there's also the mobile only concept. *Myntra* did it. They shut down their website in May 2015, redirecting all user to the mobile apps. 

<img src="/img/posts/myntra_offline.png" alt="Myntra shutdown" style="width: 100%;" />


Did it work? Market watchers say it didn't But Myntra didn't move back, they should know what they are doing. And that's the main point!


<img src="/img/posts/myntra_news.png" alt="Myntra news" style="width: 100%;" />


> It's not mobile-first; it's user-first. With the user at the center of his own world his overall experience needs to be improved. It just so happens that his journey usually starts (and indeed, can entirely happen) on mobile.


#  So… what should we do? Should we follow the mobile trend?


Yes, definitely! But not blindly… do it wisely!

We should focus on users. Not only customers but all users! We should deep analyze our user's behavior before taking any decision.

<center>
	<img src="/img/posts/mobilefirst_how.png" alt="How" width="200px" />
</center>

## 1. Good tracking system and concept

UX tracking != Revenue tracking

* Whole dev team should have access to data.
* Events for each feature (every ticket must have a subtask for tracking)
* Clear events to clearly understand user actions in the apps


In UX tracking we don't really care about real data associated to the actions -- like IDs, values, etc... We just want to understand which actions user is doing, how he uses the apps.


**If using GA (or GTM)**

* Category &#8211; use it for the feature / section name
* Action &#8211; use it for the user action
* Label &#8211; use it for extra info (where / how) or leave it empty

Example:

* Wishlist &#8211; Add &#8211; Catalog
* Wishlist &#8211; Add &#8211; PDV
* Wishlist &#8211; Delete &#8211; Catalog
* Wishlist &#8211; Delete &#8211; PDV
* Wishlist &#8211; Delete &#8211; Wishlist

## 2. A/B Testing

Powerful tool and very good if well used. I'm not a fan, if tracking is really good and we don't want to test many changes, we could avoid using it. Why?

* Impact in performance
* Increase of the SDK size


If used try to have a very well defined process on how to use it. Marketing teams should not exclusively own A/B testing.


##  3. Users feedback

Take your users feedback very seriously

* Try to get local feedback, if possible
* Read all reviews on the store
* Read all emails sent to the mobile app support inbox
* Answer bad reviews and encourage users to explain their issues via support email


# Let's see a real use case we had in AIG.


While doing image upload in the app, users were complaining the fact they were forced to crop the product images into squares. As a PO, it would make totally sense to force it, images in backend were squares… they should upload with crop! We decided to add a no crop option and we added proper tracking to analyze user behavior on its usage.


<img src="/img/posts/mobilefirst_ga_aig.png" />


So.. no crop won by far to crop.

**Decision**: keep both options.

**User feedback + Tracking = Conscious decisions**