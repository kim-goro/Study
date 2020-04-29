> [목차](index.md)  
## 22. HTML5 API
- [XMLHttpRequest 비동기 통신 처리하기](#xmlhttprequest-비동기-통신-처리하기)
- [Fetch API를 활용한 비동기 통신 처리하기](#fetch-api를-활용한-비동기-통신-처리하기)
- *Geolocation*
- *히스토리 관리*
- *교차 출처 간 메시징*
- [웹 워커](#웹 워커)
- *타입 배열과 배열 버퍼*
- *Blob 832*
- *파일 시스템 API*
- *클라이언트 측 데이터베이스*
- *웹 소켓*

<br><br>
<br><br>







# XMLHttpRequest로 비동기 통신 처리하기  
`XMLHttpRequest객체`를 사용하면 백그라운드로 서버와의 통신을 할 수 있습니다. `XMLHttpRequest객체`는 `XML`을 위한
 `HttpRequest`가 아니라 어떠한 형태의 데이터도 서버로부터 받거나 보낼 수 있습니다. 대표적인 예로 JSON메시지 포멧이 있습니다.  
`XMLHttpRequest`는 비동기로 처리하는데 이 말은 서버로 요청을 보내고 받는 동안 이후의 자바스크립트 코드는 막히지 않고
 계속 실행되고 클릭이나 상요자의 입력을 계속 처리할 수 있습니다.  
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>XMLHttpRequest 예제</title>
</head>
<body>
<div id="user"></div>
<script>
function httpGet(url, successCallback, errorCallback) {
  const req = new XMLHttpRequest(); // `XMLHttpRequest`은 생성자 함수라서 `new`키워드를 통해 인스턴스를 생성합니다.
  req.onload = () => {
    if (req.status >= 200 && req.status < 300) {
      successCallback(req.response);
    } else { 
      errorCallback(new Error(req.statusText));
    }
  }
  req.onerror = errorCallback; // 에러 발생 시 호출될 콜백 함수를 매개변수의 실패 콜백 함수로 정의합니다.
  req.open('GET', url);
  req.setRequestHeader('Accept', 'application/json');
  req.send();
}

const userEl = document.getElementById('user');

httpGet('https://api.github.com/users/jeado', 
  data => {
    const user =  JSON.parse(data);
    userEl.innerHTML = 
      `<img src="${user.avatar_url}" /> 
       <br> 사용자이름 : ${user.login}, 깃헙주소: ${user.html_url}`
  }, error => alert(error));
</script>  
</body>
</html>
```  
> 주석
```javascript
  req.onload = () => {
    if (req.status >= 200 && req.status < 300) {
      successCallback(req.response);
    } else { 
      errorCallback(new Error(req.statusText));
    }
  }
```
- HTTP 요청이 완료되면 호출될 콜백 함수를 정의합니다. 요청이 완료되면 `onload`에 연결된 함수를 호출하는데
 이때 `req객체`의 `status`는 HTTP 상태 코드가 됩니다. 200 이상이고 300 미만이면 성공으로 간주하고 매개변수의
 성공 콜백 함수(successCallback)에 응답을 전달하며 호출합니다. 그 외의 상태 코드는 실패로 간주하고 상태 텍스트를
 에러 메시지로 하여 에러와 함께 실패 콜백 함수를 호출합니다.  
```javascript
  req.open('GET', url);
```
- HTTP 요청을 초기화합니다. 이때 HTTP 메소드를(GET,POST,PUT.DELETE) 첫 번째 인자로 전달하고 URL을 두 번째 인자로 전달합니다.
 부가적으로 3번째 인자로 비동기 여부를 불린값으로 전달할 수 있습니다. 기본은 비동기로 전송합니다.  
```javascript
  req.setRequestHeader('Accept', 'application/json');
```
- HTTP 요청의 헤더를 정의합니다. `Accept헤더`를 `application/json`으로 정의했습니다. `Accept헤더`는 요청하는
 클라이언트가 받을 수 있는 데이터 타입을 정의합니다.  
```javascript
  req.send();
```
- POST 요청과 같이 HTTP 몸통(body)을 같이 보내야할 때 문자열을 인자로 전달할 수 있습니다.
```javascript
httpGet('https://api.github.com/users/jeado', 
  data => {
    const user =  JSON.parse(data);
    userEl.innerHTML = 
      `<img src="${user.avatar_url}" /> 
       <br> 사용자이름 : ${user.login}, 깃헙주소: ${user.html_url}`
  }, error => alert(error));
```
- `httpGet()`를 호출합니다. Github의 `/jeado` 사용자 정보를 가져오기 위해 Github에 HTTP 요청을 보냅니다.
 요청이 성공하면 JSON메시지를 받고 해당 메시지를 객체화하여 만들어진 객체를 `user`에 할당합니다. 그리고 `user객체`의
 속성을 이용하여 사용자 정보를 `userEl`내부에 추가합니다.  


<br><br>
<br><br>






# Fetch API를 활용한 비동기 통신 처리하기
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Fetch API 예제</title>
</head>
<body>
<div id="user"></div>
<script>
const userEl = document.getElementById('user');
const reqPromise = 
  fetch('https://api.github.com/users/jeado', { 
    headers: { Accept: 'application/json' },
    method: 'GET'
  });
reqPromise
  .then(res => {
    if (res.status >= 200 && res.status < 300) {
      return res.json();
    } else {
      return Promise.reject(new Error(`Got status ${res.status}`));
    }
  })
  .then(user => {
    userEl.innerHTML =
      `<img src="${user.avatar_url}" /> 
       <br> 사용자이름 : ${user.login}, 깃헙주소: ${user.html_url}`
  })
  .catch(error => alert(error));
</script>
</body>
</html>
```  
> 주석
```javascript
const reqPromise = 
  fetch('https://api.github.com/users/jeado', { 
    headers: { Accept: 'application/json' },
    method: 'GET'
  });
```
- Github의 `jeado` 사용자 정보를 가져오기 위해 `fetch API`를 사용하여 HTTP 요청을 보냅니다. `fetch API`는
 첫 번째 인자로는 요청할 URL을 작성하고 다음으로 옵션 객체를 전달합니다. 옵션 객체에는 헤더와 HTTP 메소드 등을
 정의할 수 있습니다. 여기선 `Accept헤더`를 `applcation/json`으로 하는 GET 메소드 요청을 하도록 옵션을 정의했습니다.  
```javascript
reqPromise
  .then(res => {
    if (res.status >= 200 && res.status < 300) {
      return res.json();
    } else {
      return Promise.reject(new Error(`Got status ${res.status}`));
    }
  })
```
- 요청에 대한 응답이 왔을 때 호출되는 콜백 함수를 `then()`으로 등록합니다. 콜백 함수는 응답 객체를 매개변수로
 전달 받는데 응답 객체를 이용하여 응답 상태나 내용을 확인할 수 있습니다. 응답 객체의 `status`속성은 상태 코드를 반환합니다.
 그리고 JSON 메소드는 응답 본문을 JSON으로 파싱하여 처리된 결과를 `Promise`로 반환합니다. 여기선 상태 코드가 200 이상이고
 300 미만이 아닐 경우 에러와 함께 `Promise`를 거절 처리하여 반환합니다. (다음 `then`으로 이어지지 않고 바로 `catch`로 전달되어
 에러 처리가 됩니다)
```javascript
  .then(user => {
    userEl.innerHTML =
      `<img src="${user.avatar_url}" /> 
       <br> 사용자이름 : ${user.login}, 깃헙주소: ${user.html_url}`
  })
```
- 앞의 `Promise`에서 반환한 응답 본문이 객체로 파싱되어 콜백 함수에 전달됩니다. `user` 객체의 속성을 이용하여
 사용자 정보를 `userEl`내부에 추가합니다.  

<br><br>
<br><br>






# 웹 워커
**웹 워커**는 무거운 작업의 스크립트를 백그라운드에서 동작할 수 있게 합니다. 작업을 수행하는
 최소한의 단위를 스레드라고 하는데 사용자의 입력이나 화면의 렌더링 등을 다루는 메인 스레드를 방해하지 않고 별도의
 스레드에서 스크립트를 실행하게 하는 것이 웹 워커 입니다.  
```html
<!-- 피보나치 수열 예제 -->
<!-- part4/161/161.html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>웹워커 예제</title>
</head>
<body>
  <div>
    <input type="number" id="number">
    <button id="start-btn">피보나치수열 계산시작</button>      
  </div>
  <div id="result"></div>
  <script>
    const result = document.getElementById('result');
    let isCalculation = false;
    if (window.Worker) {
      const fibonacciWorker = new Worker('fibonacci.js'); <!-- 이때 실행할 자바스크립트 파일의 경로를 인자로 전달합니다. -->
      document.getElementById('start-btn')
        .addEventListener('click', e => {
          if (isCalculation) {
            return;
          }
          const value = document.getElementById('number').value;
          fibonacciWorker.postMessage({ num: value });
          result.innerHTML = '계산중...';
          isCalculation = true;
        });
      fibonacciWorker.onmessage = function(e) {
        result.innerHTML= e.data;
        isCalculation = false;
      };
      fibonacciWorker.onerror = function(error) { // 워커 스크립트에서 에러가 발생하면 `onerror`콜백을 통해 에러를 잡을 수 있습니다.
        console.error('에러 발생', error.message);
        result.innerHTML= error.message;
        isCalculation = false;
      };
    }
  </script>
</body>
</html>
```  
> 주석
```javascript
      document.getElementById('start-btn')
        .addEventListener('click', e => {
          if (isCalculation) {
            return;
          }
          const value = document.getElementById('number').value;
          fibonacciWorker.postMessage({ num: value });
          result.innerHTML = '계산중...';
          isCalculation = true;
        });
```
- 시작 버튼을 클릭하면 `id`가 `number`인 `<input>`요소에 입력한 숫자값을 피보나치 워커에 `postMessage`를 이용하여 전달합니다.
 메인 스크립트와 워커 스크립트 간의 메시지 전달은 이벤트 방식으로 동작해서 한쪽에서 `postMessage()`로 메시지를 전달하면 상대편의
 `onmessage`에 등록된 함수를 통해 전달된 메시지를 받을 수 있습니다.  
```javascript
      fibonacciWorker.onmessage = function(e) {
        result.innerHTML= e.data;
        isCalculation = false;
      };
```
- 워커 스크립트에서 `postMessage`로 데이터를 전달하면 워커 인스턴스의 `onmessage`속성에 등록한 콜백 함수가 호출됩니다.
 콜백 함수는 이벤트가 인자로 전달되는데 이벤트의 `data`를 통해 워커 스크립트에서 잔달한 메시지를 받을 수 있습니다.
 여기서는 피보나치 수열의 계산된 결과가 `data`에 담겨있습니다.  
<br><br>
```html
<!-- 앱 워커에 의해 별도의 스레드에서 실행되는 스크립트 입니다 -->
<!-- part4/161/fibonacci.js -->
function fibonacci(num) {
  if (num <= 1) {
    return 1;
  }
  return fibonacci(num - 1) + fibonacci(num - 2);
}

onmessage = function(e) { // `onmessage`에 함수를 등록합니다. 메인 스크립트에서 `postMessage`로 메시지를 전달하면 등록된 콜백 함수가 실행됩니다.
  const num = e.data.num;
  console.log('메인 스크립트에서 전달 받은 메시지', e.data);
  if (num == null || num === "")  {
    throw new Error('숫자를 전달하지 않았습니다.');
  }
  const result = fibonacci(num);
  postMessage(result);
}
```  