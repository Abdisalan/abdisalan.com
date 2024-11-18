---
title: "The Tragedy of Running an Old Node Project"
date: "2024-11-17T17:21:55.415Z"
template: "post"
draft: false 
slug: "/posts/tragedy-running-old-node-project"
description: "You want it to just work. But it resists."
category: "Web Development"
tags:
  - "Life"
  - "Node"
socialImage: "media/20241117-gatsby-sad.png"
---

![Sad stick figure next to gatsby logo](media/20241117-gatsby-sad.png)

# Updating my website

It&rsquo;s been a long while since I wrote anything on this site. The framework I used was [Gatsby](https://www.gatsbyjs.com/docs) which in 2020 was one of the hot ways of getting a good looking blog up and running quickly. With over 41 dependencies and god knows how many more dependencies are under those 41. This thing was a beast even in 2020 standards.

Time to run it after not touching it for nearly 4 years.

# How hard could it be to run an old node project?

Pretty hard actually.

```
npm install

<1000 lines of red and failures later>

Failure could not find python2...
```

What? Why do I need python2 to install a node package. What&rsquo;s it doing? Do I have python2? There probably something wrong with this old `node_modules` folder. Let me delete it.

```
rm -rf node_modules/
npm install

...

Failure could not find python2
```

Okay, you win. I&rsquo;ll install python2 even though this is ridiculous.

&#x2026;

30 mintues later after fiddling with installing python2 using pyenv, hoping to not ruin my python3 setup, I finally got python2.

# The complexity grows. What am I getting myself into

```
npm install

...

npm remove_cv_t’ is not a member of ‘std’; did you mean ‘remove_cv’
```

What? That looks like C, or maybe C++. Why the heck is C++ code building so that I can install some node packages that just manage basic web files.

Some Google searching and I land on a GitHub issues page for `node-gyp` and apparently my C++ version is below 14. **That does not seem right.** I&rsquo;ve got to have something *really* wrong because I&rsquo;m down the wrong path.

Then it dawns on me. Do I have the right node version or is there another one I used 4 years ago back in 2020? The only one in my node version manager is **v16**.

I tried doing some Googling on how to know what node version was used in a package.json. Turns out you can&rsquo;t unless the project specifically sets it.. and I didn&rsquo;t. Great.

Now all I&rsquo;m thinking is &ldquo;Please let me find this node version because I don&rsquo;t want to start over with this blog site.&rdquo; Then I stumble upon another GitHub issues page facing a similar C++ compile error. And they mention a broken build using v12.3 and the user should revert back to v12.2

I install node v12 and everything works.

Two hours of my life gone, just to pick up where I left off.

I&rsquo;m tired and going to bed.

