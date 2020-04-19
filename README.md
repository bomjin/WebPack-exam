# Webpack이란?
웹팩에 대한 개념을 잡기 위해서는 우선 모듈 번들러(Module Bundler)가 무엇인지 알아야 한다.

# Module
프로그램을 구성하는 구성 요소의 일부
관련된 데이터와 함수들이 묶여서 모듈을 형성하고 파일 단위로 나뉘는 것이 일반적이다.
모듈화 프로그래밍은 기능별로 파일을 나눠가며 프로그래밍을 하는 것으로 유지보수가 쉽다는 장점이 있다.
# Bundler
번들러는 지정한 단위로 파일들을 하나로 만들어서 요청에 대한 응답으로 전달할 수 있는 환경을 만들어주는 역할을 한다.
번들러를 사용하면 소스 코드를 모듈별로 작성할 수 있고 모듈간 또는 외부 라이브러리의 의존성도 쉽게 관리할 수 있다.
# Webpack
웹팩(Webpack)은 자바스크립트 정적 모듈 번들러(Static Module Bundler)이다
웹팩에서 모든 것은 모듈이다. 자바스크립트, 스타일시트, 이미지 등 모든 것을 자바스크립트 모듈로 로딩해서 사용한다.
웹팩의 주요 네 가지 개념으로 Entry, Output, Loader, Plugin이 있다.
# 1. Entry
의존성 그래프의 시작점을 웹팩에서는 엔트리(Entry)라고 한다.
웹팩은 엔트리를 통해서 필요한 모듈을 로딩하고 하나의 파일로 묶는다.
여러개의 엔트리가 존재할 수 있다.
# 2. Output
엔트리에 설정한 자바스크립트 파일을 시작으로 하나로 묶는다. 그후 번들된 결과물을 처리할 위치를 output에 기록한다.
# 3. Loader
웹팩은 오직 JavaScript와 Json만 이해할 수 있다.
로더는 다른 Type의 파일(img, font, stylesheet 등)을 웹팩이 이해하고 처리 가능한 모듈로 변환 시키는 작업을 한다.
# 4. Plugin
로더가 파일단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다.
로더가 변환하는 동안 플러그인은 bundle optimization, asset management and injection of environment과 같은 일을 진행할 수 있다.
실습
# 1. package.json 파일 생성
실습을 위한 디렉터리(react-webpack)를 만든 후 아래 명령어를 통해 초기화
```
npm init -y
```
# 2. Webpack 및 사용할 라이브러리 설치
```
npm install -save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader clean-webpack-plugin css-loader html-loader html-
webpack-plugin mini-css-extract-plugin node-sass react react-dom sass-loader style-loader webpack webpack-cli webpack-dev-server
```
```
3. ./src/test.js 생성
// ./src/test.js

console.log("webpack test");
4. ./webpack.config.js 생성
// ./webpack.config.js

const path = require("path");

module.exports = {
  entry: "./src/test.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none"
};
entry
```
웹팩이 빌드할 파일을 알려주는 역할

여기서는 src/test.js 파일 기준으로 import 되어 있는 모든 파일들을 찾아 하나의 파일로 합치게 된다.

 # output
웹팩에서 빌드를 완료하면 output에 명시되어 있는 정보를 통해 빌드 파일을 생성

mode : 웹팩 빌드 옵션
production : 최적화되어 빌드 되어지는 특징
development : 빠르게 빌드하는 특징
none : 아무 기능 없이 웹팩으로 빌드
```
5. ./package.json에 내용 수정
  "scripts": {
    "build": "webpack"
  },
  ```
webpack 명령어가 실행되면 디폴트로 실행할 파일은 같은 경로에 있는 webpack.config.js에 내용을 가지고 빌드 된다.

# 6. Build 하기
```
npm run-script build
```

정상적으로 실행이 되었다면 build 폴더와 bundle.js 파일이 생성된다.



# 7. Webpack으로 HTML 빌드하기
웹팩은 자바스크립트 파일뿐만 아니라 다른 파일들도 모듈로 관리 할 수 있는데 Loader 기능이 자바스크립트 이외의 파일을 웹팩이 이해할 수 있게 해준다.

로더 사용법
```
module : {
rules: {
   test: '가지고올 파일 정규식',
   use: [
       {
           loader: '사용할 로더 이름',
           options: { 사용할 로더 옵션 }
       }
   ]
}
}
```
```
1 ) ./public/index.html 생성
<!-- ./public/index.html -->
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="utf-8" />
    <title>WEBPACK4-REACT</title>
  </head>
  <body>
    <noscript>스크립트가 작동되지 않습니다!</noscript>
    <div id="root"></div>
  </body>
</html>
2 ) ./webpack.config.js 수정
// ./webpack.config.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/test.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
            template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    })
  ]
};
```
HtmlWebPackPlugin 은 웹팩이 html 파일을 읽어서 html 파일을 빌드할 수 있게 해준다.

