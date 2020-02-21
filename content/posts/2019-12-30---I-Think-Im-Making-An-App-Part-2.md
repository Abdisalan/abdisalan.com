---
title: "I Think I'm Making An App: Part 2"
date: "2019-12-30T06:02:38.337Z"
template: "post"
draft: false
slug: "/posts/i-think-im-making-an-app-part-2"
description: "I've reached the prototype stage! You can now use the code theme engine to make your own code themes using prismjs with gatsby"
category: "Projects"
tags:
  - "Prototype" 
  - "ReasonML"
socialImage: "/media/20191230-preview.png"
---

The theme engine has breathed life!

You can [play with it here](https://abdisalan.github.io/code-theme-engine/) and check out [the code on github!](https://github.com/Abdisalan/code-theme-engine)

I go into detail on what this app [does in Part 1](/posts/i-think-im-making-an-app-part-1), but if you want a quick refresher, I got you.

## What's the problem?

On the blog you're currently reading, I'm using a library called prismjs to render + highlight my code snippets. 

```javascript
// example highlighted code
const add5 = (x) => x + 5;
```

Unfortunately, prismjs is limited to only 8 themes so I want to make new themes! This app helps me do just that.

## Who was this made for?

This theme engine was made for those using `gatsby-remark-prismjs` and want to make their own theme.

If you made your gatsbyjs site with any of these popular starters, you're probably still using `gatsby-remark-prismjs` to render your code snippets and could benefit from this app!

- gatsby-starter-blog
- gatsby-starter-lumen
- gatsby-advanced-starter
- gatsby-starter-hero-blog
- gatsby-material-starter

## What does it do?

It lets you assign colors to certain keyword types in your code

For example you could make all function calls light blue and all numbers dark green, etc.

Once you've styled your theme the way you would like, you can export the theme into a css file that can be used with prismjs.

Here it is in action! 🔥🔥🔥

![Code-Theme-Engine-Gif](/media/20191230-themeEngine.gif)

## It's still a prototype

My vision is to empower anyone to create and share themes powered by prismjs.

This is just v0.0.1 and there are a few features I want to add in the future such as:

- Code snippets from other languages
- More granular control over highlighting
- Guidance on what colors work together well
- A store with most popular
- many more!

# Built with ❤️ & ReasonML

One of the main reasons I came up with this project was to get some more practice with ReasonML. If you don't know, it's a functional programming language that compiles to javascript. Writing this app turned out to be a good learning experince and I'll make sure to share my findings in the next post!

✌️