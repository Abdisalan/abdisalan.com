---
title: Diving into ReasonML as a Javascript Dev
date: "2019-10-30T23:28:53.919Z"
template: "post"
draft: false
slug: "/posts/diving-into-reasonml-as-a-javascript-dev/"
tags:
  - "ReasonML"
description: "Trying ReasonML! It's a pretty cool language"
---
## Motivation

I'm a web developer, customarily writing javascript on the front-end, but I want to write in a typed functional language. I was going to try TypeScript and then I stumbled upon [Reason](https://reasonml.github.io/).

ReasonML is a dialect of OCaml that's friendly to JavaScript developers but still has all the features you'd want in a functional language. And, by using [BuckleScript](https://bucklescript.github.io/), we can translate Reason to Javascript! With this choice, I'm off to start learning.

## Practice

As an introduction, I'm tackling the ReasonML course on [exercism.io](http://exercism.ioexercism.io/). They have an assortment of challenges to help you get familiar with the syntax and idioms. For one of my first challenges, I completed the binary search problem. Here's my solution.

```reason
let find = (numbers: array(int), num: int): option(int) => {
  let rec findHelper = (leftIndex, rightIndex) => {
    let center = (leftIndex + rightIndex) / 2;
    if (leftIndex > rightIndex) {
      None;
    } else if (numbers[center] == num) {
      Some(center);
    } else if (numbers[center] < num ) {
      findHelper(center + 1, rightIndex);
    } else {
      findHelper(leftIndex, center - 1);
    }
  }

  let size = Array.length(numbers);
  if (size == 0) {
    None
  } else {
    findHelper(0, size - 1)
  }
}
```

The code uses recursion because It helped me break down the problem into smaller, more easily solvable ones. Also because I know that BuckleScript will change the code to be more performant when transpiled to javascript. i.e. It'll use a loop instead.

[You can check out the generated code for yourself here.](https://reasonml.github.io/en/try?rrjsx=true&reason=DYUwLgBAZglgdgEwgXggCjgZwDQTgShQD4IBvAKAglEgCcQBjaeBACRGAAcRaV1hctQshIUqVGhAYg4YHnzTAIAaghCIAeggAmANyVxMKPwgl1Y8VQByAezgh9lgL4QOmEBCPosAbWmyeAF0UVAIyA0sAZRsAWxA0fzkhR3EXNw8vDEw-GSTggB48CEILS1hEdi4eBNz5VQBGQXwUqjTgd3DLKnK2Dm5aRVxE+QBaCHrmiIgnAxmJcAhMGAAvD1QAQVpaAEMATwA6UDgAczAACyzJqkyl1ZCIAAYSqdt7Wdd2j1LmCr7qh9wtw8Ywms3ITiAA)

## Adjustments

Here are some adjustments I made using the switch feature. This version is more intuitive because the condition and execution logic is on the same line and easier to follow in my opinion.

```reason
let find = (numbers, num) => {
  let rec findHelper = (leftIndex, rightIndex) => {
    let center = (leftIndex + rightIndex) / 2;
    switch(numbers[center]) {
    | _ when leftIndex > rightIndex => None
    | v when v == num => Some(center)
    | v when v < num => findHelper(center + 1, rightIndex)
    | _ => findHelper(leftIndex, center - 1)
    }
  }

  let size = Array.length(numbers);
  if (size == 0) {
    None
  } else {
    findHelper(0, size - 1);
  }
}
```

[The generated js code](https://reasonml.github.io/en/try?rrjsx=true&reason=DYUwLgBAZglgdgEwgXggCjgZwDQTgShQD4IBvAKAglEgCcQBjaeBACRGAAcRaV1hctQshIUqVGhAYg4YHnzTAIAaghCIAeggAmANyVxMKPwgl1Y8VQByAezggDVAL4QOmEGUfjMAdxhgGAAsMTABtaVkeAF1CC0sAHwgANwgfQJlklFQ4YggAZRsAWxA0CLkhLypElLSMlIAePFzYRHYuHlKZcpUIAEZBfEqIRIB9ZpY27lpFXDL5AFo+wctnLycDdaoAQVpaAEMATwA6UDgAczBgrGFUAAYIAH4IW3sIAC5mVo4ptFvcHf2x1OFyumEIi16gycQA) from this block also results in a while loop.

## Javascript Solution For Comparison

This is how I'd write the challenge in javascript for the curious (almost exactly how bucklescript wrote it!)

```javascript
const find = (ns, n) => {
  if (ns.length === 0) return undefined;
  let l = 0;
  let r = ns.length - 1;
  while (l <= r) {
    const center = Math.floor((l + r) / 2);
    if (ns[center] == n) {
      return center;
    } else if (ns[center] < n) {
      l = center + 1;
    } else {
      r = center - 1;
    }
  }
}
```

## What did we discover?

## Pros

- ReasonML is pretty straightforward to write and doesn't feel much different than javascript.
- The compiler helps cover every case in logic flow and throws an error when cases aren't covered.
- Bucklescript makes the code performant in javascript.

## Cons

- You can't write quick and dirty code; all cases must be handled.
- At times the compiler does not indicate what's gone wrong and it's annoying.
- (Maybe a pro later on) A lot of growing pains with writing type-safe code.
    - Cannot reassign variables.
    - Every statement returns a value unless assigned to a variable.

## What's next

I'm a fan of [exercism.io](http://exercism.io/) challenges and I'll try some more to get familiar with Reason. This challenge didn't give a good intro to the rich type system that Reason has so I'll have to explore that too.

I'm eager to write a web app using reasonreact and create another post of my progress. When I finish, I'll make sure to link that here. See you in the next post!
