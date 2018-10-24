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

  
  function add(a, b=a+1){
    return a+b;
  }
  add(2);     // 5 
  add(2, 1);  // 3
```
## 8.11.3 이터레이터와 for/of 문 ##

### 이터레이션 <sub>(iteration)</sub> ###
* 반복처리
* 데이터 안의 요소를 연속적으로 꺼내는 행위
* [ex] `forEach` 메서드는 배열의 요소를 순차적으로 검색하여 그 값을 함수의 인수로 넘기기를 반복

### 이터레이터 ###
* 반복 처리<sub>(iteration)</sub>가 가능한 객체
* 내부적으로 처리되는 반복 작업을 하는 `forEach`메서드와 달리, **개발자가 반복 처리를 단계별로 제어할 수 있다.**
* `value`, `done` 프로퍼티를 가진 객체 를 반환하는 `next` 메서드를 가진다.

<dl>
  <dt>
    <code>Symbol.iterator</code>
  </dt>
  <dd>
    자바스크립트의 내장 심벌
  </dd>
  <dd>
    <code>@@iterator</code>라고 표기하고 이터레이터 심벌이라고 읽는다
  </dd>
</dl>

```js
var a = [5, 4, 3];

// 배열의 이터레이터
// 베열은 이터레이터를 반환하는 Sybole.iterator 메서드를 가지고 있다.
var iter = a[Symbol.iterator]();
console.log(iter.next()); // Object {value: 5, done: false}
console.log(iter.next()); // Object {value: 4, done: false}
console.log(iter.next()); // Object {value: 3, done: false}
console.log(iter.next()); // Object {value: undefined, done: true}  
console.log(iter.next()); // Object {value: undefined, done: true}
```
<dl>
  <dt>
    <code>next()</code>
  </dt>
  <dd>
    value 프로퍼티와 done 프로퍼티를 가진 iterator result라는 객체 반환한다.
  </dd>
  <dd>
    value에는 꺼낸 값이 저장되고 done에는 반복이 끝났는지를 뜻하는 논리값이 저장된다
  </dd>
</dl>

#### 예제 8-20 배열의 이터레이터를 반환하는 함수 ####
```js
function makeIterator(array) {
  var index = 0;
  return {
    next: function() {
      if(index < array.length){
        return {value: array[index++], done: false};
      } else {
        return {value: undefined, done: true}
    }
    }
  }
}
var iter = makeIterator(["A", "B", "C"]);
console.log(iter.next()); // Object {value: "A", done: false}
console.log(iter.next()); // Object {value: "B", done: false}
console.log(iter.next()); // Object {value: "C", done: false}
console.log(iter.next()); // Object {value: undefined, done: true}
```
### 반복 가능한 객체와 for/of 문 ###
* 반복 처리를 자동으로 하도록 만들 수 있다.
* **두 가지 조건을 만족하는 객체를** 반복 처리한다.
  * `Symbol.iterator` 메서드를 가지고 있다.
  * `Symbol.iterator` 메서드는 반환값으로 이터레이터를 반환한다.

```js
// 배열의 요소를 이터레이터를 사용해서 목록으로 바꾸기
var a = [5, 4, 3];
var iter = a[Symbol.iterator]();
while(true) {
  var iteratorResult = iter.next();
  if(iteratorResult.done == true) break;
  var v = iteratorResult.value;
  console.log(v); // 5, 4, 3
}

// 위 프로그램을 for/of문으로..
var a = [5, 4, 3];

// a 이터레이터의 next 메서드를 순회할 때 마다 매번 호출한다.
// done 프로퍼티 값이 false가 아닌 동안은 value 프로퍼티 값을 변수 v에 대입해서 for/of문 안에 있는 코드를 실행한다.
for(var v of a) console.log(v); // 5, 3, 3
```
<dl>
  <dt>
    이터러블<sub>(iterator)</sub>한 객체
  </dt>
  <dd>
    <code>Symbol.iterator</code> 메서드를 가진 반복 가능한 객체
  </dd>
  <dd>
    다음 생성자로 생성한 내장 객체<br>
    <code>Array</code>, <code>String</code>, <code>TypedArray</code>, <code>Map</code>, <code>Set</code> 
  </dd>
</dl>

```js
for(var v of "ABC") console.log(v); // A, B, C
```

이터레이터 객체와 이터러블(반복 가능)한 객체는 **다른 개념**의 객체라는 점을 유의해야 한다.

|  이터레이터 객체  | 이터러블(반복 가능)한 객체  |
|---|---|
| `value`, `done` 프로퍼티를 가진 객체 를 반환하는 `next` 메서드를 가진다.  | `Symbol.iterator` 를 가진 객체  |

```js
function makeIterator(array) {
  var index = 0;
  return {
    next: function() {
      if(index < array.length){
        return {value: array[index++], done: false};
      } else {
        return {value: undefined, done: true}
      }
    }
  }
}
var iter = makeIterator(["A", "B", "C"]); // 이터레이터 객체 반환

