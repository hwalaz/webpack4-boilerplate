# Webpack Boilerplate

![Berlin](https://img.shields.io/badge/Built%20in-Berlin-critical.svg?logo=webpack) ![Repo_size](https://img.shields.io/github/repo-size/LeandroDCI/webpack4-boilerplate.svg) [![MIT License](https://img.shields.io/github/license/leandroDCI/webpack4-boilerplate.svg)](LICENSE)


- [Features](#features)
- [Project Structure](#project-structure)
- [Commands](#commands)
    - [Development](#development)
    - [Production](#production)
    - [Deploy to Github Pages](#deploy-to-github-pages)
- [Setup](#setup)
    - [Quick Setup](#quick-setup)
    - [Complete setup](#complete-setup)
        - [Create your package.json and customize it](#create-your-packagejson-and-customize-it)
        - [Install Webpack](#install-webpack)
        - [Create files](#create-files)
        - [Add HTML to your generated Bundle](#add-html-to-your-generated-bundle)
        - [Transplate your JS with Babel](#transplate-your-js-with-babel)
        - [Styling: import and inject CSS](#styling-import-and-inject-css)
        - [Import images](#import-images)
        - [Optimize CSS and Javascript assets](#optimize-css-and-javascript-assets)
        - [Deploy to Github Pages](#deploy-to-github-pages-1)

## Features

A Webpack 4 boilerplate with build-in:

- creation of HTML files to serve your webpack bundles using [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
- ECMAScript 6 to ECMAScript 5 transpiling with [babel](https://babeljs.io/) 
- CSS extraction into a single file using [style-loader](https://github.com/webpack-contrib/style-loader), [css-loader](https://github.com/webpack-contrib/css-loader) and [css-mini-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin) 
- SCSS support using [sass-loader](https://github.com/webpack-contrib/sass-loader) and [node-sass](https://github.com/sass/node-sass).
- Images import with [file-loader](https://github.com/webpack-contrib/file-loader)
- Optimization/Minification with [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin) and [optimize-css-assets-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin). 
- Github Pages publishing using [gh-pages](https://www.npmjs.com/package/gh-pages)


## Project Structure

```
Project
│
│   README.md
│   package.json
│   webpack.config.js
└───src
│   │   index.html
│   │
│   └───assets
│       └───js
│            └───index.js
│       └───scss
│            └───styles.scss
│       └───img
│            └───logo.png
│
└───dist

```

### Commands

#### Development

Run **Webpack** in **Development** mode and start coding!

```
npm run dev
```

#### Production

Run **Webpack** in **Production** mode.

```
npm run build
```

#### Deploy to Github Pages

Deploy your code to **Github Pages**: this script creates a 'gh-pages' branch and serve the production bundle to this branch (ie. [https://leandrodci.github.io/webpack4-boilerplate/](https://leandrodci.github.io/webpack4-boilerplate/))

```
npm deploy
```


## Setup

### Quick Setup

Create a directory for your new project, clone this repository, install the required modules and start coding!

```
mkdir myNewProject && cd myNewProject
clone https://github.com/LeandroDCI/webpack4-boilerplate .
npm i
npm run dev
```

### Complete setup

This is the complete setup

#### Create your package.json and customize it

```

npm init

```

#### Install Webpack

```

npm i -D webpack webpack-cli

```

create an empty configuration file

```javascript
const path = require("path");

module.exports = {
  entry: "./src/assets/js/index.js",
  output: {
    path: path.resolve(__dirname, "dist/"),
    filename: "assets/js/bundle.js",
    publicPath: ""
};
```

#### Create files
Create a file in **/src/assets/js/index.js** and insert your JS code there.

```javascript
import "../scss/styles.scss";
import logo from "../img/logo.png";

document.querySelector("#logo").src = logo

let message = "Hello Webpack";
console.log(` Message is: ${message}`);
```

create a file in  **/src/assets/js/styles.scss** and insert your SCSS styles there.

```scss

$font-stack: Helvetica, sans-serif;
$primary-color: #bb2b2b;

body {
  font: 100% $font-stack;
  color: $primary-color;
  text-align: center;
}

img {
  max-width: 500px;
}


```

insert a logo.png image in **/src/assets/img/logo.png**

#### Add HTML to your generated Bundle

Install [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) to add a index.html and generated Javascript bundle

```
npm i -D html-webpack-plugin
```

Create an HTML page

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <script src="assets/js/script.js"></script>
  </body>
</html>
```

and add the configuration to your **webpack.config.js**

```javascript
//at the beginning of the file
const HtmlWebpackPlugin = require("html-webpack-plugin");

//in the configuration -> plugins
plugins: [
  new HtmlWebpackPlugin({
    title: "Setting up webpack 4",
    template: "index.html",
    inject: true,
    minify: {
      removeComments: true,
      collapseWhitespace: true
    }
  })
];
```

#### Transplate your JS with Babel

Install [Babel](https://babeljs.io/) to transplate your ES6 down to ES5

```

npm i -D @babel/core babel-loader @babel/preset-env

```

and add the configuration to your **webpack.config.js**

```javascript
// in the configuration -> module -> rules

    //babel
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: "babel-loader",
      options: {
        presets: ["@babel/preset-env"]
      }
    }
  ];

```

#### Styling: import and inject CSS

To import and use CSS styles we need to add a [style-loader](https://github.com/webpack-contrib/style-loader) and [css-loader](https://github.com/webpack-contrib/css-loader). Css-loader will import content to a variable and style-loader will inject content into the HTML file as an inline tag. To support SCSS we also need to add [sass-loader](https://github.com/webpack-contrib/sass-loader) and [node-sass](https://github.com/sass/node-sass).

```
npm i -D style-loader css-loader sass-loader node-sass
```

and add the configuration for the loaders to your **webpack.config.js**

```javascript
// in the configuration -> module -> rules

    //style and css loader
    {
        test: /\.css$/,
        use: ["style-loader", "css-loader", 'sass-loader']
    }

```

Extracting all CSS into a single file

Styles are now injected as an inline. We will extract styles using [css-mini-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin) and we'll move the styles to an external stylesheet file.
This stylesheet will be then injected into the index.html automatically.

```
npm i -D mini-css-extract-plugin
```

and add the configuration for css-mini-extract-plugin to your **webpack.config.js**

```javascript

// at the beginning of the file
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

// in the configuration -> module -> rules
    //css extract
      {
        test: [/.css$|.scss$/],
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "sass-loader"
        ]
      }

// in the configuration -> plugins

module.exports = {

...

  plugins: [
    ...
    new MiniCssExtractPlugin({
      filename: 'app.[contenthash:8].css',
    }),
    ...
  ]
```

#### Import images

To include images we need to configure [file-loader](https://github.com/webpack-contrib/file-loader)

```
npm i -D file-loader
```

and add the configuration for file-loader to your **webpack.config.js**

```javascript
// in the configuration -> module -> rules

    //file loader
    {
        test: /\.(png|jpg|gif|svg)$/,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: '[name].[ext]',
              outputPath: 'assets/'
            }
          }
        ]
    }

```

#### Optimize CSS and Javascript assets

We want to optimize the webapp by minifying our assets with [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin) and [optimize-css-assets-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin). 
Note: Webpack 4 optimizes JS bundle by default when using **production** mode.

```
npm i -D uglifyjs-webpack-plugin optimize-css-assets-webpack-plugin
```

In webpack.config.js add

```javascript
// at the beginning of the file
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");

// in the configuration -> optimization

module.exports = {
...

  optimization: {
    minimizer: [
      new UglifyJsPlugin(),
      new OptimizeCSSAssetsPlugin()
    ]
  },

```


#### Deploy to Github Pages

We want to publish files to a new branch (called gh-pages) on GitHub using the [gh-pages](https://www.npmjs.com/package/gh-pages) module. 


```
npm i -D gh-pages
```

In **package.json** scripts add

```javascript
  "deploy": "npm run build && gh-pages -d dist",
```

This script help us to create a **gh-pages** branch on Github and also serve our bundled files on that branch.
