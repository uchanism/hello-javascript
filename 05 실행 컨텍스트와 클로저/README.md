# 실행 컨텍스트와 클로저 #
자바스크립트가 실행될 때 생성되는 하나의 실행 단위

# 5.1 실행 컨텍스트 개념 #
* 실행 가능한 코드 블록이 실행되는 환경
* 실행에 필요한 여러 가지 정보를 담고 있다.
* 실행 컨텍스트가 형성되는 경우 세 가지
    * 전역 코드
    * eval() 함수로 실행되는 코드
    * 함수안에 코드 실행

*예제 5-1*
```js
    console.log("This is global context");

    function ExContext1() {
        console.log("This is ExContext1");
    };

    function ExContext2() {
        ExContext1();
        console.log("This is ExContext2");
    }

    ExContext2();       //  This is global context 
                        //  This is ExContext1
                        //  This is ExContext2
```

## 5.2 실행 컨텍스트 생성 과정 ##
* 활성 객체와 변수 객체
* 스코프 체인

*예제 5-2*
```js
    function execute(param1, param2) {
        var a = 1, b = 2;
        function func() {
            return a+b;
        }
        return param1 + param2 + func();
    }

    execute(3,4);

    /*  
        실행 컨텍스트 스택

        1. func 실행 컨텍스트 (최상위)
        2. execute 컨텍스트 
        3. 전역 컨텍스트
    */
```

### 5.2.1 활성 객체 생성 ###
실행 컨텍스트가 생성되면 해당 컨텍스트에서 실행에 필요한 여러 가지 정보를 담을 객체를 생성하는데, 이를 활성 객체라고 한다.

*여러 가지 정보*
* 매개변수
* 사용자가 정의한 변수 및 객체

### 5.2.2 arguments 객체 생성 ###
활성 객체는 arguments 프로퍼티로 arguments 객체를 참조한다.

### 5.2.3. 스코프 정보 생성 ###
* 현재 컨텍스트의 유효 범위 나타내는 스코프 정보를 생성한다.
* 실행 중인 컨텍스트 안에서 연결 리스트와 유사한 형식으로 만들어진다.
* 해당 컨텍스트에서 특정 변수에 접근해야 할 경우, 이 리스트를 활용한다.
* 스코프 체인이라고 하며 [[scope]] 프로퍼티로 참조된다.


### 5.2.4 변수 생성 ###
* 앞서 생성된 활성 객체는 변수 객체와 명명만 틀릴 뿐 같은 객체이다.
* 호출된 함수 인자는 각각의 프로퍼티가 만들어지고 그 값이 할당된다.
* 함수 안에 정의된 변수와 내부함수가 생성된다. **(메모리에 생성하고, 초기화는 각 변수나 함수에 해당 표현식이 실행되기 전까지 이루어지지 않는다.)**

### 5.2.5 this 바인딩 ###
this 키워드를 사용하는 값이 할당된다. 여기서 this가 참조하는 객체가 없으면 전역 객체를 참조한다.

### 5.2.6 코드 실행 ###
변수 객체가 만들어진 후에, 코드에 있는 여러 가지 표현식 실행이 이루어진다.

<dl>
    <dt>
        전역 실행 컨텍스트
    </dt>
    <dd>
        arguments 객체가 없다.
    </dd>
    <dd>
        전역 객체 하나만 포함하는 스코프 체인이 있다.
    </dd>
    <dd>
        전역 코드가 실행될 때 생성된다.
    </dd>
    <dd>
        변수를 초기화하고 이것의 내부 함수는 일반적인 탑 레벨의 함수로 선언된다.
    </dd>
    <dd>
        변수 객체가 곧 전역 객체다
    </dd>
</dl>

