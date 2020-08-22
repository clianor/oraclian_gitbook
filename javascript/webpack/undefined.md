# 기초

## 웹이란?

먼저 웹팩은 웹팩의 공식 사이트에서 아래와 같이 정의하고 있습니다.

> 핵심에서 webpack은 최신 JavaScript 애플리케이션을 위한 정적 모듈 번들러입니다.  
> 웹팩은 애플리케이션을 처리 할 때 프로젝트에 필요한 모든 모듈을 매핑하고 하나 이상의 번들을 생성하는 종속성 그래프를 내부적으로 빌드합니다.

이것이 무슨 말인지 정의만 봐서는 알기 어렵습니다.

위의 내용을 간단히 정의하자면 쉽게 재사용할 수 있게 나눠둔 코드조각인 **모듈**들을 하나로 묶어 하나의 파일로 만들어 보내는 것을 **번들러**라고합니다.

결론을 말하자면 여러 파일을 묶어 하나의 파일로 만드는 역할을 수행하는 것이 웹팩이라 할 수 있습니다.

## 웹팩의 주요 속성

웹팩에는 총 6가지의 코어 컨셉이 존재하는데 이는 다음과 같습니다.

> Entry - 의존성 그래프의 시작점  
> Output -  번들된 결과물을 처리할 위치  
> Loaders - 웹팩이 이해하고 처리 가능한 모듈로 변환 시키는 작업  
> Plugins - 로더가 파일단위로 처리하는 반면 플러그인은 번들된 결과물을 처리  
> Mode - 웹팩의 실행 모드가 설정 \(설정안함, 개발 모드, 배포 모드\)  
> Browser Compatibility - ES5를 준수하는 모든 브라우저를 지원

## 실습

### 1. package.json 파일 생성

```bash
npm init -y
```

위의 명령어를 통해 package.json을 생성과 동시에 초기화 할 수 있습니다.

### 2. 라이브러리 설치

```bash
# 바벨 다운로드
npm i -D @babel/core @babel/preset-env @babel/preset-react babel-loader

# 웹팩 다운로
npm i -D webpack webpack-cli webpack-dev-server

# 웹팩 로더 다운로드
npm i -D css-loader html-loader sass-loader style-loader

# 웹팩 플러그인 다운로드
npm i -D clean-webpack-plugin html-webpack-plugin mini-css-extract-plugin node-sass 

# 리액트 다운로드
npm i -D react react-dom
```

### 3. public/index.html 생성

```markup
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Webpack Demo</title>
</head>
<body>
    <noscript>스크립트가 작동되지 않습니다!</noscript>
    <div id="root"></div>
</body>
</html>
```

### **4. src/index.js 생성**

```javascript
alert("Hello World");
```

### 5. **webpack.config.js 작성**

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname + "/dist")
  },
  mode: "none"
}
```

### 6. html-loader 및 html-webpack-html 추가

```javascript
// webpack.config.js
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname + "/dist")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.html$/i,
        loader: 'html-loader',
        options: {
          minimize: true,
        },
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html"
    })
  ]
}
```

[여기](https://webpack.js.org/loaders/html-loader/)를 참조하여 html-loader를 추가합니다.  
웹팩의 로더 추가는 아래와 같은 형태를 가집니다.

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: 정규,
        loader: 로더명'html-loader',
        options: {
          옵션
        },
      },
    ],
  },
};
```

[여기](https://webpack.js.org/plugins/html-webpack-plugin/)를 참조하여 html-webpack-html 플러그인을 추가합니다.  
웹팩에서 플러그인 추가는 다음과 같은 형태를 가집니다.

```javascript
plugins: [
  new HtmlWebPackPlugin({
    template: "./public/index.html", // public/index.html 파일을 읽는다.
    filename: "index.html" // output으로 출력할 파일은 index.html 이다.
  })
]
```

위의 설정으로 html-webpack-html 플러그인이 웹팩이 html파일을 읽어 파일을 빌드할 수 있게 해주고 html-loader 로더 추가로 html을 읽었을때 웹팩이 이해할 수 있게 해줍니다.

### 7. **Build 하기**

```javascript
"scripts": {
  "build": "webpack"
},
```

package.json 파일에 위와 같이 추가한 뒤 `npm run build`빌드합니다.

![](../../.gitbook/assets/image%20%286%29.png)

위와 같은 내용이 나오고 build 디렉토리가 추가된 것을 볼 수 있습니다.  
dist 디렉토리의 index.html 파일을 열어보면 script 태그가 추가된 것을 볼 수 있으며 index.html 파일을 라이브 서버로 실행해보면 "Hello World" alert창이 뜨는 것을 볼 수 있습니다.

### 8. React 빌드하기

#### 1. src/index.js 파일을 src/index.jsx로 파일명 변경

#### 2. webpack.config.js 파일 내용을 아래와 같이 변경

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.jsx",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname + "/dist")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: {
          loader: "babel-loader",
        }
      },
      {
        test: /\.html$/i,
        loader: 'html-loader',
        options: {
          minimize: true,
        },
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html"
    })
  ]
}
```

babel-loader를 추가함으로서 js와 jsx를 웹팩이 이해할 수 있게 하였고 resolve를 추가함으로서 jsx 파일도 js파일 내에서 import 할 수 있게 설정였습니다.

#### 3. src/index.jsx 파일 수정

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

#### 4. src/App.jsx 파일 작성

```javascript
import React from 'react';

const App = () => {
  return (
    <h3>Hello, React</h3>
  );
};

export default App;
```

#### 5. .babelrc 생성

```javascript
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

위는 바벨을 통해 ES6와 리액트를 ES5로 변환할 수 있게 해주는 설정입니다.

#### 6. 빌드

`npm run build` 로 빌드를 하고 dist 폴더의 index.html 파일을 열어 라이브 서버로 실행해보면 Hello, React가 출력되는 모습을 볼 수 있습니다.

