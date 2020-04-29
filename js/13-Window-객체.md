> [목차](index.md)  
## 13. Window 객체
- [타이머](#타이머)
- *브라우저의 Location과 Navigation*
- [브라우징 히스토리](#브라우징-히스토리)
- *브라우저와 화면 정보*
- *대화상자*
- *오류 처리*
- *Window 프로퍼티의 문서 요소*
- [다중 창과 프레임](#다중-창과-프레임)

<br><br>
<br><br>





# 타이머
> ### 일정 시간 후에 코드 실행하기_setTimeout  
자바스크립트에서 `setTimeout`은 **글로벌 객체**에 내장된 메소드입니다. 브라우저에서는 `Window전역 객체`의 메소드로 정의되고,
 서버사이드 Node.js에서는 `GLOBAL전역 객체`로 정의되어 있습니다. 따라서 별도의 객체를 생성하거나 선언하지 않아도,
 `setTimeout()` 그대로 호출하여 실행할 수 있습니다.  
`setTimeout()`은 두개의 인자를 받습니다. 첫 번째 인자에는 일정 시간 후 실행될 함수를 정의합니다. 그리고 두 번째 인자에는
 지연 시간을 지정합니다. 지연 시간을 지정할 때는 ms(millisecond, 밀리세컨드)단위로 설정합니다. 이렇듯 `setTimeout()`은
 지연 시간(두 번째 인자)이 지난 후 함수 코드(첫 번째 인자)를 실행합니다.  
```javascript
const timer = {
	run: function() { 
		if (this.t) console.log('이미 실행된 타이머가 있습니다.');

		this.t = setTimeout(function() { // 추후 타이머 관리를 위해 작성한 `setTimmeout()`코드를 `this.t`에 대입합니다.
			console.log('1초 뒤에 실행됩니다.')
		}, 1000);
	},
	cancel: function() {
		if (this.t) clearTimeout(this.t); // 호이스팅되는거 아닌가?
		this.t = undefined;
	}
};


timer.run();
timer.cancel();
timer.run();
```
> 주석
```javascript
		if (this.t) clearTimeout(this.t);
```
- `this.t`의 값이 유효한 상태에서만 `clearTimeout()`을 실행합니다. `clearTimeout()`는 `setTimeout()`로 미리 정의한
 타이머 작업을 취소시킵니다. 따라서 `this.t`로 기할당된 실행 계획이 취소됩니다.  
<br><br>

- `setTimeout()` 자체는 비동기로 실행되는 코드입니다.
```javascript
setTimeout(() => {
	console.log('JavaScript'); 
}, 0); // 지연 시간이 0이여도 `console.log('200제')`코드 다음으로 실행 스택에 쌓입니다.

console.log('200제');
```  
<br>

> ### 일정 시간마다 코드 실행하기_setInterval  
`setInterval()`는 **글로벌 객체**에 내장된 메소드입니다. `setInterval()`는 인자로 callback함수와 지연 시간을 받습니다.
 이를 통해 지연 시간을 두고 일정한 간격으로 callback함수가 계속 실행됩니다. `setInterval()`를 실행하면 결과값으로 `id`값을 반환합니다.
 `id`를 `clearInterval()`인자에 넣으면, 해당 `id`의 타이머 작업을 취소할 수 있습니다.  
```javascript
let count = 0;

const timer = setInterval(() => {
	console.log(`${count++} 번째 함수가 실행됩니다.`);
}, 1000);

clearInterval(timer);
```  

<br><br>
<br><br>





# 브라우징 히스토리
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>브라우저 히스토리 이해하기 예제</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <ul class="user-list">
    <li data-name="jay">jay</li>
    <li data-name="bbo">bbo</li>
    <li data-name="harin">harin</li>
  </ul>
  <script>
    const userList = document.querySelector('.user-list'); // 사용자 목록은 `user-list` CSS 선택자로 선택합니다.

    userList.addEventListener('click', e => {
      const liEl = e.target;
      if (liEl.tagName === 'LI') {
        const name = liEl.dataset.name;
        select(userList, liEl);
        history.pushState(name, null, name);
      }
    })

    window.addEventListener('popstate', function (e) {
      const selectedUser = document
        .querySelector(`.user-list [data-name="${e.state}"]`);
      select(userList, selectedUser);
    });

    function select(ulEl, liEl) {
      Array.from(ulEl.children)
        .forEach(v => v.classList.remove('selected'));
      if (liEl) liEl.classList.add('selected');
    }
  </script>
</body>
</html>
```
> 주석
```javascript
        const name = liEl.dataset.name;
```
- 리스트 아이템 요소의 `dataset`속성을 통해 태그에 작성된 `data-name`어트리뷰트 값을 가져와 `name`상수로 정의합니다.  
```javascript
        select(userList, liEl);
```
- `select()`를 호출하여 클릭한 대상 `<li>`요소에 `selected`클래스를 추가하고 이전에 `selected`추가된 클래스를 삭제합니다.  
```javascript
        history.pushState(name, null, name);
```
- `history객체`의 `pushState()`를 이용하여 새로운 **히스토리**를 추가합니다. `pushState()`를 호출하면 새로운 히스토리가
 추가되고 전달된 인자에 의해서 URL이 변경됩니다. 다음은 `pushState`의 각 인자들과 그에 대한 설명입니다.  
- `history.pushState(state 객체, title 문자열, url 문자열)`
  - `state 객체` : 자바스크립트 객체로 현재 히스토리에 해당하는 상태를 `history.state`로 가져올 수 있습니다.
  - `title 문자열` : 브라우저 상단 타이틀을 변경합니다.
  - `url 문자열` : 새로운 히스토리 URL입니다.  
```javascript
    window.addEventListener('popstate', function (e) {
      const selectedUser = document
        .querySelector(`.user-list [data-name="${e.state}"]`);
      select(userList, selectedUser);
    });
```
- 브라우저 상단의 뒤로가기 또한 앞으로가기를 누를 때마다 브라우저 히스토리가 실행되고 `popstate이벤트`가 발생합니다.
 `history.back()`나 `history.go()`와 같은 자바스크립트 메소드에도 이벤트는 발생합니다. 하지만 `history.pushState()`에는 `popstate이벤트`가
 발생하지 않습니다.
  - `history.back()` : 브라우저 상단의 뒤로가기를 클릭한 것과 같이 이전 히스토리로 돌아갑니다. 앞으로 가는 메소드는 `history.forward()`이다.
  - `history.go(숫자)` : 히스토리의 특정 지점으로 이동합니다. 예를 들어, `history.go(-1)`은 이전 히스토리로 돌아가고 `history.go(1)`은 앞으로 이동합니다. 이전 두 단계 뒤로 갈 경우 `history.go(-2)`로 할 수 잇습니다.  
```javascript
    function select(ulEl, liEl) {
      Array.from(ulEl.children)
        .forEach(v => v.classList.remove('selected'));
      if (liEl) liEl.classList.add('selected');
    }
```
- 전체 목록 중 하나의 아이템 요소를 선택하는 `select()`를 정의합니다. 첫 번째 인자인 목록 요소의 모든 자식들을 순회하며
 `selected` CSS 클래스를 제거합니다. 그리고 두 번째 인자인 선택할 리스트 아이템 요소에 `selected` CSS 클래스를 추가합니다.
 사용자 목록을 클릭할 때와 `popstate이벤트`가 발생할 때 호출됩니다.  

<br><br>
<br><br>






# 다중 창과 프레임
## iframe 조작하기  
태그는 다른 HTML 문서를 현재 문서에 내장시킬 수 있습니다. 각 `<iframe>`에서 읽는 문서는 독립된
 `window객체`와 `document`를 가집니다. `<iframe>`태그는 `DOM`으로 표현하면 `HTMLIFrameElement`타입으로
 `contentWindow`와 `contentDocument`속성을 가집니다. 두 속성을 통해서 독립된 `window객체`와 `document객체`에 접근할 수 있습니다.  
```html
<!DOCTYPE html>
<html>
<head>
  <title>iframe inner document</title>
</head>
<body>
</body>
</html>
```
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>iframe 예제</title>
</head>
<body>
  <h1>iframe 바깥문서</h1>
  <iframe id="iframe1" src="./157-1.html" frameborder="0" 
          width="100%" height="500px"></iframe> // 내장할 외부 HTML문서 정의
  <script>
    const iframe1 = document.getElementById('iframe1');
    iframe1.addEventListener('load', e => { // 외부 HTML문서 불러오기
      const iframeDocument = iframe1.contentDocument;
      iframeDocument.body.style.backgroundColor = "blue";

      const newEl = document.createElement('div');
      newEl.innerHTML = '<h1>iframe 안쪽 문서';
      newEl.style.color = 'white';
      iframeDocument.body.appendChild(newEl);      

      setTimeout(() => {
        const iframeWindow = iframe1.contentWindow;
        iframeWindow.location = 'https://google-analytics.com';
      }, 3000);
    });
  </script>
</body>
</html>
```  
> 주석
```javascript
      const iframeDocument = iframe1.contentDocument;
      iframeDocument.body.style.backgroundColor = "blue";
```
- `contentDocument`속성을 통하여 내장된 문서의 독립된 `document객체`에 접근합니다. 그 후 내장된 문서의 `body`요소의
 백그라운드 색상을 파란색으로 변경합니다.  
```javascript
      const newEl = document.createElement('div');
      newEl.innerHTML = '<h1>iframe 안쪽 문서';
      newEl.style.color = 'white';
      iframeDocument.body.appendChild(newEl);      
```
- 내장된 문서의 독립된 `document`에 접근이 가능하기 때문에 현재 문서에서 생성된 요소를 내장 문서에 삽입할 수 있습니다.  
```javascript
      setTimeout(() => {
        const iframeWindow = iframe1.contentWindow;
        iframeWindow.location = 'https://google-analytics.com';
      }, 3000);
```
- 3초 후 내장된 문서를 도메인이 다른 문서로 변경합니다. 이때 만약 `location`을 `google.com`으로 변경하면 에러가 발생하는데,  
> :bulb: 서버에서 응답하는 HTTP 헤더가 `X-Frame-Option`이 **동일출처**(`'same-origin'`)으로 설정되어 있기 때문입니다.
 `X-Frame-Option`을 통해 다른 페이지에 내장될 수 있는지를 정의할 수 있습니다.  
- **동일 출처 정책**에 부합하지 않으면 오직 로케이션 변경만 가능하고 그 외에 `window객체` 또는 `document객체`에 접근하여 수정하는 행위 등은 할 수 없습니다.
  - 현재 사이트의 주소가 `http://js200.com`이라면 다음은 모두 동일 출처 정책에 부합하지 않습니다.
    - `https://js200.com`(다른 프로토콜)
    - `https://js200.com:8080`(다른 포트)
    - `https://js200.org`(다른 도메인)  
  
<br>

## iframe과 메시지 교환하기
```html
<!-- part4/158/158.html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>iframe 메시지 교환하기</title>
</head>
<body>
  <div>
    <label>결제금액 :</label> <b>20000원</b>
    <br>
    <button id="checkout-btn">카드입력</button>
  </div>
  <iframe id="card-payment" width="500px" height="200px" 
          frameborder="0"></iframe>
  <script>
    const iWindow = document.getElementById('card-payment').contentWindow;
    
    document.getElementById('checkout-btn')
      .addEventListener('click', e => {  
        iWindow.location = 'payment.html';
      })

    window.addEventListener('message', e => {
      console.log(e);
    })
  </script>
</body>
</html>
```
> 주석
```javascript
    const iWindow = document.getElementById('card-payment').contentWindow;
    
    document.getElementById('checkout-btn')
      .addEventListener('click', e => {  
        iWindow.location = 'payment.html';
      })
```
- 현재 문서의 `window객체`가 아닌 `<iframe>`요소의 독립된 `window객체`를 `iWindow`에 할당합니다. 그리고 카드입력 버튼을 클릭하면
 `<iframe>` 요소의 로케이션을 바꿔 `payment.html`문서를 불러옵니다.  
```javascript
    window.addEventListener('message', e => {
      console.log(e);
    })
```
- 현재 문서의 `window객체`에 `message이벤트`에 대해 리스너 함수를 등록합니다. `<iframe>`내의 문서에서 `postMaessage`로 메시지를
 전달하면 `message이벤트`가 발생하여 등록된 리스너 함수가 호출됩니다. 그리고 전달한 메시지는 `event`파라미터의 `data`속성을 통해 접근이
 가능합니다.  

<br><br>

```html
<!-- part4/158/payment.html -->
<script>
  function submitForm() { // 폼에 `submit`버튼을 클릭하면 호출될 함수를 정의합니다.
    const form = document.getElementById('card-form');
    const formData = new FormData(form);
    const formObj = {
      cardNumber: formData.get("cardNumber"),
      holderName: formData.get("holderName"),
    }
    window.parent.postMessage(formObj, '*');
  }
</script>
<form id="card-form" onsubmit="submitForm()">
  <div>
    <label>카드번호</label>
    <input type="text" name="cardNumber">
  </div>
  <div>
    <label>이름</label>
    <input type="text" name="holderName">
  </div>
  <button type="submit">결제하기</button>
</form>
```  
> 주석
```javascript
    const formData = new FormData(form);
```
- 주어진 폼 요소에 대한 `FormData`를 생성합니다. `FormData`는 `XMLHttpRequest`를 통해 서버에 데이터를 전달할 때
 사용할 수 있습니다. 그리고 `FormData`를 폼 요소로부터 생성하면 `<input>` 요소의 `name`어트리뷰트를 통해 `value`값을 가져올 수 있습니다.  
```javascript
    const formObj = {
      cardNumber: formData.get("cardNumber"),
      holderName: formData.get("holderName"),
    }
```
- 부모 윈도우에 메시지로 전달한 `formObj`를 정의합니다. `postMessage`에는 `FormData`형식의 객체가 전달되지 않기
 때문에 별도의 객체를 정의하여 전달해야 합니다.  
```javascript
    window.parent.postMessage(formObj, '*');
```
- `window`의 `parent객체`는 `<iframe>`태그가 작성된 부모 `window객체`를 가리키니다. 그리고 `postMessage`는 메시지를 보낼 대상
 `window객체`를 통해 호출해야 합니다. 그래서 부모 `window객체`의 `postMessage`를 호출합니다. `postMessage` 첫 번째 인자는
 전달할 메시지이고 두 번째 인자는 대상 `windo객체`의 출처를 작성합니다. 출처는 문자열 `*`혹은 URI를 작성해야 하는데 `*`는
 어떠한 출처도 가리지 않음을 의미합니다. 만약 `"https://google.com"`으로 작성하면 `https://google.com`에서 호스팅되는
 문서의 `window객체`로만 메시지를 전달할 수 있습니다.  
