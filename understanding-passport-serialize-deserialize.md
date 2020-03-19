# [Understanding passportserialize deserialize](https://stackoverflow.com/questions/27637609/understanding-passport-serialize-deserialize)

```js
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser((id, done) => {
  User.findById(id, (err, user) => done(err, user));
});
```

0. What is `done` in passport?
[source code](https://github.com/jaredhanson/passport-local/blob/master/lib/strategy.js#L80)
`done` navigates to one of `success`/`error`/`fail` methods. 
Each of these options may call to the `next()`.

1. `serializeUser` determines which data of the user object should be stored in the session => attach to the session as `req.session.passport.user = {}`.

2. `deserializeUser` retrieves the whole object with the help of that key (in 1.).

```js
passport.serializeUser(function(user, done) {
    done(null, user.id);
});              │
                 │
                 │
                 └─────────────────┬──→ saved to session
                                   │    req.session.passport.user = {id: '..'}
                                   │
                                   ↓
passport.deserializeUser(function(id, done) {
                   ┌───────────────┘
                   │
                   ↓
    User.findById(id, function(err, user) {
        done(err, user);
    });            └──────────────→ user object attaches to the request as req.user
});
```
