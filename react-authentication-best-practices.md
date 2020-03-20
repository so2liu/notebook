# [React: Authentication best practices](https://medium.com/meilleursagents-engineering/react-authentication-best-practices-81db893010c9)

Two ways for users to access a view: a link / entering a URL.

# Prevent a link from being rendered

1. If chirldren should not be rendered, they should not be computed.
2. To render something else, if no user.

```js
function Protected({ user, render, fail }) {
    if (user) {
        if (fail) {
            return fail();
        }
        return null;
    }

    return render();
}

function BusinessComponent({ user }) {
  return (
    <div>
      <Protected
        user={user}
        render={() => (<h1>Welcome ${user.name}</h1>)}
        fail={() => (<a href="/signin">Signin</a></Protected>)}
      />
    </div>
  );
}
```

If connecting it to redux:

```js
import { connect } from "react-redux";
import PropTypes from "prop-types";
import { hasPermissionTo } from "helpers/Permissions";

function ProtectedComponent({ user, permissions, render, fail }) {
  if (!hasPermissionTo(permissions, user)) {
    if (fail) {
      return fail();
    }
    return null;
  }

  return render();
}

ProtectedComponent.propTypes = {
  fail: PropTypes.func,
  render: PropTypes.func.isRequired,
  permissions: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.arrayOf(PropTypes.string)
  ]),
  user: PropTypes.shape({})
};

export default connect(state => ({ user: state.user }))(ProtectedComponent);
```

## Prevent a route from being accessible

The most commonly used router for React is `react-router`.

```js
function getProtectedRoute(path, component, user, permissions) {
  if (!hasPermissionTo(permissions, user)) {
    return <Redirect exact path={path} to="/forbidden" />;
  }

  return <Route exact path={path} component={component} />;
}

function AppRouter({ user, history }) {
  return (
    <Router history={history}>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route exact path="/forbidden" component={Forbidden} />

        {getProtectedRoute("/admin", Admin, user, "administration")}
      </Switch>
    </Router>
  );
}
```
