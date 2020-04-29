> [목차](index.md)
### Javascript

<br><br>
<br><br>
# Javascript
- HTML을 제어하는 언어다.
- 자바스크립트 코드에 따라 태그의 style속성을 변경한다.
```html
<script>
  document.write("hello,world!");
</script>
```  
<br>

> ### event
- `onclick` : 어떤 이벤트가 일어났을 때 어떠한 자바스크립트 코드를 실행하는 것
- `onc` : onchange 내용이 변했을 때를 체크하는 이벤트
```html
<body>
  <input type="button" value="hi" onclick="alert('hi')">
  <input type="text" onchange="alert('changed')">
  <input type="text" onkeydown="alert('key down!')">
</body>
```  
<br>

> ### consol
- 개발자도구 > consol
- 웹consol은 현재 웹 페이지 안에 삽입돼 있는 자바스크립트인 것처럼 동작합니다.
```javascript
var 당첨자수 = 4;
var 댓글선택자 = '._3b-9>div>.UFIComment .UFICommentActorName;
Function shuffle(a){
  for(let i = a.length; i; i--){
    let j = Math.floor(Math.random() * i);
      [a[i-1], a[j]] = [a[j], a[i-1]];
    console.log(i + `,` + j);
  }
}
var list=[];
document.querySelectorAll(댓글선택자).forEach(function(e){
  list.push(e.innerText);
});

list = list.filter((v,i,a) => a.indexOf(v)==i);
shuffle(list)
console.log(list.slice(0,당첨자수));
```  
<br>

> ### 데이터 타입_문자열과 숫자
- Primitives
  - Boolean
  - Null
  - Undefined
  - Number
  - String
  - Symbol(new in ECMAScript 2015)
- Object
  - String
  
```javascript
"Hello World".toUpperCase(); // String의 Property
"Hello World".indexOf(`O`);
" Hello World ".trim();
```  
<br>

> ### 배열과 반복문
```javascript
var alist = document.querySelectorAll('a');
var i = 0;
while(i < alist.length){
  alist[i].style.color = 'powerblue';
  console.log(alist[i]);
  i = i + 1;
}
```     
```html
<script>
  
</script>

```
<br>

> ### 태그 제어하기
- https://www.w3schools.com/js/js_htmldom_css.asp
```html
<input id="night_day" type="button" value="night" onClick="
  if(document.querySelector('#night_day').value == `night`){
    document.querySelector('body').style.backgroundColor=`black`;
    document.querySelector('body').style.color=`white`;
    document.querySelector('#night_day').value=`day`;
  }else{
    document.querySelector('body').style.backgroundColor=`White`;
    document.querySelector('body').style.color=`black`;
    document.querySelector('#night_day').value=`night`;
  }
">
```

```html
<!-- target변수와 this로 리펙토링 -->
<!-- target == document.querySelector('body') -->
<input type="button" value="night" onClick="
  var target = document.querySelector('body');
  if(this.value == `night`){
    target.style.backgroundColor=`black; 
    target.style.color=`white`;
    this.value=`day`;
  }else{
    target.style.backgroundColor=`White`;
    target.style.color=`black`;
    this.value=`night`;
  }
">
```  
<br>

> ### 배열
```javascript
var coworkers = ["engine","leezche"];
var i = 0;
while(i<coworkers.length){
  document.write('<lt>'+coworkers[i]+'</li>');
  i += 1;
}
```
```javascript
// 모든 <a>태그를 가져와서 alist변수에 넣고 출력하기
var alist = document.querySelectorAll('a');
var i = 0;
while(i < alist.length ) {
  alist[i].style.color='powderblue';
  console.log(alist[i]);
  i = i+1;
}
```  
<br>

> ### 함수
- `onclick`이벤트 안에서 `this`는 이벤트가 소속된 태그를 가리키도록 약속되어 있다.
- 독립된 `nightDayHandler()`안의 코드에서 this라는 값은 더이상 `input`버튼이 아니고 전역 객체를 가지게 된다.
- 함수 안에서 `this`값이 `input`버튼을 가리키도록 `this`값을 줍니다.
- `this`인자를 `self`라는 매개변수로 받겠습니다.
```html
<script>
  function nightDayHandler(self){
  var target = document.querySelector('body');
    if(self.value == 'night'){
      target.style.backgroundColor = 'black';
      target.style.color = 'white';
      self.value = 'day';
      ...
    }else{
      target.style.backgroundColor = 'white';
      target.style.color = 'black';
      self.value = 'night';
      ...
    }
  }
</script>
<input type="button" value="night" onclick="nightDayHandler(this);">
```  
<br>

> ### 객체
```javascript
var Body = {
  setColor: function(color){
    document.querySelector('body').style.color = color;
  },
  
  setBackgoundColor: function(color){
    document.querySelector('body').style.backgroundColor = color;
  }
}

