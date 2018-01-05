# 자바스크립트 표준 #
## 9.1 ECMAScript 6 표준 ##
* 2015년 6월 발표
* ECMAScript 2015 라고도 불린다
* 하위 호환성을 위해 기존의 표준 위에 추가적인 기능들과 조금 더 나은 프로그래밍 언어의 모습을 얹어놓았다.

<em><b>ECMAScript 6 추가 기능 요약</b></em>

추가기능   |   세부 내용
----------|------------
변수, 상수 선언    |   변수 선언용 <code>let</code> 키워드와 상수 선언용 <code>const</code> 키워드 추가
화살표 표현식     |   함수 정의에 대한 새로운 화살표 표현식 <code>=></code> 추가
클래스     |   클래스 키워드 추가
객체 표현식 기능 확대     |  프로토타입 정의 기능, 함수와 속성에 대한 표현식 추가
템플릿 문자열     |   역따옴표(<code>`</code>)를 통한 문자열 추가 기능 확대
Destructuring 기능     |  객체 표현식을 통해 변수들을 매핑하여 할당 가능
함수 인자 기능 확대     |   인자 기본값 설정, 가변 인자 기능 확대
Iterator와 for-of 기능     |  Iterator 속성 정의 기능과 <code>for-of</code> 추가 키워드 정의, Iterator 속성을 간단하게 정의하기 위한 <code>function</code>과 <code>yield</code> 키워드 추가
Map과 Set 기능 추가     |    <code>Map</code>과 <code>Set</code> 키워드와 <code>WeakMap</code>, <code>WeakSet</code> 키워드 추가
Binary+Octal 표현식    |   2진수, 8진수 표현식 추가
TypedArray 기능       |   형식 기반 배열 기능 추가
모듈 기능 표준화     |     모듈 관리를 위한 <code>export</code>, <code>import</code> 표준 추가
Proxy 모듈 추가     |   객체 가상화 또는 Proxy 패턴의 다양한 기능을 기본 표준으로 추가
Symbol 모듈 추가    |   새로운 기본형에 대한 정의 기능 추가
Promise 모듈 추가     |     Promise API 기능 추가
기존 추가 API     |     <code>Math</code>, <code>Number</code>, <code>String</code>, <code>Array</code>, <code>Object</code>에 추가 API

### 9.1.1 변수, 상수 선언 키워드(let, const) ###
기존의 자바스크립트는 <code>var</code> 키워드로 변수나 상수를 자유롭게 설정할 수 있었지만, ES6에서는 상수와 변수를 구분할 수 있는 키워드를 추가하였다.

<dl>
    <dt>
        <code>let</code>
    </dt>
    <dd>
        기존 var 키워드의 일반적인 객체를 정의한다.
    </dd>
    <dt>
        <code>const</code>
    </dt>
    <dd>
        변경되지 않을 상수를 정의한다.
    </dd>
</dl>

*let와 const 키워드를 통한 변수 선언*
```js
    let myObj = {
        name: "chanhyun",
        say() {
            console.log("My name is "+this.name);
        }
    };

    myObj.say();

    const constStr = "This is a constant";
    constStr = "This will raise an error";  // TypeError: Assignment to constant variable.
```
<code>let</code>과 <code>const</code> 키워드는 블록 개념을 도입하여 <code>if</code>나 <code>for</code> 안에서 변수를 정의하면 외부에서 사용할 수 없다.

*var과 let 키워드 블록 적용 비교 - if*
```js
    if (true) {
        var varVariable = 1;
        let letVariable = 2;
        
    }

    console.log(varVariable);   // 1
    console.log(letVariable);   // ReferenceError: letVariable is not defined
```

*var과 let 키워드 블록 적용 비교 - for*
```js
    let myArr = [0,1,2,3,4,5],
        arrLeng = myArr.legnth;
    for (let i = 0; i < arrLeng ; i++) {
        if (myArray[i] > 3) {
            break;
        }
    }
    console.log(myArr[i]);  // ReferenceError: i is not defined
```

*var과 let 키워드 중복 변수명 정의 비교*
```js
    // 같은 변수명을 여러 번 정의해도 문제가 없다.
    var duplicatedName = "This is with var";
    var duplicatedName = "No problem";
    
    // 같은 변수, 상수 명을 정의하면 에러를 발생한다.
    let duplicatedName = "Syntax error will be raised";     // SyntaxError: Identifier 'duplicatedName' has already been declared
    const duplicatedName ="Syntax error will be raised";    // SyntaxError: Identifier 'duplicatedName' has already been declared
```