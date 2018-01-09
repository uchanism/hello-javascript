# 자바스크립트 표준 #
## 9.1 ECMAScript 6 표준 ##
* 2015년 6월 발표
* ECMAScript 2015 라고도 불린다
* 하위 호환성을 위해 기존의 표준 위에 추가적인 기능들과 조금 더 나은 프로그래밍 언어의 모습을 얹어놓았다.

### ECMAScript 6 추가 기능 요약 ###

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

*<b>let와 const 키워드를 통한 변수 선언</b>*
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

*<b>var과 let 키워드 블록 적용 비교 - if</b>s*
```js
    if (true) {
        var varVariable = 1;
        let letVariable = 2;
        
    }

    console.log(varVariable);   // 1
    console.log(letVariable);   // ReferenceError: letVariable is not defined
```

*<b>var과 let 키워드 블록 적용 비교 - for</b>*
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

*<b>var과 let 키워드 중복 변수명 정의 비교</b>*
```js
    // 같은 변수명을 여러 번 정의해도 문제가 없다.
    var duplicatedName = "This is with var";
    var duplicatedName = "No problem";
    
    // 같은 변수, 상수 명을 정의하면 에러를 발생한다.
    let duplicatedName = "Syntax error will be raised";     // SyntaxError: Identifier 'duplicatedName' has already been declared
    const duplicatedName ="Syntax error will be raised";    // SyntaxError: Identifier 'duplicatedName' has already been declared
```

### 9.1.2 함수 화살표 표현식 ###
익명 함수를 표현할 때 간단하게 표현할 수 있는 화살표 표현식을 추가하였다.

*<b>화살표 표현식 예</b>*
```js
    // 하나의 실행문을 함수에서 포함하고 있다면 중괄호도 생략할 수 있다.
    let myFunc = () => console.log("This is a new function literal");
    myFunc();

    var myFuncES5 = function () {
        console.log("This is a ES5 function literal");
    };
    myFuncES5();
```
*<b>인자를 받는 화살표 표현식 예</b>*
```js
    let paramFunc = (greetings, name) => {
        console.log(greetings+" , "+name);
    }
    paramFunc("Hello", "World");

    var paramFuncES5 = function(greetings, name) {
        console.log(greetings+" , "+name);
    }
    paramFuncES5("Hello", "World");
```
*<b>화살표 표현식의 컨텍스트 비교</b>*
```js
    var name = "Global";
    function Person() {
        this.name ="Unikys";

        // 함수 화살표 표현식은 콜백 함수로 실행될 때 콜백 함수를 할당한 당시의 컨텍스트를 그대로 활용한다.
        setTimeout(() => console.log("My name is "+this.name), 100);
        
        // 기존 익명 function은 글로벌 컨텍스트에 접근한다.
        setTimeout(function () {
            console.log("Global name is "+this.name);
        }, 100)

    }
    
    var person = new Person();
```

*<b> 화살표 표현식을 활용한 캡슐화 비교</b>*
```js
    // ES5
    // 괄호()를 어디에 묶어도 오류없이 실행된다. 
    (function () {
        console.log("ES5 IIFE");
    }());

    (function () {
        console.log("ES5 IIFE - 2");
    })();

    // ES6
    // 화살표 표현식을 먼저 괄호로 묶고 호출해야 한다.
    (() => {
        console.log("ES6 IIFE");
    })()

    (() => {
        console.log("ES6 IIFE");    // SyntaxError: missing ) after argument list
    }())
```
### 9.1.3 클래스(class) 키워드 ###

