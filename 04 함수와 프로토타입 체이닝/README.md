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

    self =  self();     //  a가 출력되고 리턴값 익명함수가 저장된다.
                        //  a
    
    self();             //  리턴받은 익명함수가 실행된다.
                        //  b
```
