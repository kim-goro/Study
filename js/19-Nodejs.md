> [목차](index.md)  
## 19. Node.js
- [Node.js](#node.js)
- [Node.js의 모듈](#node.js의-모듈)
- [Nodejs 예외 처리하기](#nodejs-예외-처리하기)
- [Event Emitter](#event-emitter)
- [폴더 생성하기](#폴더-생성하기)
- [파일 쓰기](#파일-쓰기)
- [파일 정보 탐색하기](#파일-정보-탐색하기)
- [파일 읽기](#파일-읽기)
- [특정 폴더 내 모든 파일 읽어오기](#특정-폴더-내-모든-파일-읽어오기)
- [http 서버 띄우기](#http-서버-띄우기)
- [웹 API 작성하기](#웹-api-작성하기)
- [API 호출하기](#api-호출하기)
- [cheerio로 크롤링하기](#cheerio로-크롤링하기)

<br><br>
<br><br>





# Node.js
```javascript
// Node.js 공식 홈페이지에 게시된 대표적인 코드
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log('Server running at http://${hostname}:${port}/');
});
```  
- 자바스크립트와 Node.js의 관계를 이해하려면, 서버(server)와 클라이언트(Client) 개념을 먼저 알아두어야 합니다.
  - **서버**(server) : 웹서비스의 HTTP 요청을 받아, 프로세스와 파일 시스템 등의 작업을 처리한 후 응답합니다.
  - **클라이언트**(client) : 서비스를 요청하는 역할을 수행합니다.  
- Node.js는 **서버**(Server)와 관련된 기술입니다. Node.js를 적용하면 서버-클라이언트 모두 자바스크립트언어로 개발한다는 큰 이점이 있다.
- Node.js는 `Chrome V8 JavaScript engine`으로 빌드된 자바스크립트 런타임입니다.  
여기서 `Chrome V8 JavaScript engine`(줄여서 V8 엔진)은 구글에서 만든 오픈소스로서, 크롬 브라우저와 Node.js내부에서 사용됩니다.
 V8 엔진은 자바스크립트로 작성된 코드를 컴퓨터가 이해할 수 있는 기계어로 변환해줍니다. `JavaScript -> C 또는 C++ -> 어셈블리어 -> 기계어`  

<br>

## node 명령어로 파일 실행하기
- `Node.js REPL`
- `node hello_node.js` : 파일 실행하기  

<br><br>
<br><br>






# node.js의 모듈
- 모듈(Module) : '코드의 모음' 또는 '코드의 블럭'입니다.  
자바스크립트 에는 모듈이란 개념이 분명하게 존재하지 않았습니다. 그렇게 자바스크립트 코드를 모듈화하기 위한 양대산맥, `CommonJs`와
 `AMD`가 등장했습니다. 이들은 자바스크립트 코드를 모듈로 만드는 합의된 일련의 표준명세입니다.  
Node.js는 `CommonJs`의 모듈 명세를 따라 모듈화를 지원하빈다. 즉, `CommonJs`와 같이 모듈을 선언할 때 `module.exports`를 사용하고,
 모듈을 로딩할 때에는 `require`을 사용합니다.
```javascript
//part4/164/commonJsTest.js
function moduleTest (x, y) {
  return x + y;
}

module.exports = moduleTest; // `module.exports`를 통해 `moduleTest`함수를 외부에서 접근가능한 모듈로 공개합니다.
```
```javascript
//part4/164/index.js
const moduleTest = require('./moduleTest');

console.log(moduleTest(3, 7));
```
```javascript
//part4/164/app.js
const greet = require('./greet.js'); // `reqire()`을 이용하여 `moduleTest()`파일에 공개된 모듈을 가져옵니다.

greet('JavaScript 200');
```
- `require()`는 Node.js에서 정의한 **내장 함수**입니다. 인수로는 문자형 값을 받습니다. 이 문자형 값은 사용하고 싶은 모듈의 이름 또는 파일 이름을 가리킵니다.  
  - `const greet = require('./greet.js');
    - `./`는 현재 작성된 파일이 있는 동일한 위치를 의미합니다.
    - Node.js가 내부적으로 `greet.js`파일을 실행하고, V8 엔진을 통해 개발자가 정의한 모듈 코드를 로딩해오는 과정을 거칩니다.
```javascript
//part4/164/greet.js
console.log('Hello');

const greet = function(name) {
  console.log('How are you? ' + name);
};

module.exports = greet;
```
- `module.exports`로 지정된 변수 또는 객체는 모듈로서 사용됩니다. 이렇게 정의된 모듈은 외부 다른 파일에서 `require()`를 통해 사용할 수 있습니다. <br>

> Nodejs 내장 모듈 사용하기  
Node.js 개발 시 유용하게 사용할 수 있는 모듈들이 라이브러리로 내장되어 있습니다. Node.js에서는 이를 두고
 **Node.js API**라고 명칭하는데, 다르게는 **코어 모듈** 또는 **내장 모듈**이라고도 부릅니다.  
Node.js 공식 홈페이지(http://nodejs.org/en/docs/)에 들어가면 화면에 **API documentation**이 있습니다.
 이를 클릭하여 Node.js API 공식 문서를 볼 수 있습니다.
-`const fs = require('fs');` : 내장 모듈을 선언
```javascript
const util = require('util');

const name = 'Tony';
const greeting = util.format('Hello %s', name); // `util.format()`를 활용하여 해당 문자열로 반환
console.log(greeting);
```  
`require()`를 사용해서 내장 모듈을 로드합니다. Node.js 코어에는 유틸리티 성격의 자바스크립트 코드로 구현된 lib폴더가 있습니다.
 Node.js는 `require()`에 `./`없이 문자열 인자를 넣어 호출하면, Node.js 코어 lib 폴더에서 그와 이름이 일치하는 모듈을 찾습니다.  

<br>

## Nodejs 예외 처리하기
- Node.js에서 에러를 처리하는 방법에는 두 가지가 있습니다.
  1. **비동기(async) 모듈** 또는 함수의 callback에서 첫 번째 매개변수로 에러 정보를 반환합니다. 이에 따라 비동기 모듈 또는 함수를 호출할 때에는 먼저 첫 번째 매개변수인 에러 정보를 우선적으로 확인해야 합니다. 에러 정보가 빈 값인 것을 확인한 이후 다음 작업을 수행하는 것을 권장합니다.  
  2. `try-catch`, `throw`입니다. 이는 자바스크립트 예외 처리와 동일하게 처리합니다. 다만 주의해야 할 점은 1번의 방법을 활용한 비동기 패턴에 `try-catch`, `throw`를 적용하는 것은 잘못된 방법입니다. 무조건 비동기 함수의 에러처리는 callback 함수를 활용해야합니다. 반대로 callback 함수로 처리하지 않는 그 외 패턴(동기-sync 패턴 등)에 대해서는 `try-catch`, `throw`을 적용하여 에러를 처리합니다.  
```javascript
"use strict";

const cbFunc = (err, result) => { // 비동기 함수에서 callback으로 사용할 함수 `cbFunc`을 정의합니다.
    if (err && err instanceof Error) return console.error(err.message);
    if (err) return console.error(err);

    console.log('에러를 반환하지 않습니다', result);
};

const asyncFunction = (isTrue, callback) => { `asyncFunction`이름의 비동기 함수를 직접 선언합니다.
    const err = new Error('This is error!!');

    if (isTrue) return callback(null, isTrue);
    else return callback(err);
};


asyncFunction(true, cbFunc);
asyncFunction(false, cbFunc);

const fs = require('fs');

try {
    const fileList = fs.readdirSync('/undefined/');
    fileList.forEach(f => console.log(f));
} catch (err) {
    if (err) console.error(err);
}
```
> 주석
```javascript
    const err = new Error('This is error!!');
```
- 자바스크립트 `Error객체`처럼 사용할 수 있는 `Node.js Core API Error모듈`이 있습니다. `new Error`를 통해 객체를 생성하여
 변수 `err`에 대입합니다. 여기서 `Error객체` 생성 시 대입된 `"This is error!!"`문자열은 `Error객체`의 `message`속성으로 값이 할당됩니다.  
```javascript
const fs = require('fs');
```
- 이번에는 비동기가 아닌 **내장 모듈**을 사용하여 예외처리, 파일 시스템과 관련된 `fs 내장 모듈`을 가져옵니다.
```javascript
try {
    const fileList = fs.readdirSync('/undefined/');
    fileList.forEach(f => console.log(f));
```
- `fs.reddirSync`는 파일 시스템(`fs`)에서 동기 패턴으로 실행되는 함수 입니다. 따라서 별도의 callback함수 없이 순서대로 실행됩니다.
 결과값이 바로 `fileList`변수에 대입되고, 그 다음 줄에서 파일 정보들을 콘솔 출력합니다.  
```javascript
} catch (err) {
    if (err) console.error(err);
}
```
- `try{}` 블록 안에서 에러가 발생하면 `catch`로 에러 정보가 전달됩니다. 예제에서 `/undefined/`경로는 누가 일부러 만들어
 두지 않았다면, 해당 경로가 없다는 에러를 반환할 것입니다. 에러가 발생한 경우 한 번 더 에러 정보 여부를 확인하고, `consol.error`로
 에러 정보가 출력됩니다.  

<br><br>
<br><br>







# Event Emitter 
프로그래밍에서 "어떤 사건의 발생"이라는 측면에서 **이벤트**라고 부르며, 발생된 이벤트에 대한 응답하는 것을 **리스너**(Listener)라고 합니다.
 `Event Emitter`는 바로 이 이벤트-리스너 패턴으로 구현됩니다.
```javascript
//part4/167/Emitter.js
class Emitter {
    constructor() {
        this.events = {};
    }

    on(type, listener) {
        this.events[type] = this.events[type] || [];
        this.events[type].push(listener);
    }

    emit (type) {
        if (this.events[type]) {
            this.events[type].forEach((listener) => {
                listener();
            });
        }
    }
}

module.exports = Emitter;
```
```javascript
//part4/167/app.js
const Emitter = require('./emitter');
const em = new Emitter();

em.on('greet', () => {
    console.log('Hello First');
});

em.on('greet', () => {
    console.log('Hello Second');
});

em.emit('greet');
```
> 주석
```javascript
//part4/167/Emitter.js
    on(type, listener) {
        this.events[type] = this.events[type] || [];
        this.events[type].push(listener);
    }
```
- 메소드 `on`은 매개변수로 `type`과 `listener`를 전달받습니다. `events`에 키(key)로 `type`을 지정하고, 해당 키 값에
 `listener`를 추가합니다. 이는 어떤 종류의 이벤트인 경우, 해당 이벤트의 `listener`들을 모아 놓는 형태로 보면 됩니다.  
```javascript
//part4/167/Emitter.js
    emit (type) {
        if (this.events[type]) {
            this.events[type].forEach((listener) => {
                listener();
            });
        }
    }
```
- 메소드 `emit`은 매개변수로 `type`을 받습니다. 따라서 `events`에 `type`으로 지정된 값이 있는지 확인하고, 값이 유효한 경우
 해당 이벤트의 `listener`들은 `forEach`로 순차적으로 돌아가면서 실행합니다.  

<br>

## Node.js에는 두 가지 종류의 이벤트가 있습니다.
  - `System Events - libuv` : 컴퓨터에서 시스템적으로 발생되는 이벤트가 있습니다. 예를 들어, 파일 읽기, 파일 열기, 인터넷에서 데이터 받기 등입니다. 컴퓨터 시스템적으로 수행되는 이벤트들을 Node.js에서는 `libuv`라는 이름의 라이브러리가 구현되어 이를 처리합니다. 이 `libuv`는 C++로 작성된 Node.js **코어 라이브러리**입니다. 시스템 상의 이벤트이기 때문에 자바스크립트가 아닌 C++코드로 작성되어 있습니다.
  - `Custom Events` : 시스템 상 이벤트를 떠나, 직접 구현하여 만들 수 있는 이벤트가 있습니다. 직접 작성된 이벤트를 다루는 것이므로 자바스크립트로 작성된 라이브러리가 수행됩니다. 즉, Node.js의 `Event Emitter`와 관련된 내장 모듈이 바로 이 이벤트들을 처리해 줍니다.  
```javascript
//Node.js의 내장 모듈 `events`를 사용해서, 앞에서 만들었던 `Event Emitter`와 동일하게 구현
//part4/168/eventEmitter.js
const Emitter = require('events');
const eventConfig = require('./config').events; // `module.exports`의 `events`속성을 가져옴

const em = new Emitter();

em.on(eventConfig.GREET, () => {
    console.log('Somewhere, someone said heelo.');
});

em.on(eventConfig.GREET, () => {
    console.log('A Greeting occurred!');
});

em.emit(eventConfig.GREET);
```
```javascript
//part4/168/config.js에 별도 관리
module.exports = {
    events: {
        GREET: 'greet'
    }
};
```  

<br><br>
<br><br>






# 폴더 생성하기  
Node.js에서 `fs 모듈`은 **파일 입출력**과 관련된 **파이 시스템 모듈**로, **서버의 정적파일**을 다루는데
 가장 많이 사용되는 모듈입니다.
```javascript
//파일 또는 폴더를 생성/삭제하고, 파일 정보와 데이터를 가져오는 방법
"use strict";

const fs = require('fs');


const checkDir = (path, callback) => { `checkDir(경로,함수)`은 명시한 경로의 파일 또는 폴더의 정보(상태)를 확인하는 함수
  fs.stat(path, (err, stats) => {
    if (err && err.code === 'ENOENT') return callback(null, true);
    if (err) return callback(err);

    return callback(null, !stats.isDirectory());
  });
};

const currentPath = __dirname; // `__dirname` : GLOBAL 변수
let path = `${currentPath}/js200`; // 현재 경로에서 "/js200"를 붙여 경로를 정의합니다.

checkDir(path, (err, isTrue) => { // 이미 동일한 이름의 폴더가 존재하거나, 그 외 다른 에러가 발생하면 false를 반환합니다.
  if (err) return console.log(err);

  if (!isTrue) {
    console.log('이미 동일한 디렉토리가 있습니다. 디렉토리명을 변경합니다.');
    path = `${currentPath}/js200-new`;
  }

  fs.mkdir(path, (err) => {
    if (err) console.log(err);

    console.log(`${path} 경로로 디렉토리를 생성했습니다.`);
  });
});
```
- `__dirname` : node 명령어로 실행된 파일의 현재 경로 정보를 반환합니다. 단, 경로에는 파일 이름은 포함하지 않습니다.
 파일이 위치한 폴더까지 **절대경로**(Absolute Path)로 경로값을 반환합니다. 여기서 절대경로란 root, 즉`/`로 시작하는 경로를 의미합니다.  
> 주석
```javascript
  fs.stat(path, (err, stats) => {
```
- `fs.stat`은 대입된 `path`경로값의 파일 존재 여부를 확인할 수 있습니다. 확인된 결과값은 `fs.State클래스`로 래핑되어 콜백 함수로
 전달됩니다. 이 클래스는 해당 경로의 파일이 `isFile()`, `isDirectory()`, `isFIFO()` 등 간단한 함수로 파일 정보를 제공합니다.  
```javascript
    if (err && err.code === 'ENOENT') return callback(null, true);
```
- 해당 경로에 어떤 파일도 존재하지 않는 경우, `fs.state 모듈`은 상태정보가 없어 에러를 반환합니다. 이 에러는 `Error 객체`로
 `code`정보가 `'ENOENT'`로 정의되어 전달됩니다. 그러나 `checkDir()`에서는 이 상태가 새로운 폴더를 만들기에 적합하기 때문에
 true를 전달하는 것이 맞습니다.  
```javascript
    return callback(null, !stats.isDirectory());
```
- 에러없이 파일 정보를 가져온 뒤 `stats.isDirectory()`를 확인합니다. `checkDir()`의 의도는 현재 경로에서 정상적으로 새로운 폴더를
 생성 가능한지 확인하는 함수 입니다. `isDirectory()`로 true가 반환되면 이미 동일한 폴더가 있다는 뜻이기 때문에, `checkDir()`는
 false를 리턴해야합니다.
```javascript
checkDir(path, (err, isTrue) => {
```
- `checkDir()`를 실행합니다. 폴더를 정상적으로 생성할 수 있는 경로임이 확인되면 true를, 이미 동일한 이름의 폴더가 존재하거나
 그 외 다른 에러가 발생하면 false를 반환합니다.  
```javascript
  fs.mkdir(path, (err) => {
    if (err) console.log(err);

    console.log(`${path} 경로로 디렉토리를 생성했습니다.`);
  });
```
- 폴더를 생성하는 예제로 간단하게 `fs.stat 모듈`을 사용했습니다. 그러나 `fs.open()`, `fs.readFile()`, `fs.wrtieFile()`와 같이
 파일을 직접 접근할 때에는 `fs.stat 모듈`사용을 권장하지 않습니다. 파일을 직접 읽고 쓰기 시에는 반드시 접근 권한을 확인해야
 하므로 이 경우에는 `fs.access 모듈`을 권장합니다.  

<br><br>
<br><br>






# 파일 쓰기  
`fs 모듈`을 활용할 때 함께 활용되는 모듈이 바로 `path`입니다. `path 모듈`은 파일/폴더 경로와 관련된 모듈입니다.
 앞에서 작성했던 것처럼 구분자(Delimiter)를 넣는다면 의도치않은 값이 대입되거나 누락되어 잘못된 경로가 반환될 수 있습니다.
 이처럼 개발자의 실수를 방지하고, 경로에서 확장자명 또는 파일명만 추출하는 등의 좀더 나은 편의성을 위해 `path 모듈`을 사용합니다.  
```javascript
"use strict";

const fs = require('fs');
const path = require('path');

const makeFile = (path, callback) => {
    fs.writeFile(path, 'New file, New content', 'utf8', (err) => {
        if (err) return callback(err);

        console.log('파일이 생성됐습니다.');
        callback(null);
    });
};

const appendFile = (path, callback) => {
    fs.appendFile(path, '\nUpdate file', (err) => {
        if (err) return callback(err);

        console.log('파일 내용을 추가합니다.');
        callback(null);
    })
};

const printErrIfExist = (err) => {
    if (err) console.log(err);
};


const filePath = path.join(__dirname, 'js200', 'hello.txt');

fs.open(filePath, 'wx', (err, fd) => {
    if (err && err.code === 'EEXIST') 
        return appendFile(filePath, (err) => printErrIfExist(err));
    if (err) return callback(err);

    return makeFile(filePath, (err) => printErrIfExist(err)); // 새로운 파일을 생성
});
```
> 주석
```javascript
const makeFile = (path, callback) => {
    fs.writeFile(path, 'New file, New content', 'utf8', (err) => {
        if (err) return callback(err);

        console.log('파일이 생성됐습니다.');
        callback(null);
    });
};
```
- 함수 `makeFile()`은 파일을 새로 생성하는 `fs.writeFile()`을 호출합니다. 첫 번째 인자에 `path`경로 값을 넣고, 두 번째 인자에는
 파일 내용 `New file, New content`문자열을, 세 번째 인자에는 파일 인코딩 정보 `'utf8'`을 넣습니다. 인자 값을 토대로 새로운 파일을
 생성하는 `fs.writeFile()`를 실행합니다. 실행 결과는 마지막 인자 콜백 함수로 전달됩니다.  
```javascript
const appendFile = (path, callback) => {
    fs.appendFile(path, '\nUpdate file', (err) => {
        if (err) return callback(err);

        console.log('파일 내용을 추가합니다.');
        callback(null);
    })
};

```
- 함수 `appendFile()`은 기존 파일에 내용을 추가하는 함수입니다. `fs.appendFile()`를 사용하여, 매개변수로 전달된 `path`경로에
 `'\nUpdate file'`문자열을 추가합니다. `fs.appendFile`로 내용을 추가한 후, 콜백 함수가 실행됩니다. 에러가 발생되면 콜백함수
 `if (err) return callback(err);`을 통해 에러를 반환합니다. 에러가 없으면 다음 코드가 실행됩니다.  
```javascript
const filePath = path.join(__dirname, 'js200', 'hello.txt');
```
- 파일 경로를 생성합니다. 현재 파일이 있는 폴더 경로 `__dirname`과, 파일이 위치할 폴더명, 그리고 파일명을 `path.join()`인자로 넣습니다.
 `path 모듈`의 `join()`는 대입된 매개변수들을 일관된 구분자(delimiter)를 두고 순서대로 하나의 문자열로 합ㅊ비니다. 따라서 `filePath`변수에
 `/...현재 실행 위치.../js200/hello.txt` 문자열이 반환됩니다.
```javascript
fs.open(filePath, 'wx', (err, fd) => {
```
- `fs.open()`를 호출합니다. 이 함수는 특정 경로의 파일 또는 폴더의 존재 연부를 확인하기 위해 사용하는데,
 `flag`값을 넣음으로써 파일 접근 권한을 동시에 확인할 수 있습니다. 여기서 대입된 `flag`는 `wx`입니다. 쓰기(write)접근 권한을 확인하며,
 이미 해당 경로로 동일한 파일이 있는 경우 에러를 반환합니다.  
```javascript
    if (err && err.code === 'EEXIST') 
        return appendFile(filePath, (err) => printErrIfExist(err));
```
- 동일한 파일이 있을 때 `"EEXITS"`코드의 `Error 객체`를 반환합니다. 파일이 이미 있는 경우에는 `appendFile()`를 호출하여 내용만 추가합니다.  

<br><br>
<br><br>






# 파일 정보 탐색하기
```javascript
//특정 폴더 내 파일들을 탐색하거나, 특정 파일의 세부 정보를 확인해야 하는 경우
"use strict";

const fs = require('fs');
const path = require('path');


const fileName = 'hello.txt';
const targetPath = path.join(__dirname, 'js200');

const filePath= path.join(targetPath, fileName);
console.log(path.parse(filePath)); // `path.parse()`를 통해 특정 경로의 파일 정보를 확인할 수 있습니다.


const isFileInPath = (path, fileName, callback) => {
  fs.readdir(path, (err, files) => { // `fs.readdir()`는 특정 경로 안에 있는 모든 파일명을 콜백 함수의 매개변수로 전달합니다.
    if (err) return callback(err);

    let isHere = false;
    files.forEach(f => {
      if (f === fileName) isHere = true;
    });

    return callback(null, isHere);
  });
};

isFileInPath(path.join(__dirname, 'js200'), fileName, (err, isTrue) => {
  if (err || !isTrue) return console.log('파일을 읽을 수 없습니다');
  
  // 탐색 결과 해당 파일이 있는 경우, 에러 예외 처리를 통과하여 `fs.stat()`함수를 실행합니다.
  fs.stat(filePath, (err, fileInfo) => { // `fs.stat()`를 통해 파일의 상세 정보를 확인할 수 있습니다.
    if (err) return console.log(err);

    return console.log(fileInfo);
  });
});
```  

<br><br>
<br><br>





# 파일 읽기
```javascript
//`fs 내장모듈`의 비동기/동기 함수 예제
"use strict";

const fs = require('fs');
const path = require('path');


const filePath = path.join(__dirname, 'js200', 'hello.txt');

fs.open(filePath, 'r', (err, fd) => {
  if (err && err.code === 'ENOENT') return console.log('읽을 수 없는 파일입니다');
  if (err) return console.log(err);

  console.log('파일을 정상적으로 읽을 수 있습니다');

  fs.readFile(filePath, 'utf-8', (err, data) => { // 비동기
    if (err) return console.log(err);

    console.log(data);
  });

  try {
    const data = fs.readFileSync(filePath, 'utf-8'); // 동기
    console.log(data);
  } catch (err) {
    console.log(err);
  }
});
```
> 주석
```javascript
fs.open(filePath, 'r', (err, fd) => {
  if (err && err.code === 'ENOENT') return console.log('읽을 수 없는 파일입니다');
  if (err) return console.log(err);

  console.log('파일을 정상적으로 읽을 수 있습니다');
```
- 파일 입출력에서는 동일한 파일 존재 여부와 접근 권한을 확인해야 합니다. 이번에는 `r flag`를 활용하여, 특정 파일을 읽는 것이 가능한지
 확인해야합니다. 만일 해당 파일이 존재하지 않으면 에러를 반환합니다.  
- 에러로 반환된 `Error 객체`의 `"ENOENT"`코드를 찾는 파일이 없다는 것을 의미합니니다. 조건식 성립함과 동시에
 콘솔 출력 후 작업을 종료합니다. 그 외의 에러 또한 `if (err) return console.log(err);`을 통해 콘솔 출력과 함께 바로 종료됩니다.
 에러가 없는 정상 파일의 경우 `console.log('파일을 정상적으로 읽을 수 있습니다');`이 실행됩니다.  
```javascript
  fs.readFile(filePath, 'utf-8', (err, data) => {
    if (err) return console.log(err);

    console.log(data);
  });
```
- 파일 읽기 가능 여부와 파일 존재 여부를 확인한 후 `fs.readFile()`를 통해 실제로 파일을 읽어옵니다. 첫 번째 인자에 읽을 대상 파일 경로를 넣고,
 두 번째에는 인코딩 정보를 넣습니다. 일반적으로 한글 깨짐 오류를 겪지 않기 위해, `"utf-8"`을 사용하여 인코딩하는 것이 좋습니다.
  인코딩을 설정하지 않는 경우에는 파일 데이터는 `Buffer`로 반환됩니다. `fs.readFile()`는 그동안 학습해온 것처럼 콜백 함수의 매개변수로
  에러와 결과값을 전달합니다.  
```javascript
  try {
    const data = fs.readFileSync(filePath, 'utf-8');
    console.log(data);
```
- 그와 다르게 `fs.readFileSync`는 동기 패턴 함수입니다. Node.js 내장 모듈 중 비동기/동기 패턴을 둘다 구현해 놓은 모듈에 한해,
 동기와 비동기 패턴 함수를 쉽게 구별하는 방법이 있습니다. 함수 이름에서 `"sync"`가 붙은 함수가 동기 **패턴 함수**, 붙어있지 않은 함수가
 **비동기 패턴** 함수 입니다. 동기 패턴 함수는 콜백 함수 없이, 결과값을 반환합니다.  
```javascript
  } catch (err) {
    console.log(err);
  }
```
- 여기서 `try-catch`구문이 없다면, 동기 패턴 함수의 예외 처리는 무방비 상태가 됩니다. 비동기에서 에러를 반환하고 예외 처리했던 것고 달리,
 동기 패턴은 짧은 문장으로 값을 반환하지만 에러에 대한 값이 반환되는 것이 아닙니다. 만일 `try-catch`없이 동기 패턴에서 에러가 발생하면,
 작업은 그 즉시 중지되고 `idle`상태에 빠져 작동되지 않습니다.  
<br>

> ### 파일 삭제하기  
`fs 모듈`에서는 파일을 삭제하는 것과 폴더를 삭제하는 방법이 다릅니다. `fs.unlink`는 파일 또는 심볼릭 링크를 삭제합니다. 그리고 파일을 삭제하기 전에는 반시 `fs.access`를 통해 파일에 접근할 수 있는지 확인해야 합니다.  
```javascript
'use strict';

const fs = require('fs');
const path = require('path');

const filePath = path.join(__dirname, 'js200', 'hello.txt');

fs.access(filePath, fs.constants.F_OK, (err) => {
  if (err) return console.log('삭제할 수 없는 파일입니다'); // 해당 파일이 `mode`상수값을 충족하지 않은 경우 콜백 함수에 에러를 전달합니다.

  // 만일 에러가 없으면 해당 파일은 mode 상수값 조건을 충족한다는 의미입니다.
  fs.unlink(filePath, (err) => err ?  // `fs.unlink`를 통해 `filePath`경로 파일을 삭제합니다.
    console.log(err) : console.log(`${filePath} 를 정상적으로 삭제했습니다`));
});
```
> 주석
```javascript
fs.access(filePath, fs.constants.F_OK, (err) => {
```
- `fs.access`을 호출하여 `filePath`경로에 대한 접근 가능 여부를 확인합니다. 여기서 두 번째 인자 `fs.constants.F_OF`는 접근과 관련된
 `mode` 정보입니다. `fs.constants`는 파일 시스템과 관련된 상수들을 그룹으로 모아놓은 상수인데, 그 안에서 `F-OK`는 파일 존재 여부를 확인할
 수 있는 상수 입니다.  
> :bulb: `fs.contants`(**파일 시스템 모듈의 상수**)는 파일 시스템과 관련된 상수들을 모아놓은 큰 범위의 그룹 상수입니다.
 크게 세종류로 구분할 수 있는데, **파일 접근**(access), **파일 상태**(stats), **파일 열기**(open)으로 나뉘고, 각각
 `fs.access()`, `fs.stats()`, `fs.open()`와 관련되어 있습니다. 이들은 주로 파일 처리를 위해 접근 권한을 확인하는 용도로
 사용합니다. 예를 들어, read-only접근을 위해 파일을 여는 권한으로 `O_RDONLY`, 파일이 다른 프로세스 호출에 의해 실행 가능한지
 접근 권한을 확인하는 `X_OK`, 파일 쓰기에서 소유권 또는 그룹서유권을 확인하는 `S_IWUSER`과 `S_IXGRP`등이 있습니다.  

<br><br>
<br><br>






# 특정 폴더 내 모든 파일 읽어오기
```javascript
'use strict';

const fs = require('fs');
const path = require('path');

const removePath = (p, callback) => {
  fs.stat(p, (err, stats) => {
    if (err) return callback(err);

    if (!stats.isDirectory()) { // 만일 경로가 필인 경우, `fs.unlink()`를 사용하여 해당 파일을 삭제합니다.
      console.log('이 경로는 파일입니다');
      return fs.unlink(p, err => err ? callback(err) : callback(null, p));
    }

    console.log('이 경로는 폴더입니다');
    fs.rmdir(p, (err) => { // 만일 경로가 폴더인 경우, `fs.rmdir()`를 사용하여 해당 폴더를 삭제합니다.
      if (err) return callback(err);

      return callback(null, p);
    });
  });
};

const printResult = (err, result) => {
  if (err) return console.log(err);

  console.log(`${result} 를 정상적으로 삭제했습니다`);
};


const p = path.join(__dirname, 'js200');

try {
  const files = fs.readdirSync(p); // `fs.readdir()`의 동기 패턴 함수 `fs.readdirSync()`를 호출합니다. 특정 폴더 안의 파일들을 배열로 반환합니다.
  if (files.length) // `length`가 0인 아닌 경우 `forEach`로 각 파일명 값에 접근하여 `removePath()`를 통해 파일들을 모두 삭제합니다.
    files.forEach(f => removePath(path.join(p, f), printResult));
} catch (err) {
  if (err) return console.log(err);
}

removePath(p, printResult); // 폴더 안 파일들을 모두 삭제한 후, `removePath()`를 호출하여 폴더를 삭제합니다.
```  

<br><br>
<br><br>

# http 서버 띄우기  
`Node.js http 내장 모듈`은 HTTP 서버와 클라이언트를 구성하는 함수를 제공합니다. 보통 웹서버를 구성할 때는 프레임워크를 사용하여
 간단하고 빠르게 웹 어플리케이션을 만들 수 있습니다. 그러나 `Node.js 코어 모듈`만 활용하여 HTTP 요청/응답 통신을 구현  
```javascript
//HTTP 요청/응답을 위해서 HTTP 서버를 만듬
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello! Node.js HTTP Server');
});

server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```
> 주석
```javascript
const server = http.createServer((req, res) => {
```
- `http.createServer()`는 웹 서버 객체를 생성합니다. 반환된 서버 객체를 통해 HTTP 요청을 받으면 `request객체`와 `response객체`가 전달되며,
 지정된 작업을 수행합니다.  
```javascript
    res.statusCode = 200;
```
- `response`와 `statusCode`속성에 요청에 대한 응답 상태 코드를 지정합니다. `statusCode`란 `http`에서 HTTP 상태 코드를 의미하는데,
 여기서 200 상태 코드는 요청을 정상적으로 처리했다는 의미입니다.  
```javascript
    res.setHeader('Content-Type', 'text/plain');
```
- `response객체`의 `setHeader()`에 헤더 정보를 지정합니다. 이 헤더 정보를 통해 서버가 클라이언트로 응답하는
 자원 종류를 지정합니다. `Content-Type`는 클라이언트로 전달되는 자원의 타입(종류)를 말합니다. 그리고 여기서
 지정된 타입은 `'text/plain'`, 텍스트 파일입니다.  
```javascript
    res.end('Hello! Node.js HTTP Server');
```
- `response객체`의 `end()`는 지정된 모든 응답 헤더와 본문이 전송되었음을 서버에 알립니다.
 해당 서버는 이 메시지를 기준으로 응답 완료로 간주합니다. 따라서 `response.end()`는 반드시 각 응답에서 호출되어야 합니다.  
```javascript
server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```
- 지정한 호스트, 포트 정보로 HTTP 서버를 실행하면 연결을 수신받습니다. 
 HTTP 서버와 클라이언트 간 통신은 메시지를 통해 주고 받습니다. 이를 **HTTP 메시지**라고 부르는데,
 메시지는 `request(요청)`와 `response(응답)` 두 가지 종류로 나뉘며 비슷한 구조를 지닙니다. 메시지를 통해
 주고받는 정보는 총 세가지 입니다. `요청 또는 응답에 대한 상태 정보(ex, url, status Code)`, `HTTP 헤더(header)`, `HTTP 메시지본문(body)`
 정보가 메시지에 담깁니다.

<br><br>
<br><br>






# 웹 API 작성하기  
**웹 API**(Application Programming Interface)는 여러 다른 어플리케이션들이 연결되어 동일한 데이터를 주고받을 수 있는 인터페이스입니다.
 기본적으로 API는 요청을 받고, 원하는 것을 응답해주는 역할을 수행합니다. 다시 말해, API를 통해 요청/응답을 주고받을 수 있습니다.
 그리고 이 웹 API를 통해 서버로 주문서 정보들이 전달되어 주문과 관련된 비즈니스 로직이 수행됩니다.  
```javascript
"use strict";

const http = require('http');
const url = require('url');

const hostname = '127.0.0.1'; // 생성할 서버의 호스트명과 포트 번호를 변수로 정의합니다.
const port = 3000;

const server = http.createServer((req, res) => {
  switch (req.method) {
    case 'GET':
      if (req.url === '/') {
        res.setHeader('Content-Type', 'text/plain');
        res.writeHead(200);
        res.end('Hello! Node.js HTTP Server');
      } else if (req.url.substring(0, 5) === '/data') {
        const queryParams = url.parse(req.url, true).query;

        res.setHeader('Content-Type', 'text/html');
        res.writeHead(200);
        res.write('<html><head><title>JavaScript 200제</title></head>');

        for (let key in queryParams) {
          res.write(`<h1>${key}</h1>`);
          res.write(`<h2>${queryParams[key]}</h2>`);
        }

        res.end('</body></html>');
      }
      break;
    default:
      res.end();
  }

});

server.listen(port, hostname, () => { // 정의한 호스트, 포트 정보로 HTTP 서버를 실행하여 연결을 수신받습니다.
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
- 실행 후 `http://localhost:3000/data?qs1=1&qs2=2`로 결과를 확인할 수 있습니다.  
> 주석
```javascript
const url = require('url');
```
- `url 모듈`은 UTRL 문자열 또는 객체형 값을 유용하게 다룰 수 있도록 도와주는 유틸리티 성격의 모듈입니다.
 URL 모듈을 활용하면 문자열 `url`도 파싱하여 원하는 구조 형태나 부분을 쉽게 가져올 수 있습니다.  
```javascript
const server = http.createServer((req, res) => {
```
- `http.createServer()`로 서버를 생성합니다. 서버를 생성하고 나면 `net.server 클래스`의 인스턴스를 반환하는데,
 이 클래스 인스턴스는 TCP 또는 IPC 서버 생성에 상요되며, 추가적인 요청 이벤트에 응답합니다. 여기서는 TCP 서버를 생성하고
 요청 이벤트에 응답하여 `response`, `request`를 콜백함수로 전달합니다.  
```javascript
  switch (req.method) {
    case 'GET':
```
- `switch`조건문을 수행하여 HTTP 메소드로 GET, POST, PUT, DELETE와 같은 API에 대한 동작을 설명합니다.
```javascript
      if (req.url === '/') {
```
-   `request`의 `url`은 `api`로 요청된 `url`의 `path`정보입니다. `http`의 `url`구조는 다음과 같이 구성되어 있는데,
 여기서 `"/"`를 포함한 `path`부분이 반환됩니다. 따라서 `url`의 `path`가 `/`인 경우에 다음 코드가 실행됩니다.  
- `http://host[:port][/][path][?query]`  
```javascript
        res.setHeader('Content-Type', 'text/plain');
        res.writeHead(200);
        res.end('Hello! Node.js HTTP Server');
```
- `response 객체`를 활용하여 웹 API의 응답 정보를 정의합니다. `setHeader()`, `writeHead()`, `end()`에 정의된
 응답 정보는 `"Http 서버 띄우기"`에서 수행한 응답 정보와 동일합니다.  
```javascript
        const queryParams = url.parse(req.url, true).query;
```
- `url.parse()`는 `url`문자열을 넣으면 `url 객체`를 반환해줍니다. `url 객체`를 살펴보면 다음과 같습니다. `url.parse()`에
 두 번째 인자는 필수는 아니지만, true르 넣으면 `Url 객체`의 query를 JSON형태로 받을 수 있습니다.  
```javascript
        res.setHeader('Content-Type', 'text/html');
        res.writeHead(200);
```
- `Content-Type`와 HTTP 상태 코드를 설정합니다. `res.setHeader('Content-Type', 'text/html');`에서 `Content-Type`으로 정의한
 `'text/html'`은 MIME 콘텐츠 형식 또는 인터넷 미디어(html) 형식의 텍스트를 의미합니다.  
```javascript
        res.write('<html><head><title>JavaScript 200제</title></head>');

        for (let key in queryParams) {
          res.write(`<h1>${key}</h1>`);
          res.write(`<h2>${queryParams[key]}</h2>`);
        }

        res.end('</body></html>');
```
- `res.write`는 응답 메시지 html 코드를 응답 본문으로 보냅니다. 이 메소드는 여러 번 호출하여 본문을 연속으로 이어서 제공할 수 있습니다.
 처음 `res.write()`가 호출되면 버퍼링 된 헤더 정보와 본문의 첫 번째 chunk(덩어리)가 클라이언트로 전송됩니다. 두 번째 `res.write()`가
 호출되면서 Node.js는 데이터가 스트리밍되는 것으로 가정하고 새 데이터를 추가로 보냅니다. 따라서 예제처럼 `head` 태그 문자열을 넣은 이후에,
 `body`태그 정보를 추가로 연달아 같이 전송하는 것이 마지막으로 `res.end()`에 데이터를 넣어 응답 전송을 완료합니다.  

<br><br>

```javascript
//HTTP 메소드 POST를 활용하여 웹 API를 작성
"use strict";

const http = require('http');
const qs = require('querystring');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  switch (req.method) {
    case 'POST':
      let body = '';

      req.on('data', (chunk) => {
        body += chunk;
      });
      req.on('end', () => {
        const obj = qs.parse(body);
        res.writeHead(200);
        res.end(JSON.stringify(obj));
      });
      req.on('error', (err) => {
        console.error(err.stack);
      });
      break;
    default:
      res.end();
  }

});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
> 주석
```javascript
const qs = require('querystring');
```
- Node.js의 `querrystring 내장 모듈`은 URL 쿼리(query) 문자열을 분석하고 다른 형식을 지정 변환하는 함수들을 제공합니다.
 일반적으로 JavaScript에서 간편하게 활용할 수 있는 JSON형식으로 변환합니다.  
```javascript
      req.on('data', (chunk) => {
        body += chunk;
      });
```
- Node.js의 HTTP 모듈로 POST API 요청할 때에는 이벤트 리스너를 통해 바디(body) 데이터를 주고받습니다. 이 이벤트 리스너의
 흐름은 앞에서 살펴봤던 `EventEmitter`처럼 리스너를 등록하고 이벤트가 생성될 때마다 실행됩니다. 여기서는 `'data'`와 `'end'`로
 이벤트를 등록합니다. 그리고 각 `'data'`이벤트는 `Buffer`형식의 데이터로 전달되는데, 위 예제처럼 문자열과 덧셈 연산을 하면
 `Buffer` 데이터가 문자형으로 자료형 변환되어 문자열 데이터를 받을 수 있습니다.
```javascript
      req.on('end', () => {
        const obj = qs.parse(body);
        res.writeHead(200);
        res.end(JSON.stringify(obj));
      });
```
- `end` 이벤트 발생은 요청 전송이 완료되는 시점의 이벤트입니다. `'data'`이벤트로 수집된 문자열 `body`변수를 `qs.parse()`를 통해
 객체 형식으로 파싱합니다. 정상 전달 완료된 경우, 응답 상태 코드 200을 반환합니다. 마지막으로 응답 데이터와 함께 `res.end()`를 호출하면서
 응답 전송을 완료합니다.  
> :bulb: 웹 브라우저에 주소를 호출하는 방법은 GET 메소드 API만 가능합니다. POST 메소드로 작성된 API를 확인하려면
 별도로 앱을 다운받아 확인할 수 있습니다. `https://www.getpostmans.com/`  

<br><br>
<br><br>





# API 호출하기  
지금까지는 HTTP 서버를 생성하고 API를 작성한 뒤 웹 브라우저 또는 Postman 프로그램으로 API를 호출했습니다. 이번에는 Node.js의
 `HTTP 내장 모듈`을 사용해서 코드로 직접 API를 호출하는 방법을 살펴보겠습니다.  
```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello! Node.js HTTP Server');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

http.get('http://localhost:3000', (res) => {
  let data = '';
  res.on('data', function(chunk) {
    data += chunk;
    console.log('data of res.on =====> ', data);
  });
  res.on('end', function() {
    try {
      console.log('end of res.on =====> ', data);
      return data;
    } catch (err) {
      if (err) console.log(err);
    }
  });
});
```
> 주석
```javascript
http.get('http://localhost:3000', (res) => {
```
- `http.get()`은 GET 메소드의 url을 요청하는 함수입니다. 보통 `http.request()`를 이용하여 요청을 하는 것이 일반적이지만,
 `http 모듈`의 `get()`을 이용하여 `body` 데이터 전송 없이 간단하게 GET메소드 API를 요청할 수 있습니다.  
```javascript
  res.on('data', function(chunk) {
    data += chunk;
    console.log('data of res.on =====> ', data);
  });
  res.on('end', function() {
    try {
      console.log('end of res.on =====> ', data);
      return data;
    } catch (err) {
      if (err) console.log(err);
    }
  });
```
- API를 작성하여 요청을 받았던 예제들과 달리, 이번 예제는 API를 요청합니다. 따라서 콜백 함수에서 오직 `res`, 즉 `reponse 객체`를 활용합니다.
 실제로 요청하는 주제가 이 예제 코드이므로, 콜백 함수로 전달되는 `response 객체` 활용ㅇ ㅣ중요합니다. 다만 `'data'`와 `'end'`이벤트 리스너를
 통해 데이터를 전달받는 방식은 `request 객체`와 `response 객체`모두 동일합니다.  

<br>

## 외부 패키지 설치하기  
지금까지 Node.js의 내장 모듈을 사용했다면, 이번에는 Node.js의 공식 외부 패키지 모듈을 사용하는 방법을 알아보겠습니다.
 Node.js의 공식 **외부 패키지 모듈**를 활용하기 위해, **npm 패키지 매니저**(Package Manager)를 사용해야 합니다.
 `npm`은 기본적으로 자바스크립트를 위한 패키지 매니저임과 동시에, Node.js의 패키지 매니저입니다. 즉, 자바스크립트를 활용하는
 거의 모든 프로젝트에서 사용되는 패키지 매니저입니다.  
**패키지**란 재사용 가능한 코드를 하나의 다발로 묶은 것을 의미합니다. 그리고 패키지 매니저는 수많은 "다발=패키지"들이
 온라인으로 많은 사람들과 공유되도록 관리합니다. 또한 개발자가 패키지를 간편하게 개발할 수 있도록 관리하는 역할도 합니다. 다시 말해,
 **패키지 매니저**란 프로그래밍에서 사용하게 되는 라이브러리와 패키지들을 관리하며, 대표적인 패키지 매니저로 자바의 `Maven`, 파이썬의 `pip`, 자바스크립트의 `npm`이 있습니다.
- `npm`을 활용하여 자신만의 패키지를 생성/관리/배포 등을 할 수 있습니다.
- `npm`은 Node.js를 설치할 때 자동으로 같이 설치됩니다.
  - `npm install npm@6.4.1 -g` : 특정 버전으로 설치
  - `npm -v` : 설치된 범위 확인
  - `npm init` : 패키지에 필요한 기본 파일들을 자동으로 생성합니다.
  - `npm install <패키지명> --save`
    - `--save`는 `package.json`파일의 `dependencies`속성에 설치 패키지명과 버전정보를 기록할 수 있습니다.
    - 미리 `package.json`파일이 작성된 경우에는 `npm install`만 실행하면 `dependencies`를 읽어 지정된 패키지들을 자동으로 설치합니다.  

<br>

## request로 간편하게 api 요청하기  
앞에서 Node.js의 `HTTP 모듈`을 활용하여 API를 직접 호출했습니다. 그때 학습했던 예제처럼
 다양한 메소드와 기능들을 활용할 순 있지만, 다소 복잡하여 코드가 직관적이지 않습니다. 그래서 이를 보완하고 간편하게
 개발할 수 있도록, API 요청하는 기능을 가진 `request 외부 패키지`를 활용
- `npm install request@2.88.0`  
앞에서처럼 GET 메소드 `url`을 호출합니다. 더 나아가 쿼리 스트링(querrystring) 값을 추가로 전송하고,
 응답 결과는 JSON으로 받을 수 있도록 설정합니다. 
```javascript
//part4/180/request.js
const request = require('request');

const url = 'http://uinames.com/api';
const json = true;
const qs = { region: 'korea', amount: 3 };

request.get({url, json, qs}, (err, res, result) => {
  if (err) return console.log('err', err);
  if (res && res.statusCode >= 400) return console.log(res.statusCode);

  result.forEach(person => {
    console.log(`${person.name}${person.surname} 님의 성별은 ${person.gender}입니다.`);
  });
});

```
> 주석
```javascript
const url = 'http://uinames.com/api';
const json = true;
const qs = { region: 'korea', amount: 3 };
```
- `api`를 요청할 정보를 정의합니다. `"http://uninames.com/api" API url`은 테스트를 위해 얼마든지 사용 가능합니다.
 응답 데이터를 JSON으로 반환받기 위해, JSON변수에 true를 대입합니다. 또한 `qs`변수 값으로 `region`과 `amount`속성이
 있는 객체를 대입합니다.  
```javascript
  if (err) return console.log('err', err);
  if (res && res.statusCode >= 400) return console.log(res.statusCode);
```
- 에러 예외처리에 대한 조건문입니다. `if (err)`에 대한 부분은 기존 callback에러 처리와 동일합니다.
 추가로 `request 패키지`는 HTTP 요청에 대한 응답을 처리할 때 반드시 HTTP Status Code(HTTP 상태 코드)입니다.
 400 이상의 상태 코드를 정상 범주에 속한 코드가 아님으로, 400 이상의 범주에 해당하는 경우 에러 발생 상황으로 간주하여 처리합니다.  
```javascript
  result.forEach(person => {
    console.log(`${person.name}${person.surname} 님의 성별은 ${person.gender}입니다.`);
  });
```
- HTTP 상태 코드가 정상인 경우 수행되는 로직입니다. 응답 데이터가 JSON으로 반환되기 때문에, 간편하게 결과값에
 대한 처리가 용이합니다. 요청한 `qs`변수값에 따라 `korea` 지역에 맞는 `ui`이름이 3개 생성됩니다. 따라서 응답 결과
 `result`는 배열에 3개의 요소가 들어가 있습니다. `forEach`로 순환하면서 각 객체 요소의 `name`, `surname`, `gender` 속성값을 출력합니다.  

<br><br>
<br><br>






# cheerio로 크롤링하기  
웹 사이트 정볼르 탐색하는 방법으로 **크롤링**(Crawing) 또는 **스크래핑**(Scraping)이 있습니다.
 "개발자 도구"를 보면, 페이지의 모든 요소들을 `DOM 트리`로 확인할 수 있습니다. 크롤링 또한 스크래핑은 가감없이 이러한
 페이지 요소를 가져오고 탐색합니다. `cheerio 패키지`가 바로 크롤링 또는 스크래핑을 위한 모듈입니다.  
`cheerio`는 웹사이트 **마크업 구문**을 분석하고, 구조 탐색 또는 조작하기 위한 함수를 제공합니다.
 또한 `jQuery 라이브러리`에서 구현하는 **하위선택자**(Selector) 방식을 적용하여, 필요없는 부분을 제외하고 원하는 정보만 가져올 수 있습니다.  
- `npm install cheerio@1.0.0-rc.2`
```javascript
//part4/181/cheerio.js
const cheerio = require('cheerio');
const request = require('request');
const fs = require('fs');

fs.readFile('./example.html', (err, data) => {
  if (err) return console.log(err);

  const $ = cheerio.load(data);

  console.log($('#body', '#html').find('li').length);
  console.log($('.son', '#people').text());
});

request('https://ko.wikipedia.org/wiki/HTML', (err, res, html) => {
  if (err) return console.log(err);
  if (res && res.statusCode >= 400) return console.log(res.statusCode);

  const $ = cheerio.load(html);
  console.log($('div[class=toc]').children().find('a').text());
});
```
```html
<!-- part4/181/example.html -->
<ul id="people">
    <li class="son">손예준</li>
    <li class="maeng">맹주원</li>
    <li class="oh">오재호</li>
</ul>
```
> 주석
```javascript
//part4/181/cheerio.js
fs.readFile('./example.html', (err, data) => {
```
- `fs.readFile()`를 통해 실행 파일과 동일한 경로의 `example.html`파일 정보를 읽어옵니다.
 `fs.readFile()`실행 결과는 콜백 함수 `err`와 `data`매개변수로 전달됩니다. 에러 발생이 없는 한 `example.html`파일의 HTML 정볼르 가져옵니다. 
```javascript
//part4/181/cheerio.js
  const $ = cheerio.load(data);
```
- `cheerio`로 정보를 탐색하기 위해 먼저 html 문서를 로드(load)해야 합니다. `fs.readFile()`로 가져온 HTML 정보를 `cherrio.load()`에 넣고,
 함수에서 반환된 결과값을 `$ 변수`는 jQuery의 `$ 함수`와 같습니다. `load()`를 통해 HTML 문서를 조작 가능한 `DOM`으로 파싱(Parsing)합니다.  
```javascript
//part4/181/cheerio.js
  console.log($('#body', '#html').find('li').length);
```
- `$`를 통해 원하는 선택자(Selector)를 골라냅니다. `html`태그 안의 `body`요솔르 찾고, `find()`를 통해 해당 `node`의 `li`태그
 개수를 확인합니다. 그러나 `examplehtml`에 해당하는 요소와 노드가 없으므로 `length`결과값을 0을 콘솔 출력합니다.
- `$(Selector, [context], [root])` : 원하는 선택자를 찾는 메소드입니다. 이때 `context`와 `root`는 필수값은 아닙니다.
 값을 지정하면 설정한 해당 `context스코프` 또는 `root스코프`에서 선택자를 찾아냅니다.
  - `.find()` : `selector`를 필터링하여, 일치하는 요소의 현재 세트에서 자손(Subset)을 가져옵니다.
  - `.text()` : 일치하는 선택자 요소 집합의 텍스트 내용을 가져옵니다. 이는 선택된 요소뿐만 아니라 해당 요소 집합 내 자손을 포함합니다.  
```javascript
//part4/181/cheerio.js
  console.log($('.son', '#people').text());
```
- `people`요소 스코프 안에서 클래스명이 `son`인 선택자를 가져옵니다. `text()`를 통해 선태자로 가져온 요소 집합에서 텍스트 내용을 반환합니다.  
```javascript
//part4/181/cheerio.js
request('https://ko.wikipedia.org/wiki/HTML', (err, res, html) => {
```
- `request 모듈`을 활용하여 `http://ko.wikipedia.org/wiki/HTML` 사이트의 HTML 문서를 읽어옵니다.
```javascript
//part4/181/cheerio.js
  console.log($('div[class=toc]').children().find('a').text());
```
- `div`태그에 클래스명이 `toc`인 요소를 찾습니다. 그리고 해당 요소 선택자의 자식 노드들 중에서 태그인
 모든 요소를 찾습니다. `a`태그 각 요소에 있는 텍스트 정보를 한꺼번에 콘솔 출력합니다.  