*<b>class 키워드 활용 예</b>*
```js
    // ES5
    function CarES5 (name) {
        this.name = name;
        this.type = "Car";
    }

    CarES5.prototype.getName = function () {
        return this.name;
    }

    var es5Car = new CarES5("ES5 car");
    
    // es5Car.getName = function () {
    //     return this.name;
    // }
    
    console.log("ES5 car.getName(); " + es5Car.getName());
    console.dir(CarES5);
    console.dir(es5Car);


    // ES6
    class Car { 
        // 생성자 설정
        constructor(name) {
            // 맴버변수 설정
            this.name = name;
            this.type = "Car";
        }
        // 속성 설정
        getName() {
            return this.name;
        }
    }

    let car = new Car("My car");
    console.log("ES6 car.getName(); " + car.getName());
    console.dir(Car);
    console.dir(car);
```
*<b>class 키워드를 통한 서브 클래스 정의 예</b>*
```js
    class Car { 
        // 생성자 설정
        constructor(name) {
            // 맴버변수 설정
            this.name = name;
            this.type = "Car";
        }
        // 속성 설정
        getName() {
            return this.name;
        }
    }
    let car = new Car("My car");
    console.log("ES6 car.getName(); " + car.getName());     // ES6 car.getName(); My car

    class SUV extends Car {
        constructor(name) {
            // 부모 클래스 생성자 호출
            super(name);
            this.type = "SUV";
        }
    }

    let suv = new SUV("My SUV");
    console.log("suv instanceof SUV: "+(suv instanceof SUV));   // suv instanceof SUV: true
    console.log("suv instaceof Car: "+(suv instanceof Car));    // suv instaceof Car:  true
    console.log("suv.getName(): "+suv.getName());   // suv.getName():  My SUV
    console.log(SUV);   // class SUV extends Car{ ... }
```

### 9.1.4 객체 표현식 기능 확대 ###
*<b>변수명과 동일한 속성 설정 기능 추가</b>*
```js
    var property1 = "New",
        property2 = "Object literal",
        namedProperty = "Functionalities";
    // ES5
    var mergedObj = {
        // 속성명과 값이 속성값의 변수명이 같더라도 항상 같이 명시해야 한다.
        property1 : property1,
        property2 : property2,
        property3 : namedProperty 
    };
    console.log(mergedObj);

    // ES6
    var newMergedObj = {
        // 변수명을 별도로 명시하지 않아도 자동으로 속성명이 할당된다.
        property1,
        property2,
        // 속성명과 속성값을 따로 명시하면 변수명과 값을 다르게 설정할 수도 있다.
        property3: namedProperty
    };
    console.log(newMergedObj);
```
*<b>산술식을 통한 속성명 정의 기능</b>*
```js
    var i = 0,
        newComputedProperty = {
            // 기존에는 속성명을 고정된 문자열로만 정의해야 했던 객체 표현식이,
            // 이제는 계삭식 또는 문자열 조합의 결과로 설정할 수 있다.
            ["property" + ++i]: i,
            ["property" + ++i]: i,
            ["property" + ++i]: i
        };
    console.log(newComputedProperty);
    /*
        {
            property1: 1,
            property2: 2,
            property3: 3
        }
    */
    
    var j = 0,
        previousComputedProperty = {};
    previousComputedProperty["property" + ++j] = j;
    previousComputedProperty["property" + ++j] = j;
    previousComputedProperty["property" + ++j] = j;
    console.log(previousComputedProperty);
    /*
        {
            property1: 1,
            property2: 2,
            property3: 3
        }
    */
```
*<b>간단한 함수 정의와 getter, setter 정의 기능</b>*
```js
    // ES6
    var newFuncDefinition = {
        // 함수를 정의할 때, function 키워드를 생략하고 바로 함수명부터 설정 가능하다.
        func() {
            console.log("This is new definition");
        },
        _name: "Unikys",
        // get, set 속성명 앞에 붙이는 것만으로 getter와 setter를 설정 할수 있다.
        get name() {
            return this._name;
        },
        set name(name) {
            this._name = name;
        }
    };



    console.dir(newFuncDefinition);
    console.log(newFuncDefinition.name);    // getter 실행?

    newFuncDefinition.name = "es6";         // setter 실행?
    console.log(newFuncDefinition.name);

    // 프로퍼티로 할당 및 접근
    // setter, getter 왜 만드는거지??
    newFuncDefinition._name = "value"   // setter?
    console.log(newFuncDefinition._name) // getter?

    //ES5
    var previousFuncDefinition = {
        func : function() {
            console.log("This is the compatible definition");
        },
        _name: "Unikys"
    };

    Object.defineProperty(previousFuncDefinition, "name", {
        get : function () {
            return this._name;
        },
        set : function (name) {
            this._name = name;
        }
    });


    console.dir(previousFuncDefinition);
    console.log(previousFuncDefinition.name);

    previousFuncDefinition.name = "es5";
    console.log(previousFuncDefinition.name);
```
<dl>
    <dt><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty">Object.defineProperty()</a></dt>
    <dd>
        객체에 직접 새로운 속성을 정의하거나 이미 존재하는 객체를 수정한 뒤 그 객체를 반환한다.
    </dd>
