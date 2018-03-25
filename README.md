# React/React-redux Project Notes

___

## Create project

1. Create starting files and folders
  ```bash
  touch index.html
  mkdir frontend
  mkdir actions
  mkdir components
  mkdir reducers
  mkdir store
  mkdir util
  touch ./frontend/{your_root_file}.jsx
  ```
  ```html
  <!-- Add `script` to index.html -->
  <script src='./bundle.js'></script>
  <!-- ... -->
  <!-- Add an div tag and set root as id -->
  <div id="root"></div>
  ```

- Set-up Git
  ```bash
  git init
  ```
- Set-up npm
  ```bash
  npm init -y
  npm install --save webpack@3.4.1 react react-dom redux react-redux babel-core babel-loader babel-preset-react babel-preset-env lodash
  ```
  * Make changes to `package.json`
    * Change `"main": "app.jsx"` to `{your_root_file}.jsx`
    * Add `"webpack": "webpack --watch"`
- Set-up Webpack
  ```bash
  touch webpack.config.js
  ```
  ```js
  // webpack.config.js
  var path = require('path');

  module.exports = {
    context: __dirname,
    entry: "./frontend/{your_root_file}.jsx",
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
- Set-up .gitignore
  ```bash
  # .gitignore
  node_modules/
  bundle.js
  bundle.js.map
  ```
  You can create and write the file in command line at the same time
  ```bash
  {
    echo 'node_modules/'
    echo 'bundle.js'
    echo 'bundle.js.map'
  } >.gitignore
  ```
