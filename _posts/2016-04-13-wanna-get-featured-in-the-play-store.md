---
layout: post
title:  "Wanna get featured in the Play store?"
author: "Liliana Sousa"
date:   2016-04-13
thumbnail: /img/posts/google-play-store-android.jpg
categories: [mobile, product-ux-design]
tags: [ios, apple]
permalink: /wanna-get-featured-in-the-play-store/
---


<center>
	<img alt="Google Play" src="/img/posts/download-play-store-600x220.png" style="width: 90%" />
</center>

If you have a considerable amount of downloads and good reviews on your app, you might get eligible to be featured in Play store. But before, Google will review your app and they are pretty picky in this process.

# Here are some basic “rules” you will need to follow (with love, from Google)

**Generic**

* Your app's targetSdkVersion must be set to the latest version in order to ensure that newer platform behaviors such as hardware acceleration and the correct default visual theme are applied.

**Permissions**

* Don’t request READ_LOGS permission on your app. Log entries contain the user’s private information, so this permission has been labeled as “Not for use by third-party applications” in the Manifest.permission guide. Instead, use ‘UncaughtExceptionHandler’ to obtain crash data
* To dial a phone number, your app should use the ACTION_DIAL action, which does not require the CALL_PHONE permission.
* The following permissions could be used to access sensitive or personally-identifiable information. Please either remove these permissions, or raise your app’s maturity level to Medium or High.
	* READ_SMS
	* READ_CONTACTS
	* READ_CALL_LOGS
	* READ_PHONE_STATE

**Notifications**

* Your app’s notification icons in the status bar must be completely white, with no colors.
* If your app creates a notification while another of the same type is still pending, avoid creating a new notification object. Instead, stack the notification.

**Store content**

* If you have wearable version, make sure to include at least one Wear screenshot in your Google Play listing. The screenshot should be added to the end of the phone category.
* The images under “screenshots” in your Google Play listing can not be all modified with additional promotional text/graphics. Some promo slides are acceptable, but you need to upload at least one unaltered, high resolution (1280 x 800 minimum) screen capture for phone, 7-inch tablet, and 10-inch tablet to give users an unmodified view of the UI before downloading.
* Make sure you add a link to your app’s privacy policy in “Additional Information” in the Google Play listing (under “Contact Developer”)

**UI & UX**

* The navigation drawer should follow the new visual guidelines and be implemented using DrawerLayout. Update your drawer pattern to match the following design guidelines:
	* The navigation drawer appears in front of both the app bar and screen’s content. It should also underlay the system status bar
	* Should be in list view (1 item per row).
	* Should not contain nested (lower-level) screens. Use a collapsible listview instead.
	* Swipe in/out from edge of screen (on any screen).
	* Selecting a drawer section should load content within the current activity.
	* Highlight drawer sections. When you open the navigation drawer from a screen that is represented inside the drawer, highlight its entry in the drawer. Vice versa, if you open the drawer from a screen that is not listed in the drawer, none of the items of the drawer should be highlighted.
	* Any view represented in the drawer has a navigation drawer indicator in its action bar that allows the drawer to be opened by touching the app icon.
	* The Navigation drawer should be accessible from anywhere in the app, the drawer enables efficient navigation from lower-level screens to other important places in your app. Any view represented in the drawer should have a navigation drawer indicator in its action bar that allows the drawer to be opened by touching the app icon. Currently the drawer is not accessible at first launch.
	* Switching between navigation drawer sections shouldn’t grow the backstack. At most there should be one top-level backstack item for the Home section.
* All interactive elements should have touch feedback (a visual state change when touched). On Android 5.0+, use surface ripples at the point of contact when users interact with UI elements.
* On Android 5.0+, the status bar should be colored to be a slightly darker version of the app’s primary color, or the current screen’s content. For full-bleed imagery, the status bar can be translucent. Set the android:colorPrimaryDark or android:statusBarColor attributes in your theme (drop the android prefix if using AppCompat) or call Window.setStatusBarColor.
* Avoid right-pointing carets to stay consistent with the platform and in order to not have the user guess as to what the meaning of those carets may be.
* Use standard fixed or scrolling tabs or Play Store-style tabs.
* To provide search in your app, use the SearchView widget as an item in the action bar. Like with all items in the action bar, you can define the SearchView to show at all times, only when there is room, or as a collapsible action, which displays the SearchView as an icon initially, then takes up the entire action bar as a search field when the user clicks the icon.