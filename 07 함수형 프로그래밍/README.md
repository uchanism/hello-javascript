# 함수형 프로그래밍 #
## 7.1 함수형 프로그래밍의 개념 ##
* 함수의 조합으로 작업을 수행한다.
* 작업이 이루어 지는 동안 필요한 데이터와 상태는 변하지 않는다.
* 오로지 함수만 변할 수 있다.

<dl>
    <dt>
        <a href="https://ko.wikipedia.org/wiki/%EC%9D%98%EC%82%AC%EC%BD%94%EB%93%9C">슈도코드(의사코드, pseudocode)</a>
    </dt>
    <dd>
        특정 프로그래밍 언어의 문법을 따라 쓰인 것이 아니라, 일반적인 언어로 코드를 흉내 내어 알고리즘을 써놓은 코드를 말한다. 의사코드는 말 그대로 흉내만 내는 코드이기 때문에, 실제적인 프로그래밍 언어로 작성된 코드처럼 컴퓨터에서 실행할 수 없으며, 큭정 언어로 프로그램을 작성하기 전에 알고리즘의 모델을 대략적으로 모델링하는 데에 쓰인다.
    </dd>
</dl>

*함수형 프로그래밍을 표현하는 슈도 코드*
<dl>
    <dt>
        <code>f1~3</code>
    </dt>
    <dd>
        특정 문자열을 암호화하는 함수
    </dd>
    <dd>
        각각 입력값이 정해지지 않고, 서로 다른 암호화 알고리즘만 있다.
    </dd>
    <dt>
        <code>pure_value</code>
    </dt>
    <dd>
        암호화할 문자열
    </dd>
    <dt>
        <code>get_encrypted(x)</code>
    </dt>
    <dd>
        암호화 함수(f1~3)를 받아서 입력받은 함수로 pure_value를 암호화한 후 반환한다.
    </dd>
    <dt>
        <code>encrypted_value</code>
    </dt>
    <dd>
        암호화된 문자열
    </dd>
</dl>

```js 
    f1 = encrypt1;
    f2 = encrypt2;
    f3 = encrypt3;

    pure_value = 'zzoon';   

    encrypted_value = get_encrtypted(f1);
    encrypted_value = get_encrtypted(f2);
    encrypted_value = get_encrtypted(f3);
    /*
        1. get_encrtypted(f1);      // 고계 함수
        2. f1(pure_value);          // 순수 함수
        3. encrypted_value
    */
```

<dl>
    <dt>
        <a href="https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D#%EC%88%9C%EC%88%98%ED%95%9C_%ED%95%A8%EC%88%98"><b>순수 함수<sub>Pure function</sub></b></a>
    </dt>
    <dd>
        함수의 실행이 외부에 아무런 영향을 미치지 않는 함수
    </dd>
    <dd>
        결과값을 얻는 것이 함수를 호출한 목적인 함수
    </dd>
    <dd>
        이미 작성된 순수 함수로 다른 작업에 활용해도 문제가 없다.
    </dd>
    </dd>
    <dt>
        <a href="https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D#%EA%B3%A0%EA%B3%84_%ED%95%A8%EC%88%98"><b>고계 함수<sub>Highter-order function</sub></b></a>
    </dt>
    <dd>
        함수를 다루는 함수
    </dd>
    <dd>
        함수를 또 하나의 값으로 간주하여 함수의 인자 혹은 반환값으로 사용 할 수 있는 함수
    </dd>
</dl>

<em><b>함수형 프로그래밍의 특성</b></em><br>
내부 데이터 및 상태는 그대로 둔채 제어할 함수를 변경 및 조합함으로써 원하는 결과를 얻어낸는 것 (높은 수준의 모듈화가 가능하다.)

## 7.2 자바스크립트에서의 함수형 프로그래밍 ##
자바스크립트느 다음을 지원 지원하기때문에 함수형 프로그래밍 가능하다.
* 일급 객체로서의 함수
* 클로저

*에제 7-1*
```js
    var f1 = function(input) {
        var result;
        result = 1;
        return result;
    }
    var f2 = function(input) {
        var result;
        result = 2;
        return result;
    }
    var f3 = function(input) {
        var result;
        result = 3;
        return result;
    }

    var get_encrypted = function(func) {
        var str = 'zzoon';

        return function() {                 // 클로저
            return func.call(null, str);
        }
    }
    
    // 일급객체로 취급되는 함수
    // 함수를 인자로 넘기고 결과를 함수로 받을 수도 있다.
    var encrypted_value = get_encrypted(f1)();      
    console.log(encrypted_value);                   // 1
    var encrypted_value = get_encrypted(f2)();
    console.log(encrypted_value);                   // 2
    var encrypted_value = get_encrypted(f3)();      
    console.log(encrypted_value);                   // 3
```
### 7.2.1 배열의 각 원소 총합 구하기 ###
<dl>
    <dt>
        <a href="https://ko.wikipedia.org/wiki/%EB%AA%85%EB%A0%B9%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D"><b>명령형 프로그래밍</b></a>
    </dt>
    <dd>
        프로그래밍의 상태와 상태를 변경시키는 구문의 관점에서 연산을 설명하는 프로그래밍 패러다임의 일종이다.
    </dd>
</dl>

