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
  - Navigate to `./build` folder
  - Run build: `npx serve`

18. Define own env for use
`webpack.dev.js`
```sh
const webpack = require('webpack')

module.exports = {
  mode: 'development',
  devtool: 'cheap-module-source-map',
  plugins: [
    new webpack.DefinePlugin({
      'process.env.name': JSON.stringify('Sothea Mab'),
    }),
  ],
}
```
`webpack.prod.js`
```sh
const webpack = require('webpack')

module.exports = {
  mode: 'production',
  devtool: 'source-map',
  plugins: [
    new webpack.DefinePlugin({
      'process.env.name': JSON.stringify('EZ-Code'),
    }),
  ],
}
```

19. Config reactjs refresh feature for real-time developer
Create a file call `ClickCounter.tsx` inside src
```sh
// Issue: - When the component made change, the state reset to default value
import { useState } from "react";

export const ClickCounter = () => {
  const [count, setCount] = useState<number>(0);
  return (
    <div>
      <button onClick={() => setCount((c) => c + 1)}>Count {count}</button>
    </div>
  );
};
```
Invoke in `App.tsx`
```sh
import { ClickCounter } from "./ClickCounter";
import "./styles.css";
export const App = () => {
  return (
    <div>
      <h1>
        React Typescript Webpack Starter <br/> Template - {process.env.NODE_ENV}{" "}
        {process.env.name}
        <ClickCounter />
      </h1>
    </div>
  );
};
```
- TO Solve:
Install this package for work together.
(Repo here)[https://github.com/pmmmwh/react-refresh-webpack-plugin.git]
```sh
# if you prefer npm
npm install -D @pmmmwh/react-refresh-webpack-plugin react-refresh

# if you prefer yarn
yarn add -D @pmmmwh/react-refresh-webpack-plugin react-refresh

# if you prefer pnpm
pnpm add -D @pmmmwh/react-refresh-webpack-plugin react-refresh
```
Update webpack for development:
```sh
const webpack = require("webpack");
const ReactRefreshWebpackPlugin = require("@pmmmwh/react-refresh-webpack-plugin");

module.exports = {
  mode: "development",
  devtool: "cheap-module-source-map",
  devServer: {
    hot: true,
    open: true,
  },
  plugins: [
    new ReactRefreshWebpackPlugin(),
    new webpack.DefinePlugin({
      "process.env.name": JSON.stringify("Sothea Mab"),
    }),
  ],
};
```
After config this, Let restart your app: `yarn start`
- Issue solved.

20. Esline config
```sh
yarn add -D eslint
yarn add -D eslint-plugin-react eslint-plugin-react-hooks
yarn add -D @typescript-eslint/parser @typescript-eslint/eslint-plugin
yarn add -D eslint-plugin-import eslint-plugin-jsx-a11y
```
To config eslint. create a file in root call `.eslintrc.js`
```sh
module.exports = {
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: "module",
  },
  settings: {
    react: {
      version: "detect",
    },
  },
  extends: [
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:import/errors",
    "plugin:import/warnings",
    "plugin:import/typescript",
    "plugin:jsx-a11y/recommended",
    "plugin:eslint-comments/recommended",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended",
  ],
  rules: {
    "no-unused-vars": "off",
    "@typescript-eslint/no-unused-vars": ["error"],
    "@typescript-eslint/no-var-requires": "off",
    "react/prop-types": "off",
    "react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
  },
};
```
Config `eslint` in `package.json`
```sh
...
 "scripts": {
    "start": "webpack serve --config webpack/webpack.config.js --env env=dev --open",
    "build": "webpack --config webpack/webpack.config.js --env env=prod",
    "lint": "eslint --fix \"./src/**/*.{js,jsx,ts,tsx,json}\""
  },
...
```
`yarn lint`

21. Prettier config
```sh
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```
Define config for prettier. Create a file in root call `.prettierrc.js`
```sh
module.exports = {
  semi: false,
  trailingComma: "es6",
  singleQuote: false,
  printWidth: 80,
  tabWidth: 2,
  endOfLine: "auto",
};
```
Config `package.json`
```sh
...
"scripts": {
    "start": "webpack serve --config webpack/webpack.config.js --env env=dev --open",
    "build": "webpack --config webpack/webpack.config.js --env env=prod",
    "lint": "eslint --fix \"./src/**/*.{js,jsx,ts,tsx,json}\"",
    "format": "prettier --write \"./src/**/*.{js,jsx,ts,tsx,json,css,scss,md}\""
  },
...
```
22. Husky config
```sh
yarn add -D husky@4 lint-staged
```
Define lint stage tool configuration
```sh
// package.json
...
{
  "name": "react-template",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  },
  "scripts": {
  ...
  },
  "devDependencies": {
    "@babel/core": "^7.25.9",
    "webpack-merge": "^6.0.1"
  },
  ...
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx,json}": [
      "eslint --fix"
    ],
    "src/**/*.{js,jsx,ts,tsx,json,css,scss,md}": [
      "prettier --write"
    ]
  }

}
...
```


23. Let add tailwincss to project
```sh
yarn add -D tailwindcss postcss autoprefixer
```
```sh
yarn tailwindcss init -p
```
`tailwind.config.js`
```sh
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx,html}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
`./src/styles.css`
```sh
/* ./src/styles.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```
`config in webpack.common.js`
```sh
yarn add -D postcss-loader
```
```sh
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
    ],
  },
  ...
```
Ensure `postcss.config.js` is Present
```sh
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

`yarn start`


24. Fix import file extension issue
```sh
// apps/dashboard-app/src/declaration.d.ts

declare module '*.png'
declare module '*.jpeg'
declare module '*.svg' {
  const content: string
  export default content
}
```
  - If need more file extension, Let add more:)

25. Fix alias for reacjs + typescript for project setup from scratch
  - Config in `alias` in `tsconfig.json`
```sh
{
  "compilerOptions": {
  ...
    "resolveJsonModule": true,
    "baseUrl": ".", 
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["src/**/*"]
}
```
  - Add `alias` config to `webpack.common.js`
```sh
...
resolve: {
    extensions: [".tsx", ".ts", ".js"],
    alias: {
      "@": path.resolve(__dirname, "../"),
    },
  },
...
```

26. How to config file `env` in react+webpack
 - Install dependacies
```sh
yarn add -D dotenv-webpack
```
 - Create a file for env like `.env.local`
```sh
REACT_APP_API_URL=http://127.0.0.1:8000/api/v1/project/
```
 - Config in webpage.command.js
```sh
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const Dotenv = require("dotenv-webpack");

module.exports = {
  entry: path.resolve(__dirname, "..", "./src/index.tsx"),
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
    alias: {
      "@": path.resolve(__dirname, "../"),
    },
  },
  ...
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "..", "./public/index.html"),
    }),
    new Dotenv({
      path: ".env.local", // Path to .env file
      safe: true, // Load .env.example
      systemvars: true, // Load all system variables
      defaults: false,
    }),
  ],
};
```
 - Create dir `types` in root and create a file inside it name `env.d.ts`
```sh
declare namespace NodeJS {
    interface ProcessEnv {
        REACT_APP_API_URL: string;
    }
  }
```
 - Test invoke it to use
```sh
const url_api = process.env.REACT_APP_API_URL as string
console.log(url_api)
const response = await CreateProject({
  url: url_api,
  method: "POST",
  data: {
    project_name: projectNameState,
    project_description: descriptionState,
  },
});
```

# End




<!-- https://youtu.be/xKQ2rEoYmXw?si=F9zMtUxG5kxpGpxd -->