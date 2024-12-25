---
publishDate: 2022-08-09T09:30:00Z
# author: Pantelis Theodosiou
title: Hide Your API Keys with an API Proxy Server
excerpt: Learn how to protect your API keys by creating an API Proxy Server using Node.js. This tutorial will guide you through the process, ensuring your keys remain secure and your application stays safe.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - api-security
  - nodejs
  - web-development
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

> Photo by [Kevin Ku](https://www.pexels.com/@kevin-ku-92347/) on [Pexels](https://www.pexels.com/@kevin-ku-92347/)

## Introduction

In this article/tutorial we are going to create an API Proxy Server using NodeJS and the main reason behind this is to show you how to hide public API keys and not expose them in the public like I was doing in past. All of us have created an application once, just to get to know a new library or a framework, and the best way to achieve this is to use an open API (like [TMDB](https://developers.themoviedb.org/)). 

Most of these open APIs require adding the API key in the request URL or in the headers before making the request. Either way, the API key can be stolen and used by people that are not actually own this key. As result, you may end up with a suspended account or your application not work as you've expected it to.

Remember this: *The API keys belong to the server side of your application* 

Without further talking, let's create this API Proxy server. For the sake of this article/tutorial, we are going to use [Weather API](https://openweathermap.org/api).

## Requirements

Before start coding, we need to have the followings:

- **API key**
  Open OpenWeatherMapAPI website, make a [free account](https://home.openweathermap.org/users/sign_up), go to **My API keys** and create one.
- **Basic knowledge of JavaScript and NodeJS**
- **Coffee**

### Dependencies and Scripts

**Create application's project folder**

```sh
cd ~/Documents/tutorials
mkdir api-proxy-server && cd api-proxy-server
npm init -y
```

**Install required NPM packages**

```sh
npm install -S express cors dotenv axios
npm install -D nodemon # for faster development
```

**Prepare scripts inside package.json file**

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon --config nodemon.json"
  }
}
```

Let's take a look at the project's folders and files just to have a clearer image of the structure.

```
server/
├── src
│   ├── utils
│   │   └── env.js
│   └── routes
│       └── index.js
├── package.json
├── nodemon.json
├── index.js
└── .env
3 directories, 6 files
```

Create and `index.js` and a `nodemon.json` file at the root of the project.

```json
// nodemon.json
{
  "verbose": true,
  "ignore": ["node_modules/"],
  "watch": ["./**/*"]
}
```

Before start editing the entry (`index.js`) file, we need to create two other useful files. The first one is the `.env` file where all of our environment variables live. It's going to be a bit reusable in order to be able to use it with other public APIs except OpenWeatherMap API.

```bash
API_PORT = 1337
API_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
API_KEY = "1d084c18063128c282ee3b41e91b6740" # not actually api key; use a valid one
```

And the other file is the `src/utils/env.js` which is responsible to read and export all the environment variables.

```javascript
require("dotenv").config();

module.exports = {
  port: Number(process.env.API_PORT) || 3000,
  baseURL: String(process.env.API_BASE_URL) || "",
  apiKey: String(process.env.API_KEY) || "",
};
```

Now it's time to edit the main file and add some logic to this proxy server application.

```js
// imports
const express = require("express");
const cors = require("cors");
const env = require("./src/utils/env");

// Application
const app = express();

// Middlewares
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cors());

// Routes
app.use("/api", require("./src/routes")); // Every request that starts with /api will be handled by this handler

// This route will handle all the requests that are not handled by any other route handler
app.all("*", (request, response, next) => {
  return response.status(404).json({ message: "Endpoint not found!" });
});

// Bootstrap server
app.listen(env.port, () => {
  console.log(`Server is up and running at http://localhost:${env.port}`);
});
```

Things here are very simple, it's a basic ExpressJs server application. On the top, we've got the required imports. Then we have some needed middleware, such as the `JSON` middleware which is responsible to parse incoming requests with JSON payloads and is based on `body-parser`. Next, we have the routes where here we manage the endpoints and their handlers. And lastly, we've got the bootstrapping of the server.

Let's take a look at the handler of the `/api` route.

```javascript
// server/src/routes/index.js
const router = require("express").Router();
const { default: axios } = require("axios");
const env = require("../utils/env");

// [GET] Current weather data
router.get("/", async (request, response, next) => {
  try {
    const query = request.query || {}; 
    const params = {
      appid: env.apiKey, // required field
      ...query,
    };
    const { data } = await axios.get(env.baseURL, { params });
      
    return response.status(200).json({
      message: "Current weather data fetched!",
      details: { ...data },
    });
  } catch (error) {
    const {
      response: { data },
    } = error;
    const statusCode = Number(data.cod) || 400;
    return response
      .status(statusCode)
      .json({ message: "Bad Request", details: { ...data } });
  }
});

module.exports = router;
```

The `GET` request handler function is taking as inputs the `query` fields that we've provided when we call it.

For example, if we didn't have this API Proxy server, our request would look like this `https://api.openweathermap.org/data/2.5/weather?q=Thessaloniki,Greece&appid={API key}`, but now that we've got it, our request looks like this `http://localhost/api?q=Thessaloniki,Greece`. There is no need to add the required field `appid` because the proxy server adds it by itself as a request parameter. We spared not showing the API key while we make the request on the OpenWeatherMap API.

## Summary

The main concept is to hide your public API keys behind an API Proxy Server, in order to prevent users to steal your keys. To achieve this is to create endpoints that wrap the endpoints of the actual open API providing the API key inside the proxy server as an environment variable in every request that needs this key. The obvious con of this approach is that this is a time-consuming process and in some cases is not even needed.

You can get the source code of this API Proxy server [here](https://github.com/ThPadelis/api-proxy-server).

## What's next

I was thinking to add some features to this proxy server. One of these is to add caching functionality in order to serve the data that was recently requested faster. The other one is to add a rate limiter in order to prevent users from overusing the API and crashing the server.

Drop a comment if you've thought of another solution to hide your API keys or if you have any tips or tricks to tell me about.

