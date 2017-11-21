# 객체지향 프로그래밍 #

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

**예제 6-1**<br>

```js
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
```
함수 Person이 클래스이자 생성자의 역활을 한다.<br>
사용자는 new 키워드로 인스턴스를 생성하여 사용할 수 있다.<br>
생선된 me = Person의 인스턴스(변수 name+ 함수 getName() + 함수 setName())

**Person 생성자로 여러개의 객체 생성시 문제 발생**<br>
```js
    var me = new Person("me");
    var you = new Person("you");
    var him = new Person("him");
```
공통적으로 사용할 수 있는 getName(), setName() 함수를 따로 생성하여 불필요하게 중복되는 영역을 메모리에 올려놓고 사용함으로 인해 자원낭비를 가져온다.

**예제 6-2**

```js
    function Person(arg) {
        this.name = arg;
    }
    Person.prototype.getName = function() {
        return this.name;
    }
    Person.prototype.setName = function(value) {
        this.name = value;
    }

    var me = new Person("me");
    var you = new Person("you");
    console.log(me.getName());
    console.log(you.getName());

```

Person 함수 객체 prototype프로퍼티에 getName(), setName() 함수 정의 후 객체를 생성 한다면 각자 따로 함수 객채를 생성하지 않고 프로토타입 체인으로 접근 할수 있다.

자바스크립트에서는 클래스 안의 **메서드를 정의할 때는 프로토타입 객체에 정의**한 후 new로 생성 된 객체에서 접근할 수 있게 하는 것이 좋다.

**예제 6-3**

```js
    Function.prototype.method = function(name, func){
        this.prototype[name] = func; // 호출한 객체의 프로토타입에 메소드를 추가한다.
    } // 전역 Function 객체의 prototype에 메서드 생성


    function Person(arg) {
        this.name = arg;
    }   // 생성자 함수 정의

    Person.method("setName", function(value) {
        this.name = value;
    });

    Person.method("getName", function(){
        return this.name;
    });
    /*
        1. Person 함수 객체의 프로토타입 체인으로 method 함수 호출
        2. Person.prototype에 setName() 메소드 생성  
    */

    var me = new Person("me");
    var you = new Person("you");

    console.log(me.getName()); 
    console.log(you.getName());
```