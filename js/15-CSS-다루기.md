> [목차](index.md)  
## 15. CSS 다루기
- *CSS 개요*
- *주요 CSS 프로퍼티*
- [CSS 제어하기](#CSS-제어하기)
- *인라인 스타일 스크립팅*
- *계산된 스타일 가져오기*
- *CSS 클래스 스크립팅*
- *스타일시트 스크립팅*

<br><br>
<br><br>





# CSS 제어하기
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <style>
    .notification-bar {
      padding: 10px;
      border: 1px solid black;
      background-color: yellow;
      position: absolute;
    }
  </style>
  <title>DOM 스타일 예제</title>  
</head>
<body>
<script>
  class NotificationBar {
    constructor() { // 생성자 함수
      this.barEl = document.createElement('div');
      this.barEl.style.display = "none";
      this.barEl.classList.add("notification-bar") // 요소의 CSS 클래스들은 `classList`로 접근합니다. `add()`를 통해 CSS 클래스들을 추가합니다.
      document.body.appendChild(this.barEl);
    }
    show(message, position = "top") {
      if (position === "top") {
        this.barEl.style.top = "10px";
        this.barEl.style.bottom = "";
      }
      if (position === "bottom") {
        this.barEl.style.top = "";
        this.barEl.style.bottom = "10px";
      }
      this.barEl.style.left = "10px";
      this.barEl.style.right = "10px";
      this.barEl.style.display = "";
      this.barEl.innerHTML = message;
    }
  }

  const noti = new NotificationBar(); // 알림바의 인스턴스를 생성
  setTimeout(() => {
    noti.show('welcome to JavaScript 200');  
  }, 1000); // 1초 후에 화면에 위쪽에 알림바를 띄웁니다.

  setTimeout(() => {
    noti.show('welcome to JavaScript 200', 'bottom');
  }, 2000); // 2초 후에 화면 아래쪽에 알림바를 띄웁니다.
</script>
</body>
</html>
```  
> 주석
```javascript
    show(message, position = "top") {
      if (position === "top") {
        this.barEl.style.top = "10px";
        this.barEl.style.bottom = "";
      }
      if (position === "bottom") {
        this.barEl.style.top = "";
        this.barEl.style.bottom = "10px";
      }
      this.barEl.style.left = "10px";
      this.barEl.style.right = "10px";
      this.barEl.style.display = "";
      this.barEl.innerHTML = message;
    }
  }
```  
- 알림바의 `show()`를 정의합니다. 매개변수로 보여줄 메시지와 위치를 정의합니다. 위치는
 기본적으로 문자열 `top`으로 합니다. CSS `top`과 `bottom`속성을 통해 알림바의 위치를 지정하는데
 빈 문자열을 값으로 할당하면 이미 적용된 값을 리셋할 수 있습니다. 마지막에 CSS `display`속성을
 리셋하여 화면에 알림바를 그립니다(생성자 함수에서 `display`가 `"none"`으로 되어있던 것을 리셋합니다).  
> :bulb: CSS 속성을 `style`속성의 키로 작성할 때 카멜케이스(camel-case) 형태로 작성해야 합니다. 예를 들면,
 css 속성인 `font-size`를 `style`속성의 키로 접근할 때에는 `fontSize`로 작성해야 합니다.  
<br>