1. Create a file .gitignore in the root
2. Create dir `src` and `build`
3. Yarn init in root
```sh
yarn init -y
```
4. Create a file call `index.html` in src
```sh
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React + Template</title>
</head>
<body>
   <div id="root"></div>
</body>
</html>
```

5. Install dependacies
```sh
yarn add react react-dom
```
6. Install typescript
```sh
yarn add -D typescript @types/react @types/react-dom
```
7. Add config for typescript compiler
Create a file in the root call `tsconfig.json`
```sh
{
  "compilerOptions": {
    "target": "ES5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "ESNext" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "moduleResolution": "node" /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */ /* Type declaration files to be included in compilation. */,
    "lib": [
      "DOM",
      "ESNext"
    ] /* Specify library files to be included in the compilation. */,
    "jsx": "react-jsx" /* Specify JSX code generation: 'preserve', 'react-native', 'react' or 'react-jsx'. */,
    "noEmit": true /* Do not emit outputs. */,
    "isolatedModules": true /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */,
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking of declaration files. */,
    "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */,
    "resolveJsonModule": true
    // "allowJs": true /* Allow javascript files to be compiled. Useful when migrating JS to TS */,
    // "checkJs": true /* Report errors in .js files. Works in tandem with allowJs. */,
  },
  "include": ["src/**/*"]
}
```
8. Create a file in src all App.tsx, it contain the root component
```sh
export const App = () => {
  return <h1>React TypeScript Webpack Starter Template</h1>;
};
```
9. Create a file in src call `index.tsx`
```sh
import ReactDOM from "react-dom";
import { App } from "./App";

// To mount
ReactDOM.render(<App />, document.getElementById("root"));
```
10. Install package for convert `React+Typescript` to javascript
```sh
yarn add -D @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript
```
11. Add `babel` config file
Create a file `.babelrc` in the root
```sh
{
  "presets": [
    "@babel/preset-env",
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ],
    "@babel/preset-typescript"
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "regenerator": true
      }
    ]
  ]
}
```
12. Install `webpack`
```sh
yarn add -D webpack-cli webpack-dev-server html-webpack-plugin
yarn add -D style-loader css-loader @babel/plugin-transform-runtime
yarn add -D webpack
```
13. Add babel loader
```sh
yarn add -D babel-loader
```
14. Config `webpack`
Create a dir in root call `webpack` and create a file call `webpack.config.js`
```sh
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.resolve(__dirname, "..", "./src/index.tsx"),
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: [
          {
            loader: "babel-loader",
          },
        ],
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: "asset/resource",
      },
      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: "asset/inline",
      },
    ],
  },
  output: {
    path: path.resolve(__dirname, "..", "./build"),
    filename: "bundle.js",
  },
  mode: "development",
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "..", "./src/index.html"),
    }),
  ],
};
```
15. Add script to run
Config inside package.json
```sh
...
 "scripts": {
    "start": "webpack serve --config webpack/webpack.config.js --open"
  },
...
```
16. To config render image such as `.png and svg` file in project
Create a file in src call `declaration.d.ts`
```sh
declare module '*.png'
declare module '*.svg' {
  const content: string
  export default content
}
```
```sh
...
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: "asset/resource",
      },
      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: "asset/inline",
      },
...
```
17. Config webpack for multi env
Lete rename `webpack.config.js` to `webpack.common.js`.
Create files like `webpack.dev.js` `webpack.prod.js` and `webpack.config.js`
- `webpack.common.js`
```sh
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.resolve(__dirname, "..", "./src/index.tsx"),
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: [
          {
            loader: "babel-loader",
          },
        ],
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: "asset/resource",
      },
      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: "asset/inline",
      },
    ],
  },
  output: {
    path: path.resolve(__dirname, "..", "./build"),
    filename: "bundle.js",
  },
  mode: "development",
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "..", "./src/index.html"),
    }),
  ],
};
```
- `webpack.dev.js`
```sh
const webpack = require("webpack");
module.exports = {
  mode: "development",
  devtool: "cheap-module-source-map",
};
```
- `webpack.prod.js`
```sh
const webpack = require("webpack");

module.exports = {
  mode: "production",
  devtool: "source-map",
};
```
To merge all both, Need to install package to work like:
```sh
yarn add -D webpack-merge
```
- `webpack.config.js`
```sh
const { merge } = require('webpack-merge')
const commonConfig = require('./webpack.common.js')

module.exports = (envVars) => {
  const { env } = envVars
  const envConfig = require(`./webpack.${env}.js`)
  const config = merge(commonConfig, envConfig)
  return config
}
```
- Config in `package.json`
```sh
  "scripts": {
    "start": "webpack serve --config webpack/webpack.config.js --env env=dev --open",
    "build": "webpack --config webpack/webpack.config.js --env env=prod"
  },
```
  - Run build: `npx serve`
  <!-- https://youtu.be/xKQ2rEoYmXw?si=F9zMtUxG5kxpGpxd -->