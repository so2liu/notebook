# Udemy Course: Working with CSS Modules

Normally, we want to limit the CSS style in a specific scope.
There are different approaches. 

## Use webpack config

1.  Eject webpack configs.

```bash
yarn eject
```

2.  Tweak both files `config/webpack.config.dev.js` and `config/webpack.config.prod.js` the part `modules.rules....test: /\.css$/`.

```js
test: /\.css$/,
use: [
  require.resolve("style-loader"),
  {
    loader: require.resolve("css-loader"),
    options: {
      importLoaders: 1,
      modules: true,
      localIdentName: "[name]__[local]__[hash:base64:5]"
    }
  },
  ...
```

3. Use `import xxx from 'xxx.css'`. It will import classes from CSS files as the properties of `xxx`.

```jsx
<button className={[xxx.Button, xxx.Red].join(" ")}>Button</button>
```
4. If using latter `react-script` version. We can save the eject step and use name `xxx.module.css`.

CSS Modules are a relatively new concept (you can dive [super-deep into them](https://github.com/css-modules/css-modules). With CSS modules, you can write normal CSS code and make sure, that it only applies to a given component.