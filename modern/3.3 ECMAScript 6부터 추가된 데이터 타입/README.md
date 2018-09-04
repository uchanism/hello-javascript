# ECMAScript 6부터 추가된 데이터 타입

* 심벌 (Symbol)
* 템플릿 리터럴 (Template literals)

## 3.3.1 심벌
자기 자신을 제외한 **그 어떤 값과도 다른 유일무이한 값**이다.

### 심벌의 생성
```js
var sym1 = Symbol();

// Symbol() 호출 할때마다 새로운 값을 만든다.
var sym2 = Symbol(); 
console.log(sym1 == sym2);  // false

// 인수를 전달하여 심벌의 설명 추가
var HEART = Symbol("하트");
console.log(HEART.toString()); // Symbol(하트)
```
**예제**
```js
// 오셀로 게임의 상태 값을 표현하는 코드

var cell; // 칸

var NONE = 0;   // 칸에 돌이 놓여 있지 않은 상태
var BLACK = -1; // 칸에 검은 돌이 놓여 있지 않은 상태
var WHITE = 1;  // 칸에 흰돌이 놓여 있는 상태

// 부적합한 프로그램
cell = WHITE;
console.log(cell == WHITE); // true
console.log(cell == 1); // true

// 심벌은 유일무이한 값이므로, cell 값을 확인 할때 NONE, BLACK, WHITE만 사용하도록 제한 할 수 있다.
var NONE = Symbol("none");
var BLACK = Symbol("black");
var WHITE = Symbol("white");

cell = WHITE;
console.log(cell == WHITE); // true
console.log(cell == 1);     // false
```
### 심벌과 문자열 연결하기
```js
// 문자열과 연결된 심벌 생성
var sym1 = Symbol.for("club");  

// 지정한 문자열로 호출
var sym2 = Symbol.for("club");
console.log(sym1 == sym2);   // true

// 심벌과 연결된 문자열 구하기
var sym1 = Symbol.for("club");
var sym2 = Symbol("club");

console.log(Symbol.keyFor(sym1));   // club
console.log(Symbol.keyFor(sym2));   // undefined

```
## 3.3.2 템플릿 리터럴

* 문자열 표현 구문
* 일부만 변경해서 반복하거나 재사용할 수 있는 틀
* 표현식의 값을 문자열에 추가하거나 여러 줄의 문자열을 표현 할 수 있다.

### 기본적인 사용법
```js
// 역따옴표(`)로 묶은 문자열
`I'm going to learn Javascript.`

//간단한 템플릿 리터럴은 큰따옴표 또는 작은따옴표로 묶은 문자열과 모습이 같다.
"I'm going to learn Javascript."

// 문자열 리터럴과 달리 일반적인 줄바꿈 문자를 사용할 수 있다.
var t = "Man errs as long as\nhe strives.";
console.log(t);

var t = `Man errs as long as
he strives.`;
console.log(t);

//이스케이프 시퀀스 문자를 그대로 출력하기
var t = "Man errs as long as\\nhe strives.";
console.log(t);

var t = String.raw`Man errs as long as\nhe strives.`;
console.log(t);
```

## 보간 표현식
템플릿 리터럴 안에는 <a href="#note1">플레이스 홀더</a>를 넣을 수 있다.<br>
${...}로 표기한다.<br>

```js
    // 문자열 안에 변수나 표현식의 결과값을 삽입
    var a = 2, b = 3;
    console.log(`${a} + ${b} = ${a+b}`);  // 2+3 = 5
    var now = new Date();
    console.log(`오늘은 ${now.getMonth()+1} 월 ${now.getDate()} 일입니다.`); // 오늘은 8 월 1 일 입니다.
```

<dl id="note1">
    <dt>플레이스 홀더</dt>
    <dd>
        사용자의 입력 값처럼 실행 시점에 외부에서 주어지는 값을 표현식에 반영하고자 할 때, 그것이 들어갈 수 있도록 마련한 장소를 뜻한다.
    </dd>
</dl>