for(var v of iter) console.log(v);  // TypeError: iter is not iterable

// iter 이터레이터를 반환하는 Symbol.iterator 메서드를 갖는 객체 생성
var iterable = {};
iterable[Symbol.iterator] = function() {return iter;}

for(var v of iterable) console.log(v); // A, B, C
```

반복 가능(이터러블)한 내장 객체의 이터레이터는 반복할 수 있다.
```js
var arr = ["A", "B", "C"];
for(var v of arr) console.log(v); // A, B, C

var iter = arr[Symbol.iterator]();
for(var v of iter) console.log(v);  //  A, B, C(이터레이터도 가능함)

// Array, String, TypedArray, Map, Set으로 생성한 객체, 이들 객체로 생성한 이터레이터도 반복 처리할 수 있다.
// 이터레이터도 Symbol.iterator 메서드를 가지므로 실행 결과가 이터레이터 자신을 가리킨다.
// 따라서 내장 객체의 이터레이터는 for/of 문으로 반복 처리할 수 있다.
 
var iter_iter = iter[Symbol.iterator]();
console.log(iter_iter === iter);  // true;
```
## 8.11.4 제너레이터 ##

(난이도가 높아서 추후에....)


## 8.11.5 템플릿 리터럴의 태그 함수 ##
### 태그가 지정된 템플릿 리터럴 ###
템플릿 리터럴 앞에 함수 이름을 적으면 템플릿 리터럴의 내용을 인수로 받는 함수를 호출 할 수 있다.
```js
func`${a} + $[b] = ${a+b}`
```

<dl>
  <dt>태그 함수<sub>(tag function)</sub></dt>
  <dd>
    첫 번째 인수는 ${...}를 기준으로 분할한 문자열을 요소로 담은 배열이다.
  </dd>
  <dd>
    두 번째 이후 인수로는 각 ${...}안에 지정된 표현식을 평가한 값이 순서대로 들어 간다.
  </dd>
  <dd>
    어떤 값도 반환할 수 있다.
  </dd>
</dl>

```js
function list() {return arguments;}
var t = list`a${1}b${2}c${3}`;

//템플릿 리터럴의 시작 부분이 ${...}면 첫 번째 인수인 배열의 첫 번째 요소로 빈 문자열이 들어온다.
//템플릿 리터럴의 마지막 부분이 ${...}면 첫 번째 인수인 배열의 마지막 요소로 빈 문자열이 들어온다.
console.log(t); // [["a", "b", "c", ""], 1, 2, 3]
```

#### 예제 8-24 HTML에서 사용할 수 없는 문자를 이스케이프 시퀀스로 바꾸기 ####
```js
  function htmlEscape(strings, ...values) {
    console.log(strings); // ["<p>", "</p>"]
    console.log(values);  // values
    var result = strings[0];
    for(var i = 0; i < values.length; i++) {
      result += escape(values[i]) + strings[i+1];
    }
    return result;

    function escapes(s) {
      return s.replace(/&/g, "&amp;")
          .replace(/</g, "&lt;")
          .replace(/>/g, "&gt;")
          .replace(/'/g, "&#039;")
          .replace(/"/g, "&quot;")
          .replace(/`/g, "&#096");
    }
  }
  var userinput = "<script>alert('test');</script>";
  var message = htmlEscape`<p>${userinput}</p>`;
  console.log(message); // <p>%3Cscript%3Ealert%28%27test%27%29%3B%3C/script%3E</p>
```

### 태그 함수의 첫 번째 인수 ###
callSite 객체라고 한다
<dl>
  <dt>동결되어 있다.</dt>
  <dd>프로퍼티의 추가, 삭제, 변경 모두 불가능하다.</dd>
  <dt>callSite 객체는 캐시된다</dt>
  <dd>이전에 처리했던 템플릿 리터럴 문자열을 만나면(플레이스 홀더 제외) 캐시된 callSite 객체를 넘긴다.</dd>
  <dt>raw 프로퍼티가 있다.</dt>
  <dd>
    배열이며 템플릿 리터럴을 ${...}로 분할한 문자열이다.<br>
    첫 번째 인수의 배열에는 이스케이프된 문자열이 들어오지만<br>
    raw 프로퍼티에는 이스케이프되지 않은 문자열이 들어온다.
  </dd>
</dl>

```js
function tag(strings, ...values) {
  console.log(strings);       //  ["a↵", "b↵", "c", "", raw: Array(4)]
  console.log(strings.raw);   //  ["a\n", "b\n", "c", ""]
}
tag`a\n${1}b\n${2}c${3}`;
```