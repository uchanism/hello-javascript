# 6.4.선행학습 #
## 클래스 팩토리 및 체이닝 ##
* 팩토리 함수
* 재귀적 체이닝
* 팩토리 함수를 참조한 체이닝

<dl>
    <dt>
        팩토리 함수 <code>factory()</code>
    </dt>
    <dd>
        클래스(일급 객체 === 함수 객체) 구조를 만들어 내는 함수
    </dd>
</dl>

<dl>
    <dt>
        재귀적 체이닝
    </dt>
    <dd>
        객체 자신을 리턴한다.
    </dd>
<dl>

```js
    var target = {

        show : function(msg, color) {
            console.log('%c'+msg,'color:'+color);
            return this;    // 객체 자신을 리턴한다.
        }
    };

    target.show('1. 기절', `red`)             // 1. 리턴받은 target 객체
            .show('2. 대박', 'blue')          // 2. target 객체(1.리턴 받은 target 객체)의 프로퍼티로 정의된 show 메소드 호출 → 리턴 받은 target 객체 
                .show('3. 성공', 'green');    // 3. target 객체(2.리턴 받은 target 객체)의 프로퍼티로 정의된 show 메소드 호출 → 리턴 받은 target 객체
```
<dl>
    <dt>
        팩토리 함수를 참조한 체이닝
    </dt>
    <dd>
        팩토리 함수 안에서 생성된 클래스를 리턴한다.
    </dd>
</dl>

```js
    function factory() {

        var target = function() {              // 클래스 생성(초기화)

        }

        target.factory = arguments.callee;     // 함수 재귀 체이닝을 위해 리턴할 클래스에 팩토리 함수 참조

        return target;      // 생성된 클래스를 리턴한다.
    }

    // 참조된 팩토리 함수의 사용(체이닝)
    var f1 = factory();     // 리턴 받은 target 클래스 참조값 할당
                            // (참조값 : 함수 factory 내부 맴버 변수 target이 가리키는 함수 객체)

    var f2 = f1.factory();  // f1 클래스의 프로퍼티로 정의된 factory 메소드 호출
                            // 변수 f2에 리턴 받은 target 클래스 참조값 할당

    var f3 = f2.factory();  // f2 클래스의 프로퍼티로 정의된 factory 메소드 호출
                            // 변수 f3에 리턴 받은 target 클래스 참조값 할당

    /*
        JS Engine은 코드의 각 줄을  좌 -> 우로 읽으며 해석한다.
    */
    var f0 = factory()          // 1. 리턴 받은 target 클래스
                
                .factory()      // 2. target 클래스(1.리턴 받은 target 클래스)의 프로퍼티로 정의된 factory 메소드 호출 → 리턴 받은 target 클래스 

                    .factory(); // 3. target 클래스(2.리턴 받은 target 클래스)의 프로퍼티로 정의된 factory 메소드 호출 → 리턴 받은 target 클래스 변수 f0에 할당

    console.dir(f1);
    console.dir(f2);
    console.dir(f3);
    console.dir(f0);
```

## 팩토리 함수 내부에서 this 바인딩 ##
함수 체이닝 내부에서 this는 함수를 호출한 객체를 바인딩 한다.

*함수 체이닝의 this 계층(트리구조)*