var Links = {
  setColor = function(color){
    var alist = document.querySelectorAll('a');
    var i = 0;
    while(i < alist.length){
      alist[i].style.color = color;
      i = i + 1;
    }
  }
}

function nightDayHandler(self){
  var target = document.querySelector('body');
  if(self.value == 'night'){
    Body.setBackgorundColor('black');
    Body.setColor('white');
    self.value = 'day';
    Links.value = 'day';
  }else{
    Body.setBackgroundColor('white');
    Body.setColor('black');
    self.value = 'night';
    Links.setColor('blue');
  }
}
```



```css
a, h1 { 
  color: black;
  text-decoration: none;
  font-size: 45px;
  <!--border-width: 5px;-->
  <!--border-color: red;-->
  <!--border-style: solod;-->
  border: 5px red solid;
  padding: 20px; <!-- border와 content사이 -->
  margin: 20px; <!-- border의 바깥 쪽 -->
  display: block; <!-- 화면 전체를 쓰고 있는 Box model을 뜻한다. -->
  width: 100px;
}
ol {
  border-right: 1px solid gray;
}

.saw { <!-- 방문했던 링크 class -->
  color: gray;
}
#active { <!-- 현재 머물고 있는 링크 ID -->
  color: red;
}
#grid{
  display: grid;
  grid-template-columns: 150px 1fr; <!-- 첫 번째 Column을 150px만,  -->
}
#grid ol { <!-- 'grid' ID로 지정한 태그 밑에 있는 <ol>를 구분하여 --> 
  margin: 0;
  width: 100px;
  padding: 20px;
  border-right: 5px solid pink;
}
@media(max-width: 800px){ <!-- screen width > 800px일 때 -->
  grid {
    display: none;
  }
  #grid ol{
    border-right: none;
  }
  h1{
    border-bottom: none;
  }
}
```

```html
<!DOCTYPE HTML>
<HTML>
  <head>
    <title>WEB1-CSS</title>
    <meta charset="utf-8">
    <link ref="stylesheet" href="style.css" />
    <script src = "color.js"/>
  </head>
    
  <body>
    <h1><a href="index.html">WEB</a></h1>
    <div>
      <ol>
        <li><a href="1.html" style="color:red; text-decoration:underline">HTML</a></li>
        <li><a href="2.html" class="saw" id="active">CSS</a></li>
        <li><a href="3.html">JavaScript</a></li>
        <input type="button" value="night" onclick="nightDayHandler(this);">
      </ol>
    </div>
    <div> 
    </div>  
  </body>
</HTML>
```
  
<br><br>
------------------------------------------------------
<br><br>

> ### 기타 
- 라이브러리 : 소프트웨어를 만드는 내가 가져다 쓰는 
- 프레임워크 : 프레임워크 안에 우리가 들어가서 작업한다.  
- `DOM객체`
  - `document객체` : 어떤 웹 페이지의 태그를 삭제하고 싶거나 자식 태그를 추가하고 싶다.
- `window객체` : 웹 펭지가 아니라 웹 브라우저 자체를 제어해야 할때, 현재 열린 페이지의 주소를 알아내거나 새 창을 열거나 화면 크기를 Js로 알아올 때
- `Ajax` : 웹 페이지를 리로드하지 않고도 정보를 변경하고 싶다면
- `cookie` : 웹 페이작 리로드돼도 현재 상태를 유지하고 싶다면, 개인화된 서비스 제공시
- `offline wep application` : 인터넷이 끊겨도 동작하는 웹 페이지
- `webRTC` : 화상 통신 웹 앱
- `Speech API` : 사용자의 음성을 인식하고 정보로 전달하고 싶다면
- `WebGL` : 3차원 그래픽으로 게임을 만들고 싶다면
- `WebVR` : 가상현실에 관심이 많다면

<br>

> #### jquery
- `CDN` Content Delivery Network을 활용하는 것이 이점
```javascript
var Body = {
  setColor: function(color){
    $('body').css('color', color);
    // document.querySelector('body').style.color = color;
  },
  
  setBackgoundColor: function(color){
    $('body').css('backgroundColor', color);
    // document.querySelector('body').style.backgroundColor = color;
  }
}
```

```html
<script
  src="https://code.jquery.com/jquery-3.4.1.min.js"
  integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo="
  crossorigin="anonymous">
</script>
<script src = "color.js"/>
```  
<br>

> #### UI vs API
- UI : User Interface, 사용자가 시스템을 제어하기 위해 사용하는 조작 장치
- API : Apllication Programming Interface, 어플리케이션을 만들기 위해 구성한 조작장치를 말함
  - `alert()`
