# [What you should know about CORS](https://dev.to/nicolus/what-you-should-know-about-cors-48d6)

## What CORS is, and what it is not?

0. If the protocol, port and host of two URLs are all same, these two URLs has the same origins. 
1. The "same origin policy" is the security measure preventing you from making ajax requests to a different domain. 
2. Cross-origin writes (links, redirect, form submit) and embedding (`<script />`, `<img />`...) are normally allowed. Cross-origin reads is not allowed.  
3. CORS are a way to bypass  SOP in some cases to allow one specific website to make requests to your server. (Typically to allow your font-end app to make requests to your API.)

## How CORS work?
Assuming the front-end is on domain-a.com and API on domain-b.com. 
1. Browser: call preflight request (HTTP ver is `OPTIONS`)
2. Server: always answer to Preflight Requests with 200 response containing `Access-Control-Allow-Origin` and a few other headers. 
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://domain-a.com
Access-Control-Allow-Methods: GET, POST, OPTIONS, DELETE
Access-Control-Max-Age: 3600
```

## Tricky things about COURS
- Every request should **also** contain the same `Access-Control-Allow-Headers`.
- Not all requests will trigger a Preflight Request (you've always been able to make a link or POST a form to a different website)
- The allowed domain must include the protocol
- You can either allow every domain with `Access-Control-Allow-Origin: *`, or only one. => have to change the content of the header dynamically.
```js
app.use(function(req, res, next) {
  const allowedOrigins = [
    "http://www.mydomain.com",
    "https://www.mydomain.com",
    "http://www.myotherdomain.com",
    "http://www.myotherdomain.com",
  ];
  const origin = req.headers.origin;
  if(allowedOrigins.indexOf(origin) > -1){
    res.setHeader('Access-Control-Allow-Origin', origin);
  }
  return next();
});
```
- If you make a request to a local file, Firefox will consider that it's always on the same domain and allow the request. Webkit based browsers like Chrome and Safari won't. 