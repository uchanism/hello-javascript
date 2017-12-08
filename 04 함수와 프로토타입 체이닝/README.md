# 함수와 프로토타입 체이닝 #
* 함수 생성
* 함수 객체
* 다양한 함수 형태
* 함수 호출과 this
* 프로토타입과 프로토타입 체이닝

## 4.1 함수 정의 ##
함수를 생성하는 방법은 3가지가 있고, 각각의 방식에 따라 함수 동작이 차이가 있다.
* 함수 선언문
* 함수 표현식
    * 익명 함수 포현식
    * 기명 함수 표현식
* Function() 생성자 함수

### 4.1.1 함수 리터럴 ###
*함수 리터럴을 통한 add() 함수 정의*
```js
    function add(x, y) {
        return x + y;
    }
    
```
1. function 키워드 : function 키워드로 시작한다.
2. 함수명 : 함수 몸체의 내부 코드에서 자신을 재귀적으로 호출하거나<br>
             자바스크립트 디버거가 해당 함수를 구분하는 식별자로 사용된다.<br>
             **함수명은 선택 사항**이다. 함수명이 없는 함수는 익명 함수라 한다.
3. 매개변수 리스트 : 매개변수 타입을 기술하지 않는다.
4. 함수 몸체 : 실제 함수가 호출 때 실핼되는 코드 부분이다. 

### 4.1.2 함수 선언문 방식으로 함수 생성하기 ###
리터럴 형태와 같다. 주의할 점은 반드시 **함수명이 정의되어 있어야** 한다.

*예제 4-1 add()함수 생성(함수선언문 방식)*
```js
    function add(x, y) {
        return x + y;
    }

    console.log(add(3,4));  // 함수명 add를 통한 호출
```
### 4.1.3 함수 표현식 방식으로 함수 생성하기 ###
함수도 하나의 값처럼 취급된다. 따라서 함수도 변수에 할당하는 것이 가능하다.

<dl>
    <dt>
        함수 표현식
    </dt>
    <dd>
       함수 리터럴로 함수를 만들고, 여기서 생성된 함수를 변수에 할당하여 함수를 생성하는 것
    </dd>
    <dd>
        <strong>외부 코드는 함수 변수를 통해서</strong> 실제 함수를 호츨하는 것이 가능하다.
    </dd>
</dl>
    
*예제 4-2 add() 함수 생성(함수 표현식방식)*
```js
    var add = function (x, y) { // 익명함수의 참조값을 갖는 add 함수 변수
        return x + y;
    }

    var plus = add; // 익명함수의 참조값을 plus 변수에 할당
    /*
        익명 함수 표현식의 호출은 함수 변수에 함수 호출 연산자인 ()를 붙여서 호출이 가능하다.
    */
    console.log(add(3,4));      //  7
    console.log(plus(5,6));     //  11
```

*예제 4-3 기명 함수 표현식의 함수 호출 방법*
```js
    var add = function sum(x, y) {
        return x + y;
    };

    console.log(add(3,4));  // 7  
    console.log(sum(3,4));  // ReferenceError: sum is not defined
                            // 함수 표현식에서 사용된 함수 이름이 외부 코드에서 접근이 불가능 하다.
```
*예제 4-4 함수 표현식 방식으로 구현한 팩토리얼 함수*
<dl>
    <dt>
        팩토리얼
    </dt>
    <dd>
        자기 자신의 수에 자신의 수보다 1 작은 수가 1이 될때까지 곱하는 것(기호: !)<br>
        ex) 5! = 5*4*3*2*1<br>
    </dd>
</dl>

```js
    var factorialVal = function factioral(n) {
        if(n <= 1) {
                return 1;
        }
        return n *factorial(n-1);   // 함수 내부에서 이뤄지는 재귀 호출
        /*
            factorial(3) = return 3 * fatorial(2),
                fatorial(2) = return 2 * fatorial(1),
                    fatorial(1) = return 1 

            3 * 2 * 1 = 6
        */
    }

    console.log(factorialVar(3));   // 6
    console.log(factorial(3));

```
### 4.1.4 Function() 생성자 함수를 통한 함수 생성하기 ###

<dl>
    <dt>Function() 생성자</dt>
    <dd>
        기본 내장 생성자 함수
    </dd>
    <dd>
        모든 함수는 Function() 생성자 함수로 부터 생성된 객체다.
    </dd>
    <dd>
        new Function (arg, arg2, ... argN, functionBody)
    </dd>