<em><strong>Node.js에서 최상위 코드는 전역 코드가 아니다</strong></em><br>
일반적으로 자바스크립트 파일, 이를테면 filename.js가 하나의 모듈로 동작하고 이 파일의 최상위에 변수를 선언해도 **그 모듈의 지역 변수**가 된다.<br> 하지만 var를 사용하지 않을 경우 전역 객체인 global에 들어가고, 이는 전역 객체를 오염시키는 원인이 되므로 주의해야 한다.
```js
    var a = 10;
    b = 15;

    console.log(global.a);  //  undefined
    console.log(global.b);  //  15
```
## 5.3 스코프 체인 ##
* 유효 범위를 나타낸다.
* 현재 사용되는 변수가 어디에 선언된 변수인지 정확히 알 수 있다.
* [[scope]] 프로퍼티로 각 함수 객체 내에서 연결 리스트 형식으로 관리된다.
* 각 실행 컨텍스트의 변수 객체가 구성요소인 리스트와 같다.
* 오직 **함수만이 유효 범위의 한 단위**가 된다.
* 각각의 함수는 [[scope]] 프로퍼티로 **자신이 생성된 실행 컨텍스트의 스코프 체인을 참조**한다.
* **실행된 함수의 [[scope]] 프로퍼티를 기반**으로 새로운 스코프 체인을 만든다.


### 5.3.1 전역 실행 컨텍스트의 스코프 체인 ###
*예제 5-3*
```js
    //  1. 전역 컨텍스트 생성
    //  2. 변수 객체 생성
    //  3. 변수 객체 자신을 가리키는 스코프 체인 생성
    //  4. var1,2 생성 
    //  5. 전역 객체 this 바인딩
    var var1 = 1;
    var var2 = 2;

    console.log(var1);  //  1
    console.log(var2);  //  2
```

### 5.3.2 함수를 호출한 경우 생성되는 실행 컨텍스트의 스코프 체인 ###
*예제 5-4*
```js
    var var1 = 1;
    var var2 = 2;
    
    function func() {
        var var1 = 10;
        var var2 - 20;
        console.log(var1);  //  10
        console.log(var2);  //  20
    }

    func();

    console.log(var1);  //  1
    console.log(var2);  //  2
    
    /*
        1. 전역 실행 컨텍스트 생성
        2. 변수 객체 생성
        3. 변수 객체 자신을 가리키는 스코프 체인 생성
        4. 변수 var1, var2, 함수 func 생성
        5. 전역 객체 this 바인딩
        6. 함수 func 실행 컨텍스트 생성
        7. 변수 객체 생성
        8. 0 전역 변수 객체, 1 func 변수 객체를 가리키는 스코프 체인 생성
        9. 변수 var1, var2 생성 
       10. 전역 객체 this 바인딩
    */
```
<dl>
    <dt>
        스코프 체인 생성법
    </dt>
    <dd>
        스코크 체인 = 현재 실행 컨텍스트의 변수 객체 + 상위 컨텍스트의 스코프 체인
    </dd>
    <dd>
        현재 실행되는 함수 객체의 [[scope]] 프로퍼티를 복사하고, 새롭게 생성된 변수 객체를 해당 체인 <strong>제일 앞에</strong> 추가한다.
    </dd>
</dl>

