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