</dl>

*예제 4-5 Function() 생성자 함수를 이용한 add()함수 생성*
```js
    var add = new Function('x', 'y', 'return x + y');
    
    console.log(add(3,4));   // 7
    
```
### 4.1.5 함수 호이스팅 ###
* 함수 생성 방법에 따라 **동작 방식이 약간 차이**가 있다.
* 함수 표현식만을 사용할 것을 권하고 있다.

| 함수 선언문 방식    | 함수 표현식 방식   |
|---|---|
| 함수가 생성된 위치와 상관없이<br>유효 범위는 **맨 처음**부터 시작한다   |  **생성된 이후** 호출이 가능하다 |

*예제4-6 함수 선언문 방식과 함수 호이스팅*
```js
    add(2,3)    //  5
                // add() 함수 정의 전 호출

    function add(x, y){
        return x +y ;
    }

    add(3,4)    //  7
```
*예제 4-7 함수 표현식 방식과 함수 호이스팅*
```js
    add(2,3);   //  add is not a function
                //  변수의 생성과 초기화가 분리되어 진행된다.

    var add = function(x, y) {
        return x + y;
    }

    console.log(add(3,4));  //   7
```
## 4.2 함수 객체 : 함수도 객체다 ##

### 4.2.1 자바스크립트에서는 함수도 객체다 ###
*예제 4-6 add() 함수도 객체처럼 **프로퍼티를 가질 수 있다.***

```js
    function add(x, y) {        //  함수 생성할 때 함수 코드는 함수 객체의 [[Code]] 내부프로퍼티에 자동으로 저장된다.
        return x + y;
    }

    add.result = add(2,3);
    add.status = 'OK';

    console.log(add.result);    //  7
    console.log(add.status);    // OK

    console.dir(add());
```

### 4.2.2 자바스크립트에서 함수는 값으로 취급된다. ###
<em>함수는 아래 나열한 기능이 모두 가능한 **일급<sub>First Class</sub> 객체** 다</em>

* 리터럴에 의해 생성
* 변수나 배열의 요소, 객체의 프로퍼티 등에 할당 가능
* 함수의 인자로 전달 가능
* 함수의 리턴값으로 리턴 가능
* 동적으로 프로퍼티를 생성 및 할당 가능

이런 특성으로 함수형 프로그래밍 가능하다.

<dl>
    <dt>
        일급 객체
    </dt>
    <dd>
        다른 객체들에 적용 가능한 <strong>연산을 모두 지원</strong>하는 객체를 가리킨다
    </dd>
</dl>

#### 4.2.2.1 변수나 프로퍼티의 값으로 할당 ####
*예제 4-9 변수나 프로퍼티에 함수값을 할당하는 코드*
```js
    var foo = 100;
    var bar = function () { retrun 100; }
    console.log(bar());     //  100

    var obj = {};
    obj.baz = function(){ retrun 200; }
    console.log(obj.baz()); // 200
```

#### 4.2.2.2 함수 인자로 전달 ####
*4-10 함수를 다른 함수의 인자로 넘긴 코드*
```js
    var foo = function(func) {
        func();
    }

    foo(function() {    // 리터널 방식으로 생선된 익명함수를 인자로 넘긴다.
        console.log('Function can be used as the argument.');
    }); //  Function can be used as the argument.
```
#### 4.2.2.3 리턴값으로 활용 ####
*예제 4-11 함수를 다른 함수의 리턴값으로 활용한 코드*
```js
    var foo = function() {
        return function () {
            console.log('this function is the return value.');
        };
    };

    var bar = foo();    // 리턴값으로 전달되는 함수가 변수에 저장 된다.
    bar();  // () 함수 호출 연산자를 이용해 bar()로 리턴된 함수를 실행 한다.
            // this function is the return value.
```
### 4.2.3 함수 객체의 기본 프로퍼티 ###

*예제 4-12 add() 함수 객체 프로퍼티를 출력하는 코드*
```js
    function add(x, y) {
        return x + y;
    }

    console.dir(add);
```
<dl>
    <dt>argumetns</dt>
    <dd>
        함수 호출 시 전달된 인자값을 나타낸다.
    </dd>
    <dt>caller</dt>
    <dd>
        자신을 호출한 함수를 나타낸다.
    </dd>
    <dt>name</dt>
    <dd>
        함수의 이름
    </dd>
    <dt>__proto__</dt>
    <dd>
        프로토타입을 가리키는 [[Prototype]]라는 내부 프로퍼티
    </dd>