loader 는 html 파일을 읽었을 때 html-loader를 실행하여 웹팩이 이해할 수 있게 하고 options 로는 minimize라는 코드 최적화 옵션을 사용하고 있다.

3 3 ) Build 하기
npm run-script build
정상적으로 실행이 되었다면 ./build/index.html 파일이 생성된다.



minimize 옵션이 켜져 있어서 파일 내용이 한줄로 되어 있다.

minimize 옵션을 끈다면 줄바꿈된 형태로 나온다.

HtmlWebPackPlugin 은 웹팩 빌드시 output에 있는 bundle.js를 자동으로 import 한다.

# 8. Webpack으로 React 빌드하기
```
1 ) ./src/index.js 생성
import React from "react";
import ReactDOM from "react-dom";
import Root from "./Root";

ReactDOM.render(<Root />, document.getElementById("root"));
2 ) ./src/Root.js 생성
import React from 'react';

const Root = () => {
  return (
    <h3>Hello, React</h3>
  );
};

export default Root;
```

# 3 ) ./.babelrc 생성
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
바벨(Babel)은 ES6에서 ES5로 자바스크립트를 변환해주는 역할을 한다.

위 코드는 바벨이 ES6와 리액트를 ES5로 변환할 수 있게 해주는 내용이다.

# 4 ) ./webpack.config.js 수정
```
// ./webpack.config.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ['babel-loader'],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    })
  ]
};
```
# 5 ) Build 하기
```
npm run-script build
```
build 후 index.html 파일을 열면 Hello, React 가 보인다.




# 9. Webpack에서 CSS 사용하기
1 ) ./src/style.css 생성
```
/*./src/style.css */

.title {
    color: #2196f3;
    font-size: 52px;
    text-align: center;
}
2 ) ./src/Root.js 수정
// ./src/Root.js

import React from 'react';
import './style.css';

const Root = () => {
  return (
    <h3 className="title">Hello, React</h3>
  );
};

export default Root;
```
이후 npm run-script build 를 하면 다음과 같이 에러가 발생한다.



CSS를 읽게 하기 위해 css-loader를 적용해야 한다.

# 3 ) ./webpack.config.js 수정
```
css-loader를 추가

// ./webpack.config.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ['babel-loader'],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.css$/,
        use: ['css-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    })
  ]
};
```
# 4 ) Build 하기
```
npm run-script build
```

정상적으로 build가 되지만 ./build/index.html을 웹으로 열면 css 적용이 되지 않았다.



이유는 웹팩에서 css 파일을 읽은 후 어딘가에 저장을 해야하기 때문이다.

이럴 때 css를 index.html에 합치는 방법도 있지만 파일을 추출해 보도록 한다.

# 5 ) ./webpack.config.js 수정
webpack.config.js에 css를 추출해서 파일로 저장하는 플러그인을 적용
```
// ./webpack.config.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ['babel-loader'],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
};
```
추가한 코드 중 use: [MiniCssExtractPlugin.loader, 'css-loader'] 부분의 작동 순서는 오른쪽에서 왼쪽 순서로 실행 된다.

의미는 css-loader 로 css 파일을 읽고 MiniCssExtractPlugin.loader 로 읽은 css를 파일로 추출

# 6 ) Build 하기
```
npm run-script build
```
성공적으로 build 했다면 ./build/style.css 파일이 생성이 된다.



./build/index.html 파일을 웹브라우저로 열면 css가 적용된 것을 확인할 수 있다.



# 10. Webpack에서 SCSS 사용하기
SCSS 가 뭔지 몰라서 아래 링크의 글을 참고했다.

HTML, CSS, SASS, SCSS
```
1 ) ./src/style.scss 생성
// ./src/style.scss

$fontColor: #2196f3;
$fontSize: 52px;


.title {
  color: $fontColor;
  font-size: $fontSize;
  text-align: center;
}
2 ) ./src/Root.js 수정
import React from 'react';
import './style.scss';       // 수정한 부분

const Root = () => {
  return (
    <h3 className="title">Hello, React</h3>
  );
};

export default Root;
```
이 상태에서 npm run-script build 를 수행하면 에러가 발생한다.



이는 웹팩이 scss 파일을 읽을 수 없어서 나오는 에러이므로 웹팩이 scss 파일을 읽을 수 있게 loader를 설정해 주어야 한다.

# 3 ) ./webpack.config.js 수정
```
// ./webpack.config.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ['babel-loader'],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
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
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
};
use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
```
css 때 와 같이 오른쪽에서 왼쪽으로 작동을 한다.

먼저 sass-loader로 scss 파일을 읽고 css로 변환한 후 css-loader로 css 읽는다. 그 후 MiniCssExtractPlugin으로 읽은 CSS를 파일로 추출

