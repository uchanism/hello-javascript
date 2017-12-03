# 객체지향 프로그래밍 #

## 객체지향 언어의 특성 ##

* 클래스, 생성자, 메서드
* 상속
* 캡슐화

### 클래스 기반 언어 ###
* java, C++ 등등
* 클래스로 객체의 기본 형태 및 기능 정의 후 생성자로 인스턴스 생성
* **인스턴스는 클래스에 정의된 구조이고 보통 런타임에 바꿀수 없다.**
* 정확성, 안전성, 예측성 등의 관점에서 좀 더 나은 결과를 보장
### 프로토타입 기반 언어 ###
* **객체의 자료구조, 메서드 등을 동적으로 바꿀수 있다.**

## 6.1 클래스, 생성자, 메서드 ##

자바스크립트는 거의 모든 것이 객체이고, 특히 함수 객체로 많은 것을 구현해낸다.<br>
**클래스, 생성자, 메서드 모두 함수로 구현 가능하다**

### 예제 6-1 ###

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

### Person 생성자로 여러개의 객체 생성시 문제 발생 ###
```js
    var me = new Person("me");
    var you = new Person("you");
    var him = new Person("him");
```
공통적으로 사용할 수 있는 getName(), setName() 함수를 따로 생성하여 불필요하게 중복되는 영역을 메모리에 올려놓고 사용함으로 인해 자원낭비를 가져온다.

### 예제 6-2 ###

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

### 예제 6-3 ###

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
## 6.2 상속 ##

* 프로토타입을 이용한 상속
* 클래시 기반의 상속

### 6.2.1 프로토타입을 이용한 상속 ###
#### 예제 6-4 ####
```js
    function create_object(o){ 
        function F() {} // 함수 객체 생성
        F.prototype = o; // 생선한 F 함수 객체의 프로토타입에 인자로 받은 객체를 참조
        return new F(); // F 함수 객체를 생성자로 하는 새로운 객체 생성 후 반환
    }
```
앞에서 소개한 create_object() 함수는 ECMAScript 5에서 Object.create() 함수로 제공된다.

#### 예제 6-5 ####
```js
    var person = {
        name : "zzoon",
        getName : function() {
            return this.name;
        },
        setName : function(arg) {
            this.name = arg;
        }
    }

    function create_object(o) {
        function F() {};
        F.prototype = o;
        return new F();
    }

    var student = create_object(person);

    student.setName("me");
    console.log(student.getName());

```
부모 객체에 해당하는 person 객채와 이 객체를 프로토타입 체인으로 참조할 수 있는 자식 객체 student를 만들어서 상속의 개념을 구현했다.

#### jQuery 1.0의 extend 함수 ####
자식 객체 자신의 메서드 재정의, 추가로 기능 확장시킬수 있어야한다.<br>
* 범용적 함수
* 객체에 자신이 원하는 객체 혹은 함수를 추가 시킨다.
* obj[i] = prop[i] 얕은 복사로 객체(배열, 함수 객체 포함)인 경우 **해당 객체를 복사하지 않고, 참조 한다. 이는 두번째 객체의 프로퍼티가 변경되면 첫 번째의 객체의 프로퍼티도 같이 변경된다.**

```js
    jQuery.extend = jQuery.fn.extend = function(obj,prop) { // jQuery prototype(jQuery.fn)에 extend 함수 생성
        if (!prop) {prop = obj; obj = this;}
        // 인자가 하나만 들어올 경우 인자로 들어온 객체의 프로퍼티를 현재 객체(this)에 복사하겠다.

        for (var i in prop) obj[i] = prop[i]; // 루프를 돌면서 두번째 들어온 인자 객체의 프로퍼티를 첫번째 들어온 인자 객체로 복사한다.
        // 두 개가 들어올 경우 첫 번째 인자(obj) 객체에 두 번째 인자(prop) 객체의 프로퍼티를 복사하겠다.

        return obj;
    }
```
깊은 복사를 하려면 복사하려는 대상이 객체인 경우 빈객체를 만들어 extend함수를 재귀적으로 다시 호출하는 방법을 쓴다. 주의할 점은 함수 객체인 경우 그대로 야은 복사를 진행한다.

