# [Please stop use local storage](https://www.rdegges.com/2018/please-stop-using-local-storage/)

## What is Local Storage?

- New in HTML5
- Easy to use by `localStorage...`
- SessionStorage will be deleted when the user closes their browser tab

## Why cool?

- Pure Javascript (cookies need to be created by a web server)
- It provides at least 5MB of data storage (cookies only 4KB)

## What're bad?

- Only string
- Synchronous
- Not used by webworkers
- Limited storage size.
- No data protection. It can be access by all Javascript code on this page.

## Bad practices:

- Store JWTs in local storage.

## Good practices:

- Sensitive data are always stored with a server-side session.
  - User Ids
  - Session Ids
  - JWTs
  - Personal information
  - API keys
  - Anything else
- How to do it?
  1. User logs in
  2. Create a session identifier for them and store it in a cryptographically signed cookie.
  3. Set the httpOnly cookie flag in cookie library. (Read)
  4. Set the SameSite=strict cookie flag in cookie library to protect CSRF attack. Secure=true to ensure cookies can only be set over an encrypted connection.
  5. Use session ID to retrieve their account details.
- Non-String Data that isn't sensitive:
  - IndexedDB
- Off-Line Data:
  - IndexedDB + Cache API
