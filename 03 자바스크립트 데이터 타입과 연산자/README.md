# 자바스크립트 데이터 타입과 연산자 #
## 자바스크립트의 데이터 타입 ##
* 기본 타입
    * 숫자(Number)
    * 문자열(String)
    * 불린값(Boolean)
    * undefined
    * null
* 참조 타입
    * 객체(Object)
        * 배열(Array)
        * 함수(Function)
        * 정규표현식

## 3.1 자바스크립트 기본 타입 ##
* 숫자
* 문자열
* 불린값
* null
* undefined
<dl>
    <dt>특징</dt>
    <dd>자체가 하나의 값을 나타낸다.<dd>
</dl>

*예제 3-1 자바스크립트 기본 타입*
```js
    // 숫자 타입
    var intNum = 10;
    var floatNum = 0.1;
    
    // 문자열 타입
    var singleQuoteStr = 'single quote string';
    var dobuleQuoteStr = "double quote string";
    var singleChar = 'a';

    //불린 타입
    var boolVar = true;

    // undefined 타입
    var emptyVal;

    // null 타입
    var nullVar = null;

    console.log(
        typeof intNum, // number
        typeof floatNum, // number
        typeof singleQuoteStr, // string 
        typeof singleChar, // string
        typeof boolVar, // boolean
        typeof emptyVal, // undefined 
        typeof nullVar // object
    );
```
### 3.1.1  숫자 ###
* 하나의 숫자형(number)만 존재한다.
* 정수형이 따로 없고, 모든 숫자를 실수로 처리한다.


* 예제 3-2 자바스크립트 나눗셈 연산 *
```js
    var num = 5 /2;

    console.log(num); // 2.5
    console.log(Math.floor(num)); // 2 (정수형)

```

<dl>
    <dt>
        Math.floor(x)
    <dt>
    <dd>
        함수는 주어진 숫자보다 작거나 같은 정수중에서 가장 큰 수를 반환합니다.
    </dd>
    <dd>
        <code>
            console.log(Math.floor(1.5)); // 1 <br>
            console.log(Math.floor(-1.5)); // -2
        </code>
    </dd>
</dl>