</dl>
(위 프로퍼티들은 ECMA 표준 프로퍼티가 아니다)

모든 함수는 **length**와 **prototype 프로퍼티**를 가져야 한다.

<dl>
    <dt>
        Function.prototype
    </dt>
    <dd>
        ECMA 표준에 <strong>함수 객체</strong>라고 정의하고 있다.
    </dd>
    <dd>
        <strong>모든 함수의 부모 역활</strong>을 하는 프로토타입 객체다.<br> 때문에 프로퍼티나 메서드를 상속받아 그대로 사용할 수 있따.
    </dd>
    <dd>
        부모 객체는 모든 객체의 조상격인 <strong>Object.prototype 객체</strong> 다
    </dd>
</dl>

*Function.prototype의 프로퍼티*
* constructor 프로퍼티
* toString() 메서드
* **apply(thisArg, argArray) 메서드**
* **call(thisArg, [, arg1 [,arg2,]]) 메서드**
* bind(thisArg, [, arg1 [,arg2, ]]) 메서드

#### 4.2.3.1 length 프로퍼티 ####
* 모든 함수가 가져야 하는 표준 프로퍼티다.
* 정상적으로 실행 될 때 기대되는 **인자의 개수**를 나타낸다

*예제 4-13 함수 객체의 length 프로퍼티*
```js
    function func0() {

    }
    
    function func1(x) {
        return x;
    }
    
    function func2(x, y) {
        return x + y;
    }

    function func3(x, y, z) {
        return x + y + z;
    }

    console.log('func0.lenght = '+func0.length);    // func0.lenght = 0
    console.log('func1.lenght = '+func1.length);    // func1.lenght = 1
    console.log('func2.lenght = '+func2.length);    // func2.lenght = 2
    console.log('func3.lenght = '+func3.length);    // func3.lenght = 3

```

#### 4.2.3.2 prototype 프로퍼티 ####
* 함수가 생성될 때 만들어진다.
* 모든 함수는 객체로서 **prototype 프로퍼티**를 가지고 있다.
* 모든 객체의 부모를 나타내는 내부 프로퍼티인  **`[[Prototype]]`(__proto__)과 다르다.**
* **constructor 프로퍼티** 하나만 있는 프로토타입 객체를 가르킨다.

| `[[Prototype]]`(__proto__)    | prototype 프로퍼티   |
|---|---|
| 자신의 부모 역활을 하는 프로토타입 객체를 가리킨다.   |  생성자로 사용될 때 **이 함수를 통해 생성된<br> 객체의 부모 역활**을 하는 프로토타입 객체를 가리킨다. |

<dl>
    <dt>
        constructor 프로퍼티
    </dt>
    <dd>
        자신과 연결된 함수를 가르킨다.
    </dd>
</dl>

*예제 4-14 함수 객체와 프로타입 객체와의 관게를 보여주는 코드*
```js
    function myFunction() { //   함수 생성과 동시에 이 함수와 연결된 프로토타입 객체가 생성된다.
        return true;
    }

    console.dir(myFunction.prototype);                  //  myFunction() 함수의 프로토타입 객체이므로 constructor 프로퍼티가 있으며, 
                                                        //  프로토타입 객체 역시 객체이므로 __proto__ 프로퍼티가 있다.
    
    console.dir(myFunction.prototype.constructor);      //  프로토타입 객개와 매핑된 myFunction() 함수를 가리키고 있다.
```
함수 객체의 프로토타입 프로퍼티와 프로토타입 객체의 construcotr 프로퍼티는 서로 참조 한다.

이렇듯 **함수 객체**와 **프로토타입 객체**는 서로 밀접하게 연결돼 있다.

이러한 개념은 이후 프로토타입 체이닝을 이해하는 기본 지식인 만큼 잘 숙지해야 한다.

## 4.3 함수의 다양한 형태 ##

### 4.3.1 콜백 함수 ###
* 익명 함수의 대표적인 용도
* 어떤 이벤트가 발생했거나 특정 시점에 도달했을때 시스템에서 호출한다.
* 특정 함수의 인자로 넘겨서, 코드 내부에서 호출한다.
* 대표적인 예로는 이벤트 핸들러 처리가 있다.

