---
layout:     post
title:      "A journey to Progressive Web Apps"
date:       2017-05-23 12:10:00
author:     "José Santos"
categories: [engineering-quality]
tags:       [pwa, web-components]
permalink:  /a-journey-to-progressive-web-apps/
thumbnail:  /img/posts/pwa-thumb.png
---

<center>
	<img alt="Progressive Web Apps" src="/img/posts/pwa.png" style="width: 90%" />
</center>


## INTRODUCTION

The internet has evolved a great deal since the first time I’ve access it with my 24 kbps modem. All flashy GIF are gone. Rarely I find these days, weird mosaic backgrounds or midi music playing jingles in Christmas time while snowy javascript animations falls from the top of the screen. This evolution came mostly from the creativity of developers who tried without rest to experiment technics and technologies to overcome the browser limitations. From Flash to Dynamic HTML ,Iframes to AJAX, innovation was always the goal to reach success.

But with the growth of the mobile access from cellphones and tablets, consumers created this new need of a more performant and reliable platform. Globalization also brought to emergent countries a new type of user thanks to the mobile. Those users from basic devices to flaky internet connection made developers to rethink how we build for the internet.
The progressive web apps user experiences were born and today let me tell you how in Jumia Travel we went from our standard mobile web site to a more progressive web approach to bring to our users a rich, reliable, fast an engaging experience.

## Jumia Travel

Jumia Travel is a leading traveling company with more than 25’000 hotels in Africa only. In our main market, sub-Saharan Africa, it’s important to know that 75% of the mobile connections are 2G networks. Many of our users have intermittent connectivity which make the "standard" user experience a little hard to love. Also the majority of users have a low-end device with data limitations at very high costs. This two factors make difficult to induce the download of our native apps leading to steep drop-off rates and high customer-acquisition costs.

We knew we could do better, we could find a way to both save data consumption and improve the user experience by delivering a faster web application.
To achieve just that we start working last year around the concept of Progressive Web App.

## Progressive web apps

PWA are user experiences that have the reach of the web and are:

1. Reliable

Load instantly and never show the downasaur, even in uncertain network conditions.

2. Fast

Respond quickly to user interactions with silky smooth animations and no janky scrolling.

3. Engaging

Feel like a natural app on the device, with an immersive user experience.

We found this definition on Google developer’s website. And what does it mean?
It should be an application that always works in any network condition, should be fast for users and let them interact as soon as possible even if the page was not fully loaded. Should feel like a native application, rich in user experiences and capabilities. It sounded good! It sounded like it solved all our issues for our users. But how to achieve just that.

## How we build our PWA

When building a PWA we faced a series of challenges. I could say the first one was the fact there was no much where to look at. In our perspective PWA’s are still a very young concept or idea and we didn’t know where to go and how to get there.

### The search for the values

Somewhere around February 2016 I started to investigate more on how I could build a web application that felt like a natural application. I was suggested to look at a new Google technology called Polymer. For me, it was my introduction to Web Components. Web Components looked nice, they are a set of four technologies, shadow dom, templates, html imports and custom elements and the latest caught my attention. I knew if I wanted to create a good performance application I needed to make a mentality change. This was going to be a total change of methodology.

### The custom way of building quality

Custom Elements are basically a native API for you as a developer to create custom HTML tags. We can apply a default css to style the element, attach javascript to define the behaviors of the element and set custom properties to react with the element.
With those custom elements I was able to orchestrate a single page application. Polymer not only introduced me to web components but also make it more productive. With polymer attached to my custom elements I had a set of features available to manage elements bindings, listeners or observers. I was able to compose elements with custom classes called behaviors, this extended the possibility to reuse code in order to avoid duplicated code.
From a single custom element to a full functional application seemed to be the next logical step and there was much more from the Polymer team to use in our advantage. Web components from  the Polymer catalog like the app-layout elements provided an application shell for us, and the Polymer Cli all that we needed to generate our service worker but also to build and deploy the application.

~~~~
class HelloElement extends HTMLElement {
  // Monitor the 'name' attribute for changes.
  static get observedAttributes() {return ['name']; }

  // Respond to attribute changes.
  attributeChangedCallback(attr, oldValue, newValue) {
    if (attr == 'name') {
      this.textContent = `Hello, ${newValue}`;
    }
  }
}

// Define the new element
customElements.define('hello-element', HelloElement);
~~~~
[Example from mozilla](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements)

We felt we were ready to go.

### Go hands on

In four weeks I build a prototype based in our Android Native App. Users were able to go from the home screen (home page) to a hotel list, hotel detail and hotel room page. It was a little more than half of the main funnel. We this prototype I was able to understand the effort in building Jumia Travel’s PWA but also I was able to compare some metrics in term of performance and data consumption, after all those were the main reason behind this project.
The numbers were promising:

* 2x faster in 2G connection

* 6x less data compare to the native app

