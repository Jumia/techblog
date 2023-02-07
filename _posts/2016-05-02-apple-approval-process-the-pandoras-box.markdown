---
layout: post
title:  "Apple approval process, the Pandora's box"
date:   2016-05-02 14:08:17 +0000
author: "Liliana Sousa"
thumbnail: /img/posts/pandora-apple-ios-icon.png
categories: [mobile]
tags: [apple, ios]
permalink: /apple-approval-process-the-pandoras-box/
---


<center>
	<img src="/img/posts/pandora-apple-ios-icon.png" style="width: 100px" />
</center>


During our experience releasing iOS apps, we already got them rejected a couple of times.    

**The Apple review process is a pandora's box.**    

You never know when they will perform the review, who and how.    


# When

Internet mythology around Apple says that the best day of the week to submit your app is Thursday. But you’ll never know for sure if that’s true and data from our experience does not say that.

See it for yourself:


<table style="width: 100%">
<tbody>
<tr>
<th>Date of Submit</th>
<th>Day of the Week</th>
<th>Date of Approval</th>
<th>Duration (days)</th>
</tr>
<tr>
<td>07/10/2014</td>
<td>Tuesday</td>
<td>17/10/2014</td>
<td>10</td>
</tr>
<tr>
<td>27/10/2014</td>
<td>Monday</td>
<td>31/10/2014</td>
<td>4</td>
</tr>
<tr>
<td>04/12/2014</td>
<td>Thursday</td>
<td>12/12/2014</td>
<td>8</td>
</tr>
<tr>
<td>07/01/2015</td>
<td>Wednesday</td>
<td>16/01/2015</td>
<td>9</td>
</tr>
<tr>
<td>04/02/2015</td>
<td>Monday</td>
<td>06/02/2015</td>
<td>2</td>
</tr>
<tr>
<td>13/03/2015</td>
<td>Friday</td>
<td>20/03/2015</td>
<td>7</td>
</tr>
<tr>
<td>23/03/2015</td>
<td>Monday</td>
<td>08/04/2015</td>
<td>16</td>
</tr>
<tr>
<td>14/05/2015</td>
<td>Thursday</td>
<td>22/05/2015</td>
<td>8</td>
</tr>
<tr>
<td>02/07/2015</td>
<td>Thursday</td>
<td>09/07/2015</td>
<td>7</td>
</tr>
<tr>
<td>10/09/2015</td>
<td>Thursday</td>
<td>12/09/2015</td>
<td>2</td>
</tr>
<tr>
<td>26/11/2015</td>
<td>Thursday</td>
<td>03/12/2015</td>
<td>7</td>
</tr>
<tr>
<td>04/02/2016</td>
<td>Thursday</td>
<td>12/02/2016</td>
<td>8</td>
</tr>
<tr>
<td>04/03/2016</td>
<td>Friday</td>
<td>10/03/2016</td>
<td>6</td>
</tr>
<tr>
<td>13/04/2016</td>
<td>Wednesday</td>
<td>18/04/2016</td>
<td>5</td>
</tr>
</tbody>
</table>


# Who

Yes, unfortunately, having your app rejected or not also has the “who” factor.

We had apps being rejected for reasons that were in place for several versions before… and they passed until someone decided it shouldn’t.

Tip: you should always try to argue responding on Resolution Center before resubmitting the app. Resubmit the app makes the process to restart from zero… responding to the issue reported might save you some time if you really don’t need to change your app.


# How

There are some obvious reasons for which your app can be rejected.. but some others you would never think about it.

But some reasons are peculiar and it’s worth to take a look and review them before submit:

* Metadata rejection
	* The app name should be exactly the same as on the ipa. No keywords should be added on this field.
	* On the description do not mention brand names and SPECIALLY do not mention the word Android (yes.. big FAIL but we’ve been there :) )
	* Screenshots must show the app as your submitting. If you’re making a redesign update you should replace the screenshots at the same time.
	* Avoid showing brands or Android devices on the screenshots.
	* If your app requires authentication, provide an account for them to test that is working as expected.
	* Make sure all links (like privacy policy) are working correctly.
	* If your app uses location and this usage is not optimized, you will need to add to the app description information about battery usage “Continued use of GPS running in the background can dramatically decrease battery life.”.
	* Provide on “What’s New” field only what they will be able to test during review. If a feature is not active at that time you might get you app rejected.
* SPAM detector
	* If you have two similar apps for different regions in the world you probably think you could use the same icon for both. Don’t, Apple might accuse you for SPAM. Ideally merged the apps into only one. If not possible, change the logos even if they are just slightly different.
* UX
	* Do not request data from user that is not essential to use the app. Ex: if on registration form you are requesting “Gender” or “Birthday” as a mandatory field, and these are not relevant for the app usage, Apple will ask you, at least, to change to optional.
* Code
	* You app shall not crash :)
	* You are not allowed to use private libraries, you can only use APIs which Apple has cleared for public use.

Official info about reasons for rejection: [Common App Rejections](https://developer.apple.com/app-store/review/rejections/)
