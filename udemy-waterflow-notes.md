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
