---
title: Trying out Reason React
date: "2019-11-20T02:13:26.596Z"
template: "post"
draft: false
slug: "/posts/trying-reason-react/"
tags:
  - "ReasonML"
  - "Reason-React"
description: "Finally got some time to try out React in ReasonML! Here's what I've learned!"
socialImage: "/media/reasonxkcd.png"
---

![](/media/../public/media/reasonxkcd.png)

As a software engineer who's mostly been writing  javascript, I was thinking of switching up my language on the front-end.

If you haven't read [my last blog post](/posts/diving-into-reasonml-as-a-javascript-dev/), I've been trying out this language called ReasonML, or Reason for short. It's a javascript-developer friendly syntax for OCaml.

It was created over at facebook so of course, there's a library that lets you write React in ReasonML!

I wanted to learn more, so I created a small app following their getting started guide and this is what I've learned.

## The Start

If there's anything to I took away from this, it's that when writing in reason-react, I can quickly find out what could happen in a component and why it's happening.  

Here's how.

We're creating a component that loads a random post from the popular comic [xkcd](https://xkcd.com/). Normally their API does not allow requests from other sites due to CORS issues but luckily [someone made a proxy that we can use](https://github.com/mrmartineau/xkcd-api).

You can follow along with the code for my component using [the GitHub gist here.](https://gist.github.com/Abdisalan/b7f9ffc8bde730729865463b7d46ec6a)

## Defining the State

```reason
type state =
  | NotAsked
  | Loading
  | Failure
  | Success(post);
```

We define the 4 states of our component. It can either be in its initial state where no post is loaded, the post is loading, and either failed to load or was a success. The `Success(post)` state is wrapping the post object and can be used later.

## Rendering

For every state, we made a corresponding render function.

In Reason, you can match type structures in a switch statement (a little different from the javascript switch). It's called [pattern matching](https://reasonml.github.io/docs/en/pattern-matching) and is a very powerful feature of the language.

```reason
switch (state) {
  | NotAsked =>
    <>
      <button onClick={_event => dispatch(LoadPost)}>
        {ReasonReact.string("Random Post")}
      </button>
    </>
  | Loading => <> {ReasonReact.string("Loading")} </>
  | Failure => <> {ReasonReact.string("Failed to load post")} </>
  | Success(post) =>
    <>
      <button onClick={_event => dispatch(LoadPost)}>
        {ReasonReact.string("Random Post")}
      </button>
      <h2> {ReasonReact.string(post.title)} </h2>
      <img width="100%" src={post.img} alt={post.title} title={post.alt} />
    </>
  };
```

Notice that in the success state, we can use the post object like it's a function parameter.

Note: I know that alt and title props are swapped, but don't look at me that's how xkcd gives the data 🤷🏾‍♂️

That's all fine and dandy, but how do you get things done and change the state?

## Actions

```reason
type action =
  | RequestPost
  | LoadedPost(post)
  | LoadPostFailed;

let reducer = (_state, action) =>
  switch (action) {
  | RequestPost => Loading
  | LoadedPost(post) => Success(post)
  | LoadPostFailed => Failure
  };
```

This may look familiar to those experienced with react-redux. This is not a replacement for redux but a way to handle state transformations within a component.

This isn't even unique to reason-react, it's eventually using react hooks' `useReducer()`.

What's different here is we get that sweet type definition as well as pattern matching to gracefully transition our states.

The type system will recognize that our reducer will only accept the `type action` as an input and `type state` as an output. If a new programmer came in and wanted another state in this reducer, they would need to add to our defined state types.

## How to handle side effects

In functional programming, side effects are any changes to state that occur from data outside the function's inputs. Common examples of these are making network requests to an API, and referring to state from another object.

This component needs to request an xkcd comic image, which is a side effect, so we can't include it in the reducer that determines the component state.

What we can do is use the `useEffect` hook to request the comic when in the loading state.

```reason
let fetchPost = () => {
  Random.init(int_of_float(Js.Date.now())); /* seed random generator with the current time */
  let postID = Random.int(2230); /* how many posts as of Nov 19, 2019  */
  Js.Promise.(
    Fetch.fetch("https://xkcd.now.sh/?comic=" ++ string_of_int(postID))
    |> then_(Fetch.Response.json)
    |> then_(json => json |> Decode.post |> (post => Some(post) |> resolve))
    |> catch(_err => resolve(None))
  );
};

React.useEffect1( /* Reason needs to know how many dependencies there are, (1) */
  () =>
    if (state == Loading) {
      Js.Promise.(
        fetchPost()
        |> then_(result =>
              switch (result) {
              | Some(users) => resolve(dispatch(LoadedPost(users)))
              | None => resolve(dispatch(LoadPostFailed))
              }
            )
        |> ignore
      );
      None;
    } else {
      None; /* return none because no cleanup needed */
    },
  [|state|], /* run effect with every change in state */
);
```

## What have we learned?

- Defined state - We know all states the components could be in.
- Rendering - Each state maps to its component and nothing is shared.
- Type checks - You can only dispatch certain actions and they always result in the same state given the same action.
- Side effects - Encapsulated in `useEffect` hook to keep our reducer pure and predictable.

 A lot of the benefits can be attributed to the react hooks, but they're amplified by ReasonML's awesome type system. New programming patterns like this are a refreshing break from normal react and I want to dive deeper!

## Thanks to React-Hooks Example from BuckleScript

[An example app can be found here.](https://github.com/BuckleScript/bucklescript/tree/1cca292c2e24ae34abcc49b2bef33fd38bee36e8/jscomp/bsb/templates/react-hooks)

This was a modification of the `FetchedDogPictures` component with added JSON validation, reducer, and some tweaks to the render.

If you want to also get started writing ReasonML then I recommend following [their starter guide!](https://reasonml.github.io/docs/en/installation)