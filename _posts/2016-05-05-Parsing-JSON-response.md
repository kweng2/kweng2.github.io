---
title: How NOT to Parse JSON Response
updated: 2016-05-05
---

### Bad Practice for Parsing JSON Response

It is possible to force an AJAX request to be synchronous via [jQuery](http://api.jquery.com/jquery.ajax/), and would look like this:

```
var jqXHRObject = $.ajax({
  type: "GET",
  url: "/go/isInitialOAuthSignup",
  dataType: "json",
  async: false
};
```
This can be very tempting, due to the synchronous nature. And even more conveniently, it returns a ```jqXHR``` object implementing the Promise interface (supported by jQuery since version 1.5).

And this returned jqXHR for backwards compatibility even exposes a responseText property. So if we're expecting a JSON response, it may be tempting to do this:

```
JSON.parse(jqXHRObject.responseText)
```

What if the server threw and error and didn't respond with ```application/JSON```?

A simple response header check can avoid this problem.