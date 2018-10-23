# ECMAScript 6부터 추가된 함수의 기능 #
* 화살표 함수
* 나머지 매개변수
* 인수의 기본값
* 이터레이터
* 제너레이터
* 템플릿 리터럴
* 태그 함수

## 8.11.1 화살표 함수 표현식으로 함수 정의하기 ##
함수 리터럴(익명 함수)의 단축 표현

### 화살표 함수 표현식의 작성법 ###
```js
// 기존 함수 리터럴
var square = function(x) {return x*x}

// 화살표 함수 표현식
var square = (x) => {return x*x};

// 인수가 여러 개 있으면 쉼표로 구분
var f = (x, y, z) => { ... };

// 인수가 하나만 있으면 인수를 묶는 괄호 생략 가능
var f = x => {return x*x};

// 인수가 없으면 인수를 묶는 괄호 생량 불가능
var f = () => {...};

// 함수 몸통 안의 문장이 return뿐이면 중괄호와 return 키워드를 생략 가능
var square = x => x*x;

// 함수 몸통 안에 return 문장만 있더라도 반환값이 객체 리터럴이면 그룹 연산자인 ()로 묶어야 한다
var f = (a,b) => ({x:a, y:b });

// 화살표 함수도 즉시 실행 함수(IIFE)로 사용가능
(X => X*X)(3); // 9
```

### 함수 리터럴과 화살표 함수의 차이점 ###
1. this의 값이 함수를 정의할 때 결정된다.
```js
var obj = {
  say: function() {
    console.log(this);                        // object {say:}
    var f = function() {console.log(this);};  // object window
    f();
    var g = () => console.log(this);          // object {say:}
    g();
  }
};

obj.say();
```
화살표 함수는 call이나 apply 메서드를 사용하여 this를 바꾸어 호출해도 this 값이 바뀌지 않는다.
```js
var f = function() {console.log(this.name);};
var g = () = console.log(this.name);
var tom  => {name: "Tom"};

f.call(tom);  // Tom
g.call(tom);  // undefined
```
2. arguments 변수가 없다
```js
var f = () => console.log(arguments); // Uncaught ReferenceError: arguments is not defined
                                      // node.js 환경에서는 에러가 발생하지 않는데??
f();
```
3. 생성자로 사용할 수 없다.
```js
  var Person = (name, age) => {this.name = name; this.age = age;};
  var tom = new Person("Tom", 17);  // TypeError: Person is not a constructor
```
4. yield 키워드를 사용할 수 없다.
화살표 함수 안에서는 yield 키워드를 사용할 수 없다. 따라서 화살표 함수를 제너레이터로 사용할 수 없다.

## 8.11.2 인수에 추가된 기능 ##
### 나머지 매개변수(rest parameters) ###
함수의 인자가 들어가는 부분에 ...을 입력하면 그만큼의 인수를 배열로 받을 수 있습니다. 
```js
function f(a, b, ...args) {
  console.log(a, b, args);
}
f(1, 2, 3, 4, 5, 6);  // 1 2 [3, 4, 5 6]

var sum = (...args) => {
  for(var i=0, s=0; i<args.length; i++) s+=args[i];
  return s;
};
sum(1, 2, 3, 4, 5); // 15
```
### 인수의 기본값 ###
* 함수의 인자에 대입(=) 연산자를 사용해서 기본값을 설정할 수 있다.
* 기본값을 설정한 인자에 호응하는 인수를 생략하거나 undefined를 넘기면 대입 연산자 우변의 값이 기본값이 된다. 
* 다른 인자의 값도 기본값으로 사용할 수 있다.
```js
  function multiply(a, b=1) {
    return a*b;
  }
  multiply(3);    // 3
  multiply(3, 2)  // 6

  // 
  function add(a, b=a+1){
    return a+b;
  }
  add(2);     // 5 
  add(2, 1);  // 3
```