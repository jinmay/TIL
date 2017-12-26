# Webpack

**Module bundler**

Webpack은 자바스크립트 모듈 번들러로 자바스크립트로 작성된 모듈을 번들링 하여 하나 또는 여러개의 번들로 만들어 주는 도구이다.

여러개의 흩어져있는 .js 파일들을 하나 혹은 여러개의 번들로 만들어 주는 것이라고 이해하려고 한다.

예시코드

~~~js
// webpack.config.js

module.exports = {
    entry: './index.js',
    output: {
        path: '/webpack_practice/',
        filename: 'bundle.js'
    },    
    module:{
        rules: [
            {
                test: /\.js$/,
                loader: "babel-loader",
                options: {
                    presets: ['es2015']
                }
            }
        ]
    },
    plugins: [new webpack.optimize.UglifyJsPlugin()]
};
~~~

웹팩의 주요 네 가지 개념에 대해서 알아보자 : **entry / output / loader / plugin**

### entry

웹팩에서 모든 것은 모듈이다.  js / css / jpg 등등을 모듈로 로딩해서 사용한다고 한다.

자바스크립트가 로딩하는 모듈이 많아질수록 서로간의 의존성은 증가하게 되고 또한 관리하기도 어려워지는데 **이 모든 모듈의 의존성의 시작을 entry(엔트리) 라고 한다.**

웹팩은 엔트리를 통해서 필요한 모듈을 로딩하고 하나의 파일로 묶는 역할을 한다.

~~~js
// example
module.exports = {
	entry: './index.js', // 여러번들을 생성하는 진입점을 허용하기 위해 array로 지정해도 된다.
}
~~~

이용할 html에서 모든 js의 의존성이 시작되는 파일이 현재폴더의 index.js 파일이다.

### output

번들링 작업을 마친 결과물에 대한 설정을 한다.

~~~js
module.exports = {
  entry: './index.js',
  output: {
    path: './practice/webpack/'
    filename: 'bundle.js'
  }
}
~~~

* path : 결과물이 어디에 위치할 지 지정한다
* filename : 번들링 작업 결과물 파일의 이름을 정한다

### loader

웹팩은 js 뿐만 아니라 css / png 같은 파일들도 모듈로 관리한다. 

웹팩에서 로더를 이용하면 js 파일 말고도 다른 파일들을 핸들링 할 수 있다. (.html / .css / .jpg)

~~~js
// index.js
require('example.css');

module.exports = {
  entry: './index.js',
  output: {
    path: './practice/webpack',
    filename: 'bundle.js'
  },
  module: {
    
  }
}
~~~



