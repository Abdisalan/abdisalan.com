---
title: "How To Download A File using ReasonML"
date: "2020-02-13T03:44:23.711Z"
template: "post"
draft: false
slug: "/posts/how-to-download-a-file-using-reasonml"
description: "How to prepare any file for downloading!"
tags:
  - "ReasonML"
socialImage: "/media/20200105-form.png"
---

The solution is to use javascript. For now.

Some of the methods we use here, such as `Blob` aren't yet implemented in ReasonML (or I just couldn't find them), so we need to put some javascript into our ReasonML code using interop.

Here's a working version using codesandbox you can play with.

[https://codesandbox.io/s/how-to-download-arbitrary-data-reasonml-yoxyg](https://codesandbox.io/s/how-to-download-arbitrary-data-reasonml-yoxyg)

## Javascript Interop

In ReasonML, you can wrap javascript with `[%bs.raw {|  ...code goes here... |}]` and it is accessible to the Reason code.

## How Downloading works

First, we create a blob (Binary Large OBject) with our text MIME type.
```javascript
var content = new Blob(["I AM TEXT"], {type: "text/plain"});
```

Then we create an object url that can reference the data in our Blob and we add this url to the anchor tag.
```javascript
document.getElementById("download")
        .setAttribute("href", window.URL.createObjectURL(content));
```

Lastly, we set the download attribute so that we can name our downloaded file.

```javascript
document.getElementById("download")
        .setAttribute("download", fileName);
```

In conclusion, the link to the blob causes the browser to download the file, and gives it our name.

## All the code together:

### index.html
```html
<html>
<body>
    <a href="" id="download">Download File</a>
</body>
</html>
```
### index.re
```reason
let setupDownload = [%bs.raw
    {|
    function() {
        var fileName = "data.txt";
        var content = new Blob(["I AM TEXT"], {type: "text/plain"});

        document.getElementById("download")
                .setAttribute("href", window.URL.createObjectURL(content));

        document.getElementById("download")
                .setAttribute("download", fileName);
    }
    |}
];

setupDownload();
```