*예제 4-15 window.onload 이벤트 핸들러 예제 코드*
```html
    <!DOCTYPE html>
    <html>
        <body>
            <script>
                window.onload = function() {    // window.onload 이벤트 핸들러에 익명 함수 연결
                    alert('This is the callback function');
                }
            </script>
        </body>
    </html>
```

### 4.3.2 즉시 실행 함수 ###
* 익명함수를 응용한 형태다.
* 정의함과 동시에 바로 실행한다.
* 다시 호출할 수 없다. 따라서 **최초 한 번의 실행만을 필요로 하는 초기화 코드 부분** 등에 사용할 수 있다.
* 리터럴을 괄호()로 둘러싸고 () 괄호 쌍을 추가한다.<br> 이때 괄호 안에 값을 추가해 즉시 실행 함수의 인자로 넘길수 있다.
* 함수 이름은 있든 없든 무관하다.
* 변수 유효 범위 특성 때문에 사용한다.
* 전역 네임스페이스를 더럽히지 않는다.
* 변수 이름 충돌 문제를 방지할 수 있다.

<dl>
    <dt>변수 유효 범위</dt>
    <dd>
        기본적으로 변수를 선언할 경우 프로그램 전체에서 접근할 수 있는 전역 유효 범위를 가지게 된다.
    </dd>
    <dd>
        함수 내부에서 정의된 매개변수와 변수들은 함수 코드 내부에서만 유효하다.<br>(여기서 변수는 <strong>var 문을 사용해서 정의</strong>해야 한다. 그렇지 않으면 함수 내의 변수라도 전역 유효범위를 갖게 된다.)<br>
        함수 외부의 코드에서 함수 내부의 변수를 액세스 불가능하다.
    </dd>
</dl>

*4-16 즉시 실행 함수 예제 코드*
```js
    (function (name) {
        console.log('This is the immediate function --> '+ name);   // This is the immediate function --> foo
    })('foo');
```

jQuery와 같은 자바스크립트 라이브러리나 프레임워크 소스들에서 사용된다.


*예제 4-17 jQuery에서 사용된 즉시 실행 함수*
```js
    (function(windw, undefined){
        //...
    }

    )(window);
```

### 4.3.3 내부 함수 ###
* 함수 내부에 정의된 함수다.
* 함수 밖에서 선언된 변수나 함수의 접근이 가능하다.
* 자신을 둘러싼 함수 외부에서 접근이 불가능하다.

*예제 4-18 내부 함수 예제 코드*
```js
    function parent() {
        var a = 100;
        var b = 200;

        function child() {
            var b = 300;

            console.log(a); //  부모함수 parent()의 변수 a에 접근  
                            //  100  
            
            console.log(b); //  300
        }

        child();
    }

    parent(); 
    child();    // child is not defined
```

부모 함수에서 내부함수를 외부로 리턴하면 부모 함수 밖에서도 내부함수를 호출하는 것이 가능하다.

*예제 4-19 함수 스코프 외부에서 내부 함수 호출하는 예제 코드*
```js
    function parent() {
        var a = 100;    // 자유변수

        var child = function () {
            console.log(a);
        }

        return child;
    }
    
    var inner = parent();   //  child 함수 변수 리턴되고 inner 변수에 내부 함수 child()를 참조한다.
    
    inner();    //  부모 함수 스코프의 변수를 참조하는 클로저
                //  100;
```

### 4.3.3 함수를 리턴하는 함수 ###
* 호출함과 동시에 다른 함수로 바꿀수 있다.
* 자기 자신을 재정의하는 함수를 구현 할 수 있다.

## 4-20 자신을 재정의하는 함수 예제 코드 ##
```js
    var self = function () {
        console.log('a');
        
        return function () {
            console.log('b');
        }
    }

    self =  self();     //  a가 출력되고 리턴된 익명함수가 저장된다.
    
    console.log(self);
    self();             //  리턴받은 익명함수가 실행된다.
                        //  b
```

## 4.4 함수 호출과 this ##
다른 언어와 달리 자유로운 함수 호출이 가능하다.

### 4.4.1 arguments 객체 ###
함수 형식에 맞춰 인자를 넘기지 않더라도 에러가 발생하지 않는다.

