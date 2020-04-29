> [목차](index.md)
## CSS
- [CSS](#CSS)
- [CSS 기본 문법](#CSS-기본-문법)
- [CSS 박스 모델](#CSS-박스-모델)
- [Media query](#Media-query)
- [CSS 코드의 재사용](#CSS-코드의-재사용)

<br><br>
<br><br>


# CSS
- Cascading Style Sheets
- 최신 CSS기능을 사용하려면 현재 환경에서 사용할 수 있는지 확인 [Can I Use](https://caniuse.com/)
1. 태그를 통하는 문법
  - `style`태그는 HTML의 문법이면서, 동시에 태그 안쪽의 내용은 CSS의 언어로 처리해야 한다.
  - `font`태그는 HTML의 문법이다.
2. 속성을 이용하는 방법  

<br><br>
<br><br>


# CSS 기본 문법
- `ID 선택자` > `class 선택자`
- 특별한 디자인 구성은 `ID 선택자`에 작성하여 페이지당 단 한 번만 쓰도록 한다.
- 동일한 선택자인 경우 나중에 선언할수록 점수가 높음
```html
<style> 
  a { <!--선택자(Selector)-->
    <!-- 선언, 효과, delaration -->
    color: black; <!-- Property: Value-->
  }
</style>
```   
```html
ol { <!--태그 선택자-->
  border-right: 1px solid gray;
}
.saw { <!--class 선택자-->
  color: gray;
}
#active { <!--ID 선택자-->
  color: red;
}
```

<br><br>
<br><br>




# CSS 박스 모델
- 블록 레벨 엘리먼트(Block level Element) : 화면 전체를 쓰는 태그들, 줄바꿈
- 인라인 엘리먼트(Inline Element) : 자신의 컨텐츠 크기만큼 쓰는 태그들
- 두 차이는 `display`라는 속성의 기본값일 뿐이다. 
- `content` -> `padding` -> `border` -> `margin`

<br>

## Grid
- 효율적인 레이아웃을 짜기위해 `Flexbox`를 이어 `Grid`가 탄생되었다.
- `div` : (블록 레벨 엘리먼트) division의 약자로, 의미 없는 태그
- `span` : (인라인 엘리먼트)
```html
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
```  

<br><br>
<br><br>




# Media query
- 반응형 디자인(responsiveWeb)

```html
<!DOCTYPE HTML>
  <head>
    <title>WEB1-CSS</title>
    <meta charset="utf-8">
    <style>
      @media(min-width: 800px){ <!-- screen width < 800px일 때 -->
        div {
          displayL none;
        }
      }
      @media(max-width: 800px){ <!-- screen width > 800px일 때 -->
        div {
          displayL none;
        }
      }
    </style>
  </head>
    
  <body>
    <div>response</div>
  </body>

</HTML>
```  

<br><br>
<br><br>





# CSS 코드의 재사용
- `style.css`파일을 분리하면 캐싱(cache)을 할 수 있다.
- 빠르게 웹을 표시할 수 있으며 네트워크 트래픽을 훨씬 적게 쓸 수 있다.
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
  <head>
    <title>WEB1-CSS</title>
    <meta charset="utf-8">
    <link ref="stylesheet" href="style.css" />
  </head>
    
  <body>
    <h1><a href="index.html">WEB</a></h1>
    <div>
      <ol>
        <li><a href="1.html" style="color:red; text-decoration:underline"><HTML</a></li>
        <li><a href="2.html" class="saw" id="active"><CSS</a></li>
        <li><a href="3.html"><JavaScript</a></li>
      </ol>
    </div>
    <div> 
    </div>  
  </body>
</HTML>
```