*예제 5-5*
```js
    var value = "value1";

    function printFunc() {
        var value = "value2";

        function printValue() {
            return value;
        }

        console.log(printValue());
    }
    
    printFunc();    //  vluae2
    
    /*
        1. 전역 실행 컨텍스트 생성
        2. 변수 객체 생성
        3. 변수 객체 자신을 가리키는 스코프 체인 생성
        4. 변수 value, 함수 printFunc 생성
        5. 전역 객체 this 바인딩
        6. printFunc 실행 컨텍스트 생성
        7. printFunc 변수 객체 생성
        8. 0 전역 변수 객체, 1 printFunc 변수 객체를 가리키는 스코프 체인 생성
        9. 변수 value, 함수 printValue 생성
       10. printValue 실행 컨텍스트 생성
       11. printValue 변수 객체 생성.
       12. 0 전역 변수 객체, 1 printFunc 변수 객체 2 printValue 변수 객체를 가리키는 스코프 체인 생성
    */
```
*예제 5-6*
```js
    var value = "value1";

    function printValue() {     //  0 전역 변수 객체, 1 printValue 변수 객체를 가리키는 스코프 체인 생성
        return value;           //  (전역 실행 컨텍스트에서 생성된 함수 객체이므로 위와 같이 스코프 체인이 생성된다)
                                //  *각각의 함수는 [[scope]] 프로퍼티로 **자신이 생성된 실행 컨텍스트의 스코프 체인을 참조**한다.
    }

    function printFunc(func) {
        var value = "value2";
        console.log(func());
    }
    printFunc(printValue);      // value1

    /*
        1. 전역 실행 컨텍스트 생성
        2. 변수 객체 생성
        3. 변수 객체 자신을 가리키는 스코프 체인 생성
        4. 변수 value, 함수 printValue, printFunc 생성
        5. printFunc 실행 컨텍스트 생성
        6. printFunc 변수 객체 생성
        7. arguments 객체 생성
        8. 0 전역 변수 객체, 1 printFunc 변수 객체를 가리키는 스코프 체인 생성
        9. 변수 value 생성, 인자로 들어온 func() 호출 
       10. printValue 실행 컨텍스트 생성
       11. printValue 변수 객체 생성
       12. 0 전역 변수 객체, 1 printValue 변수 객체를 가리키는 스코프 체인 생성
    */
```

*스코프 체인을 임의로 수정가능한 키워드 with*
* 성능을 높이고자 지양한다.
* 표현식을 실행한다.
* 현재 실행 컨텍스트의  스코프 체인에 추가된다(활성화 객체의 바로 앞에)
* 다른 구문(블록 구문일 수도 있음)을 실행하고 실행 컨텍스트의 스코프 체인을 전에 있던 곳에 저장한다.
* 함수 선언은 with 구문의 영향을 받지 않는다.
* 함수 표현식은 안에서 with 구문과 함께 실행 될수 있다.

```js
    var y = { x:5 };

    function withExamFunc() {
        var x = 10;
        var z;

        with(y) {                   // with로 인해 스코프 체인 제일 앞에 y 객체가 추가된다.
            z = function() {
                console.log(x);     // 5
            }
        }
    }

    withExamFunc();
    /*
        1. 전역 실행 컨텍스트 생성
        2. 전역 변수 객체 생성
        3. 전역 변수 객체 자신의 가리키는 스코프 체인 생성
        4. 변수 y, 함수 withExamFunc 생성
        5. withExamFunc 실행 컨텍스트 생성
        6. withExamFunc 변수 객체 생성
        7. 0 전역 변수 객체, 1 withExamFunc 변수 객체를 가리키는 스코프 체인 생성
        8. 변수 x, 함수 변수 z 생성
        9. z 실행 컨텍스트 생성
       10. z 변수 객체 생성
       11. 0 전역 변수 객체, 1 withExamFunc 변수 객체, 2 z 변수 객체, 3 y 객체를 가리키는 스코프 체인 생성
    */
``` 
<em><strong>자바스크립트 실행 과정</strong></em>
1. 실행 컨텍스트 생성
2. 변수 객체 생성
3. arguments 객체 생성
4. 스코프 체인 생성
5. 함수안에 정의된 변수 및 함수 생성
6. this 바인딩

## 5.4 클로저 ##
### 5.4.1 클로저의 개념 ###
이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수를 클로저라고 한다.

