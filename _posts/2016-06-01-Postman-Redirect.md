---
title: Postman Handling Redirects
updated: 2016-06-01
---

## POST requests being redirected

[Postman](https://www.getpostman.com/) is great alternative to CURL, but it makes some assumptions to help you out, which aren't always helpful.

The best example of this is when making a POST request to an address that resolves in a redirect. Suppose you're making a request to ```www.xyz.com``` and the server really wants you to hit ```https://www.xyz.com```, the response will be a 3xx redirect.

If you were to curl ```www.xyz.com``` with POST request, you'll see the 3xx response. But Postman being "smart", will actually continue and make a get request to the redirect address. By this point your intented POST request is lost.