*예제 4-21 함수 형석에 맞춰 인자를 넘기지 않더라도 함수 호출이 가능함을 나타내는 예제 코드*
```js
    function func(arg1, arg2) {
        console.log(arg1, arg2);
    }
    func();         //  넘기지 않은 인자는 undefined 값이 할당된다.
                    //  undefined undefined
    func(1);        //  1 undefined
    
    func(1,2);      //  1 2
    
    func(1,2,3);    //  초과된 인수는 무시된다.
                    //  1 2 
```

<dl>
    <dt>
        arguments 객체
    </dt>
    <dd>
        함수 호출 시 배열 형태의 인자들과 함께 함수 내부로 전달된다.
    </dd>
    <dd>
        <strong>유사 배열 객체</strong> 다.
    </dd>
</dl>

*예제 4-22 arguments 객체 예제 코드*
```js
    function add(a, b) {
        console.dir(arguments);
        return a+b;
    }

    console.log(add(1));        //  NaN
    console.log(add(1,2));      //  3
    console.log(add(1,2,3));    //  3

```
*arguments 객체 구성(__prpto__프로퍼티는 제외)*
* 함수 호출 시 넘겨진 인자 (배열 형태) : 첫 번째 인자는 인덱스 0, 두 번째 인덱스 2 ...
* lenght 프로퍼티 : 인자의 개수
* callee 프로퍼티 : 현재 실행 중인 함수의 참조값(예제에서는 add() 함수)

*arguments 객체를 사용하여 인자들에 배열 형태로 접근*
```js
    function sum() {
        var result = 0;

        for(var i=0; i < argumetns.length; i++) {
            result += arguments[i];
        }

        return reselt;
    }

    console.log(sum(1,2,3));    //  6
```

### 4.4.2 호출 패턴과 this 바인딩 ###
* 함수 호출 시 **this 인자**가 함수 내부로 암묵적으로 전달된다.
* <strong>함수가 호출되는 방식(호출패턴)</strong>에 따라 this가 다른 <strong>객체를 참조(this 바인딩)</strong>한다.

#### 4.4.2.1 객체의 메서드 호출할 때 this 바인딩 ####
메서드 내부 코드에서 사용된 this는 **해당 메서드를 호출한 객체로 바인딩** 된다.

*예제 4.23 메서드 호출 사용 시this 바인딩*
```js
    var myObject = {
        name: 'foo',
        sayName: function() {
            console.log(this.name);
        }
    };

    var otherObject = {
        name: 'bar'
    };


    otherOjbect.sayName = myObject.sayName;

    //  this는 자신을 호출한 객체에 바인딩된다.
    myObject.sayName();     //  foo
    otherObject.sayName();  //  bar
```

#### 4.4.2.2 함수를 호출할 때 this 바인딩 ####
* 함수 내부 코드에서 사용된 **this는 전역 객체에 바인딩**된다.
* 브라우저에서 실행 시 전역 객체는 window 객체가 된다.
* 내부 함수의 this는 전역 객체에 바인딩 된다.


<em id="4-24">예제 4-24 전역 객체와 전역 변수의 관계를 보여주는 예제 코드</em>
```js
    var foo = "I'm foo";

    console.log(foo);           //  I'm foo

    // 모든 전역 변수는 실제로 전역 객체의 프로퍼티들이다.
    console.log(window.foo)      //  I'm foo
    console.log(this.foo)        //  I'm foo

    // Node js Test
    console.log(global.foo);    //  undefined
    console.log(this.foo);      //  undefined

    global.foo = "i'm nodeJS foo";
    console.log(global.foo);    //  i'm nodeJS foo
    console.log(this.foo);      //  undefined

    console.dir(global);

    console.log(foo);   //  I'm foo
```

*예제 4-25 함수를 호출할 때 this 바인딩을 보여주는 예제코드*
```js
    var test = 'This is test';
    console.log(window.test);

    var sayFoo = function () {
        console.log(this.test);     // this는 전역객체에 바인딩된다.
    }

    sayFoo();       // This is test
```

