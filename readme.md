# create react app without CRA(create-react-app)

## `Base Support`

### init
```bash
yarn init
```

### install webpack for dev
```bash
yarn add webpack webpack-cli webpack-dev-server  html-webpack-plugin -D
```

### install babel for dev
```bash
yarn add @babel/core @babel/preset-env @babel/preset-env babel-loader -D
```

### create webpack.config.js
```js
const path = require('path');
module.exports = {   
  entry: './src/index.js',   
  output: {     
     filename: 'main.js',      
     path: path.resolve(__dirname, 'dist'),   
     },
  };
```

### install react
```
yarn add react react-dom
``` 

### config plugin in webpack.config.js
```
plugins:[   
  new HtmlWebPackPlugin({      
    template: path.resolve( __dirname, 'public/index.html' ),      
    filename: 'index.html'   
  })
]
```

### create a simple react App
```jsx
import React from 'react';
export default ()=>{   
  return <div>Hello World.</div>
}
```
```jsx
import React from 'react';
import ReactDom from 'react-dom';
import App from './App';
ReactDom.render(<App />,document.getElementById('root'));
```

### config models in webpack.config.js
```js
module:{   
  rules:[{      
    test:/\.js$/,      
    exclude:/node_modules/,      
    use: {         
      loader: 'babel-loader',         
      options: {            
        presets: ['@babel/preset-env','@babel/preset-react']         }      
      }   
    }]
  }
```

## `Typescript Support`

### 1. add typescript
```bash
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

### 2. add the tsconfig.json
```
npx tsc --init
```
or create a file: tsconfig.json
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": false,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ],
  "exclude": ["build", "node_modules"],
  "typeRoots": [
    "node_modules/@types"
  ]
}
```

### 3. add ts-loader
```
ts-loader
```

### 4. update webpack.config.js
```js
module: {
  rules: [
    { test: /\.(ts|tsx)$/, loader: 'ts-loader', exclude: /node_modules/ },
    {
      test: /\.(js|jsx)$/,
      use: [
        {
          loader: 'source-map-loader',
        },
        {
          loader: 'babel-loader',
        },
      ],
      exclude: /node_modules/,
    },
  ],
},
resolve: {
    extensions: ['.ts', '.tsx', '.js'],
    alias: {
      '@': path.resolve(__dirname, './src')
    }
}
```

## `HTML/CSS/SASS Support`
### add HTML/CSS loaders
```bash
yarn add css-loader html-loader sass sass-loader style-loader -D
```

### update webpack.config.js
```js
module: {
    rules: [
      {
        // look for .css or .scss files
        test: /\.(css|scss)$/,
        // in the `src` directory
        use: [
          {
            loader: 'style-loader',
          },
          {
            loader: 'css-loader',
          },
          {
            loader: 'sass-loader',
            options: {
              sourceMap: true,
            },
          },
        ],
      },
      {
        test: new RegExp('.(' + fileExtensions.join('|') + ')$'),
        type: 'asset/resource',
        exclude: /node_modules/,
        // loader: 'file-loader',
        // options: {
        //   name: '[name].[ext]',
        // },
      },
      {
        test: /\.html$/,
        loader: 'html-loader',
        exclude: /node_modules/,
      },
    ],
  },
```