#### jQuery 1.7의 extend 함수 중 일부 코드 ####
```js
    for (; i < length; i++){
        
        if ((options = arguents[i]) != null) { // 인자로 넘어온 프로퍼티를 option에 참조 시킨 후 null이 아니면 진입한다.
            
            for (name in options) {
                src = target[name]; // 반환될 복사본 프로퍼티를 가르킨다
                copy = options[name]; // 복사할 원본 프로퍼티를 가르킨다

                
                if ( target === copy) {
                    continue;
                }

                if(deep && copy && (jQuery.isPlainObject(copy) || (copyIsArray = jQuery.isArray(copy)) ) ) { 
                    /*
                        deep 플래그 : 사용자가 Boolean 값을 넣어 깊은 복사를 할 것인지 선택할 수 있게 한다.
                        copy 프로퍼티가 객체, 배열인 경우 재귀 호출를 하기위해 진입한다.
                    */
                    if(copyIsArray) {
                        copyIsArray = false;
                        clone = src && jQuery.isArray(src) ? src : {};
                        
                    } else {
                        clone = src &&jQuery.isPlainObject(src) ? src : {};
                    }
                    // src가 같은 배열 혹은 객체이면 clone에 src를 참조 시킨다. 같은 배열 혹은 객체가 아니면 빈 객체를 할당한다.
                    
                    /*
                    [이해불가]
                    이는 복사본에 같은 이름의 프로퍼티가 있는 경우, 원본과 똑같은 배열이거나 객체라면 새롭게 할당시키지 않고, 복사복의 해당 프로퍼티에 추가 될 수 있게 하기 위해서다.
                    
                    5 ~ 7 내용 

                    */

                    target[name] = jQuery.extend(deep, clone, copy);
                } else if(copy !== undefined) {
                    target[name] = copy;
                }
            }
        }
        return target;
    }
```

#### 예제 6-6 ####
```js
    var person = { 
        name : "zzoon",
        getName : function() {
            return this.name;
        },
        setName : function(arg) {
            this.name = arg;
        }
    };

    function create_object(o){
        function F() {};
        F.prototype = o;
        return new F();
    }

    function extend(obj,prop) {
        if (!prop){prop = obj; obj = this;}
        for(var i in prop) obj[i] = prop[i];
        return obj;
    };

    var student = create_object(person);
    var added = {
        setAge : function(age) {
            this.age = age;
        },
        getAge : function() {
            return this.age;
        }
    };

    extend(student, added);

    console.dir(student);

    student.setAge(25);
    console.log(student.getAge());


    //얕은 복사 
    
    //깊은 복사
    
    
```
### 6.2.2 클래스 기반의 상속 ###
* 클래스 기반의 상속이라고는 하나 프로토타입을 적절이 엮어서 상속을 구현한다.
* 클래스의 역활을 하는 **함수로 상속을 구현**한다.

#### 예제 6-7 ####
```js
    function Person(arg) {
        this.name = arg;
    }

    Person.prototype.setName = function(value) {
        this.name = value;
    };
    
    Person.prototype.getName = function() {
        return this.name;
    };

    function Student(arg) {
        // Person.apply(this, arguments); // 부모 클래스 생성자 호출
        // 함수 안에서 새롭게 생성된 객체를 apply 함수의 첫 번째 인자로 넘겨 Person 함수를 실행시킨다.
        
    }

    var you = new Person("iamhjoo"); 
    Student.prototype = you; // Student 함수 객체의 prototype에 Person 함수 객체의 인스턴스를 참조한다.
    

    console.dir(Student);

    var me = new Student("zzoon"); 
    /*
        부모 클래스인 Person의 생성자를 호출하지 않는다.
        인스턴스를 생성할 때 "zzoon"을 인자로 넘겼으나, 이를 반영하는 코드는 어디에도 없다.

    */
    
    console.dir(me);
    console.log(me.getName());

    me.setName("zzoon"); // me 객체에 name 프로퍼티가 만들어진다.
    
    console.dir(me);   
    console.log(me.getName());
```

`var me = new Student("zzoon")`<br>
이렇게 부모의 생성자가 호출되지 않으면, 인스턴스 초기화가 제대로 이루어지지 않아 문제가 발생 할 수 있다.

`Student.prototype = you;`<br>
현재는 자식 클래스의 객체가 부모 클래스의 객체를 프로토타입 체인으로 직접 접근한다.

이 구조는 자식 클래스의 prototype에 메소드를 추가할 때 **문제가 된다.**(여기서 문제란 뭘까? 인스턴스의 this?)