*예제 5-7*
```js
    function outerFunc() {
        var x = 10;         // x : 자유 변수
        var innerFunc = function() { console.log(x); }
        return innerFunc;   // innerFunc : 클로저
    }
    
    var inner = outerFunc();
    inner();    //  10;
    
    /*
        1. 전역 실행 컨텍스트 생성
        2. 변수 객체 생성
        3. 0 전역 변수 객체를 가리키는 스코프 체인 생성
        4. 함수 outerFunc, 변수 inner 생성
        5. outerFunc 실행 컨텍스트 생성
        6. outerFunc 변수 객체 생성
        7. 0 전역 변수 객체, 1 outerFunc 변수 객체를 가리키는 스코프 체인 생성
        8. 변수 x, 함수 변수 innerFunc 생성
        9. innerFunc 실행 컨텍스트 생성
       10. innerFunc 변수 객체 생성
       11. 0 전역 변수 객체, 1 outerFunc 변수 객체, 2 innerFunc 변수 객체를 가리키는 스코프 체인 생성
    */
```
*예제 5-8*
```js
    function outerFunc(arg1, arg2){
        var local = 8;
        function innerFunc(innerArg){
            console.log((arg1 + arg2)/(innerArg + local));
            // 2 + 4/2 + 8 
        }
        return innerFunc;
    }

    var exam1 = outerFunc(2, 4);
    exam1(2);   // 0.6

    /*
    
    */
```

### 5.4.2 클로저의 활용 ###
클로저는 성능적인 면과 자원적인 면에서 약간 손해를 볼 수 있으므로 무차별적으로 사용해서는 안된다.

#### 5.4.2.1 특정 함수에 사용자가 정의한 객체의 메서드 연결하기 ####
*예제 5-9*
```js
    function HelloFunc(func) {
        this.greeting = 'hello';
    }

    HelloFunc.prototype.call = function(func) {
        func ? func(this.greeting) : this.func(this.greeting);
    }

    var userFunc = function(greeting) {
        console.log(greeting);
    }

    var objHello = new HelloFunc();
    objHello.func = userFunc;
    objHello.call();        //  hello                
```

*예제 5-10*
```js
    /* 예제 5-9 START */
    function HelloFunc(func) {
        this.greeting = 'hello';
    }

    HelloFunc.prototype.call = function(func) {
        func ? func(this.greeting) : this.func(this.greeting);
    }

    var userFunc = function (greeting) {
        console.log(greeting);
    }

    var objHello = new HelloFunc();
    objHello.func = userFunc;
    objHello.call();        //  hello  
    /* 예제 5-9  END */

    function saySomething(obj, methodName, name) {
        return (function(greeting) {                    //  클로저 : 익명 함수
            return obj[methodName](greeting, name);     //  자유변수 : obj, methodName, name 
        });
    }

    function newObj(obj, name) {
        obj.func = saySomething(this, "who", name);
        return obj;
    }

    newObj.prototype.who = function(greeting, name) {
        console.log(greeting + " " + (name || "everyown") );
    }

    var obj1 = new newObj(objHello, "zzoon");

    /*  var obj1 = new newObj(objHello, "zzoon"); 실행 과정

        1. objHello.func = saySomething(newObj, "who", "zzoon");
        2. objHello.func = (function(greeting) { return  obj[methodName](greeting, name);});
        3. return obj1 = objHello;
    */


    obj1.call();     //  hello zzoon
    
    /*  obj1.call() 실행 과정

        1. function(func) {
                func ? func(this.greeting) : obj1.func(obj1.greeting);
           }
        2. function(greeting) { 
                return  obj[methodName](greeting, name)
           };
        3. return newObj["who"](obj1.greeting, "zzoon");
    */  
```

*인자 event 외의 원하는 인자를 더 추가한 이벤트 핸들러 만들기*
```html
    <!DOCTYPE html>
    <html>
        <head>
            <title></title>
        </head>
        <body>
            <button type="button">click!</button>    
        </body>
        <script type="text/javascript">
            (function() {
                var btn = document.getElementByTagName('button');

                btn.onclick = showAlert;
                
                function showAlert(event) {
                    alert("btn click");
                }
            })();
        </script>
    </html>
```