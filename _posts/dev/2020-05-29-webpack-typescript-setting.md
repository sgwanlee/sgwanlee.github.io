---
title: TypeScript + React + Babel + Webpack 설정
layout: post
comments: true
category: [dev, react]
tags: [react]
---

React에 Typescript 설정을 추가해 보기

<!--more-->

이 글에서는 기존에 React 프로젝트에서 Typescript 설정을 추가하는 방법을 다룹니다.
<br/>
<br/>
<br/>

### 1. Package 설치

```
npm i --save @babel/preset-typescript
npm i --save @types/react
npm i --save @types/react-dom
npm i --save @types/react-router-dom
npm i --save @types/react-transition-group
npm i --save @types/styled-components
npm i --save @types/webpack-env
```

<br/>
<br/>

### 2. `.babelrc` 설정

presets 에 `@babel/preset-typescript` 추가합니다. presets는 제일 마지막에 있는 preset이 제일 먼저 적용되는데, 확인해보진 않았지만, stackoverflow에서 preset-typescript를 preset-env 앞에 두었을 때 제대로 동작이 안된다고 하는 글을 본 적이 있습니다.

```
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "entry",
        "debug": false
      }
    ],
    "@babel/preset-react",
    "@babel/preset-typescript"  // 추가된 부분
  ],
  "plugins": [
    [
      "module-resolver",
      {
        "root": ["./src"]
      }
    ],
    "@babel/plugin-proposal-class-properties"
  ]
}

```

<br/>
<br/>

### 3. `webpack.config.js` 설정

```

  rules: [
    test: /\.(js|jsx|ts|tsx)$/,
    exclude: /node_modules/,
    loader: [
      {
        loader: "babel-loader",
        options: {
          plugins: isStagingOrProduction ? prodBabelPlugins : devBabelPlugins
        }
      }
    ]
  ]
```