*예제 7-2-1*
```js
    /*
        배열의 각 원소 합을 구하기
    */
    function sum(arr) {
        var len = arr.length;
        var i = 0, sum = 0;

        for (; i<len; i++) {
            sum += arr[i];
        }

        return sum;
    }

    var arr =[1, 2, 3, 4];
    console.log(sum(arr));      // 10
```
*예제 7=2=2*
```js
    /*
        배열의 각 원소 곱 구하기
    */
    function multiply(arr){
        var len = arr.length;
        var i = 0, result = 1;

        for(; i<len; i++){
            result *= arr[i];
        }
        
        return  result

    }
    
    var arr = [1,2,3,4];
    console.log(multiply(arr));     // 24     
```
배열의 각 원소를 또 다른 방식으로 산술하여 결과값을 얻으려면 새로운 함수를 다시 구현해야 하는데, 함수형 프로그래밍을 이용하면 이런 수고를 덜 수 있다.

*에제 7-2-3*
```js
    function reduce(func, arr, memo) {
        var len = arr.length,
            i = 0,
            accum = memo;
        
        for(; i<len; i++) {
            accum = func(accum, arr[i]);
        }

        return accum;
    }
```
*에제 7-2-4*
```js
    function reduce(func, arr, memo) {
        var len = arr.length,
            i = 0,
            accum = memo;
        
        for(; i<len; i++) {
            accum = func(accum, arr[i]);
        }

        return accum;
    }

    var arr = [1,2,3,4];
    
    var sum = function(x, y) {
        return x+y;
    };

    var multiply = function(x, y) {
        return x*y;
    };

    console.log(reduce(sum, arr, 0));
    console.log(reduce(multiply, arr, 1));

```
### 7.2.2 팩토리얼 ###
<dl>
    <dt>
        <b>팩토리얼</b>
    </dt>
    <dd>
        자기 자신의 수에 자신의 수보다 1 작은 수가 1이 될때까지 곱하는 것(기호: !)
    </dd>
    <dd>
        ex) 5! = 5*4*3*2*1
    </dd>
</dl>

*예제 7-3-1*
```js
    /*
        명령형 프로그래밍 방식 팩토리얼
    */

    function fact(num){
        var val = 1;
        for (var i = 2; i <=num; i++){
            val = val * i;
        }
        
        return val;
    }

    console.log(fact(10));
```
*예제 7-3-2*
```js
    /*
        재귀 호출을 이용한 명령형 프로그래밍 방식 팩토리얼
    */
    function fact(num) {
        if (num == 0) return 1;
        else return num* fact(num-1);   // 재귀 호출
    }

    console.log(fact(10));
```
<em><b>성능을 고려한 함수형 프로그래밍</b></em><br>
20!을 실행할 때는 앞에서 실행한 10!을 중복하여 계산한다.<br>
중복되는 값, 즉 앞서 연산한 결과를 캐시에 저장하여 사용할 수 있는 함수를 작성한다면 성능 향상에 도움이 된다.

*예제 7-3-3*
```js
    
    var fact = function() {
        var cache = {`0`: 1};       
        var func = function(n) {
            var result = 0;

            if (typeof(cache[n] === 'number')) {
                result = cache[n];  // 캐시에 저장된 값이 있으면 그 값을 반환 한다.
            } else {
                result = cache[n] = n * func(n-1);  // 연산한 값을 저장한다.
            }

            return result;
        }

        return func;
    }();

    console.log(fact(10));
    console.log(fact(20));
```

<dl>
    <dt>
        <b>메모이제이션<sub>memoization</sub> 패턴</b>
    </dt>
    <dd>
        계산 결과를 저장해 놓아 이후 다시 계산할 필요 없이 사용할 수 있게 한다
    </dd>
    <dd>
        주의할 점은 한 번 값이 들어간 겨우 계속 유지되므로 이를 초기화하는 방법 역시 제공돼야 한다. (jQuery에서는 cleanDate(이전에는 removeDate)라는 메서드 제공)
    </dd>
</dl>

```js
    function Calculate(key, input, func) {
        Calculate.date = Calculate.date || {};      // 사용자는 자신이 원한느 값을 원하는 키 key로 저장해 놓을 수 있다.
                                                    // 이후에는 해당 키로 저장해 놓은 값을 받아서 사용할수 있다.
        if (!Calculate.date[key]) {
            var result;
            result = func(input);
            Calculate.date[key] = result;
        }

        return Calculate.date[key];
    }


    var result = Calculate(1, 5, function(input) {
        return input * input;
    })

    console.log(result);

    result = Calculate(2, 5, function(input) {
        return input * input / 4;
    });

    console.log(result);

    console.log(Calculate(1));
    console.log(Calculate(2));
```
<em><b>범용적인 메모이제이션 패턴</b></em>
```js
    Function.prototype.memoization = function(key) {
        var arg = Array.prototype.slice.call(arguments, 1 );
        this.date = this.date || {};

        return this.date[key] !== undefined ?
            this.date[key] : this.date[key] = this.apply(this, arg);
    };

    function myCalculate1(input) {
        return input * input;
    }

    function myCalculate2(input) {
        return input * input / 4 ;
    }

    myCalculate1.memoization(1, 5);
    myCalculate1.memoization(2, 4);
    myCalculate2.memoization(1, 6);
    myCalculate2.memoization(2, 7);

    console.log(myCalculate1.memoization(1));   // equal to console.log(myCalculate1.date[1]);
    console.log(myCalculate2.memoization(2));   // equal to console.log(myCalculate2.date[2]);
    console.log(myCalculate1.memoization(1));   // equal to console.log(myCalculate2.date[1]);
    console.log(myCalculate2.memoization(2));   // equal to console.log(myCalculate2.date[2]);
```

