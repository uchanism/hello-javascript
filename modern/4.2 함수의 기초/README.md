# 4.2 함수의 기초 #
## 4.2.6 함수의 실행 흐름 ##
* 호출한 코드에 있는 인수가 함수 정의문의 인자에 대입된다.
* 함수 정의문의 중괄호 안에 작성된 프로그램이 순차적으로 실행된다.
* return 문이 실행되면 호출한 코드로 돌아간다. return 문의 값은 함수의 반환값이 된다.
* return 문이 실행되지 않은 상태로 마지막 문장이 실행되면, 호출한 코드로 돌아간 후에 undefined가 함수의 반환값이 된다.

## 4.2.7 함수 선언문의 끌어올림 ##
자바스크립트 엔진은 변수 선언문과 마찬가지로 함수 선언문을 프로그램의 첫머리로 끌어 올린다.

## 4.2.8 값으로서의 함수 ##
자바스크립트에서는 **함수가 객체다.**<br>
함수 선언문으로 함수를 선언하면 내부적으로는 그 함수 이름을 변수 이름으로 **한 변수와 함수 객체가 만들어지고, 그 변수에 함수 객체의 참조가 저장된다.**

이 변수 값을 다른 변수에 할당하면 그 변수 이름으로 함수를 실행할 수 있다.<br>
또한 함수를 다른 함수의 인수롤 넘길 수도 있다.

## 4.2.9 참조에 의한 호출과 값에 의한 호출 ##
함수는 원시 값을 인수로 넘겼을 때와 객체로 넘겼을 때 다르게 동작한다.

### 인수가 원시 값일 때 ###
```js
function add1(x) { return x = x + 1; }
var a = 3;
var b = add1(a);
console.log("a = " + a + ", b = " + b ); // a = 3, b = 4
```
인수에 원시 값을 넘기면 **그 값 자체**가 인자에 전달된다.

이를 가리켜 **값의 전달**이라고 부른다.<br>
이때 변수 a와 변수 x는 다른영역의 메모리에 위치한 **별개의 변수**다.<br>
따라서 **x 값을 바꾸더라도 a 값은 바뀌지 않는다.**
### 인수가 객체일 때 ###
```js
function add1(p) {
  p.x = p.x + 1;
  p.y = p.y + 1; 
  return p;
}
var a = { x:3, y:4 };
var b = add1(a);
console.log(a, b); // // Object { x: 4, y: 5 } Object { x: 4, y: 5 }
```
인수로 객체를 넘겼을 때 전달되는 값은 **참조 값**이다.<br>
이를 가리켜 **참조 전달**이라고 부른다.

### 인수 여러 개를 우아하게 전달하는 방법(elegant way) ####
#### 함수에 넘겨야 하는 인수 개수가 만하지면 발생되는 문제 ####
* 인수의 순서를 착각하기 쉽다.
* 함수가 받는 인수 개수를 바꾸면 함수의 호출 방법이 바뀌므로 프로그램 전체를 수정해야 한다.

객체의 프로퍼티에 인수를 담아서 넘기면 이러한 문제를 우아하게 해결 할 수 있다.
```js
function setBallProperties(x, y, vx, vy, radius) {
  ...
}

// 함수의 인수를 객체의 프로퍼티에 담아서 함수에 넘기기
var parameters = {
  x: 0,
  y: 0,
  vx: 10,
  vy: 15,
  radius: 5
}
function setBallProperties(params) {
  ...
}

setBallProperties(parameters);

```
이때 함수 안에서 프로퍼티를 읽는 코드는 `params.vx`처럼 표현하면 되므로 **인수 순서가 바뀌는 문제가 발생하지 않는다.** <br>
또한 인수를 추가하는 경우에도 프로퍼티만 추가하면 되므로 **함수를 호출하는 방법을 바꿀 필요가 없다.**

**함수 안에서 객체의 프로퍼티를 수정하면 호출한 코드에 있는 인수 객체의 프로퍼티가 함께 바뀌므로 주의해야한다.**

## 4.2.10 변수의 유효 범위 ##
### 전역 유효 범위와 지역 유효 범위 ###
<dl>
  <dt>
    유효범위 <sub>Scope<sub>
  </dt>
  <dd>
    변수에 접근할 수 있는 범위
  </dd>
  <dt>
    lexical scope
  </dt>
  <dd>
    프로그램의 구문만으로 유효 범위를 정하는 어휘적 범위
  </dd>
  <dt>
    dynamic scope
  </dt>
  <dd>
    프로그램 실행 중에 유효 범위를 정하는 동적 범위
  </dd>
</dl>

* 대다수의 프로그래밍 언어와 마찬가지로 자바스크립트도 어휘적범위 <sub>(lexical scope)</sub>를 채택하고 있다.
* 자바스크립트 변수는 변수의 유효 범위에 따라 전역 변수와 지역변수로 나눈다.
```js
  var a = "global"; // 전역 변수
  function f() {
    var b = "local";  // 지역 변수
    console.log(a); // global
    return b;
  }

  f();
  console.log(b); // ReferenceError: b is not defined
```
### 변수의 충돌 ###
전역 변수와 지역 변수가 충돌하면 전역 변수를 숨기고 지역 변수를 사용하게 된다.
```js
var a = "global";
function f() {
  var a = "local";
  console.log(a); // local
  return a;
}
f();
console.log(a); // global
```
두 변수는 이름이 같지만 **다른 위치의 메모리에 있는 별개의 변수다.**
### 함수 안에서의 변수 선언과 변수 끌어올림 ###
* 함수 안에서 선언된 지역 변수의 유효범위는 함수 전체다.
* 함수 안의 변수 선언부를 함수의 첫머리로 끌어올린다.

