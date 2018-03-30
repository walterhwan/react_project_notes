# React/React-redux Project Notes

___

# Creating a new project

#### Setup Rails

```bash
rails new $project_name
--database=postgresql # add this if using Postgres.
--skip-turbolinks # add this to skip the turbolinks gem.
```

```ruby
gem 'bcrypt'
gem 'better_errors'
gem 'binding_of_caller'
gem 'pry-rails'
gem 'annotate'
gem 'jquery-rails' # (When using rails > 5.1)
```

```ruby
bundle install
```
When using Rails 5.1+, modify `application.js `
```js
//= require rails-ujs
//= require jquery
//= require jquery_ujs
//= require_tree .
```

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
or you can append to the file
```bash
{
  echo node_modules/
  echo bundle.js
  echo bundle.js.map
  echo .byebug_history
  echo .DS_Store
  echo npm-debug.log
} >>.gitignore
```
#### Set up npm
```bash
npm init -y
npm install --save webpack@3.10.1 react react-dom react-router-dom redux react-redux babel-core babel-loader babel-preset-react babel-preset-env babel-preset-es2015 redux-logger lodash
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
    path: path.resolve(__dirname, "app", "assets", "javascripts"),
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

___
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

___
# Useful imports

```js
// required in rootReducer.js
import { combineReducers } from 'redux';
// required in store.js
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';
// required in object_container.jsx
import { connect } from 'react-redux';
// required in root.jsx
import { Provider } from 'react-redux';
```


___
# Redux files

### Making your store.js

```js
let configureStore = (preloadedState = {}) => (
  createStore(
    rootReducer,
    preloadedState,
    applyMiddleware(logger, ...otherMiddleware)
  )
);
```

### Making your entry.jsx

```js
document.addEventListener("DOMContentLoaded", () => {
  const root = document.getElementById('root');
  ReactDOM.render(<h1>Hello</h1>, root);
})
```

### Generate a uniq id using `Date`

```js
export const uniqueId = function() {
  return new Date().getTime();
};
```

### Making `$.ajax` request
```js
export const fetchSearchGiphys = (searchTerm) => (
  $.ajax({
    method: 'GET',
    url: `http://api.giphy.com/v1/gifs/search?q=${searchTerm}&api_key=dc6zaTOxFJmzC&limit=2`,
  })
);

export const postUser = (user) => (
  $.ajax({
    url: '/api/users',
    method: `POST`,
    data: { user }
  })
);
```

# Authentication in React

Create `utils/session.js`
```js
export const postUser = (user) => ()
export const postSession = (user) => ()
export const deleteSession = () => ()
```

Create `actions/session.js`
```js
export const RECIEVE_CURRENT_USER = 'RECIEVE_CURRENT_USER';
export const LOGOUT_CURRENT_USER = 'LOGOUT_CURRENT_USER';
const recieveCurrentUser = (user) => {}
const logoutCurrentUser = () => {}
export const createNewUser = (formUser) => dispatch => {}
export const login = (formUser) => dispatch => {}
export const logout = () => dispatch => {}
```

Create `reducers/session.js`
```js
export default (state = { currentUser: null }, action) => {
  Object.freeze(state);
  switch (action.type) {
    case RECIEVE_CURRENT_USER:
      return Object.assign({}, { currentUser: action.user });
    case LOGOUT_CURRENT_USER:
      return { currentUser: null };
    default:
      return state;
  }
};
```

Create `components/session/signup_containers.js`
```js
const mapDispatchToProps = dispatch => ({
  createNewUser: formUser => dispatch(createNewUser(formUser)),
});
```

Create `components/session/signup.jsx`

Create `components/session/login_containers.js`

Create `components/session/login.jsx`


___
# Check-list when making an react component

<!-- TODO: complete this section -->
1. `import React from 'react';`
- `export default Object`
