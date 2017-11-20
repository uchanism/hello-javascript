# 객체지향 프로그레밍 #

**객체지향 언어의 특성**

* 클래스, 생성자, 메서드
* 상속
* 캡슐화

**클래스 기반 언어**
* java, C++ 등등
* 클래스로 객체의 기본 형태 및 기능 정의 후 생성자로 인스턴스 생성
* **인스턴스는 클래스에 정의된 구조이고 보통 런타임에 바꿀수 없다.**
* 정확성, 안전성, 예측성 등의 관점에서 좀 더 나은 결과를 보장

**프로토타입 기반 언어**
* **객체의 자료구조, 메서드 등을 동적으로 바꿀수 있다.**

## 6.1 클래스, 생성자, 메서드 ##

자바스크립트는 거의 모든 것이 객체이고, 특히 함수 객체로 많은 것을 구현해낸다.<br>
**클래스, 생성자, 메서드 모두 함수로 구현 가능하다**

**예제 6-1**

    function Person(arg) {
        this.name = arg;

        this.getName = function() {
            return this.name;
        }

        this.setName = function(value) {
            this.name = value;
        }
    }

    var me = new Person("zzoon");
    console.log(me.getName());

    me.setName("iamhjjo")
    console.log(me.getName());

**함수 Person → 클래스이자 생성자의 역활 
(new 키워드로 인스턴스 생성)**

생선된 me = Person의 인스턴스(name 변수+ getName()함수 + setName() 함수)
