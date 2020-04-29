> [목차](index.md)
## 1. 데이터 타입
 - [데이터타입의 종류](#데이터타입의-종류)
 - [변수 선언과 데이터 할당](#변수-선언과-데이터-할당)
 - [불변 객체](#불변-객체)
 - [undefined와 null](#undefined와-null)
 - [데이터 타입에 관한 배경지식](#데이터-타입에-관한-배경지식)

<br><br>
<br><br>





# 데이터타입의 종류
- 기본형(primitive type)
  - number
  - string
  - boolean
  - null
  - undefined
  - `ES6` symbol
- 참조형(reference type)
  - Object
  - Array
  - Function
  - Date
  - RegExp
  - `ES6` Map, WeakMap
  - `ES6` Set, WeakSet

<br><br>
<br><br>






# 변수 선언과 데이터 할당
## 변수 선언
```javascript
var a;
a = 'abc';
var a ='abcdef';
```
- 데이터의 식별자는 `a`이다.
- `abc' 데이터를 위한 별도의 메모리 공간 확보
- 주소를 변수 영역에 저장 ( type에 따른 메모리 용량을 가변적으로 대응하기 위함 )
- 기존 문자열에 상관없이 `abcdef`를 새로 만들어 다시 새 주소를 저장함
- 이처럼 변수 영역과 데이터 영역을 분리하면 중복된 데이터에 대한 처리 효율이 높아진다.  
<br>

## "기본형은 불변성(immutablilty)을 뜁니다."
- 기본형은 값이 담긴 주솟값을 바로 복제하는 반면
- 참조형은 값이 담긴 주솟값들로 이루어진 묶음을 가리키는 주솟값을 복제합니다.
- 메모리와 데이터, 식별자와 변수의 개념을 구분할 수 있어야 합니다.  
<br>

## 불변값 ( 기본형 )
- 불변성 여부를 구분할 때의 변경 가능성의 대상은 '데이터영역' 메모리 입니다.
- 변수에 새 값을 할당할지라도 주소메모리에 위치한 데이터들은 다른 값으로 변경할 수 없습니다.
- 데이터의 주소를 참조하는 `참조 카운트`가 0인 메모리 주소는 가비지 컬렉팅(GC)의 수거대상이 됩니다.  
<br>

## 가변값 ( 참조형 )
- 변수영역 - 데이터영역 -> 객체@5001의 변수영역
- 객체의 변수(프로퍼티)영역이 별도로 존재.
- 데이터영역은 기존의 메모리 공간을 그대로 활용하고 있으니 다른 값을 대입할 수 있다.
```javascript
var obj={
  x: 3,
  arr: [3,4,5]
};
```
- 참조형 데이터의 프로퍼티에서 다시 참조형 데이터를 할당하는 경우 `중첩객체`라고 합니다.  
<br> 

## 변수 복사 비교  
- 변수를 복사하는 과정은 기본형 데이터와 참조형 데이터 모두 같은 주소를 바라보게 되는 점에서 동일합니다.
- 참조형은 한 단계를 더 거치게 된다는 차이점
- '가변'은 참조형 데이터 내부 프로퍼티가 변경될 때만 성립됩니다.

```javascript
var a = 10;
var b = a;
var obj={ c: 10, d: 'ddd' };

var obj2 = obj1; // obj1 === obj2
b = 15; // a!== b
obj2.c = 20;

obj2 = { c:20, d: 'ddd' }; // obj1 !== obj2
```

<br><br>
<br><br>






# 불변 객체
## 불변 객체를 만드는 간단한 방법
- 값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 변하지 않아야 하는 경우가 있음.
- 참조형 데이터의 '가변'은 데이터 자체가 아닌 내부 프로퍼티를 변경할 때만 성립하므로, 메모리 내 기존 데이터는 변하지 않습니다.
- 얕은 복사 : 기존 정보를 복사해서 새로운 객체를 반환하는 함수
  - :grey_exclamation:`프로토타입 체이닝` 상의 모든 프로퍼티를 복사하는 점, getter/setter를 복사하지 않는 점에서 얕은 복사
```javascript
var copyObject = function(target){
  var result = {};
  for (var prop in target){
    result[prop] = target[prot];
    }
    return result;
};
```
- copyObject를 이용한 객체 복사 : `immutable.js`, `bobab.js`등의 라이브러리 이용 가능
```javascript
var user = {
  name : 'Jaenam',
  gendet: 'male'
};

var user2 = copyObject(user);
user2.name ='Jung';

if(user!==user2){
 console.log('유저 정보가 변경되었습니다.');
}
console.log(user.name, user.name);
console.log(user === user2);
```  

<br><br>
<br><br>





# 얕은 복사와 깊은 복사
- 얕은 복사(Shallow copy) : `copyObject` 바로 아래 단계의 값만 복사하는 방법, 주솟값만 복사하는 의미
- 깊은 복사(deep copy) : 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법입니다.
```javascript
var user = {
 name: 'kim'
 urls: {
  portpolio : http://naver.com',
  blog : http://youtube.com'
  }
};

var user2 = copyObject(user);
user2.name = 'han';
consol.log(user.name === user2.name); // false -> 'user'객체에 직접 속한 프로퍼티에 대해서는 완전히 새로운 데이터가 만들어짐

user.urls.portpolio = 'http://daum.com';
consol.log(user.urls.portpolio === user2.urls.portpolio) // true -> 한 단계 더 들어간 urls의 내부 프로퍼티들은 기존 데이터를 참조한 것

user2.urls = copyObject(user.urls); // 한 단계 더 복사
user2.urls.blog = 'http://tistory/com';
console.log(user.urls.blog === user2.urls.blog) // false
```
```javascript
// 재귀호출하여 깊은 복사
var copyObejct = function(target){
 var result ={};
 if(typeof target === 'object' && target !== null){
  for(var prop in target){
   result[prop] = copyObjectDeep(target[prop]);
  }
 } else {
  reutrn result;
};
```
- `hasOwnProperty` 매 `프로토타입 체이닝`을 통해 상속된 프로퍼티를 복사하지 않을 수 있다.
- `ES6`의 `Object.getOwnPropertyDescriptor`
- `JSON`객체로 전달할 수 있다.
```javascript
var copyObejctViaJSON = function (target){
 return JSON(JSON.stringify(target));
};

var obj={
 a:1,
 b:{
  c: null,
  d: [1,2],
  func1: function(){console.log(3);
 },
 func2: function(){consol.log(4);}
};

var obj2 = copyObejctViaJSON(obj);
abj2.a = 3;
abj2.b.c = 4;
obj.b.d[1] = 3;
 
consol.log(obj);
consol.log(obj2);
```

<br><br>
<br><br>





# undefined와 null
- 자바스크립트 엔진이 undefined를 반환하는 경우
  - 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
  - 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
  - return문이 없거나 호출되지 않는 함수의 실행 결과
- 명시한 'undefined'는 하나의 값으로, 순회의 대상이 될 수 있다.
- 자바스크립트가 반환하는 'undefined'는 존재 자체가 없음을 나타낸다.
- '비어있는 요소`null`'는 순회 대상에서 제외됩니다.
```javascript
var arr1 = [undefined, 1]l
var arr2 = [];
arr2[1] = 1;

arr1.forEach(function (v,i) {consol.log(v,i);} ); // undefined 0/1 1
arr2.forEach(function (v,i) {consol.log(v,i);} ); // 1 1

arr1.map(funtion (v,i) { return v+i; }); // [NaN, 2]
arr2.map(funtion (v,i) { return v+i; }); // [empty, 2]

arr1.filter(function (v) { return !v; }); // [undefined]
arr2.filter(function (v) { return !v; }); // []

arr1.reduce(funtion (p,c,i) { return p+c+i; }, ''); // undefined011
arr2.reduce(funtion (p,c,i) { return p+c+i; }, ''); // 11
```

<br><br>
<br><br>






# 데이터 타입에 관한 배경지식  
## 메모리와 데이터
- 각 비트는 고유한 식별자(unique identifier)을 통해 위치를 확인할 수 있다.
- 바이트 단위의 식별자는 메모리 주솟값(Memory address)를 통해 서로 구분하고 연결할 수 있습니다.
- C/C++, Java 등의 정적 타입 언어는 데이터 타입별로 할당할 메모리 영역을 2바이트, 4바이트로 구분합니다.
- Javascript는 구분하지 않고 8바이트를 확보합니다.  
  
<br>

## 식별자와 변수
- 변수(valuable) : 변경 가능한 데이터가 담길 수 있는 공간
 - 변수 선언시 메모리에 식별자를 저장하고 undefined를 할당
 - 기본형 데이터를 할당시 별도의 공간에 데이터를 저장하고, 그 주소를 변수의 값 영역에 할당합니다.
 - 참조형 데이터는 내부 프로퍼티들을 위한 별도의 변수 영역을 확보합니다.
 - 불변 객체로 이용하기 위해서는 깊은 복사해야 합니다. ( 내부 프로퍼티 전부 복사 혹은 라이브러리 활용 )
-식별자(identifier) : 변수명