*예제 4-26 내부 함수의 this바인딩 동작을 보여주는 예제코드*
```js
    var value = 100;

    var myObject = {
        value: 1,
        func1: function () {
            this.value += 1;
            console.log('func1() called. this.value : '+ this.value);  //   func1() called. this.value : 2

            func2 = function () {
                this.value += 1;
                console.log('func2() calles. this.value : '+ this.value); //  func1() called. this.value : 101

                func3 = function () {
                    this.value += 1;
                    console.log('func3() calles. this.value : '+ this.value); //  func1() called. this.value : 102
                }

                func3();
            }
            
            func2();
        }
    };

    myObject.func1();   // this는 myObject(자신을 호출한 객체)가 된다.

```
*예제 4-27 내부 함수의 this 바인딩 문제를 해결한 예제 코드*
```js
    var value = 100;

    var myObject = {
        value: 1,
        func1: function () {
            var that = this;    //  관례상 this값을 저장한느 변수의 이름을 that 이라고 짓는다.
            
            소ㅑㄴ.value += 1;
            console.log('func1() called. this.value : '+ this.value);  //   func1() called. that.value : 2

            func2 = function () {
                that.value += 1;
                console.log('func2() calles. this.value : '+ that.value); //  func1() called. that.value : 3

                func3 = function () {
                    that.value += 1;
                    console.log('func3() calles. this.value : '+ that.value); //  func1() called. that.value : 4
                }

                func3();
            }
            
            func2();
        }
    };

    myObject.func1(); 
```

#### 4.4.2.3 생성자 함수를 호출할 때 this 바인딩 ####
* 기존 함수에 new 연산자를 붙여 호출하면 생성자 함수로 동작한다.
* 함수 이름의 첫 문자를 대문자로 쓰기를 권하고 있다.

*생성자 함수가 동작하는 방식*
1. <em>**빈 객체 생성 및 this바인딩**</em><br>
생성자 함수로 생성된 빈 객체는 this로 바인딩된다.<br>
빈 객체는 자신을 생성한 **생성자 함수의 prototype 프로퍼티**가 가리키는 객체를 **자신의 프로타입 객체**로 설정한다.
2. <em>**this를 통한 프로퍼티 생성**</em><br>
함수 코드 내부에서 this를 사용해서, 생성된 빈 객체에 동적으로 프로퍼티나 메서드를 생성할 수 있다.
3. <em>**생성된 객체 리턴**</em><br>
일반적인 경우로 특별하게 리턴문이 없을 경우, **this로 바인딩된 새로 생성한 객체가 리턴**된다.<br>(생성자 함수가 아닌 일반 함수를 호출할 때 리턴값이 명시되어 있지 않으면 undefined가 리턴된다.)<br>
리턴값이 다른 객체를 반환하는 경우 생성자 함수를 호출하더라도<br> this(생성한 객체)가 아닌 해당 객체가 리턴된다.

*예제 4-28 생성자 함수의 동작 방식*
```js
    var Person = function (name) {
    /*
        1. 함수 코드 실행 전
            Person() 생성자 함수의 prototype 프로퍼티가 가리키는 
            Person.prototype 객체를 [[Prototype]] 링크로 연결해서 
            자신의 프로토타입으로 설정한 빈 객체를 생성하고
            생성자 함수 코드에서 사용되는 this로 바인딩 된다.
    */
        this.name = name;   //  2. this가 가리키는 빈 객체에 name 프로퍼티를 동적 생성
        
        // 3. 리턴값이 특별히 없으므로 this로 바인딩한 빈 객체가 리턴값으로 반환돼서 foo 변수에 저장된다.
    }

    var foo = new Person('foo');

    console.log(foo.name);      // foo
```

*객체 리터럴 방식과 생성자 함수를 통한 객체 생성 방식의 차이*
<table>
    <thead>
        <tr>
            <th>
                구분
            </th>
            <th>
                리터럴
            </th>
            <th>
                생성자
            </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>
                재생성 여부
            </th>
            <td>
                불가능
            </td>
            <td>
                가능
            </td>
        </tr>
        <tr>
            <th>
                프로토타입 객체<br>(__proto__ 프로퍼티)
            </th>
            <td>
                Object(Object.prototype)    
            </td>
            <td>
                Person(Person.prototype)    
            </td>
        </tr>
        <tr>
            <th>
                객체 생성자 함수
            </th>
            <td>
                Object()    
            </td>
            <td>
                생성자 함수 자체(Person())
            </td>
        </tr>
    </tbody>
</table>

*예제 4-29 객체 생성 두가지방법(객체 리터럴 vs 생성자 함수)*
```js
    var foo = {
        name: 'foo',
        age: 35,
        gender: 'man'
    };

    console.dir(foo);

    function Person(name, age, gender, position) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    var bar = new Person('bar', 33, 'woman');
    console.dir(bar);

    var baz = new Person('baz', 25, 'woman');
    console.dir(baz);

```

