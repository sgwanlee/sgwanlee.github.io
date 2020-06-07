---
title: Webpack + Babel + Typescript 그리고 절대경로 import시 버그 해결
layout: post
comments: true
category: [dev, rails]
tags: [rails]
---

<!--more-->

사용된 package version입니다.

```
  webpack 4.41.0
  babel-loader: 8.0.6
  @babel/preset-typescript: 7.10.1
  babel-plugin-module-resolver: 4.0.0
```

<br/>
<br/>

# 모듈을 찾지 못하는 상황

가장 이상했던 점이 상대경로로 가져왔을 때는 문제가 없는데, 절대경로로 가져왔을 때는 module을 찾지 못한다는 점.
의심이 들어서 `.tsx` extension을 붙여주니 제대로 module을 인식하네요.

```javascript
import Counter from "./Conter"; // ok
import Coutner from "components/Counter"; // can't not resolve the module
import Counter from "components/Coutner.tsx"; // ok
```

webpack에서 아래와 같이 `resolve.extension`을 살장헤주었지만, 변화가 없고..

```js
resolve: {
  extension: ["ts", "tsx", "js", "jsx"];
}
```

`.babelrc` 를 살펴보아도 별반 이상한 점이 없는데..

```js
  {
    "presets": [
      "@babel/preset-env",
      "@babel/preset-typescript"
    ],
    "plugins": [
      [
        "module-resolver",
        {
          "root": ["./src"]
        }
      ]
    ]
  }
```

<br/>
<br/>

# 해결책!

아무래도 extension을 인식하지 못하서 발생하는 에러여서 혹시나 하는 마음에 `module-resolver`에 extension을 설정해주니 버그 해결되었습니다!

```js
  {
    "presets": [
      "@babel/preset-env",
      "@babel/preset-typescript"
    ],
    "plugins": [
      [
        "module-resolver",
        {
          "extension": ["ts", "tsx", "js", "jsx"];
          "root": ["./src"]
        }
      ]
    ]
  }

```
