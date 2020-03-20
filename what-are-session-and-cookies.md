# [What are session and cookies?](https://dev.to/abisekhsubedi/session-and-cookies-what-are-those-38dl)

## Why we care?
1. Web is stateless. If I change the color, fresh the page, it's back to the original color. 
2. Important and complex things like user logins, the site is probably using a database. 
3. Simple things can be stored in cookies, session variables, `sessionStorage` and `localStorage`.

## Session
Until user close the tab.
In JS, you can add items to `sessionStorage` by using the `setItem` method and a key-value pair. 
```js
// add an item to it
sessionStorage.setItem("firstSessionKey", "firstSessionValue");

// to retrieve that item
const bucketForValue = sessionStorage.getItem("firstSessionKey");
```

## Cookies
Cookies are a mean of storing info on the user's computer. 