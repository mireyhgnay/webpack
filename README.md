# webpack
webpack setting!!

### 1. npm init

```bash
npm init
```

👉  init를 입력하면 package-name을 설정하라고 뜨는데 enter를 누르면 자동으로 폴더 이름이 입력된다.     
(단, 한글은 인식하지 못하므로 한글일 경우 영어로 package 이름을 설정해줘야 한다.)     
이후에 계속 뭘 물어보지만 enter, enter, …      
그리고 마지막에 Is this OK?라고 물어보는데 yes라고 입력해도 되고 그냥 enter를 눌러도 된다.     

👉 이 모든 과정이 끝나면 폴더에 package.json 파일이 생성된다.

### 2. webpack 설치

```bash
npm install webpack webpack-cli --save-dev
```

👉  설치가 끝나면 node_modules 폴더와 package-lock.js 파일이 생성된다

### 3. html, js, css파일들이 들어갈 src 폴더 생성

👉   프로젝트 폴더 아래 src 폴더를 만들어, html, js, css 등 소스코드를 넣어준다.

👉  폴더 구조 (웹팩설정 = 프로젝트 폴더명)
- 폴더 구조
![스크린샷 2022-10-18 오전 12 24 13](https://user-images.githubusercontent.com/111990266/196453442-16e01365-7493-4e1b-95dc-55b9d0a2d11f.png)
    

### 4. webpack.config.js 파일 생성
👉   webpack이 해당 프로젝트 안에서 어떤 식으로 구상되어 실행될 것인지 정하는 파일    
👉   위치는 프로젝트 폴더 바로 아래**(package.json 파일과 같은 위치)**    

```jsx
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: `${__dirname}/dist`,
  },
};
```

- 우리가 프로젝트 내부에서 여러 js파일이나 css 파일을 생성해도 결국 최종적으로 webpack이 확인하는 파일은 entry에 있는 js파일(**index.js**)이다.
- webpack이 실행되면 해당 js파일 **(index.js)을 참고하여, webpack은 최종적으로 js파일을 만들게 되는데, 이 파일이 바로 output 안에 있는 main.js**파일이다. 
(참고로 이 파일은 dist라는 폴더에 저장되어집니다. dist폴더는 webpack를 실행해야 생기는 파일입니다.)
- 현재 설정 상으로는 프로젝트 내부의 파일을 변경하게 되면, npx webpack으로 패키징을 해주고 실행해야 변경된 사항이 적용됨 → 번거로우니 자동화를 시작해봅시다.

### 5. 자동화하기
**webpack dev serve 설치**

```bash
npm install --save-dev webpack-dev-server
```

**webpack.config.js 에 devServer 관련 코드 추가**
```jsx
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: `${__dirname}/dist`,
  },

////// 추가 영역 //////
  devServer: {
    static: './dist', 
  }, ////// 추가 영역 //////
};
```

**HTML webpack Plugin  설치**

```bash
npm install --save-dev html-webpack-plugin
```

**webpack.config.js에 html webpack plugin 관련 코드 추가**

```jsx
const path = require('path');
////// 추가 영역 //////
const HtmlWebpackPlugin = require('html-webpack-plugin');
////// 추가 영역 //////

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: `${__dirname}/dist`,
    },

    devServer: {
        static: './dist', 
    }, 
  
////// 추가 영역 //////
    plugins: [new HtmlWebpackPlugin({
        template: './src/index.html'
    })],////// 추가 영역 //////
};
```

👉 src에 위치한 index.html 파일을 dist파일로 옮겨, 서버가 실행되면 index.html이 보이도록 설정하는 과정입니다.     
👉 만약 index.html이 아닌 다른 html 파일을 띄우길 원하신다면, 해당 파일 이름을 index.html 대신 적어주시면 됩니다.    

**최종 webpack.config.js 코드**

```jsx
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: `${__dirname}/dist`,
    },

    devServer: {
        static: './dist', 
    }, 
  
    plugins: [new HtmlWebpackPlugin({
        template: './src/index.html'
    })],
};
```

**package.json파일의 scripts 영역에 아래 코드 추가**

```jsx
"scripts": {
    "build": "wepback ",
    "start": "webpack serve --open",
    //.. 생략 ..
  },
```

### 6. 실행해보기

프로젝트 폴더 터미널에서 npm start 입력 → index.html 로딩 완료 ☞ 성공!

```
npm start 하고 오류로 삽질했는데 … 결국 Node 버전이 너무 낮아서 안되었던 것이였다!!
제일 최신 버전인 18.11.0 (npm use 18.11.0)으로 변경하니 성공했다!!!!🥳
```

---

**@Reference**
[webpack :: 디렉토리에 웹팩 설치 및 기본 설정](https://art-coding3.tistory.com/m/56)
