---
title: "I Think I'm Making An App: Part 1"
date: "2019-12-20T02:07:14.036Z"
template: "post"
draft: false
slug: "/posts/i-think-im-making-an-app-part-1"
description: "I want to show my process. How I go from concept to code then product. Hope this helps inspire your own method for making apps!"
tags:
  - "Planning" 
socialImage: "/media/20191219-QuickMockup.png"
---
I realized something.

I don't need to finish a project before I can write about it!

I can write about the process. That's something you want to see right?

## The Problem

The code formatting for my site... isn't the best. There are so many themes and colors I can choose from in vscode but why not for my blog?

I use a library called [gatsby-remark-prismjs](https://www.notion.so/abdisalan/Making-an-app-that-creates-custom-code-highlighting-for-your-gat-888a028fe30e44f5ad6aaa90feb52e6d) and that changes the code from this markdown

    ```javascript
    // Example javascript code
    function add(x, y) {
    	return x + y;
    }
    ```

into this highlighted sample
```javascript
// Example javascript code
function add(x, y) {
    return x + y;
}
```

The problem is that it's only [limited to a few themes](http://prismjs.com) and there isn't an easy way to create your own.

So I've decided to make my own editor for creating code formatting themes!

## How gatsby-remark-prismjs works

Trust me, this becomes relevant soon.

It converts code in TWO steps:

1. Parse the code text and generate html "tokens" for each keyword or character
2. Apply css to each token according to a theme

Go ahead and see!

Right click on the code example above 👆and click inspect. It'll look something like this:

```html
<div class="gatsby-highlight" data-language="javascript">
    <pre class="language-javascript">
        <code class="language-javascript">
            <span class="token keyword">function</span>
            <span class="token function"> add</span>
            <span class="token punctuation">(</span>
            ...
        </code>
    </pre>
</div>
```

Along with these html tags, there's a css file from prism that decides what color each token becomes!

```css
// function and class-name tokens are yellow
.token.function,
.token.class-name {
    color: #b58900; /* yellow */
}
```

## The Plan

Create an app that lets us interactively change the colors that format our code

We can leave prism's token detection, however we want to be able to edit the css theme.

Here's a quick mock up of the apps's interface.

![](/media/../public/media/20191219-QuickMockup.png)

- The preview section shows you how your theme looks with some sample code.
- The configuration section lists what the color for each token type (functions, variables, comments, etc.) and allows you to change them.
- And lastly, when you're happy with the result of your code theme, you can export the config to a css file you can include in your blog.

Since I'm currently learning ReasonML, I think this project could be a good learning experience. I've got a rough idea on how this will be made, but more of that in Part 2!

In the mean-time, got any questions? Feel free to reach me on twitter!