</dl>

이전에는 객체가 생성되면 내부적으로 객체의 프로토타입이 `__proto__`로 숨겨진 속성으로 포현되었다. 그러나 ES6에서는 <storng>객체 표현식으로 정의할 수 있게 되었다.</strong> 이는 기존의 `Object.create()`으로 생성하던 객체와 프로토타입의 기능을 그대로 객체 표현식으로 옮겨온 것으로 생각하면 된다.

<dl>
    <dt><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create">Object.create() </a><br>
    </dt>
    <dd>
        지정된 프로토타입 객체 및 속성(property)을 갖는 새 객체를 만듭니다.
    <dd>
</dl>

*<b>__proto__ 속성 정의 기능</b>*
```js
    //ES6
    var car = {
        name: "Default",
        type: "Car",
        getName() {
            return this.name;
        }
    }

    var suvES6 = {
        __proto__: car,
        name: "My car",
        type: "SUV"
    };

    //ES5
    var suvES5 = Object.create(car, {
        name: {
            writable: true,
            configurable: true,
            value: "My car"
        },
        type: {
            writable: true,
            configurable: true,
            value: "SUV"
        }
    });

    var suvES5Other = Object,create(car);
    suvES5Other.name = "My car";
    suvES5Other.type = "SUV"; 
```
### 9.1.5 템플릿 문자열 표현식 ###
* 기존에 없었던 새로운 기능이다
* 역따옴표(<code>\`</code>)로 여닫는 것을 <code>\`템플릿 문자열 표현식`</code> 이라고 한다.
* 기존에는 문자열 중간중간에 다른 변수를 넣어야 할때 문자열을 함치는 등의 작업이 필요했지만, 템플릿 문자열 이용하면 간단하게 해결 할 수 있다.

*<b>템플릿 문자열 표현식 활용 예</b>*
```html
    <html>
        <body>
            <lable>Name :</lable>
            <input id="name" />
            <div id="greetings">
            </div>
        </body>
        <script>
            (() => {
                let inputName = document.getElementById("name"),
                    divGreetings = document.getElementById("greetings");

                inputName.addEventListener("change", () => {
                    divGreetings.innerHTML = `Hello, ${inputName.value.toUpperCase()}`;
                });
            })()
        </script>
    </html>
```
<dl>
    <dt>
        <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase">toUpperCase()</a>
    </dt>
    <dd>
        대문자로 변환된 새로운 문자열이 반환됩니다.
    </dd>
    <dt>
        <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals"> Template literals</a>
    </dt>
    <dd>
        Template literal은 내장된 표현식을 허용하는 문자열 리터럴입니다.<br>
        여러 줄로 이뤄진 문자열과 문자 보간기능을 사용할 수 있습니다.<br>
        이전 버전의 ES2015 명세 사양에서는 "template string"(템플릿 문자열) 라고 불려 왔습니다.
    </dd>
    <dd>
        <em><b>Syntax</b></em><br>
        <code>`string text`</code><br>
        <code>`string text line1<br>
               string text ling2`
        </code><br>
        <code>`string text ${expression} string text`</code><br>
        <code>tag `string test ${expression} string text`</code><br>
    </dd>
</dl>

동적 변수 문자열 할당과 여러 줄 문자열을 쉽게 지원한다. (이전 문자열을 여러 줄로 출력할 때 특수문자 "\n"으로 줄의 끝을 표기해줘야 했다.)

<em><b>템플릿 문자열을 통한 여러 줄 출력</b></em>
```html
    <html>
        <body>
            <lable>Name: </lable>
            <input id="name"/>
            <pre id="greetings"></pre>
            <pre id="greetingsES5"></pre>
        </body>
        <script>
            (() => {
                let inputName = document.getElementById("name"),
                    preGreetings = document.getElementById("greetings"),
                    preGreetingsES5 = document.getElementById("greetingsES5");

                inputName.addEventListener("change", () => {
                	preGreetings.innerHTML = `Hello,
${inputName.value.toUpperCase()}\nWelcom to ES6`;       // \n 표기하지 않고 개행만으로 보이는 대로 출력한다.
      		
      		preGreetingsES5.innerHTML = "HELLO, \n" + inputName.value.toUpperCase() + "\nI'm using ES5";    // ES5
                })
            })()
        </script>
    </html>
```

자바스크립트가 서버에서 동작하여 템플릿을 제공해야 할 때 코드상에서 보이는 것과 유사하게 프로그래밍 할 수 있어서 편리하다.

전체 문자열을 분석하거나 입력된 값에 따라서 다른 문자열이나 현재 문자열을 변경해서 처리할 수도 있다.

<em><b>템플릿 문자열의 입력 분석 예</b></em>
```html
    <script>
        function analyze(strings, ...values) {
            console.log(strings);
            console.log(values);    // 하나의 배열로 나머지 실제 값들을 받는다.

            return "Conditional string";    // 조건부로 다른 문자열을 보여주거나 특수문자 변환 등의 전처리가 필요하다면 이러한 함수 호출을 통해서 변경하면 된다.
        }

        var name = "World";
        console.log(analyze `Hello, ${name}!\nECMAScript${2*2+2} is easy`)  //  함수 뒤에 괄호를 열지 않고 바로 템플리 문자열 표현식을 사용하면 함수에 첫번째 인자는 변숫값(${...})을 기준으로 나눈 문자열들을 배열로 넘긴다.
                                                                            // 변수를 이후 인자로 넘긴다.
    </script>
```
<dl>
    <dt>
        <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters">rest parameter</a>
    </dt>
    <dd>
        나머지 매개변수(rest parameter) 구문은 정해지지 않은 수(an indefinite number, 부정수) 인수를 배열로 나타낼 수 있게 합니다.
    </dd>
    <dd>
        <em><b>Syntax</b></em><br>
        <code>
            function f(a, b, ...theArgs) {
                // ...
            }
        </code>
    </dd>
</dl>

<em><b>Strings 인자의 raw 속성</b></em>
```js
    function analyze(strings, ...values) {
        console.log(strings);
        console.log(values);    
        
        console.log(strings.raw[1] === "!\\nECMAScript");   // 첫 번째 인자인 strings의 속성으로 raw라는 배열이 인자로 넘어오는데, 이 인자는 각 문자열을 실제로 어ㄸ허게 가지고 잇는지 보여주는 순수 문자열이다.
                                                            // true

        return "Conditional string";
    }

    var name = "World";
        
    console.log(analyze `Hello, ${name}!\nECMAScript${2*2+2} is easy`)

```
<em><b>String.raw 함수의 추가</b></em>
```js
    var name = "World";
    
    String.raw `Hello, ${name}!\nECMAScript{2*2+2} is easy`     // 순수 문자열로 전체 문자열을 변경할 수 있다.
                                                                // 서버에서 템플릿 문자열을 가지고 웹페이지를 그릴 때 매우 유용하게 사용할 수 있다.
    consnole.log(String.raw `Hello, ${name}!\nECMAScript{2*2+2} is easy`);  
```