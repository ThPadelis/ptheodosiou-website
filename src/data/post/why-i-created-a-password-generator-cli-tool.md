---
publishDate: 2021-07-24T00:00:00Z
title: Why I Created a Password Generator CLI Tool?
excerpt: Discover why I created a lightweight, easy-to-use, and open-source password generator CLI tool, published as an NPM package. Learn about its features, benefits, and how it can enhance your development workflow.
image: https://images.unsplash.com/photo-1637144113536-9c6e917be447?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1674&q=80
tags:
  - cli-tools
  - npm-packages
  - open-source
---

# Why I Created a Password Generator CLI Tool?

> _Photo by [Paulius Dragunas](https://unsplash.com/@paulius005) on [Unsplash](https://unsplash.com/)_

Yes it's true, I created my own password generator CLI tool, and published it as an NPM package. It has only ~12kB, easy-to-use and it's open-source.

You can find the package on this link:
https://www.npmjs.com/package/@thpadelis/gpass

You can find the source code on this link:
https://github.com/ThPadelis/gpass

## Why I did it?

I built [gPass](https://www.npmjs.com/package/@thpadelis/gpass) because I was finding it way too tiring to visit a website, configure the settings and generate a new one every time I wanted to use a password.

I wanted something to help me with this obstacle and be easy to use for everyone who uses a terminal.

Another reason was that I haven't work on something personal in long days. I felt that the routine was drowning me and I wanted to change it.

As result, I created this small project of mine and published it.

## How does it work?

Firstly you need to install the package on your machine.

```bash
npm install -g @thpadelis/gpass
```

After installation, you are ready to use the `gpass` alias. You can use the `gpass -h` option to list all the available options.

### Examples

Following are some very basic usage examples of the tool.

#### Create password with length of 15 chars

```bash
gpass -l 15
# output
# gV6|yA5&lA1=iV9
```

#### Create PIN

```bash
gpass -p
# output
# 5176246270
```

#### Create 5 passwords with length of 10 characters and no symbols

```bash
gpass -l 10 -n 5 -es
# output
# dY6tA6nM7q
# hK4eB6qE5p
# cD5pM0eJ0l
# cM4bV5uQ6s
# sD3bG1yV2n
```

#### Create password and copy it to clipboard

```bash
gpass -cp
```

You can find more examples in the [README.md](https://github.com/ThPadelis/gpass#readme) file.

## Outcome

At the end of the day, this is just another tool - not the first for sure, not the best but is part of me and my try to contribute to the open-source community.

If you like, please consider dropping a comment, like or even a new feature idea.