### 3.1.2 문자열 ###
* string
* 작은 따옴표(')sk 큰 따옴표("")로 생성한다.
* **한 번 정의된 문자열은 변하지 않는다.**

* 예제 3-3 자바스크립트 문자열 예제 *

```js
    var str = 'test'
    console.log(str[0], str[1], str[2], str[3]) // test

    str[0] = 'T'; // 첫번째 문자열  대문자로 변경
    console.log(str) 
    // 출력값 test
    // 한번 생성단 문자열은 읽기만 가능하지 수정이 불가능히다. 
```

### 3.1.3 불린값 ###
* boolean
* true와 false 값을 나타내는 타입 

### 3.1.4 null과 undefined ###
* '값이 비어있음' 나타낸다.
* 값이 할당되지 않는 변수는 undefined 타입이며, 값 또한 undefined이다.
* **undefined는 타입이자, 값을 나타난다**
* null 타입의 변수의 경우는 명시적으로 값이 비어있을을 나타내는 데 사용한다. 

*예제 3-4 null 타입 변수 체크*

```js
    /*
        null 타입 변수인지를 확인할 때 typeof 연산자가 아닌
        일치 연산자(===)를 사용해서 변수의 값을 직접 확인해야 한다.
    */
    var nullVar = null

    console.log(typeof nullVar === null); // false
    console.log(nullVal === null);; // treu

```
<dl>
    <dt>
        null
    </dt>
    <dd>
        null 은 null 또는 빈 값의 JavaScript 리터럴 표현이다.<br>
        즉, 객체 값이 존재하지 않는다는 것을 의미한다.<br> 
        또한, <strong>JavaScript 의 원시 값 들 중의 하나이다.</strong>
    </dd>
    <dd>
        ECMAScript의 버그 <code>typeof null // object</code><br> 
        때문에 null 타입변수 인지 확인할 때 일치 연산자(===)를 사용한다.
    </dd>
<dl>

## 3.2 자바스크립트 참조 타입(객체 타입) ##
* 배열, 함수, 정규표현식 등도 객체다
* '이름(key):값(value)' 형태의 프로퍼티들을 저장하는 컨테이너로서,<br>해시<sub>Hash</sub>라는 자료구조와 유사하다.
* 여러 개의 프로퍼티들을 포함할 수 있다.<br>기본 타입의 값을 포함하거나 다른 객체를 가리킬 수도 있다.
* 함수 포함한 프로퍼티를 메서드라고 부른다.

### 3.2.1 객체 생성 ###
* 클래스라는 개념이 없다
* 객채를 생성하는 방법
    * 기본 제공 Object() 객체 생성자 함수 이용
    * 객체 리터럴
    * 생성자 함수를 이용

#### 3.2.1.1 Object() 생성자 함수 이용 ####
자바스크립트에서는 객체를 생성할 때, 내장 `Object()` 생성자 함수를 제공한다.

*예제 3-5 Object() 생성자 함수를 통한 객체 생성*

```js
    // Object()를 이용해서 foo 빈 객체 생성
    var foo = new Object(); 

    // foo 객체 프로퍼티 생성
    foo.name = 'foo';
    foo.age = 30;
    foo.gender = 'male';

    console.log(typeof foo); // object
    console.log(foo);   // { name: 'foo', age: 30, gender: 'male' }
```

#### 3.2.1.2 객체 리터럴 방식 이용 ####
* 객체 리터럴이란 객체를 생성하는 표기법을 의미한다.
* 중괄호 ({})를 이용해서 객체를 생성한다.
* 중괄호 안에 "프로퍼티 이름": "프로퍼티값" 형태로 표기하면,<br>
해당 프로퍼티가 추가된 객체를 생성할 수 있다.
* 프로퍼티값으로는 값을 나타내는 어떤 표현식도 올 수 있다.

*예제 3-6 객체 리터럴 방식으로 객체 생성*

```js
    var foo = {
        name : 'foo',
        age : 30,
        gender: 'male'
    }

    console.log(typeof foo); // object
    console.log(foo)    // { name: 'foo', age: 30, gender: 'male' }
```
#### 3.2.1.3 생성자 함수 이용 ####
자바스크립트의 경우는 **함수를 통해서 객체를 생성할 수 있다.**

### 3.2.2 객체 프로퍼티 읽기/쓰기/갱신 ###
*객체의 프로퍼티에 접근하는 방법*
* 대괄호([])표기법
* 마침표(.) 표기법

<em id="iss43">예제 3-7 객체 리터럴 방식을 통한 foo 객체 생성</em>
```js
    var foo = {
        name : 'foo',
        major : 'computer science'
    };
    //  프로퍼티 읽기
    console.log(foo.name);      // foo
    console.log(foo['name']);   // foo
    console.log(foo.nickname);  // undefined 존재하지 않는 프로퍼티


    foo.major = 'electronics engineering';  // 프로퍼티 갱신
    console.log(foo.major);     // electronics engineering
    console.log(foo['major']);  // electronics engineering

    foo.age = 30;           // 프로퍼티 동적 생성
    console.log(foo.age);   // 30

    /*
        대괄호 표기법만을 사용해야 하는 경우
        - 표현식
        - 예약어
    */
    foo['full-name'] = 'foo bar';   // '-' 연산자가 있는 표현식  
    console.log(foo['full-name']);  // foo bar
    console.log(foo.full-name);     
    /* 
        node : name is not defined
        browser : NaN
    */
    console.log(foo.full);          // undefined
    console.log(name);              // error : name is not defined


    
    /*
        표현식, 예약어 테스트
    */
    // 표현식
    foo['first==name'] = 'yoo';         // '==' 동등 연산자 표현식
    console.log(foo.first==name);       // error : name is not defined
    console.log(foo['first==name']);    // yoo

    // 예약어
    foo['return'] = 'return';       // 'return' 예약어
    console.log(foo.return);    // 정상적으로 return 출력된다?
```
<dl>
    <dt>
        NaN (Not a Number) 값
    </dt>
    <dd>
        수치 연산을 해서 정상적인 값을 얻지 못할 때 출력<br>
        ex) <code>1 - "hello" // NaN</code>
    </dd>
<dl>

### 3.2.3 for in 문과 객체 프로퍼티 출력 ###
*예제 3-6 for in 문을 통한 객체 프로퍼티 출력*
```js
    var foo = {
        name : 'foo',
        age : 30,
        marjor : 'computer schiense'
    };
    
    for(prop in foo) {  // foo 객체의 포함된 프로퍼티에 대해 루프를 실행
        console.log(prop, foo[prop]); // 프로퍼티명 값 출력
    }

```
### 3.2.4 객체 프로퍼티 삭제 ###
객체의 프로퍼티를 delete 연산자를 이용해 즉시 삭제할 수 있다.
<strong>객체 자체를 삭제하지는 못한다.</strong>
```js
    var foo = {
        name : 'foo',
        nickname : 'genius'
    };

    console.log(foo.nickname);  // genius
    delete foo.nickname;        // 프로퍼티 삭제
    console.log(foo.nickname);  // undefined

    delete foo;             // 객체 삭제 시도
    console.log(foo.name)   // 객체는 삭제되지 않고 정삭적으로 foo 출력                  

```