### 함수 안에서의 변수 선언 생략 ###
변수 선언하지 않은 상태에서 값을 대입하면 전역변수로 선언된다.
```js
  function f() {
    a = "local";  // 전역 변수
    console.log(a); // local
    return a;
  }
  f();
  console.log(a); // local
```

## 4.2.11 블록 유효 범위 : let과 const ##
* '블록 유효 범위'를 갖는 변수를 선언한다.
* 중괄호(`{}`) 안에서만 유효하다.
* let은 변수를 선언하고 const는 한 번만 할당할 수 있는 상수를 선언한다.

### let 선언자 ###
* 블록 유효 범위를 갖는 지역 변수를 선언한다. 
* 사용법은 var문과 같다.
```js
let x;
let a, b, c;
let x = 5,
    y = 7;
```
var로 선언한 변수와 let으로 선언한 변수의 차이점은 let으로 선언한 변수의 유효 범위가 블록 안이라는 점이다.

```js
let x = "outer x";  // 전역 유효범위 
{ 
  console.log(x);

  // 중괄호 안쪽 유효범위
  let x = "inner x";
  let y = "inner y";
  console.log(x); // inner x
  console.log(y); // inner y
}

console.log(x); // outer x
console.log(y); // ReferenceError: y is not defined
```
중괄호 안에 있는 변수 이름과 중괄호 바깥에 있는 변수 이름이 같지만 중괄호 안에서는 중괄호 내부의 변수가 유효하다

var 문과 달리 **let 문으로 선언한 변수를 끌어올리지 않는다.**
```js
console.log(x); // ReferenceError: x is not defined
let x = 5;
```
똑같은 이름을 가진 변수를 선언하면 문법 오류가 발생한다.
```js
let x;
let x;  // SyntaxError: Identifier 'x' has already been declared
```
### const 선언자 ###
* 블록 유효 범위를 가지면서 한 번만 할당할 수 있는 변수(상수)를 선언한다.
* 반드시 초기화해야 한다.

```js
const c = 2;

// 다시 대입을 시도하면 타입 오류 발생
c = 5;  //  TypeError: Assignment to constant variable.
```
상수 값이 객체, 배열일 경우는 프로퍼티 또는 프로퍼티 값을 수정할 수 있다.
```js
const origin = {x:1, y:2};
origin.x =3;
console.log(origin); // Object {x:3, y:2}
```
## 4.2.12 함수 리터럴로 함수 정의하기 ##
* 이름이 없는 함수이므로 익명 함수 또는 무명함수라고 한다.
* 함수 선언문에는 끝에 세미콜론을 붙일 필요가 없지만 함수 리터럴을 사용할 때는 끝에 반드시 세미콜론을 붙여야 한다.
* 자바스크립트 엔진은 함수 리터럴로 정의한 함수는 끌어올리지 않는다.
* 익명 함수에도  이름을 붙일 수 있다.
```js
// 함수 선언문
// function square(x) {
//   return x * x;
// }

console.log(square(5)); // TypeError: square is not a function

// 함수 리터럴 
var square = function(x) {
  return x * x;
}

console.log(square(5)); // 10
```

익명 함수 코드는 디버거에 모두 anonymous function이라고 표시되므로 함수를 구별할 수 없다는 단점이 있다.
단, 이름이 붙은 익명 함수는 어떤 함수인지 확인할 수 있다.
```js
var square = function sq() {
  return x * x;
};
```
### 4.2.13 객체의 메서드 ###
* 객체의 프로퍼티 중에서 함수 객체의 참조를 값으로 담고 있는 프로퍼티를 메서드라고 한다.
* 메서드를 정의할 때는 프로퍼티 값으로 함수 리터럴을 대입한다.
* 메서드 또한 프로퍼티의 일종이므로 나중에 추가할 수 있다.
* 일반적으로 메서드가 속한 객체의 내부 데이터(프로퍼티 값) 상태를 바꾸는 용도로 사용한다.

```js
var circle = {
  center: {x:1.0, y:2.0},   
  radius: 2.5,
  area: function () {
    return Math.PI * this.radius * this.radius; // this는 그 함수를 메서드로 가지고 있는 객체를 가리킨다. circle
                                                // 즉 this.radius가 circle.raidus 다
  }
};

circle.area() // 19.634954084936208

circle.translate = function(a, b) {
  this.center.x = this.center.x + a;
  this.center.y = this.center.y + b;
};

circle.translate(1, 2);
circle.center;  // Object {x: 2, y: 4}
```

일반적인 객체 지향 언어에서는 데이터와 그 상태를 바꾸는 메서드를 하나로 묶는 용도로 객체를 사용한다.
이러한 객체를 기본 부품으로 삼아 프로그램을 만들어 가는 기법을 가리켜 객체 지향 프로그래밍이라고 한다.

### 4.2.14 함수를 활용하면 얻을 수 있는 장점 ###
#### 재사용할 수 있다. ####
* 반복 작업을 함수 하나로 만들어 프로그램이 간결해 진다.
* 같은 알고리즘을 사용하지만 시작 값이 다른 처리를 인수 값만 바꿔 호출하도록 수정하면 함수 하나만 만들어 쓸 수 있다.
#### 만든 프로그램을 이해하기 쉰다. ####
* 함수 이름으로 프로그램을 읽을 때 큰 흐름을 쉽게 파악할수 있다.
#### 프로그램 수정이 간단해진다. ####
* 똑같은 처리를 프로그램 곳곳에 작성하지 않고 함수로 정리해 두면 해당 함수만 수정하면 되므로 수정이 간단해진다.