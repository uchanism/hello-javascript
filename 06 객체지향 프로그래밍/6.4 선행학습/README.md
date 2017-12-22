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

    target.show('1. 기절', `red`)             // return target객체 (변수 target이 가리키는 객체의 참조값)
            .show('2. 대박', 'blue')          // return target객체 재할당
                .show('3. 성공', 'green');    // return target객체 재할당
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
    var f1 = factory();     // target 클래스 참조값 할당
                            // 참조값 : 함수 factory 내부 맴버 변수 target이 가리키는 함수 객체

    var f2 = f1.factory();  // f1 클래스의 프로퍼티로 정의된 팩토리 메소드 호출
                            // 변수 f2에 target 클래스 참조값 재할당

    var f3 = f2.factory();  // f2 클래스의 프로퍼티로 정의된 팩토리 메소드 호출
                            // 변수 f3에 target 클래스 참조값 재할당

    var f0 = factory()          // target 클래스 참조값 할당   
                
                .factory()      // target 클래스의 프로퍼티로 정의된 팩토리 메소드 호출 
                                // target 클래스 참조값 재할당

                    .factory(); // target 클래스의 프로퍼티로 정의된 팩토리 메소드 호출 
                                //  target 클래스 참조값 재할당

    console.dir(f1);
    console.dir(f2);
    console.dir(f3);
    console.dir(f0);
```