# 4 ) Build 하기
npm run-script build


성공적으로 SCSS가 적용이 된 것을 확인할 수 있다.



## 11. Webpack 개발 서버 적용하기
지금까지는 소스코드를 수정할 때마다 웹팩으로 직접 빌드를 했으나 매번 하기에는 번거롭다.

이런 불편함을 해결하기 위해 소스코드를 수정할 때마다 자동으로 웹팩이 빌드해주는 webpack-dev-server 를 적용하려 한다.

# 1 ) ./webpack.config.js 수정
devServer 를 추가
```
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  devServer: {
    contentBase: path.resolve("./build"),
    index: "index.html",
    port: 9000
  },
  mode: "none",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ['babel-loader'],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
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
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
};
2 ) ./package.json 수정
scripts 부분에 내용 추가

"scripts": {
    "build": "webpack",
    "start": "webpack-dev-server --hot"
}
```
이후 터미널에서 npm start 를 실행하고 로컬호스트에 접속한다.



./src/Root.js 에 출력될 문자를 아무거나 바꾸고 저장하면 자동으로 빌드가 된다.





# 12. clean-webpack-plugin 적용하기
이번에는 build 디렉터리를 clean-webpack-plugin 을 통해서 빌드될 때마다 안 쓰는 파일들을 삭제할 수 있도록 해본다.

# 1 ) ./webpack.config.js 수정
plugins의 MiniCssExtractPlugin에 filename을 style-test.css로 변경
```
plugins: [
    new HtmlWebPackPlugin({
        template: './public/index.html', 
        filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
        filename: 'style-test.css'   // 수정한 부분
    })
]
```
이후 npm run-script build 를 한 후 build 디렉터리를 보면 사용하지 않는 style.css 가 존재하는 걸 볼 수 있다.



# 2 ) ./webpack.config.js 재수정
clean-webpack-plugin 을 추가하고 plugins 에 적용
```
// const CleanWebpackPlugin = require("clean-webpack-plugin");  // 추가 코드
const {CleanWebpackPlugin} = require("clean-webpack-plugin"); // 추가 코드

module.exports = {
     ...,
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', 
      filename: 'index.html' 
    }),
    new MiniCssExtractPlugin({
      filename: 'style-test.css'
    }),
    new CleanWebpackPlugin()  								// 추가 코드
  ]
};
```
19.10.30 현재 위 상태로 코드를 수정한 뒤 빌드를 하면 아래와 같이 에러가 발생한다.



TypeError: CleanWebpackPlugin is not a constructor 이 곳을 참고하여

const CleanWebpackPlugin ==> const {CleanWebpackPlugin} 로 변경하니 정상적으로 동작하였다.

# 3 ) Build 하기
npm run-script build
사용 안 하는 style.css 가 삭제된 것을 확인할 수 있다.



# 13. Webpack 빌드 모드 나누기
웹팩은 빌드 모드가 Development 와 Production 에 따라 차이가 있다.

Development : 빠르게 빌드하기 위해 빌드할 때 최적화를 안 하는 특징
Production : 빌드할 때 최적화 작업을 함
파일을 두 가지로 나누어 적용해 보기로 한다.

# 1 ) ./config/webpack.config.dev.js 생성
19.10.30 위와 같은 오류가 발생하여

const CleanWebpackPlugin ==> const {CleanWebpackPlugin} 로 변경

// ./config/webpack.config.dev.js
```
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const {CleanWebpackPlugin} = require("clean-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "../build")
  },
  mode: "development",
  devServer: {
    contentBase: path.resolve(__dirname, "../build"),
    index: "index.html",
    port: 9000
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"]
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "style.css"
    }),
    new CleanWebpackPlugin()
  ]
};
2 ) ./config/webpack.config.prod.js 생성
19.10.30 위와 같은 오류가 발생하여

const CleanWebpackPlugin ==> const {CleanWebpackPlugin} 로 변경

// ./config/webpack.config.prod.js

const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const {CleanWebpackPlugin} = require("clean-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.[contenthash].js",
    path: path.resolve(__dirname, "../build")
  },
  mode: "production",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"]
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      },
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "style.css"
    }),
    new CleanWebpackPlugin()
  ]
};
3 ) ./package.json 수정
"scripts": {
  "start": "webpack-dev-server --config ./config/webpack.config.dev --hot",
  "build": "webpack --config ./config/webpack.config.prod"
 },
 ```
기존 webpack.config.js 와 다른점은 mode를 development 로 했냐 production 으로 했냐의 차이이다.

지금은 큰 차이가 없지만 development 와 production 에 맞는 플러그인들을 적용하면서 붙여 나가기 시작하면 mode에 따라 강점이 생길 것 이다.

npm start 와 npm run-script build 가 정상 작동하는지 확인해 본다.
