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

        var target = function() {              // 클래스 생성

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
```js
    function CreateTree(name){
        var upTree = this;      // CreateTree 함수를 호출한 객체
        function tree(){        // 리턴될 클래스 생성

        }
                                        // 리턴될 클래스에 프로퍼티 생성
        tree._name = name;              // name 할당 
        tree.upTree = upTree;           // CreateTree 함수를 호출한 객체 할당
        tree.CreateTree = CreateTree;   // 함수 재귀 체이닝을 위해 리턴될 클래스에 CreateTree 함수 할당

        return tree;
    }

    var tree = CreateTree('level 1 tree')               // 1. 리턴 받은 tree 클래스
                    .CreateTree('level 2 tree')         // 2. tree 클래스(1. 리턴 받은 tree 클래스)의 프로퍼티로 정의된 CreateTree 메소드 호출 → 리턴 받은 tree 클래스
                        .CreateTree('level 3 tree');    // 3. tree 클래스(2. 리턴 받은 tree 클래스)의 프로퍼티로 정의된 CreateTree 메소드 호출 → 리턴 받은 tree 클래스

    console.dir(tree);
```


*트리구조 함수 체이닝을 적용한 factory 함수*
```js
    /*
        함수 factory()

        - 실행 환경은 브라우저를 전제로 한다.
        - 호출한 클래스(부모 클래스)를 상속받은 자식 클래스를 만들어 리턴한다.
        - factory 함수 체이닝을 위해 리턴 될 자식 클래스 프로퍼티에 factory 함수를 할당 한다.
    */
    function factory() {
        var parent = this === window ? Function : this;
        /*  
            parent 변수 : 호출한 클래스
            
            factory 함수 최초 호출 시 함수 내부의 this는 전역 객체인 window가 된다.
            최초 호출 시에 처리를 위해 모든 함수(클래스)의 부모역활을 하는 Function을 할당 한다. 

            
            호출한 객체가 window가 아닌 factory함수로 생성 된 클래스이면
            ex) A.factory();

            parent는 A클래스가 된다.
        */


        var F = function(){}        // 부모 클래스의 프로토타입과 자식 클래스의 프로토타입을 연결하는 브릿지 클래스 생성

        var target = function(){}   // 리턴될 자식 클래스 생성

        F.prototype = parent.prototype;     // 프로토타입 체인 구성(부모 클래스의 프로토타입을 가르키는 브릿지 클래스 F)
        
        target.prototype = new F();         // 프로토타입 체인을 이용한 부모 클래스 상속

        // 계층 구조를 확인하기 위해 생성자 및 속성에 참조 변수(부모, 자식 클래스) 할당
        target.prototype.constructor = target;  // 자식 클래스 할당
        target.parent = parent;                 // 부모 클래스 할당

        target.level = parent.level ? parent.level + 1 : 1;     //  함수 체이닝 레벨 확인을 위해 level 프로퍼티에 레벨 할당

        target.factory = aguments.callee;   // 함수 체이닝위해 factory 함수 할당

        return target; 
    }

    var A = factory();
    var B = A.factory();
    var C = B.factory();
    console.dir(C);
```
## 트리 구조의 클래스에서 생성자로 콜 되었을 경우 ##
*생성자 함수로 실행시 this 바인딩*
```js
    var Aclass = function(name) {
        this.name = name;
    }
    
    var Bclass = function(age) {
        this.age = age;
    }
    
    var Cclass = function(job) {
        this.job = job;
    }

    // 각 클래스를 this 바인딩 후 초기화
    var init = function (name, age, job) {
        Aclass.call(this, name);
        Bclass.call(this, age);
        Cclass.call(this, job);
    }

    var abcObj = new init("chanhyun", 29, "developer");

    console.dir(abcObj);
```
*트리구조 클래스 초기화*

```js

    /*
        var init = function(func, arg1, arg2, age3, argN....){ .... }

        아래를 전제로 실행한다.
        - 클래스 초기화는 역 주행 순을 고려하여 인자를 전달한다. 
        - 초기화되는 첫번째 인자는 콜백함수다. 
    */
    var Aclass = function(name) {
        this.name = name;
    }
    
    var Bclass = function(age) {
        this.age = age;
    }
    
    var Cclass = function(job) {
        this.job = job;
    }

    Aclass.parent = Bclass;
    Bclass.parent = Cclass;

    // factorial 패턴을 활용하여 트리 형태로 된 클래스를 역주행
    var init = function(func, job, age, name){


        if(func.parent){
            arguments.callee.call(this, func.parent, job, age, name);
            /*
                1. arguments.callee.call(Bclass "dev", 29, "ych") 실행 컨택스트 생성
                    
                    function(Bclass, "dev", 29, "ych" ){  
                        if(func.parent){
                            arguments.callee.call(this, func.parent, job, name, age);
                        };
                        
                        ........
                    };
                
                2. arguments.callee.call(Cclass "dev", 29, "ych") 실행 컨택스트 생성
                    function(Cclass, "dev", 29, "ych" ){  
                        if(func.parent){
                            arguments.callee.call(this, func.parent, job, name, age);
                        };
                        
                        ........
                    };

            */
        }
        
        // argument의 인덱스(초기화 시, 전달 되는 인자의 인덱스) =  slice(시작 인덱스(부모 클래스 갯수) + 마지막 인덱스(부모 클래스 갯수 + 맴버 변수 갯수 ))

        // 시작 인덱스 할당
        // 클래스의 부모 클래스 갯수 할당
        var pLength = func.parent ? func.parent.p : 0 ;
        
        // 마지막 인덱스 할당
        // 부모 클래스 갯수 + 맴버 변수 갯수 할당
        var cLength = func.p = func.length + pLength;
      

        func.apply(this, Array.from(arguments).slice(pLength+1, cLength+1))
    }

    var abcTreeObj = new init(Aclass,"dev", 29, "ych");

    console.dir(abcTreeObj);

```
