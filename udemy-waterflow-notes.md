# Title

## Sub Title

[TOC]

### Props type check

```
npm install --save prop-types
```

example:

```js
import PropTypes from "prop-types";

Person.propTypes = {
  click: PropTypes.func,
  name: PropTypes.string,
  age: PropTypes.number,
  changed: PropTypes.func
};
```

### Ref to get a reference of a element

#### Class-based components

```js
import React, { Component } from "react";

class Person extends Component {
  constructor() {
    super(props)
    this.inputElementRef = React.createRef();
  }

  componentDidMount(){
    this.inputElementRef.current.focus()
  }

  render(){
    <Person ref={this.inputElementRef}>
  }
}
```

#### React Hooks

```js
import React, { useState, useEffect, useRef } from "react";

const cockpit = props => {
  const toggleBtnRef = useRef(null);

  useEffect(() => {
    toggleBtnRef.current.click();
  }, []);

  toggleBtnRef.current.click();

  return <button ref={toggleBtnRef}>Button</button>;
};
```

### Context

New folder called `context` for context code.

#### Old way

```js
// auth-context.js
import React form `react`

const authContext = React.createContext({
  authenticated: false,
  login: ()=>{}
})
```

```js
import AuthContext from "auth-context";

<AuthContext.Provider
  value={{ authenticated: this.state.authenticated, login: this.loginHandler }}
>
  <Person>Person Info</Person>
</AuthContext.Provider>;
```

```js
class Person extends Component {
  render() {
    return (
      <AuthContext.Consumer>
        {context =>
          context.authenticated ? <p>Authenticated</p> : <p>Please login</p>
        }
      </AuthContext.Consumer>
    );
  }
}
```

`context` can only be accessible in JSX part. What if other parts want to use context?

#### Another way: Add `static contextType`

```js
import AuthContext from "auth-context";

class Person extends Component {
  constructor(props) {
    super(props);
  }
  static contextType = AuthContext;

  componentDidMount() {
    console.log(this.context.authenticated);
  }
}
```

#### In React Hooks

```js
import AuthContext from "auth-context";

const cockpit = props => {
  const authContext = useContext(AuthContext);
};
```

## How to plan a react app?

![Planning a React App](/mdImg/plan-a-app.png)

### Step1

![](/mdImg/plan-app-step1.png)

### Step2

![](/mdImg/plan-app-step2.png)

### Step3

We manage the state in BurgerBuilder.caveat

### Handle HTTP globally (for error handle)

#### Request

```js
axios.interceptors.request.use(
  request => {
    console.log(request);
    // edit request and then
    return request;
  },
  error => {
    console.log(error);
    return Promise.reject(error);
  }
);
```

#### Response

```js
axios.interceptor.response.use(
  response => {
    console.log(response);
    return response;
  },
  error => {
    console.log(error);
    return Promise.reject(error);
  }
);
```

### Spinner (for loading)

Just google it...

### React router

#### Absolute vs relative paths

##### Absolute Paths

By default, if you just enter ` to="/some-path"`` or `to="some-path"` , that's an absolute path.

Absolute path means that it's always appended right after your domain. Therefore, both syntaxes (with and without leading slash) lead to `example.com/some-path` .

##### Relative Paths

Sometimes, you might want to create a relative path instead. This is especially useful, if your component is already loaded given a specific path (e.g. posts ) and you then want to append something to that existing path (so that you, for example, get `/posts/new` ).

If you're on a component loaded via `/posts` , `to="new"` would lead to `example.com/new` , NOT `example.com/posts/new` .

`<Link to={props.match.url + '/new'}>` will lead to `example.com/posts/new` when placing this link in a component loaded on `/posts` . If you'd use the same `<Link>` in a component loaded via `/all-posts` , the link would point to `/all-posts/new` .

#### Use `NavLink` instead of `Link`

- This will add a class `class='active'`.
- `<NavLink activeClassName="my-active">` changes the class name `active` to `my-active`.
- Useful properties for `NavLink` are also `activeStyle` ...

#### Router parameter

...

#### Navigating programmatically

```js
this.props.history.push({ pathname: "/" + id });
```

or

```js
this.props.history.push("/" + id);
```

#### Redirect

##### `<Redirect>`

##### Use history prop

```js
this.props.history.replace("posts");
```

The user won't back to the submit page.

#### Lazy loading

- Downloading the component only when it's used.
- Highly dependent on webpack configuration. The following method is only for create-react-app and react-route.
  [Watch this](https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/8138598#content)

#### Lazy loading in newest react

```js
import React, {Suspense} from 'react'

const Posts = React.lazy{()=>import('./containers/Posts')}

<Route path="posts" render={()=>(
  <Suspense fallback={<div>Loading...</div>}>
    <Posts />
  </Suspense>
)}>
```

#### Router step by step

1.  yarn add react-router-dom
2.  wrap app with <BrowserRouser>
3.  <Route path="/" component={BurgerBuilder}/>
4.  use exact or <Switch> to avoid /
5.  The component will get the special properties from router. HOC withRouter(burger) can eject these special properties of router.
6.  Add link by this.props.history.push('/checkout'), this.props.history.goBack(), this.props.history.replace('/checkout')
7.  Send parameters by query:

Sender:

```js
const queryParams = [];
for (let i in this.state.ingredients) {
  queryParams.push(
    encodeURIComponent(i) + "=" + encodeURIComponent(this.state.ingredients)
  );
}
const queryString = queryParams.join("&");
this.props.history.push({
  pathname: "/",
  search: "?" + queryString
});
```

Receiver:

```js
const query = new URLSearchParams(this.props.location.search);
const ingredients = {};
for (let param of query.entries()) {
  // ['salad', '1']
  ingredients[param[0]] = +param[1];
}
this.setState((ingeredients: ingredients));
```

8. Pass the parameters by render.

```js
   <Route path={this.props.match.path+"/contact-data"} render={()=>(<ContactData ingredients={this.state.ingredients} />)}>
```
