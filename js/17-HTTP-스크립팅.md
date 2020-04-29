> [목차](index.md)  
## 17. HTTP 스크립팅
- [HTML 폼 활용하기](#HTML-폼-활용하기)
- [스크롤 처리하기](#스크롤-처리하기)
- *[script] 요소를 활용한 HTTP : JSONP*
- *Server-Sent 이벤트를 활용한 Comet*

<br><br>
<br><br>






# HTML 폼 활용하기
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">    
  <title>HTML 폼 활용하기 예제</title>
</head>
<body>
  <form name="order"> <!-- 폼 태그와 입력 요소는 `name`속성을 기반으로 자바스크립트 상에서 해당 요소에 접근할 수 있습니다. -->
    <fieldset name="userInfo">
      <legend>주문자 정보</legend>
      이름: <input name="name" type="text">
      전화번호: <input name="tel" type="tel">      
    </fieldset>
    <fieldset name="productInfo">
      <legend>상품 정보</legend>      
      상품명: <input name="productName" type="text">
      색상: 
      <select name="color">
        <option value="black">검은색</option>
        <option value="yellow">노란색</option>
      </select>
    </fieldset>
    <button id="btn1" type="button">button 처리</button>
    <button type="submit">submit 제출</button>
  </form>
  <script>    
    const orderForm = document.forms.order,
          userField = orderForm.elements.userInfo,
          productField = orderForm.elements.productInfo;
    document.getElementById('btn1')
      .addEventListener('click', e => {
        const { name, tel } = userField.elements;
        console.log(`${name.value} 사용자(${tel.value})로 주문합니다.`);
      });
    orderForm.addEventListener('submit', e => {
      e.preventDefault();
      const { productName, color } = productField.elements;
      console.log(
        `${productName.value} 상품 ${color.value}색을 주문합니다.`
      );

      orderForm.method = 'GET';
      orderForm.submit();
    });
  </script>
</body>
</html>
```
> 주석
```javascript
    const orderForm = document.forms.order,
          userField = orderForm.elements.userInfo,
          productField = orderForm.elements.productInfo;
```
- `<form>`요소는 `name`어트리뷰트 값을 key로하여 `document.fomrs객체`를 통해 가져올 수 있습니다. 그리고 `<form>`요소의
 자식 요소들은 `name`어트리뷰트 값을 key로하여 `elements`속성을 통해 가져올 수 있습니다. `<fieldset>`요소 또한 `name`
 어트리뷰트 값으로 가져옵니다.  
```javascript
    document.getElementById('btn1')
      .addEventListener('click', e => {
        const { name, tel } = userField.elements;
        console.log(`${name.value} 사용자(${tel.value})로 주문합니다.`);
      });
```
- `id`가 `btn1`인 요소를 클릭하면 주문자 정보 `<fieldset>`요소의 자식 `<Input>`요소들의 값을 콘솔에 출력합니다.
 `<filedset>`요소 또한 `<form>`요소와 마찬가지로 자식 `<input>`요소를 `name`어트리뷰트 값을 키로하여 `elements`속성을 통해 가져옵니다.  
```javascript
    orderForm.addEventListener('submit', e => {
      e.preventDefault();
      const { productName, color } = productField.elements;
      console.log(
        `${productName.value} 상품 ${color.value}색을 주문합니다.`
      );

      orderForm.method = 'GET';
      orderForm.submit();
    });
```
- 폼 요소는 `submit`버튼을 클릭하면 `submit`이벤트를 발생합니다. 이때 해당 폼의 `<input>` 정보를 이용해 주어진 URL에 HTTP 요청을 보냅니다.
 기본적으로 HTTP GET요청을 내고 서버의 응답에 따라 화면을 갱신합니다.  
> :bulb: 이러한 기본 로직을 막고 싶으면 이벤트 객체의 `preventDefault()`를 호출하면 됩니다. 그리고 폼 요소의 `method`속성을
 변경하고 `submit()`를 호출하면 변경된 메소드로 HTTP 요청을 보낼 수 있습니다.  
- HTTP GET 요청을 보내게 되면 `<input>`요소들의 `name`과 `value`어트리뷰트 들을 조합하여 HTTP 질의 문자열을 생성합니다.
 예제에서의 질의 문자열은 `"name=값&tel=값&productName=값&color=값"`입니다.  

<br><br>
<br><br>






# 스크롤 처리하기
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>DOM 네비게이션 예제</title>
  <link rel="stylesheet" href="./css/scroll.css">
</head>
<body>
  <section class="hero">
    <h1>스크롤을 아래로 내려보세요.</h1>
  </section>
  <nav>
    <a href="https://javascript-200.com">자바스크립트 200제</a>
  </nav>
  <section class="articles">
  </section>
  <script>
    const nav = document.querySelector('nav');
    const navTopOffset = nav.offsetTop;
    window.addEventListener('scroll', e => { // 스크롤을 할 때마다 호출되는 리스너 함수를 등록합니다.
      if (window.pageYOffset >= navTopOffset) {
        nav.style.position = 'fixed';
        nav.style.top = 0;
        nav.style.left = 0;
        nav.style.right = 0;
      } else {
        nav.style.position = '';
        nav.style.top = '';
      }
    });
  </script>
</body>
</html>
```
> 주석
```javascript
    const navTopOffset = nav.offsetTop;
```
- `offsetTop`속성으 통해 부모로부터 얼마나 멀리 떨어져 있는지를 픽셀 단위 숫자값으로 가져옵니다. `offsetLeft`는 브라우저
 좌측으로부터 얼마나 떨어져 있는지를 알 수 있습니다.  
```javascript
      if (window.pageYOffset >= navTopOffset) {
        nav.style.position = 'fixed';
        nav.style.top = 0;
        nav.style.left = 0;
        nav.style.right = 0;
      } else {
        nav.style.position = '';
        nav.style.top = '';
      }
```
- `window객체`의 `pageYoffset`속성을 이용하면 현재 스크롤된 화면이 브라우저 상단으로부터 얼마나 멀리 떨어져 있는지를 알 수 있습니다.
 마찬가지로 `pageXOffset`는 브라우저 원쪽으로부터 얼마나 떨어져 있는지를 알 수 있습니다. 스크롤 시 브라우저 상단으로부터의 차이가
 네비게이션 요소의 브라우저 상단과의 차이보다 커직나 같을 경우 내비게이션을 상단에 고정시킵니다. 그 외에는 원래 상태로 변경해 줍니다.
- `pageYOffset` : 브라우저 자체가 스크롤한 길이
- `offsetTop` : 브라우저 시작부분과 해당 태그까지의 길이  