*생성자 함수를 new를 붙이지 않고 호출할 경우*
일반 함수 호출과 생성자 함수를 호출할 때 this 바인딩 방식이 다르기 때문에<br>
객체 생성을 목적으로 작성한 생성자 함수를 new 없이 호출하거나 일반 함수를 new를 붙여서 호출할 경우 코드에서 오류가 발생 할수 있다.

*예제 4-30 new를 붙이지 않고 생성자 함수 호출 시의 오류*
```js
    var qux = Person('qux', 20, 'man'); //  생성자함수를 new 없이 호출
                                        //  this는 전역 객체로 바인딩 되고 의도와는 다르게 전역객체에 동적으로 프로퍼티가 생성된다.
    
    console.log(qux);                   //  일반 함수를 호출할 때는 undefined가 리턴된다.
                                        //  undefined
    
    console.log(window.name);   //  qux
    console.log(window.age);    //  20
    console.log(window.gender); //  man

```
일반 함수와 생성자 함수의 구분이 없으므로, 생성자 함수로 사용하는 첫글자를 대문자로 표기하는 네이밍 규칙을 권장한다.<br>
그러나 결국 new를 사용해서 호출 하지 않을 경우 코드의 에러가 발생한다.

*강제로 인스턴스 생성하기*
```js
    function A (arg) {
        if (!(this instanceof A)) {     // new로 호출이 안됬을 경우
            return new A(arg);
        }
    /*   
        if (!(this instanceof aruments.callee))

        arguments.callee가 곧 호출된 함수를 가리킨다.
        이와 같이 하면, 특정 함수 이름과 상관없이 
        이 패턴을 공통을 공통으로 사용하는 모듈을 작성할 수 있는 장점이 있다.
    */
        this.value = arg ? arg : 0;
    }

    var a = new A(100);
    var b = A(10);

    console.log(a.value);       //  100
    console.log(b.value);       //  10
    console.log(global.value);  //  undefined
```

#### 4.4.2.4 call과 apply 메서드를 이용한 명시적인 this 바인딩 ####
* this를 특정 객체에 명시적으로 바인딩시킨다.
* 모든 함수의 부모 객체인 Function.prototype 객체의 메서드다.
* 호출하는 주체가 함수고, this를 특정 객체에 바인딩 할 뿐 결국 본질적인 기능은 **함수호출**이다.
* this를 원하는 값으로 명시적으로 매핑해서 특정 함수나 메서드를 호출 할 수 있는 장점이 있다.

모든 함수는 다음과 같은 형식으로 메서드를 호출하는 것이 가능하다.
```js
    function.apply(thisArg, argArray);
```
<dl>
    <dt>
        thisArs
    </dt>
    <dd>
        apply() 메서드를 호출한 함수 내부에서 사용한 this에 바인딩할 객체를 가리킨다.
    </dd>
    <dd>
        this로 명시적으로 바인딩 된다.
    </dd>
    <dt>
        argArray
    </dt>
    <dd>
        함수를 호출할때 넘길 인자들의 배열을 가리킨다.
    </dd>
    <dd>
        호출한 함수의 인자로 사용된다.
    </dd>
</dl>

*예제 4-31 apply() 메서드를 이용한 명시적인 this 바인딩*
```js
    function Person(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    var foo = {};

    Person.apply(foo, ['foo', 30, 'man']);  // Person('foo', 30, 'man') 함수를 호출 하면서, this를 foo객체에 명시적으로 바인딩한다.

    console.dir(foo);   // { name: 'foo', age: 30, gender: 'man' }
```

