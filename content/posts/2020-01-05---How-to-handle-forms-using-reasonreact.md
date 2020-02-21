---
title: "How to handle Forms in ReasonML using ReasonReact"
date: "2020-01-05T19:58:40.259Z"
template: "post"
draft: false
slug: "/posts/how-to-handle-forms-using-reasonreact"
description: "I explain how to capture form events using the ReactEvent module from ReasonReact."
category: "Tutorial"
tags:
  - "ReasonML"
  - "ReasonReact"
socialImage: "/media/20200105-form.png"
---

This covers how to:

- ✅ Create a form
- ✅ Get data from the form for processing
- ✅ Handle submitting form data

👩‍💻 Code is available on github so [feel free to download!](https://github.com/Abdisalan/blog-code-examples/tree/master/form-events) 👨‍💻

We will create a form with one text input field called name. Then we will extract the data from the field and store it in state. Lastly, we will handle what happens when the form is submitted.

## Create a form
```reason
/* Form.re */
[@react.component]
let make = () => {
  <form>
    <label> {React.string("Name")} </label>
    <input type_="text" name="name" />
    <button type_="submit"> {React.string("submit")} </button>
  </form>;
};
```

### Looks like this
![Form with name input](media/20200105-form.png)

We've added a form element with a label, input and button. It doesn't do much right now, but we'll fix that.

## Get data from the form for processing

Next, we're going to capture and store the data from the name field.
```reason
/* Form.re */
[@react.component]
let make = () => {
  let (name, setName) = React.useState(() => ""); /* highlight-line */

  let onChange = (e: ReactEvent.Form.t): unit => {/* highlight-line */
    let value = e->ReactEvent.Form.target##value;/* highlight-line */
    setName(value);/* highlight-line */
  };/* highlight-line */

  <form>
    <label> {React.string("Name")} </label>
    <input type_="text" name="name" value=name onChange />/* highlight-line */
    <button type_="submit"> {React.string("submit")} </button>
  </form>;
};
```

Incase you're not familiar with "pipe first" syntax
```reason
e->ReactEvent.Form.target##value;
/* 👆is syntax sugar for 👇 */
ReactEvent.Form.target(e)##value;
```

We've added a state hook to store the name and an onChange handler. The handler takes the name from the React event when the input changes and stores it in the component state.

We were able to do this because ReasonReact exposes an interface for all events from React. The module ReactEvent also has methods for handling events from the keyboard, mouse, clipboard, animation and so much more.

## Handle submitting form data

```reason
/* Form.re */
[@react.component]
let make = () => {
  let (name, setName) = React.useState(() => "");

  let onChange = (e: ReactEvent.Form.t): unit => {
    let value = e->ReactEvent.Form.target##value;
    setName(value);
  };

  let onSubmit = (e: ReactEvent.Form.t): unit => {/* highlight-line */
    ReactEvent.Form.preventDefault(e);/* highlight-line */
    /* code to run on submit *//* highlight-line */
  };/* highlight-line */

  <form onSubmit>/* highlight-line */
    <label> {React.string("Name")} </label>
    <input type_="text" name="name" value=name onChange />
    <button type_="submit"> {React.string("submit")} </button>
  </form>;
};
```

We've added an onSubmit handler that captures the submit event. The handler also calls the preventDefault method to stop the browser from making the form request for you.

That's it!

We were able to create a form, capture the data when the form changes, and run our own method when the form is submitted.

This should help you get started making your own forms in your ReasonReact app!