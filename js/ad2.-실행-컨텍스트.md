> [목차](index.md)
## 2. 실행 컨텍스트
 - [실행 컨텍스트](#실행-컨텍스트)
 - [활성화된 실행 컨텍스트의 수집 정보](#활성화된-실행-컨텍스트의-수집-정보)
 - [environmentRecord와 호이스팅](#environmentrecord와-호이스팅)
 - [호이스팅 규칙](#호이스팅-규칙)
 - [함수-선언문과-함수-표현식](#함수-선언문과-함수-표현식)
 - [스코프-체인](#스코프-체인)

<br><br>
<br><br>





# 실행 컨텍스트  
## 실행 컨택스트란?
- `Excution Context`는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체이다.
- 실행 컨텍스트는 전역 공간에서 자동으로 생성되는 전역 컨텍스트와 eval 및 함수 실행에 의한 컨텍스트 등이 있습니다.
- 자바스크립트는 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 호이스팅(hoisting)하고 외부 환경 정보를 구성하고 this값을 설정하는 등의 동작을 수행한다.  
```javascript
var a = 1; // 전역 컨텍스트

function outer() {
  function inner() {
    consol.log(a);
    var a = 3;
  }
  inner();
  console.log(a);
}
outer();
console.log(a);
```  

<br>

## 실행 순서
1. 자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화
2. 코드들을 실행에 필요한 환경 정보들을 모아 컨텍스트를 구성
3. 이를 콜 스택(call stack)에 쌓아올렸다가, 가장 위에 쌓여있는 컨텍스트를 실행
4. 전역 공간에 실행할 코드가 없으면, 전역 컨텍스트도 제거되고 콜스택은 비워집니다.
- 실행 컨텍스트와 콜 스택
  - " "
  - "전역 컨텍스트"
  - "전역 컨텍스트" + "outer"
  - "전역 컨텍스트" + "outer" + "inner"
  - "전역 컨텍스트" + "outer"
  - "전역 컨텍스트"
  - " " 
  
<br><br>
<br><br>






# 활성화된 실행 컨텍스트의 수집 정보
- `VariableEviroment`
  - 최초 실행 시의 스냅샷(snapShot)을 유지
  - 현재 컨텍스트 내의 식별자들에 대한 정보
  - 외부 환경 정보
- `LexicalEnviorment` : 변경사항이 실시간으로 저장됨
  - `environmentRecord` : 매개변수명, 변수의 식별자, 선언한 함수의 함수명 등을 수집합니다.
  - `outerEnviornmentReference` : 바로 직전 컨텍스트의 `LexcicalEnvironment`정보를  
- `ThisBinding` : `this`식별자가 바라봐야 할 대상 객체 

<br><br>
<br><br>





# environmentRecord와 호이스팅
- `environmentRecord`에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장됩니다.
- 전역 실행 컨텍스트는 변수 객체를 생성하는 대신 자바스크립트 구동 환경이 별도로 제공하는 객체, 즉 전역 객체(global object)를 활용합니다.
- 변수 정보를 수집하는 과정을 마친 후 호이스팅 합니다. 

<br><br>
<br><br>






# 호이스팅 규칙
- 함수 선언의 호이스팅 (1)-원본 코드
  
```javascript
function a(){
  console.log("(1) "+b);
  var b = 'bbb'; // 수집 대상 1
  console.log("(2) "+b);
  function b(){ } // 수집 대상 2
  console.log("(3) "+b);
  }
}
a();
```

   1. a 함수의 실행 컨텍스트가 생성됩니다. 이때 변수명과 함수 선언의 정보를 위로 끌어올립니다. (수집합니다)
   2. 변수는 선언부와 할당부를 나누어 선언부만 끌어올리는 반면 함수 선언은 함수 전체를 끌어올립니다. 
- 함수 선언의 호이스팅 (2)-호이스팅을 마친 상태
  
```javascript
function a() {
  var b; // 수집 대상 1 : 변수는 선언부만 끌어올립니다.
  function b(){ } // 수집 대상 2 : 함수 선언은 전체를 끌어올립니다.
  
  console.log("(1) "+b);
  b = 'bbb'; // 변수의 할당부는 원래 자리에 남겨둡니다.
  console.log("(2) "+b);
  console.log("(3) "+b);
}
```
- 함수 선언의 호이스팅 (3)-함수 선언문을 함수 표현식으로 바꾼 코드
```javascript
function a(){
  var b;
  var b = function b(){ } // 바뀐 부분
  
  console.log("(1) "+b);
  b = 'bbb';
  console.log("(2) "+b);
  console.log("(3) "+b);
}
```
- 출력문
```
(1) b 함수
(2) 'bbb'
(3) 'bbb'
``` 

<br><br>
<br><br>






# 함수 선언문과 함수 표현식
- 함수 선언문(function declaraion)
  - function 정의부만 존재하고 별도의 할당 명령이 없는 것을 의미한다.
  - 선언 전에 호출 가능 (문제의 소지가 있음)
- 함수 표현식(funtion expression)
  - 정의한 function을 별도의 변수에 할당하는 것을 말한다.
  - 함수를 다른 변수에 값으로써 '할당'한 것이 곧 함수 표현식입니다.
- 함수 선언문의 경우 반드시 함수명이 정의돼 있어야 하는 반면, 함수 표현식은 없어도 됩니다.
- 함수를 정의하는 세 가지 방식
```javascript
function a(){ ~ } // 함수 선언문, 함수명 a가 곧 변수명
a();

var b = function(){ ~ } // (익명) 함수 표현식, 변수명 b가 곧 함수명
b();

var c =function(){ ~ } // 기명 함수 표현식, 변수명은 c, 함수명은 d
c();
d(); //에러
``` 

<br><br>
<br><br>






# 스코프 체인
- 스코프(Scope)란 식별자에 대한 유효범위입니다.
- 스코프 체인(Scope Chain) : 식별자의 유효범위를 안에서부터 바깥으로 차례로 검색해나가는 것
- `LexicalEnviroment`의 두 번째 수집 자료인 `outerEnviromentReference`가 담당  
- 여러 스코프에 동일한 식별자를 선언한 경우, 스코프 체인 상에서 가장 먼저 발견된 식별자에게만 접근 가능하게 됩니다.
<br>

> :bulb: `outerEnviromentReference`는 현재 호출된 함수가 선언될 당시의 `LexicalEnviroment`를 참조하면서 올라갑니다.
마지막엔 전역 컨텍스트의 `LexicalEnviroment`가 있을 겁니다. 또한 각 `outerEnviromentReference`는 오직 자신이 선언된 시점의 `LexicalEnviroment`만
참조하고 있으므로 가장 가까운 요소부터 차례대로만 접근할 수 있고 다른 순서로 접근하는 것은 불가능할 것입니다.
만약 전역 컨텍스트의 `LexicalEnviroment`까지 탐색해도 해당 변수를 찾지 못하면 undefined를 반환합니다.

```javascript
var a = 1;
var outer = function() {
  var inner = function() {
    consol.log(a); // 5
    var a =3;
  };
  inner();
  console.log(a); // 8
};
outer();
console.log(a); // 9
```
  1. 전역 컨텍스의 `enviromentRecord`에 { a, outer } 식별자를 저장합니다.
  2. `outer`실행 컨텍스트가 활성화 되어 전역 컨텍스트의 코드는 임시중단됩니다.
  3. `outer`실행 컨텍스트의 `enviromentRecord`에 { inner } 식별자를 저장합니다.
  4. `inner`실행 컨텍스트가 활성화 되어 `outer`실행 컨텍스트의 코드는 임시중단됩니다.
  5. 식별자 `a`에 접근시 현재 활성화화 상태인 `inner` 컨텍스트의 `environmentRecord`에서 `a`를 검색합니다.
  6. `inner`함수 실행이 종료되면, `inner`실행 컨텍스트가 콜 스택에서 제거되고 `outer`실행 컨텍스트가 다시 활성화 됩니다.
  7. 식별자 `a`에 접근시 현재 활성화된 상태인 두 번째 전역 `LexicalEnviroment`에서 `a`를 검색합니다.
  8. `outer`함수 종료
  9. 식별자 `a`에 접근시 현재 활성화된 상태인 전역 컨텍스트의 `environmentRecord`에서 `a`를 검색합니다.
<br>
  
> :bulb: 식별자에 접근하고자 할때 자바스크립트 엔진은 할성화된 실행 컨텍스트의 `LexicalEnviroment`에 접근합니다.
첫 요소의 `enviromentRecord`에서 식별자가 없으면 `outerEnviromentReference`에 있는 `enviromentRecord`로 넘어가는 식으로 검색합니다.

> :bulb: 전역 공간에서는 전역 스코프에서 생성된 변수에만 접근할 수 있습니다.
`outer`함수 내부에서는 `outer`및 전역 스코프에서 생성된 변수에 접근할 수 있지만 `inner`스코프 내부에서 생성된 변수에는 접근하지 못합니다.
`inner`함수 내부에서는 `inner`,`outer`,전역스코프 모두에 접근할 수 있습니다. 이를 변수 은닉화(variable shadowing)라고 합니다.  

> :bulb: 전역 컨테스트의 `LexicalEnviroment`에 담긴 변수를 전역분라 하고, 그 밖의 함수에 의해 생성된 실행 컨텍스트의 변수들은 모두 지역변수입니다. 안전안 코드 구성을 위해 가급적 전역 변수의 사용은 최소화하는 것이 좋습니다.


