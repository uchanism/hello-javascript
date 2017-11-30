# 자바스크립트 데이터 타입과 연산자 #
## 자바스크립트의 데이터 타입 ##
* 기본 타입 
* 참조 타입

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

### 예제 3-1 자바스크립트 기본 타입 ###
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
## 3.1.1  숫자 ##
* 하나의 숫자형(number)만 존재한다.
* 정수형이 따로 없고, 모든 숫자를 실수로 처리한다.


### 예제 3-2 자바스크립트 나눗셈 연산 ###
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

## 3.1.1 문자열 ##
* string
* 작은 따옴표(')sk 큰 따옴표("")로 생성한다.
* **한 번 정의된 문자열은 변하지 않는다.**
### 예제 3-3 자바스크립트 문자열 예제 ###
```js
    var str = 'test'
    console.log(str[0], str[1], str[2], str[3]) // test

    str[0] = 'T'; // 첫번째 문자열  대문자로 변경
    console.log(str) 
    // 출력값 test
    // 한번 생성단 문자열은 읽기만 가능하지 수정이 불가능히다. 
```

## 3.1.3 불린값 ##
* boolean
* true와 false 값을 나타내는 타입 

## 3.1.4 null과 undefined ##
* '값이 비어있음' 나타낸다.
* 값이 할당되지 않는 변수는 undefined 타입이며, 값 또한 undefined이다.
* **undefined는 타입이자, 값을 나타난다**
* null 타입의 변수의 경우는 명시적으로 값이 비어있을을 나타내는 데 사용한다. 

### 예제 3-4 null 타입 변수 체크 ###
```js
    /*
        null 타입 변수인지를 확인할 때 typeof 연산자가 아닌
        일치 연산자(===)를 사용해서 변수의 값을 직접 확인해야 한다.
    */
    var nullVar = null

    console.log(typeof nullVar === null);
    console.log(nullVal === null);;

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
<dl>
