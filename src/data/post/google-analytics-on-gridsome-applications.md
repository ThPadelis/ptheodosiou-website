---
publishDate: 2022-03-10T09:30:00Z
# author: Pantelis Theodosiou
title: Google Analytics on Gridsome applications
excerpt: Learn how to integrate Google Analytics into your Gridsome applications, enabling you to track and analyze your website's performance effectively.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - gridsome
  - javascript
  - google-analytics
  - web-development
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

> Photo by [Olya Kobruseva](https://www.pexels.com/@olyakobruseva/) on [Pexels](https://www.pexels.com/photo/laptop-beside-eyeglasses-and-smartphone-7887842/).

Analytics, as we all know, is an essential component of any website. Google Analytics is the most effective free tool available. This article demonstrates how to integrate Google Analytics into a Gridsome site.

I was trying to add Google Analytics using one of the available plugins, but they don't work as I expected. So, I thought "Why don't you go with the traditional manual way?" The existing plugins have some limitations and if you are stuck in the same place and you wanted to also customize your analytics with custom events, follow along.

## Create Universal tracking on Google analytics

Go to [Google Analytics](https://analytics.google.com).

Navigate to the **Admin** section (by clicking on the small gear icon in the bottom left), then select "Create Property."

![Google Analytics Dashboard](https://prnt.sc/VLbdcQH9sY1L)

Complete the form. Because we're collecting analytics for the website, make sure "**Create a Universal Analytics property**" is enabled.

![Create property](https://prnt.sc/S7FUAOW9x1ub)

When you finish the setup, you will be redirected to a page that contains **setup scripts** and your **tracking ID**.

![Tracking ID](https://prnt.sc/8Emilj09_pEs)

## Configure the Gridsome website to send analytics data

Paste the following code into the `main.js` file. Make sure to include your **tracking ID**.

```js
export default function (Vue, { router, head, isClient }) {
  // Global site tag (gtag.js) - Google Analytics
  head.script.push({
    src: "https://www.googletagmanager.com/gtag/js?id=UA-********-*", // replace it with your tracking id
    async: true,
  });

  if (isClient) {
    // Google Analytics
    window.dataLayer = window.dataLayer || [];
    function gtag() {
      dataLayer.push(arguments);
    }
    if (typeof window.gtag === "undefined") window.gtag = gtag; // So we can you gtag() on our components
    gtag("js", new Date());
    gtag("config", "UA-********-*");
  }

  // rest code...
}
```

> **Note**: It is necessary to check for if it's `isClient` because the window will not function during the build.

When someone visits your website, you should see an increase in "Active users" on your Google Analytics dashboard after you build and deploy your Gridsome application.

Let's say we want to count the number of share of a specific blog post. Add the following code on respective component's share function.

```js
async share() {
    // ...code
    const key = this.to; // Where to share (eg. Facebook, LinkedIn)
    // Google Analytics Event
    const params = {
        method: key,
        content_type: "article",
        item_id: location.href,
    };
    gtag("event", "share", params);
	// code... 
}
```

Counting only share events may not be sufficient, but when you add `gtag.js` to your site, the snippet includes a config command that sends a `pageview` by default. 

For more [events](https://developers.google.com/analytics/devguides/collection/gtagjs/events) and more detailed information, read the Google Analytics [documentation](https://developers.google.com/analytics/devguides/collection/gtagjs/pages).