#### 예제 6-8 ####
``` js
/*
    두 클래스의 프로토타입 사이에 중개자를 만들어 부모, 자식 클래스의 인스턴스를 독립적으로 만들다.

*/
function Person(arg) {
    this.name = arg;
}

Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}

Person.method("setName", function(value) {
    this.name = value;
});

Person.method("getName", function() {
    return this.name;
});

function Student(arg) {}

function F() {}; // 부모객체의 프로토타입과 자식 프로토타입을 연결하는 빈 함수
F.prototype = Person.prototype;
Student.prototype = new F(); 
Student.prototype.constructor = Student;
Student.super = Person.prototype;

var me = new Student();
me.setName("zzoon");
console.log(me.getName());

```
#### 즉시 실행 함수와 클로저를 활영하여 최적화된 함수 ####
```js
    var inherit = function(Parent, Child) {
        var F = function() {}; // 자유변수
        return function(Perent, Child) { 
        /*
            자유변수 F() 함수를 지속적으로 참조하고 가비지 컬렉션의 대사잉 되지 않고 계속 남아있다.

            이를 이용해 F()의 생성은 단 한 번 이루어지고 함수를 계속해서 호출해도 함수 F()의 생성르 새로 할 필요가 없다.
        */
            F.prototype = Parent.prototype;
            Child.prototype = new F();
            Child.prototype.constructor = Child;
            Child.super = Parent.prototype;
        };
    }();

    // 최적화 함수를 이용한 상속 구현

```

## 6.3 캡슐화 ##

### 캡슐화란? ###
* 여러 가지 정보를 하나의 틀 안에 담는 것
* 정보 은닉
* C++이나 Java에서는 public, private 맴버를 선언함으로써 외부 노출 여부 결정한다. 하지만 javascript는 **이러한 키워드를 지원하지 않는다.**

#### 예제 6-9 ####
``` js
    var Person = function(arg) {
        var name = arg ? arg : "zzoon" ; // private 멤버 name 선언

        /*
            public 메서드로 getName(),setName() 선언
        */
        this.getName = function() { 
            return name;
        }
        this.setName = function(arg) {
            name = arg;
        }
    };

    var me = new Person();
    
    console.dir(me);
    console.log(me.getName());
    
    me.setName("iamhjoo");
    
    console.log(me.getName());
    console.log(me.name); 
```

`this.getName = function() { return name;}`<br>
this 객체의 프로퍼티로 선언하면 외부에서 new 키워드로 생성한 객체로 접근할 수 있다.

`var name = arg ? arg : "zzoon" ;`<br>
var로 선언된 맴버들은 외부에서 접근이 불가능하다.<br>

public 메서드가 클로저 역활을 하면서 private 맴버인 name에 접근할 수 있다.<br>

이것이 자바스크립트에서 할 수 있는 기본적인 **정보 은닉 방법이다.**
#### 예제 6-10 ###
```js
/*
    *모듈 폐턴*
    private 맴버에 접근할 수 있는 메서드들이 담겨있는 객체를 반환하는 생성자 함수로 여러 자바스크립트 라이브러리에서 쉽게 볼 수 있는 구조다.
*/
    var Person = function(arg) {
        var name = arg ? arg : "zzoon";

        return {
            getName : function() {
                return name;
            },
            setName : function() {
                name = arg;
            }
        };
    }

    var me = Person(); /* or var me = new Person(); */
    
    console.dir(me);

    console.log(me.getName());
```
#### 예제 6-11 ###
```js
    var ArrCreate = function(arg) {
        var arr = [1,2,3]; // private 멤버가 객체나 배열이면 얕은 복사로 참조만을 반환한다.
            
        return {
            getArr : fuction() {
                return arr;
            }
        }
    }
    
    var obj = ArrCreate();
    var arr = obj.getArr()
    arr.push(5); // 사용자가 이후 이를 쉽게 변경 할 수 있는 문제가 발생한다.
    console.log(obj.getArr());
```
#### 예제 6-12 ####
에졔 6-10 에서 사용자가 반환받은 객체는 Person 함수 객체의 프로토타입에는 접근할 수 없다.<br> 이는 **Person을 부모로 하는 프로토타입을 이용한 상속이 용이하지 않다**는 것을 의미한다.<br> 이를 보완하려면 **객체가 아닌 함수**를 반환하는 것이 좋다.  

```js
    var Person = function(arg) {
        var name = arg ? arg : "zzoon";

        var Func = function() {} // 프로토타입을 이용한 상속이 용이하도록 반환할 함수 객체 생성
        Func.prototype = {
            getName : function() {
                return name;
            },
            setName : function(arg) {
                name =  arg;
            }
        };

        return Func;
    }();

    var me = new Person();

    console.dir(me);
    console.log(me.getName());

```
**[6.4 복습 필요]** 
## 6.4 객체지향 프로그래밍 응용 예제 ##    
### 6.4.1 클래스의 기능을 가진 subClass 함수 ###
기존 클래스와 같은 기능을 하는 자바스크립트 함수를 아래 세 가지를 활용하여 구현 한다.
* 함수의 프로토타입 체인
* extend 함수
* 인스턴스를 생성할 때 생성자 호출(여기서는 생성자를 _init 함수로 정한다.)

### 6.4.1.1 subClass 함수 구조 ###
subClass()는 상속받을 클래스에 넣을 변수 및 메서드가 담긴 객체를 인자로 받아 부모 함수를 상속 받는 자식 클래스를 만든다.

