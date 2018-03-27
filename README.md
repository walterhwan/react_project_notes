# React/React-redux Project Notes

___

# Creating a new project

#### Create starting files and folders
```bash
# You can set variable in bash as such
entry_file_name=entry.jsx
# And use it by adding $ in front of it
echo entry_file_name # => entry.jsx
```
```bash
mkdir frontend frontend/components frontend/actions frontend/reducers frontend/store frontend/util css
touch frontend/$entry_file_name
```
For `index.html`
```bash
touch index.html css/reset.css css/style.css
```
```html
<!-- Add `script` to index.html -->
<script src='./bundle.js'></script>
<!-- Add css -->
<link rel="stylesheet" type="text/css" href="./css/reset.css">
<link rel="stylesheet" type="text/css" href="./css/style.css">
<!-- ... -->
<!-- Add an div tag and set root as id -->
<div id="root"></div>
```

#### Set up Git
```bash
git init
touch .gitignore
```
Add `.gitignore`
```bash
# .gitignore
node_modules/
bundle.js
bundle.js.map
```
You can create and write the file in terminal at the same time
```bash
{
  echo 'node_modules/'
  echo 'bundle.js'
  echo 'bundle.js.map'
} >.gitignore
```
#### Set up npm
```bash
npm init -y
npm install --save webpack@3.10.1 react react-dom redux react-redux babel-core babel-loader babel-preset-react babel-preset-env babel-preset-es2015 redux-logger lodash
```
Make changes to `package.json`
* Change `"main": "app.jsx"` to `$entry_file_name`
* Add `"webpack": "webpack --watch"`

#### Set up Webpack
```bash
touch webpack.config.js
```
```js
// webpack.config.js
var path = require('path');

module.exports = {
  context: __dirname,
  entry: "./frontend/$entry_file_name",
  output: {
    path: path.resolve(__dirname),
    filename: "bundle.js"
  },
  module: {
    loaders: [
      {
        test: [/\.jsx?$/, /\.js?$/],
        exclude: /(node_modules)/,
        loader: 'babel-loader',
        query: {
          presets: ['env', 'react']
        }
      }
    ]
  },
  devtool: 'source-map',
  resolve: {
    extensions: [".js", ".jsx", "*"]
  }
};
```
```bash
npm run webpack
```

# Useful `Object#methods` and `lodash` methods TL;DR
<!-- TODO: add use-cases and TL;DR -->
`Object.keys`
```js
// it returns An array of strings that represent all the enumerable properties of the given object.
let anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']
```
`Object.assign`
```js
//  it copy the values of all enumerable own properties from one or more source objects to a target object.
// It will return the target object. (but it will not deep clone nested objects!)
let o1 = { a: 1 };
let o2 = { b: 2 };
let o3 = { c: 3 };

let obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```
`Object.freeze`
```js
// it prevents the passed it variable been changed. (but it will not prevent nested object being changed)
Object.freeze(state);
```

`let merge(targetObject, ...otherObject)`
```js
// TODO: search for use-cases
import merge from 'lodash/merge';
merge({}, state)
```

# Useful Imports

```js
import { combineReducers } from 'redux';
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';
import { connect } from 'react-redux';
import { Provider } from 'react-redux';
```

# Making your store.js

```js
let configureStore = (preloadedState = {}) => (
  createStore(
    rootReducer,
    preloadedState,
    applyMiddleware(logger, ...otherMiddleware)
  )
);
```

# Making your entry.jsx

```js
document.addEventListener("DOMContentLoaded", () => {
  const root = document.getElementById('root');
  ReactDOM.render(<h1>Hello</h1>, root);
})
```

# Generate a uniq id using `Date`

```js
export const uniqueId = function() {
  return new Date().getTime();
};
```

## Check-list when making an react component

<!-- TODO: complete this section -->
1. `import React from 'react';`
- `export default Object`
```js
export const createKitten = (parentId, kitten) => dispatch => (
  CatAPIUtil.createKitten(parentId, kitten).then(kitten => dispatch(receiveCat(kitten)))
);
```
