---
publishDate: 2022-10-21T09:30:00Z
# author: Pantelis Theodosiou
title: Copy to Clipboard with JavaScript
excerpt: Learn how to implement the copy-to-clipboard feature using JavaScript, enabling users to easily copy text with a simple click of a button or icon.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - javascript
  - web-development
  - user-experience
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

> Photo by [Caio](https://www.pexels.com/@caio/) on [Pexels](https://www.pexels.com/photo/black-frame-wayfarer-sunglasses-on-white-notebook-229820/)

When creating advanced web pages and applications, you may want to include the copy feature. This allows your users to copy text by simply clicking a button or icon rather than highlighting the text and pressing a couple of keys on the keyboard.

This capability is typically used when someone has to copy an activation code, recovery key, code snippet, or other similar information. You can also include features such as an alert or text on the screen (which could be a modal) to notify the user that the text has been copied to their clipboard.

Prior to its deprecation, you would have handled this with the `document.execCommand()` command. Despite being frequently used, it struggles with huge text. Since it's synchronous, the user experience is negatively impacted. Additionally, there are problems with permissions that differ between browsers.

These issues are resolved with the [Async Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API).

In this blog post, we'll examine a technique for using the Clipboard API to copy text and images to the clipboard.

### The Asynchronous Clipboard API

Now that the Clipboard API is available, you can read from and write to the system clipboard asynchronously as well as respond to clipboard actions (cut, copy, and paste).

#### Clipboard API permissions

The clipboard can be accessed, which creates various security issues. To reduce security threats, the Clipboard API has been made safe.

* Only pages provided through HTTPS are supported by the Clipboard API.
* Only when the page is the active browser tab is access to the clipboard permitted.
* It is never okay to read from the clipboard without permission.

The two permissions connected to the Clipboard API are listed below:

* `clipboard-write` permission: Limits who can use the write method.
* `clipboard-read` permission: Limits who can use the read technique of the clipboard.

According to how we use the Clipboard API, developers must always ask the browser's permission before doing any operation. Using `navigator.permissions` we must look for the `clipboard-write` permission to see if we have write access.

```js
navigator.permissions.query({ name: "write-on-clipboard" }).then((result) => {
  if (result.state == "granted" || result.state == "prompt") {
    console.log("Write access granted!");
  }
});
```

Using the subsequent code, we can also confirm that `clipboard-read` access exists:

```js
navigator.permissions.query({ name: "read-text-on-clipboard" }).then((result) => {
  if (result.state == "granted" || result.state == "prompt") {
    console.log("Read access granted!");
  }
});
```

### Text to Clipboard

Let's discuss how to copy text to the clipboard right now. With the new Clipboard API, we must utilize the `writeText()` function, which only accepts one parameter: copying text to the clipboard.

```js
export const App = () => {
  const copyText = async () => {
    try {
      await navigator.clipboard.writeText("Some text to copy");
      console.log("Clipboard content has been copied.");
    } catch (error) {
      console.error("Failed to copy content to clipboard.");
      console.log(err.name, err.message);
    }
  };

  return (
    <div className="wrapper">
      <button className="btn" onClick={copyText}>copy text</button>
    </div>
  );
};
```

**Result**

![](https://pantelis.theodosiou.me/assets/static/copy-text.3b38e07.f96ee7e315d28db1d031906200970c5b.gif)

### Image to Clipboard

To copy photos, we must use the Clipboard API's `write()` method, which accepts just one parameter, an array holding the data to be copied to the clipboard, like the previous function.

For instance, by using the following code, we can retrieve a photo from any remote URL and copy it to the clipboard.

```js
export const App = () => {
  const copyImage = async () => {
    try {
      const response = await fetch("/logo-with-shadow.png");
      const blob = await response.blob();
      await navigator.clipboard.write([
        new ClipboardItem({
          [blob.type]: blob,
        }),
      ]);
      console.log("Image copied to clipboard.");
    } catch (error) {
      console.error("Failed to copy image to clipboard.");
      console.log(error.name, error.message);
    }
  };

  return (
    <div className="wrapper">
      <button className="btn" onClick={copyImage}>copy image</button>
    </div>
  );
};
```

**Result**

![](https://pantelis.theodosiou.me/assets/static/copy-image.3b38e07.247c57fce916df025ea8ffc3b722cc40.gif)

Up until this point, only `text/plain` `text/html` and `image/png` were supported by the Async Clipboard API for copying to and pasting from the system clipboard. The browser often does this sanitization in order to, for instance, eliminate embedded `script` components or `javascript:` links from an HTML string or to stop PNG [decompression bomb](https://en.wikipedia.org/wiki/Zip_bomb) assaults.

### Summary

This concise tutorial has explained how to use the new Clipboard API to copy text and other data, such as photos, to the clipboard. When writing to or reading from a user's local computer, use caution to keep the process secure and transparent.