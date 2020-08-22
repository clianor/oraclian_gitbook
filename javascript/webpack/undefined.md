---
description: >-
  https://velog.io/@hih0327/Webpack-%EA%B8%B0%EC%B4%88#9-webpack%EC%97%90%EC%84%9C-css-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
  참고
---

# 기초

## 웹팩이란?

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

### 9. CSS 사용하기

#### 1. src/App.css 파일 추

```css
.title {
    color: #2196f3;
    font-size: 52px;
    text-align: center;
}
```

#### 2. src/App.jsx 파일 수정

```javascript
import React from 'react';
import './App.css';

const App = () => {
  return (
    <h3 className="title">Hello, React</h3>
  );
};

export default App;

```

#### 3. webpack.config.js 수정

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
      },
      {
        test: /\.css$/,
        use: ['css-loader']
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

css-loader를 추가하여 웹팩이 css를 이해할 수 있게 하였습니다.

하지만 여기까지 작업을하면 build를 하여도 css가 저장이 되지 않습니다.  
[여기](https://webpack.js.org/plugins/mini-css-extract-plugin/)를 참조하여 mini-css-extract-plugin을 추가합니다.

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
}
```

### 10. SCSS 사용하기

#### 1. src/App.css 파일을 src/App.scss로 변경한다.

#### 2. src/App.scss 파일을 다음과 같이 수정한다.

```css
$fontColor: #2196f3;
$fontSize: 52px;


.title {
    color: $fontColor;
    font-size: $fontSize;
    text-align: center;
}
```

#### 3. src/App.jsx 파일을 다음과 같이 수정한다.

```javascript
import React from 'react';
import './App.scss';

const App = () => {
  return (
    <h3 className="title">Hello, React</h3>
  );
};

export default App;
```

#### 4. webpack.config.js 수정

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
}
```

sass-loader를 추가하여 웹팩이 scss를 이해할 수 있게 하였습니다.

### 11. webpack-dev-server적용

개발 할때는 매번 빌드해서 확인할 수 없으므로 개발 서버를 이용하여 코드 수정시 웹팩이 자동으로 빌드하고 반영해주는 webpack-dev-server를 적용합니다.

#### 1. webpack.config.js 수정

[여기](https://webpack.js.org/configuration/dev-server/)를 참고하여 작업합니다.

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: "./src/index.jsx",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname + "/dist")
  },
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 9000
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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
}
```

#### 2. package.json 수정

```javascript
"scripts": {
  "build": "webpack",
  "dev": "webpack-dev-server --hot"
},
```

 위와 같이 작성하고 `npm run dev` 로 실행을하면 이제 파일을 매번 빌드하지 않아도 파일이 수정되면 바로 브라우저에서 확인할 수 있습니다.

### 12. clean-webpack-plugin 적용

clean-webpack-plugin 은 빌드될 때 사용되지 않는 파일들을 삭제해주는 플러그인입니다.

#### 1. webpack.config.js 수정

[여기](https://webpack.js.org/guides/output-management/#cleaning-up-the-dist-folder)를 참고하여 작업합니다.

```javascript
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: "./src/index.jsx",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname + "/dist")
  },
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 9000
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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    }),
    new CleanWebpackPlugin()
  ]
}
```

### 13. 웹팩 모드 나누기

위에서 웹팩의 주요 속성중에 Mode가 있다고 이야기를 했었습니다.  
웹팩의 빌드 모드 중에 Production과 Development가 있는데 특징은 아래와 같습니다.

1. Production - 빌드할때 최적화 작업을 한다.
2. Development - 빠르게 빌드하기 위해 최적화 작업을 하지 않는다.

#### 1. config 폴더를 만들어 내부에 webpack.config.dev.js와 webpack.config.prod.js 파일을 작

```javascript
// webpack.config.dev.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: "./src/index.jsx",
  output: {
    filename: "index.js",
    path: path.resolve(__dirname, "../dist")
  },
  devServer: {
    contentBase: path.join(__dirname, '../dist'),
    compress: true,
    port: 9000
  },
  mode: "development",
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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    }),
    new CleanWebpackPlugin()
  ]
}
```

```javascript
// webpack.config.prod.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: "./src/index.jsx",
  output: {
    filename: "bundle.[contenthash].js",
    path: path.resolve(__dirname, "../dist")
  },
  mode: "production",
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
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
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
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    }),
    new CleanWebpackPlugin()
  ]
}
```

#### 2. 기존의 webpack.config.js 파일 삭제

#### 3. package.json 파일 수정

```javascript
"scripts": {
  "build": "webpack --config ./config/webpack.config.prod.js",
  "dev": "webpack-dev-server --config ./config/webpack.config.dev.js --hot"
},
```

이제 `npm run build`와 `npm run dev`를 할때 기존과는 조금 다른 결과들을 볼 수 있습니다.

가장 큰 차이점으로는 개발 모드로 실행했을 경우에는 dist 폴더가 생기지 않는다는 점입니다.

그 이유는 개발 모드로 실행했을시 빌드된 결과를 메모리에 적재시킨뒤 읽어오기 때문이며 그렇게해서 개발 할때 빌드하고 읽어오는 속도를 올리기 위함입니다.

개발 모드가 아니라 운영 모드로 빌드시에는 기존의 index.js파일이 생겨나는 것이 아니라 해시를 이용하여 알아보기 힘든 파일 명으로 생성이 됩니다.

### 14. 이미지 사용하기

여기까지 작업하면 이제 리액트의 개발환경이 셋팅되었다고 할 수 있지만 아직 문제가 있습니다.

지금 상태에서는 이미지를 처리할 수 없다는 점입니다.

이를 처리하기 위해서 file-loader를 이용하겠습니다.

[여기](https://webpack.js.org/loaders/file-loader/)를 참고하시면됩니다.

#### 1. file-loader 라이브러리 다운로

```javascript
npm i -D file-loader
```

#### 2. webpack.config.dev.js와 webpack.config.prod.js의 rules 아래의 내용 추가

```javascript
{
  test: /\.(png|jpe?g|gif)$/i,
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]',
  },
},
```

#### 3. src/assets 폴더를 만들고 img.jpg 파일을 하나 추가

#### 4. src/App.jsx 파일 수정

```javascript
import React from 'react';
import './App.scss';
import Image from './assets/img.jpg';

const App = () => {
  return (
    <>
      <h3 className="title">Hello, React</h3>
      <img src={Image} />
    </>
  );
};

export default App;

```

#### 5. 이제 빌드 또는 개발모드로 실행

이제 이미지가 보이는 것을 볼 수 있습니다.