## 3.3 참조 타입의 특성 ##
<dl>
    <dt>참조 타입<dt>
    <dd>
        객체는 모든 연산이 <strong>실제값이 아닌 참조값으로 처리된다.</strong>
    </dd>
</dl>

*에제 3-9 동일한 객체를 참조하는 두 변수 objA와 objB*
```js
    var objA = {        // 변수 objA는 객체를 가르키는 참조값을 저장하고 있다.
        var :40     
    }
    var objB = objA;    // 변수 objB에 objA 값(객체를 가르키는 참조값)을 할당 한다.

    console.log(objA.val);  // 40
    console.log(objB.val);  // 40

    objB.val = 50           
    console.log(objA.val);  // 50
    console.log(objB.val);  // 50
``` 

### 3.3.1 객체 비교 ###
*예제 3-16 기본 타입과 참조 타입의 비교 연산*
```js
    var a = 100;
    var b = 100;

    var objA = [value: 100];
    var objB = [value: 100];
    var objC = objB;    // objB 객체 참조값을 할당 

    // 기본타입 변수으므로 값을 비교한다.
    console.log(a == b);            // true 

    // 객체는 참조 타입의 경우는 참조값을 비교한다.
    console.log(objA == objB);      // false
    console.log(objB == objC);      // true
```

### 3.3.2 참조에 의한 함수 호출 방식 ###
* 기본 타입
    * *값에 의한 호촐(Call By Value) 방식*<br>
       함수 호출시 인자로 기본 타입의 값을 넘길 경우, 매개변수로 **복사된 값**이 전달된다.<br>
       때문에 함수 내부에서 매개변수를 이용해 값을 변경해도,<br> **실제로 호출된 변수의 값이 변경되지 않는다.**
* 참조 타입
    * *참조에 의한 호출(Call By Reference) 방식*<br>
      함수 호출 시 인자로 참조 타입인 객체를 전달할 경우,<br> 
      객체의 참조값이 그대로 함수 내부로 전달된다.<br> 
      때문에 함수 내부에서 참조값을 이용해서 **실제 객체의 값을 변경 할 수 있다.**

*예제 3-11 Call by Value와 Call by Reference 차이*
```js
    var a = 100;
    var objA = { value: 100 };

    function changeArg(num, obj) {
        num = 200;
        obj.value = 200;

        console.log(num);   // 200
        console.log(obj);   // {value: 200}
    }

    changeArg(a, objA);

    console.log(a);         // 100
    console.log(objA);      // 200

```

## 3.4 프로토타입 ##

* 객체의 **부모 역활을 하는 객체와 연결되어 있다** 
* 부모 객체의 프로퍼티를 자신 것처럼 쓸 수 있다.
* 프로토타입 객체(프로토타입)라고 부른다 

*예제 3-12 객체생성 및 출력*
```js
    var foo = {
        name: 'foo',
        age: 30
    };

    console.log(foo.toString());    
    /* 
        foo 객체는 toString() 메소드가 없지만 
        프로토타입에 toString() 메서드가 이미 정의되어 있어 에러를 발생 하지 않는다.
    */
    console.dir(foo);       
    /*
        foo 객체에 __proto__ 프로퍼티가 있다는 것을 확인할 수 있다.
        이 프로퍼티가 foo 객체의 부모인 프로토타입 객체(Object)를 가리킨다.
        이 객체의 다음 부분에 toString() 메서드가 정의되어 있다. 때문에 정상적으로 메서드가 출력된 것이다.
    */
    console.log(foo.__proto__.toString);    // [Function: toString]
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
               42 
            </td>
            <td>
                foo[name]이라고 접근하면 ......<br>
                toString()이라는 메서드를 자동으로 호출해서 이를 문자열로 바꾸려는 시도를 한다.
            </td>
            <td>
            </td>
        </tr>
        <tr>
            <td>
                43
            </td>
            <td>
                <a href="#iss43">4 대골호 표기법만을 사용해야 하는 경우 <br>
                접근하려 하고자 하는 프로퍼티 표현식이거나 예약어 일 경우다.</a>
            </td>
            <td>
            </td>
        </tr>
    </tbody>
</table>