A new challenge was ahead. How to deliver this app project to any of the development teams knowing that none had any experience in Web components or Polymer. But also how to transmit the idea and new mentality that building this web application was closer to build a native application than actually the standard client to server requests. This PWA acts more like a native app, with state management, highly cached data and a complete new type of architecture which brought to us as a new development model, the Demo driven development. With this model we create custom elements that are atomic systems, self maintaining and testable by unit.
This challenge was very important to me and I understood it was the only way this project could be successful. Internally I’ve promoted trainings for each team. It was both a theoretical and practical knowledge sharing.

### Bigger may not be better

<img style="padding: 0 0 16px 16px" align="right" src="/img/posts/app-build-components.png" alt="PRPL" />

As soon as the application began to grow beyond the prototype and closer to the expected final result, our tests started to show that we were failing in performance. The Progressive Web App was slower compared to the current mobile experience. One of the major risks to build a Single Page Application is to add too much code to load in order to start the application. An action was to be taken to take us on the right course again. We broke down the app in small pieces and make sure that we lazy loaded features only when needed. We used the PRPL pattern, which stands for Push, Render, Pre-cache, Lazy-load. The idea behind PRPL is to make sure that the first render happens as soon as possible, and to do that the developer should serve by priority elements that are visible and useful for the first interaction, using lazy-load techniques through html import, service workers or by HTTP2 push. The result is that some applications features were downloaded only when requested or by the service worker in the background if the application was idle. This allowed to slim down the app shell by 200% and to get a faster first render.

### Unexpected Roadblocks

With performance on track, we were using caching with the service worker for files and indexDB for data to make sure we were doing the best use of the user data consumption. Now it was time to face other new challenges, URL compatibility, SEO and Tracking are some of the examples.

URL’s are important, even more if your current website is already indexed in most of the search engines. To build a single application that knows how to respond from any request (organical or paid), we had to build custom elements that were able to understand the URI and to load the correct page fragment. Users were able then, to share URL that worked correctly between any of our websites or client applications. In this way we started to worry more about maintaining compatibility with our current system and thus with the SEO value that we already had.
To make sure we kept the app rich in current features and searchable, we added all the SEO content that we had in our previous mobile experience. All the meta data, title, micro data any semantic value, everything was there. But at this stage all this content was given after the load of the page from API calls and not by the server response. To tackle this issue we decided to reuse what we already had. Since Jumia Travel has a basic feature phone website, we used this experience to feed all the search crawlers with our SEO content. This way we were sure that any crawler without Web components support could "see" our SEO content. Since we wanted to serve our PWA into a serverless environment inside our CDN for faster download response, it was easier for us to avoid to worry so much about SEO in the PWA by using our current platform.
For tracking it was important to understand that in this architecture model, events like document ready or window load happend only once, and any pageviews should be conceptually considered fired when we were actually switching each page fragment since the entire application is running in a single page. In PWAs, Analytics aren't easy.

## The results

<img src="/img/posts/jumia-detail_2x.jpg" align="right" style="padding: 0 0 16px 16px" alt="Jumia Travel" />

One year after the prototype was started we launched the PWA for a limited set of users with specific browser versions. And while results were arriving we opened the restrictions to other browsers or browsers versions.

In May of 2017, Google published our results, we had

* 33% higher conversion compared to our previous mobile site

* 50% lower bounce rate

* 12X more users versus native apps (Android & iOS)

* 5X less data used

* 2X less data to complete first transaction

* 25X less devices storage required

[Find more information about Jumia Travel show case](https://developers.google.com/web/showcase/2017/jumia)

### Think back

<img src="/img/posts/jumia-travel-compatibility.png" align="right"  style="padding: 0 0 16px 16px" alt="Jumia Travel compatibility expo in 20 devices at Google I/O'17" title="Jumia Travel running in 20 devices at Google I/O'17" />

For me, working in this PWA was a unique experience and a lesson on how to think and recreate the concept on building application for the web. Developers have been asking for a more centric technology and tools to build modern applications that breaks all the conceptual rules of the old web. With web components and service workers we were given the tools to extends our creativity beyond our limits. I was able to reorganize all the vendors tools that I’ve been using for the last decade and to focus more on the user experience without searching too much if there’s something already done to boost my productivity.
Well designed PWA are in fact faster, engaging and reliable, but they are also Hard, and to build one is a culture of taking good experient or data driven decisions, since there's no real guidelines or good practices to be found. PWAs experiences are still very new and I believe there's is a lot of terrain undiscovered.

## Some references
 * [Taylor Savage showcasing Jumia Travel PWA in the Chromedev Summit 2016](https://www.youtube.com/watch?v=Ihdp63FaRKA&t=12m44s)
 * [Jumia Travel Show case at Google I/O'17](https://www.youtube.com/watch?v=cuoZenpQveQ&list=PLNYkxOF6rcICniLJ2rfj0FexlA-9zmJJE&index=2&t=12m45s)
 * [Jumia Travel among many other PWA at Google I/O'17](https://www.youtube.com/watch?v=_ssDaecATCM&list=PLNYkxOF6rcICniLJ2rfj0FexlA-9zmJJE&index=6&t=15m12s)
