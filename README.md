## Package.json
### scripts
- `start` to serve with webpack in development mode
- `build` to build the bundle in production mode

## Webpack
Official Webpack docs https://webpack.js.org/guides/getting-started/#basic-setup

### entry points and build folder
```javascript
entry: "./src/index.js",
output: {
  filename: "main.js",
  path: path.resolve(__dirname, "build"),
}
```
### plugins
- `HtmlWebpackPlugin` to include index.html from the public folder in the build folder 
```javascript
new HtmlWebpackPlugin({
  template: path.join(__dirname, "public", "index.html"),
})
```

### dev server
- We are using `webpack-dev-server` to serve locally together with the script `start` in port 3000
```javascript
devServer: {
  static: {
    directory: path.join(__dirname, "build")
  }
  port: 3000
}
```
## Modules
### Babel
Allows us to use all ES6 features and having them transpiled down to JS versions that all browsers can understand
We'll parse file types specified in `test` excluding `node_modules` with the appropriate loader
```javascript
module: {
  rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ["babel-loader"],
    }
  ]
}
```

### Styles
Styles are transformed with basic loaders, i.e., no SASS/LESS postprocessors except postcss, that it's used for autoprefixing 
```javascript
{
  test: /\.css$/,
  use: ['style-loader', 'css-loader', 'postcss-loader']
}
```

NOTE to make postcss work is necessary to install autoprefixer and add it to the config file:
```javascript
// postcss.config.js
module.exports = {
  plugins: [require('autoprefixer')],
}
```
together with a browserlist config block in `package.json`:
```json
// package.json
"browserslist": [
  "last 2 version",
  "not dead",
  "iOS >= 9"
]
```

## Babel
pass all js files through Babel 
```javascript
resolve: {
  extensions: ["*", ".js", ".jsx"]
}
```

### .babelrc
- `preset-env` preset that transforms the latest JS into compatible one
- `preset-react` transforms react syntax into compatible JS

```json
{
  "presets": [
    "@babel/preset-env",
    ["@babel/preset-react", {
      "runtime": "automatic"
    }]
  ]
}
```

### TODO
- [ ] Typescript compatible
- [ ] Move items in public to build folder
- [ ] Manage .env
- [ ] Check build chunks
- [ ] Check uglyfication and minification
- [x] Add PostCSS autoprefixer