```js
    
    var SubperClass = subClass(obj); 
    // obj : 상속받을 클래스에 넣을 변수, 메서드가 담긴 객체
    // 참고로 SuperClass는 Function을 상속 받는다.
    
    var SubClass = SuperClass.subClass(obj);
    //SuperClass를 상복받는 subClass를 만든다.

    function subClass(obj) {
        /*
            1. 자식 클래스 (험수 객체) 생성
            2. 생성자 호출
            3. 프로토타입 체인을 활용한 상속 구현
            4. obj를 통해 들어온 변수 및 메서드를 자식 클래스에 추가
            5. 자식 함수 객체 반환
        */
    }
```

### 6.4.1.2 자식 클래스 생성 및 상속 ###
```js
    function subClass(obj) {
        var parent = this; // this는 부모 클래스를 가르킨다
            
        
        var F = function() {}; // 부모 객체의 프로로타입과 자식 객채의 프로토타입을 연결할 빈 함수 객체

        var child = function() { // 자식 클래스
            
        };

        F.prototype = parent.prototype; // 프로토타입 체인 구성
        child.prototype = new F(); // 부모 객체 상속

        child.prototype.constructor = child;
        child.parent = parent.prototype;
        child.parent_consturctor = parent;

        //.......
        return child;
    }
```
### 6.4.1.3 자식 클래스 확장
```js
    /*
        인자로 넣은 객체를 자식 클래스에 넣어 확장 한다.
    */
    for (var i in obj) {
        if (obj.hasOwnProperty(i)) {
            child.prototype[i] = obj[i]; // 얕은 복사 
        }
    }

```
<dl>
    <dt>
        hasOwnPeroperty 메서드
    </dt>
    <dd>
        Object.prototype 프로퍼티에 정의도이 있는 메서드
    </dd>
    <dd>
        인자로 넘기는 이름에 프로퍼티가 객체 내 있는지 판단한다.<br>
        <strong>프로퍼티를 찾을 때, 프로토타입 체인을 타고 올라가지 않고 해당 객체 내에서만 찾는다.</strong> 
    </dd>
</dl>

### 6.4.1.4 생성자 호출 ####
```js
    /*
        - 부모, 자식 클래스의 생성자 호출
        - 자식을 또 다른 함수가 다시 상속 받을 때 상위 클래스의 상위 클래스인
          SuperClass의 생성자가 호출 한다.
    */
    var child = function() {
        var _parent = child.parent_consturctor;

        if (_parent && _parent !== Function) { // 현재 클래스의 부모 생성자가 있고 부모가 Function(최상위 클래스)이 아닐 경우 실행한다. 
            _parent.apply(this, arguments); // 부모 함수의 재귀적 호출
        }

        if (child.prototype.hasOwnProperty("_init")) { // _init 프로토타입 체인 방지를 위해 hasOwnProperty 함수 사용
            child.prototype._init.apply(this, arguments);
        }
    }
```
### 6.4.1.5 subClass 보완 ###
#### 예제 6-13 ###
```js
    /*
        최종 subClass 
    */
    function subClass(obj) {
        var parent = this === window ? Function : this; // 최상위 클래스가 Function 객체을 상속받도록 처리
        
        var F = function() {};

        var child = function() {
            var _parent = child.parent; //

            if (_parent && _parent !==Function) {
                
                _parent.apply(this, arguments);
            }

            if (child.prototype._init) {
                child.prototype._init.apply(this, arguments);
            }
        };

        F.prototype = parent.prototype; // 하위 클래스 상속을 위한 프로토타입 체인
        child.prototype = new F();
        child.prototype.constructor = child;
        child.parent = parent; // 
        child.subClass = arguments.callee;

        for (var i in obj) {
            if (obj.hasOwnProperty(i)) {
                child.prototype[i] = obj[i];
            }
        }

        return child;
    }
```
### 6.4.1.6 subClass 활용 ###
### 예제 6-14 ###
```JS
    /*
        subClass 함수를 통한 상속
    */
    var person_obj = {
        _init : function() {
            console.log("person init");
        },
        getName : function() {
            return this._name;  
        },
        setName : function(arg) {
            this._name = arg;
        }
    };

    var student_obj = {
        _init : function() {
            console.log("student init");
        },
        getName : function() {
            return "student name : "+this._name;
        }
    }

    var Person = subClass(person_obj);
    var person = new Person();
    person.setName("zzoon");
    console.log(person.getName());

    var Student = Person.subClass(student_obj);
    var student = new Student();
    student.setName("iamhjoo");
    console.log(student.getName());

    console.log(Person.toString());
```