call() 메서드의 경우는 기능은 같지만, 두 번째 인자에서 각각 하나의 인자로 넘긴다.
```js
    Person.call(foo, 'foo',30,'man');
```
*예제 4-32 apply() 메서드를 활용한 arguments 객체의 배열 표준 메서드 slice() 활용 코드*
```js
    function myFunction() {
        console.dir(arguments); // __proto__ : Object.prototype
    
        arguments.shift();      //  유사 배열 객체이므로 shift() 메서드가 없다. 
                                //TypeError: arguments.shift is not a function

        var args= Array.prototype.slice.apply(arguments);   
        //  Array.prototype.sice() 메서드를 호출해라. 이때 this는 arguments 객체로 바인딩해라.
        // arguments.slice()와 같은 형태로 메서드 호출
        // arguments 객체를 복사한 새로운 배열을 생성하여 리턴한다.
        
        console.dir(args);  // __proto__ : Array.prototype
    }

    myFunction(1,2,3);
```
<dl>
    <dt>
        <a target="_blank" href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice">slice() 메소드</a>
    </dt>
    <dd>
        <code>arr.slice() arr.slice(begin) arr.slice(begin, end)</code>
    </dd>
    <dd>
        slice() 메소드는 어떤 배열의 begin부터 end까지(end는 불포함)에 대한 shallow copy를 새로운 배열 객체로 반환합니다.<br>원본 배열은 수정되지 않습니다.
    </dd>
    <dd>
        <dl>
            <dt>
                begin (Optiona)
            </dt>
            <dd>
               0을 시작으로 하는 추출 시작점에 대한 인덱스를 의미합니다.
            </dd>
            <dd>
                begin 이 undefined인 경우에는, 0번 인덱스부터 slice 합니다.
            </dd>
            <dt>
                end (Optiona)
            </dt>
            <dd>
                추출을 종료 할 0 기준 인덱스입니다. end 인덱스를 제외하고 추출합니다.
            </dd>
            <dd>
                end가 생략되면 slice는 배열의 끝까지(arr.length) 추출합니다.
            </dd>
        <dl>
    </dd>
</dl>

*예제 4-33 slice() 메서드 사용 예제*
```js
    var arrA = [1,2,3];    
    var arrB = arrA.slice(0);   //  [1]
    var arrC = arrA.slice();    //  [1,2,3]
    var arrD = arrA.slice(1);   //  [1,2]  
    var arrE = arrA.slice(1,2); //  [2]  
```

#### 4.4.3.1 규칙 1) 일반 함수나 메서드는 리턴값을 지정하지 않을 경우, undefined 값이 리턴된다. ####
*예저 4-34 return 문 없는 일반 함수의 리턴값 확인*
```js
    var noReturnFunc = function () {
        console.log('This function has no return statement');
    }

    var result = noReturnFunc();

    console.log(result);    //  This function has no return statement
                            //  undefined

    // 메서드 리턴값  Test
    Function.prototype.method = function(name, func) {
        this.prototype[name] = func;
    }

    function NoReturn() {};

    NoReturn.method('method', function () {
        console.log('This method has no return statement');
    });

    var noReturn = new NoReturn();

    console.log(noReturn.method());     //  This method has no return statement
                                        //  undefined

```

#### 4.4.3.2 규칙 2) 생성자 함수에서 리턴값을 지정하지 않을 경우 생성된 객체가 리턴된다.####
* 생성자 함수에서 별도의 리턴값을 지정하면 지정된 객체나 배열이 리턴된다
* 생성자 함수에 지정한 별도의 리턴값이 객체가 아닌 불린,숫자,문자열의 경우<<br> 이러한 리턴값을 무시하고 this로 바인딩된 객체가 리턴된다.

*예제 4-35 생성자 함수에서 명시적으로 객체를 리턴했을 경우*
```js
    function Person(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gentder = gender;

        return {name: 'bar', arg:20, gender:'woman'};
    }

    var foo = new Person('foo', 30, 'man'); // 새로 생성되는 foo 객체가 아니라, 리턴값에서 명시적으로 넘긴 객체나 배열이 리턴된다
    
    console.dir(foo);   
```

*예제 4-38 생성자 함수에서 명시적으로 기본타입(불린, 숫자, 문자열) 값을 리턴했을 경우*
```js
    function Person(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gentder = gender;

        return 100;
    }

    var foo = new Person('foo', 30, 'man');
    console.log(foo);   //  Person {name;"foo", age:30, gender:"man"}

```
<table>
    <caption style="font-size:24px;font-weight:bold;text-align:left;">ISSUE LIST</caption>
    <colgroup>
        <col style="width:10%">
        <col style="width:45%">
    </colgroup>
    <thead>
        <tr>
            <th>
                페이지
            </th>
            <th>
                내용
            </th>
             <th>
                풀이
            </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
               103 ~ 104
            </td>
            <td>
                <a href="#4-24">
                    4.4.2.2 함수를 호출할 때 this 바인딩<br>
                    .....<br>
                    전역 객체란 무엇인가?? (브라우저, Node.js)<br>
                    .....<br>
                    <storng>자바스크립트의 모든 전역 변수는 실제로는 이러한 전역 객체의 프로퍼티 들이다</strong>
                </a>
            </td>
            <td>
            </td>
        </tr>
    </tbody>
</table>