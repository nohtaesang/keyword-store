# You don't know js (this와 객체 프로토타입, 비동기와 성능) - 카일 심슨
## this
* 오해
    * this가 함수 그 자체를 가리킨다는 것은 오해이다.
    * this가 바로 함수의 스코프를 가리킨다는 것은 오해이다.
        * 어떤 면은 맞지만, 잘못 이해한 것이다.
        * 분명한 건 this는 어떤 식으로도 함수의 렉시컬 스코프를 참조하지 않는다는 사실이다.
* this 는?
    * this는 작성 시점이 아닌 런타임 시점에 바인딩 된다.
    * 함수 호출 당시 상황에 따라 콘텍스트가 결정된다.
    * 함수 선언 위치와 상관 없이 this 바인딩은 오로지 어떻게 함수를 호출했느냐에 따라 정해진다.
    * 어떤 함수를 호출하면 실행 콘텍스트가 만들어진다.
        * 여기엔 함수가 호출된 근원(콜스택)과 호출 방법, 전달된 인자 등의 정보가 담겨있다.
        * this 레퍼런스는 그중 하나로, 함수가 실행되는 동안 이용할 수 있다.
* 호출부
    * 함수를 호출한 지점
    * 호출 스택을 생각해 보면 된다.
    * 호출부는 현재 실행 중인 함수 직전의 호출 코드 내부에 있다.
    * this 바인딩 규칙을 좌우한다.

* 기본 바인딩
    * 가장 평범한 함수인 '단독 함수 실행'에 관한 규칙이다.
    * 나머지 규칙에 해당하지 않을 경우 적용되는 this의 기본 규칙이다.
    ```
    function foo(){
        console.log(this.a)
    }
    var a = 2
    foo() // 2
    ```
    * foo() 함수 호출 시 this.a 는 전역 객체 a이다.
    * 기본 바인딩이 적용되어 this는 전역 객체를 참조한다.
    * foo()의 호출부
        * foo()는 지극히 평범한 있는 그대로의 함수 레퍼런스로 호출했다.
        * 나머지 규칙을 논할 여지도 없이 기본 바인딩이 그대로 적용됐다.
        * 엄격 모드에서는 전역 객체게 기본 바인딩 대상에서 제외된다. (this는 undefined)
* 암시적 바인딩
    * 두 번쨰 규칙은 호출부에 콘텍스트 객체가 있는지, 즉 객체의 소유/포함 여부를 확인하는 것이다.
    ```
    function foo(){
        console.log(this.a);
    }
    var obj ={
        a:2,
        foo:foo
    }
    obj.foo() // 2
    ```
    * 앞에서 선언한 foo() 함수를 obj에서 프로퍼티로 참조하고 있다.
    * foo() 를 처음부터 foo 프로퍼티로 선언하든, 나중에 레퍼런스로 추가하든 객체가 이 함수를 정말로 '소유' 하거나 '포함' 한 것은 아니다.
    * 그러나 호출부는 obj 콘텍스트로 foo()를 참조하므로 obj 객체는 함수 호출 시점에 함수 레퍼런스를 '소유' 하거나 '포함' 한다고 볼 수 있다.
    * foo() 호출 시점에 이미 obj 객체 레퍼런스는 준비된 상태이다.
    * 함수 레퍼런스에 대한 콘텍스트 객체가 존재할 때 암시적 바인딩 규칙에 따르면 바로 이 콘텍스트 객체가 함수 호출 시 this에 바인딩 된다.
    * foo() 호출 시 obj는 this 이니 this.a는 obj.a가 된다.

* 암시적 소실
    ```
    function foo(){
        console.log( this.a )
    }
    var obj={
        a:2,
        foo:foo
    }
    var bar = obj.foo;
    var a = 'global'
    bar() // global
    ```
    * bar는 obj의 foo를 참조하는 변수처럼 보이지만 실은 foo를 직접 가리키는 또 다른 레퍼런스이다.
    * 게다가 호출부에서 그냥 평범하게 bar()를 호출하므로 기본 바인딩이 적용된다.
    ```
    function foo(){
        console.log(this.a)
    }
    function doFoo(fn){
        fn();
    }
    var obj={
        a:2,
        foo:foo
    }
    var a = 'global'
    doFoo(obj.foo) // global
    ```
    * 인자로 전달하는 건 일종의 암시적인 할당이다.
    * 따라서 예제처럼 함수를 인자로 넘기면 암시적으로 레퍼런스가 할당된다.
    * 이처럼 콜백 과정에서 this 바인딩의 행방이 묘연해지는 경우가 많다.
    * 콜백 호출 시 this를 개발자가 의도적으로 변경하면 더 복잡해질 수 있다.
    * 이 문제는 this를 고정해서 해결할 수 있다.

* 명시적 바인딩
    * 암시적 바인딩에서는 함수 레퍼런스를 객체에 넣기 위해 객체 자신을 변형해야 했다.
    * 함수 레퍼런스 프로퍼티를 이용하여 this를 간접적으로 바인딩했다.
    * 그런데 함수 레퍼런스 프로퍼티를 객체에 더하지 않고 어떤 객체를 this에 바인딩에 이용하겠다는 방법은 없을까?
        * 이럴 때 모든 자바스크립트 함수가 함께 사용할 수 있는 유틸리티가 있는데, call() 과 apply이다.
        * 두 메서드는 this에 바인딩 할 객체를 첫째 인자로 받아 함수 호출 시 이 객체를 this로 세팅한다.
        * this를 지정한 객체로 직접 바인딩 하므로 이를 명시적 바인딩이라 한다.
        ```
        function foo(){
            console.log(this.a)
        }
        var obj={
            a: 2
        }
        foo.call(obj) //2
        ```
        * foo.call() 에서 명시적으로 바인딩하여 호출하므로 this는 반드시 obj가 된다.
        * 객체 대신 단순 원시값을 인자로 전달하면 원시 값에 대응되는 객체로 래핑된다.
    * 하지만 아쉽게도 이렇게 명시적으로 바인딩 해도 앞에서 언급한 this 바인딩이 도중에 소실되거나 프레임워크가 임의로 덮어써 버리는 문제는 해결할 수 없다.

* 하드 바인딩
    * 명시적 바인딩을 약간 변형한 꼼수가 있다.
    ```
    function foo(){
        console.log(this.a)
    }
    var obj = {
        a: 2
    }
    var bar = function(){
        foo.call(obj)
    }
    bar(); // 2
    setTimeout(bar, 100) // 2
    bar.call(window) // 2
    ```
    * 함수 bar()는 내부에서 foo.call(obj) 로 foo를 호출하면서 obj를 this에 강제로 바인딩하도록 하드 코딩한다.
    * 따라서 bar를 어떻게 호출하든 이 함수는 항상 obj를 바인딩 하여 foo를 실행한다.
    * 이런 바인딩은 명시적이고 강력해서 하드 바인딩이라고 한다.
    * 하드 바인딩은 함수를 감싸는 형태의 코드는 다음고 같이 인자를 넘기고 반환 값을 돌려받는 창구가 필요할 때 주로 쓰인다.
    ```
    function foo(something){
        console.log( this.a, something)
        return this.a + something
    }
    var obj ={
        a: 2
    }
    var bar = function(){
        return foo.aplly(obj, arguments)
    }
    var b = bar(3) // 2, 3
    console.log(b) // 5
    ```
    * 재사용 가능한 헬퍼 함수를 쓰는 것도 같은 패턴이다.
    ```
    function foo(something){
        console.log( this.a, something)
        return this.a + something
    }
    function bind(fn, obj){
        return function(){
            return fn.apply(obj, arguments)
        }
    }
    var obj ={
        a: 2
    }
    var bar = bind(foo, obj)
    var b = bar(3) // 2, 3
    console.log(b) // 5
    ```
    * 하드 바인딩은 매우 자주 쓰는 패턴이어서 ES5 내장 유틸리티 Function.prototype.bind로 구현되어 있다.
    * bind()는 주어진 this 콘텍스트로 원본 함수를 호출하도록 하드 코딩된 새 함수를 반환한다.
* API 호출 콘텍스트
    * 많은 라이브러리 함수와 자바 스크립트 언어 및 호스트 환경에 내장된 여러 새로운 함수는 대개 콘텍스트라 불리는 선택적인 인자를 제공한다.
    * 이는 bind()를 써서 콜백 함수의 this를 지정할 수 없는 경우를 대비한 일종의 예비책이다.
    * 예를 들어, 다음과 같은 함수는 편의상 내부적으로 call() 이나 apply()로 명시적 바인딩을 대신해준다.
    ```
    function foo(el){
        console.log(el, this.name)
    }
    var obj={
        name:'taesang'
    }
    [1, 2, 3].forEach(foo, obj)
    ```
    * foo 호출시 obj를 this로 사용한다.

* new 바인딩
    * 네 번째 바인딩 규칙이다.
    * new 연산자
        * 자바스크립트에서 new는 의미상 클래스 지향적인 기능과 아무 상관이 없다.
            * 생성자
                * 자바스크립트의 생성자는 앞에 new 연산자가 있을 때 호출되는 일반 함수에 불과하다.
                * 클래스에 붙은 것도 아니고 클래스 인스턴스화 기능도 없다.
                * 심지어 특별한 형태의 함수도 아니다.
                * 단지 new를 사용하여 호출할 때 자동으로 붙들려 실행되는 그저 평범한 함수이다.
            * new는 함수를 생성하는 호출이라고 볼 수 있다.
            * 과정
                * 새 객체가 만들어진다.
                * 새로 생성된 객체의 [[Prototype]]이 연결된다.
                * 새로 생성된 객체는 해당 함수 호출 시 this로 바인딩 된다.
                * 이 함수가 자신의 또 다른 객체를 반환하지 않는 한 new와 함께 호출된 함수는 자동으로 새로 생성된 객체를 반환한다.
                ```
                function foo(a){
                    this.a = a
                }
                var bar = new foo(2)
                console.log(bar.a) //2
                ```
                * 앞에 new를 붙여 foo()를 호출했고 새로 생성된 객체는 foo 호출 시 this에 바인딩 된다.
                * 따라서 결국 new는 함수 호출 시 this를 새 객체와 비인딩 하는 방법이다.

* 바인딩의 우선 순위
    ```
    function foo(){
        console.log(this.a)
    }
    var obj1 ={
        a: 2,
        foo:foo
    }
    var obj2 = {
        a:3,
        foo:foo
    }
    obj1.foo() // 2
    obj2.foo() // 3
    obj1.foo.call(obj2) // 3
    obj2.foo.call(obj1) // 2
    ```
    * 명시적 바인딩이 암시적 바인딩보다 우선순위가 높음을 알 수 있다.
    * 따라서 암시적 바인딩을 확인하기 전에 먼저 명시적 바인딩이 적용됐는지 살펴야 한다.
    ```
    function foo(something){
        this.a = something
    }
    var obj1={
        foo: foo
    }
    var obj2={}
    
    obj1.foo(2) // 2
    obj1.a // 2

    obj1.foo.call(obj2, 3)
    obj2.a // 3

    var bar = new obj1.foo(4) 
    obj1.a // 2
    bar.a // 4
    ```
    * new 바인딩이 암시적 바인딩보다 우선순위가 높다.
    * 하드 바인딩의 물리적인 작동 원리
        * Function.prototype.bind()는 어떤 종류든 자체 this 바인딩을 무시하고 주어진 바인딩을 적용하여 하드 코딩된 새 래퍼 함수를 생성한다.
        * 따라서 명시적 바인딩의 한 형태인 하드 코딩이 new 바인딩보다 우선 순위가 높고 new로 오버라이드할 수 없다.
        ```
        function foo(something){
            this.a = something
        }

        var obj1 ={}
        var bar = foo.bind(obj1);
        bar(2)
        obj1.a // 2

        var baz = new bar(3)
        obj1.a // 2
        baz.a // 3
        ```
        * bar 는 obj1에 하드 바인딩 됐는데, 짐작대로 new bar(3) 실행 후에도 obj1.a 값은 3으로 바뀌지 않는다.
        * 그 대신 obj1에 하드 바인딩 된 bar() 호출은 new로 오버라이드할 수 있다.
        * 굳이 new로 하드 바인딩을 오버라이드 하려는 이유는 무엇일까?
            * 기본적으로 this 하드 바인딩을 무시하는 함수를 생성하여 함수 인자를 전부 또는 일부만 미리 세팅해야 할 때 유용하다.
            * bind() 함수는 최초 this 바인딩 이후 전달된 인자를 원 함수의 기본 인자로 거장하는 역할을 한다.
            ```
            function foo(p1, p2){
                this.val = p1+p2
            }

            // new 호출시 오버라이드 되므로 null을 넣어도 된다.    
            var bar = foo.bind(null, "p1")
            var baz = new bar("p2")
            baz.val = p1p2
            ```      
* this 확정 규칙
    * new로 함수를 호출 했는가?
        * 맞으면 새로 생성된 객체가 this이다.
    * call과 apply로 함수를 호출, 이를테면 bind 하드 바인딩 내부에 숨겨진 형태로 호출됐는가?
        * 맞으면 명시적으로 지정된 객체가 this이다.
    * 함수를 콘텍스트, 즉 객체를 소유 또는 포함하는 형태로 호출했는가?
        * 맞으면 바로 이 콘텍스트 객체가 this이다.
    * 그 외의 경우에 this는 기본값으로 세팅된다.

* 바인딩 예외
    * this 무시
        * call, apply, bind 메서드에 첫 번째 인자로 null 또는 undefined를 넘기면 this 바인딩이 무시되고 기본 바인딩 규칙이 적용된다.
            * null같은 값으로 바인딩을 하는 이유
                * apply는 함수 호출 시 다수의 인자를 배열 값으로 펼처보내는 용도로 자주 쓰인다.
                * bind()도 유사한 방법으로 인자들(미리 세팅된 값들)을 커링하는 메서드로 많이 사용한다.    
                ```
                function foo(a, b){
                    console.log(a, b)
                }
                foo.apply(null, [2,3]) //a:2, b:3
                foo.call(null, 2, 3) //a:2 ,b:3

                var bar = foo.bind(null, 2)
                bar(3) // a:2, b:3
                ```
    * 간접 레퍼런스
        * 간접 레퍼런스가 생성되는 경우로, 함수를 호출하면 무조건 기본 바인딩 규칙이 적용되어 버린다.
        * 간접 레퍼런스는 할당문에서 가장 빈번하게 발생한다.
        ```
        function foo(){
            console.log(this.a)
        }
        var a = 2;
        var o = {a: 3, foo: foo};
        var p = {a: 4}
        o.foo() //3
        (p.foo = o.foo)(); // 2
        ```
        * 할당 표현식 p.foo = o.foo 의 결괏값은 원 함수 객체의 레퍼런스이다.
        * 실제로 호출부는 처음 예상과 달리 p.foo(), o.foo()가 아니고 foo() 이다.
    * 소프트 바인딩
        * 하드 바인딩은 함수의 유연성을 떨어트린다.
        * this를 암시적으로 바인딩 하거나 나중에 다시 명시적으로 바인딩 하는 식으로 수동으로 오버라이드 하는 것은 불가능하다.
        * 그래서 나온 것이 소프트 바인딩이다.
        * 호출 시점에 this를 체크하는 부분에서 주어진 함수를 래핑하여 전역 객체나 undefined일 경우엔 미리 준비한 대체 기본 객체로 세팅한다.
        * 그 외의 경우 this는 손대지 않는다.
        ```
        function foo(){
            console.log(this.name)
        }
        var obj = {name:'a'}
        var obj2 = {name:'b'}
        var obj3 = {name:'c}
        var fooObj = foo.softBind(obj)
        fooObj() // a

        obj2.foo = foo.softBind(obj)
        obj2.foo() // b

        fooObj.call(obj3) // c

        setTimeout(obj2.foo, 10)  // a
        ```
* 어휘적 this
    * ES6부터는 이 규칙들을 따르지 않는 특별한 함수가 있다.
        * 화살표 함수
            * Enclosing scope를 보고 this를 알아서 바인딩 한다.
            ```
            function foo(){
                return (a) => {
                    console.log(this.a) // this는 foo() 에서 상속된다.
                }
            }
            var obj ={ a: 2}
            var obj2 ={ a: 3}
            var bar = foo.call(obj) // 2
            bar.call(obj2) // 2 (3이 아니다.)
            ```
            * foo() 내부에서 생성된 화살표 함수는 foo() 호출 당시 this를 무조건 어휘적으로 포착한다.
            * foo()는 obj 에 this가 바인딩 되므로 bar의 this역시 obj로 바인딩 된다.
            * 화살표 함수의 어휘적 바인딩은 절대로 오버라이드할 수 없다. (new로도 불가능)
            * 화살표 함수는 this를 확실히 보장하는 수단으로 bind() 를 대체할 수 있고 겉보기에도 끌리는 구석이 있다.
            * 하지만 결과적으로 더 잘 알려진 렉시컬 스코프를 쓰겠다고 기존의 this 체계를 포기하는 형국이라는 점을 간과하면 안 된다.
            * ES6 이전에도 화살표 함수의 기본 기능과 크게 다르지 않은, 나름대로 많이 쓰이는 패턴이 있다.
            ```
            function foo(){
                var self = this; // this를 어휘적으로 포착한다.
                setTimeout(function(){
                    console.log(self.a)
                },100)
            }
            var obj={a: 2}
            foo.call(obj) // 2
            ```
            * self = this나 화살표 함수 모두 bind() 대신 사용 가능한 좋은 해결책이지만 this를 제대로 이해하고 수용하기보단 골치 아픈 this에서 도망치려는 꼼수라고 할 수 있다.
        * 오직 렉시컬 스코프만 사용하고 가식적인 this 스타일의 코드는 접어둔다.
        * 필요하면 bind() 까지 포함하여 완전한 this 스타일의 코드를 구사하되 self = this나 화살표 함수 같은 소위 '어휘적 this'꼼수는 삼가야 한다.
        * 두 스타일 모두 적절히 혼용하여 할 수는 있겠지만, 동일 함수 내에서 똑같은 것을 찾는데 서로 다른 스타일이 섞여 있으면 관리도 안되고 혼란스러운 코드가 될 것이다.
* 정리
    *    
        * 함수 실행에 있어서 this 바인딩은 함수의 직접적인 호출부에 따라 달라진다.
        * 일단 호출부를 식별한 후 다음 4가지 규칙을 열거한 우선순위에 따라 적용한다.
            * new 로 호출했다면 새로 생성된 객체로 바인딩된다.
            * call 이나 apply 또는 bind로 호출했다면 주어진 객체로 바인딩된다.
            * 호출의 주체인 콘텍스트 객체로 호출됐다면 바로 이 콘텍스트 객체로 바인딩 된다.
            * 기본 바인딩에서 엄격 모드는 undefined, 그 밖엔 전역 객체로 바인딩 된다.
    * 
        * 실수로 또는 얘기치 않게 기본 바인딩 규칙이 적용되는 경우를 조심해야 한다.
        * this 바인딩을 안전하게 넣고 싶으면 ø = Object.call(null) 처럼 DMZ 객체를 자리 끼움 값으로 바꿔 넣어 뜻하지 않은 부수 효과가 전역 객체에서 발생하지 않게 해야한다.
    * 
        * ES6 화살표 함수는 표준 바인딩 규칙을 무시하고 렉시컬 스코프로 this를 바인딩 한다.
        * 즉, 에두른 함수 호출로부터 어떤 값이든 this 바인딩을 상속한다.




## 객체
* 객체 생성방식
    * 리터럴
        * var myObj ={
            key:value
            //...
        }
    * 생성자
        * var myObj = new Object()
        * myObj.key = value
    * 유일한 차이점은 리터럴 형식은 한 번에 선언으로 다수의 키/값 쌍을 프로퍼티에 추가할 수 있지만, 생성자 형식은 한 번에 한 프로퍼티만 추가할 수 있다.
    * 생성자 형식으로 객체를 생성하는 일은 매우 드물다.
    * 거의 리터럴 형식을 사용하며 대부분의 내장 객체도 마찬가지다.

* 타입
    * 종류
        * null
        * undefined
        * boolean
        * number
        * string
        * object
        * symbol
    * 단순 원시 타입은 객체가 아니다.
        * string, number, boolean, null, undefined
        * typeof null 이 object로 나오기 때문에 null의 타입이 객체라고 잘못 알고 있는 경우가 많다.
        * 자바스크립트는 모든 것이 객체다, 라는 말은 옳지 않다.
    * 반면 복합 원시 타입이라는 독특한 객체 하위 타입이 있다.
        * function은 객체(정확히는 호출 가능한 객체)의 하위 타입이다.
        * 배열 역시 추가 기능이 구현된 객체의 일종이다.

* 내장 객체
    * 내장 객체라고 부르는 객체 하위 타입도 있다.
    * 일부는 이름만 보면 대응되는 단순 원시 타입과 직접 연관되어 보이지만 실제 관계는 뜻밖에 복잡하다.
    * 종류
        * String
        * Number
        * Boolean
        * Object
        * Function
        * Array
        * Date
        * RegExp
        * Error
    * 내장 객체는 진짜 타입 처럼 보이는 데다 자바의 String 클래스처럼 타 언어와 유사한 겉모습 때문에 꼭 클래스처럼 느껴진다.
    * 그러나 이들은 단지 자바스크립트의 내장 함수일 뿐 각각 생성자로 사용되어 주어진 하위 타입의 새 객채를 생성한다.
    * 다행히도 자바스크립트 엔진은 상황에 맞게 (리터럴로 선언한)원시 값을 (생성자를 이용하여 생성한)객체로 자동 강제 변환하므로 명시적으로 객체를 생성하지 않아도 된다.
    * 여러 자바스크립트 커뮤니티에서도 되도록 생성자 형식은 지양하고 리터럴 형식을 권장한다.
    * 객체 래퍼 형식이 없는 null 과 undefined는 그 자체로 유일 값이다.
    * 반대로 Date 값은 리터럴 형식이 없어서 반드시 생성자 형식으로 생성해야 한다.
    * Objects, Arrays, Functions, RegExps는 형식과 무관하게 모두 객체이다.
    * 생성자 형식은 리터럴 형식보다 옵션이 더 많은 편이다.
    * 어느 쪽이든 결국 생성되는 객체는 같으므로 좀 더 간단한 리터럴 형식을 많이 쓴다.
    * 추가 옵션이 필요한 경우에만 생성자 형식을 사용한다.
    * Error 객체는 예외가 던져지면 알아서 생성되니 명시적으로 생성할 일은 드물다.

* 객체의 내용
    * 객체는 특정한 위치에 저장된 모든 타잆의 값, 즉 프로퍼티로 내용이 채워진다.
    * 내용이러고 하니 마치 실제로 객체 내부에 프로퍼티 값들이차곡차곡 보관된 느낌이 들지만 겉모습만 그렇다.
    * 엔진이 값을 저장하는 방식은 구현 의존적이다.
        * 이는 객체 컨테이너에 담지 않는 게 일반적이다.
        * 객체 컨테이너에는 실제로 프로퍼티 값이 있는 곳을 가리키는 포인터 역할을 담당하는 프로퍼티명이 담겨있다.
            * var myObject ={
                a: 2
            }
            * myObject.a // 2
            * myOjbect["a"] // 2
        * myObject 객체에서 a 위치의 값에 접근하려면 '.'연산자  또는 '[]' 연산자를 사용한다.
        * 일반적으로 .a 구문을 '프로퍼티 접근'
        * ["a"] 접근을 '키 접근' 이라고 한다.
        * 차이점은, 키 접근은 UTF-8/유니코드 호환 문자열이라면 모두 프로퍼티 명으로 쓸 수 있다.
            * ex) Super-Fun!
        * 또한 키 접근 구문에선 위치를 문자열로 나타낼 수 있어서 문자열을 프로그램으로 조합하는 일도 가능하다.
        * 객체 프로퍼티명은 언제나 문자열이다.
        * 문자열 이외의 다른 원시 값을 쓰면 우선 문자열로 변환된다.
        * 배열 인덱스로도 사용하는 숫자도 마찬가지이다.

* 계산된 프로퍼티명
    * myObj[ ] 같은 프로퍼티 접근 구문은 myObj[preifx + name] 형태의 계산식 값으로 키 이름을 나타낼 때 유용하다.
    * ES6 부터는 계산된 프로퍼티명이라는 기능이 추가되었다.
    * 객체 리터럴 선언 구문의 키 이름 부분에 해당 표현식을 넣고 [ ]로 감싸면 된다.
        * var prefix =  'foo'
        * var myObj = {
            [prefix + "bar"]: 'hello',
            [prefix + "baz"]: 'world'
        };
        * myObj[foobar] //hello
        * myObj[foobaz] //world
    * 계산된 프로퍼티명은 ES6 심볼에서 가장 많이 사용되지 않을까 싶다.

* 프로퍼티와 메서드
    * 엄밀히 말해 함수는 결코 객체에 속하는 것이 아니다.
    * 객체 레퍼런스로 접근한 함수를 그냥 메서드라 칭하는 건 그 의미를 지나치게 확대해 해석한 것이다.
    * this 래퍼런스를 스스로 지닌 함수도 있고 호출부의 객체 레퍼런스를 가리킬 때도 있긴 하지만 그렇다고 이렇게 사용되는 함수가 다른 함수들보다 메서드답다고 말하는 건 이상하다.
    * this는 호출부에서 런타임 시점에 동적으로 바인딩 되므로 객체와는 기껏해야 간접적인 관계일 수 밖에 없기 때문이다.
    * 객체에 존재하는 프로퍼티에 접근할 때마다 반환 값 타입에 상관없이 항상 프로퍼티 접근을 하고 이런 식으로 함수를 가져왔다고 해서 저절로 함수가 메서드가 되는 건 아니다.
    * 프로퍼티 접근 결과 반환된 함수 역시 마찬가지이다.
    * 같은 함수를 가리키는 개별 레퍼런스일 뿐, 뭔가 특별한 다른 객체가 '소유한' 함수라는 의미는 아니다.
    * 무난하게 결론을 내리자면, 자바스크립트에서 '함수'와 메서드란 말은 서로 바꿔 사용할 수 있다.

* 배열
    * 배열도 [ ]로 접근하는 형태이지만, 이미 언급한 대로 값을 저장하는 방법과 장소가 더 체계적이다.
    * 배열은 숫자 인덱싱, 즉 인덱스라는 양수로 표현된 위치에 값을 저장한다.
    * 인덱스는 양수이지만 배열 자체는 객체여서 배열에 프로퍼티를 추가하는 것도 가능하다.
    * . 이나 [] 구문에 상관없이 이름 붙은 프로퍼티를 추가해도 배열 길이에는 변함이 없다.
    * 키/값 저장소는 객체, 숫자 인덱스를 가진 저장소는 배열을 쓰는게 좋다.

* 객체 복사
    * 보통 이미 만들어진 객체를 다른 곳에 리터럴로 대입하면 사본이 아닌 래퍼런스로 참조하는 형식이 된다.
    * 'JSON 안전한 객체'는 쉽게 복사할 수 있으므로 하나의 대안이 될 수는 있다.
        * var newObj = JSON.parse(JSON.stringify(somObj));
        * 100% 안전한 객체여야 한다.
    * ES6부터는 Object.assign() 메서드를 제공한다.
        * 이 메서드의첫째 인자는 타깃 객체이고 둘 쨰 인자 이후는 하나 또는 둘 이상의 소스 객체로, 소스 객체의 모든 열거 가능한 것과 보유키를 순회하면서 타깃 객체로 복사한다.
        * var newObj = Object.assign({}, someObj)
        * Object.assign()은 순수하게 = 할당에 의해서만 복사되므로 writable 같은 특수한 프로퍼터의 속성은 타깃 객체에 복사되지 않는다.
* Object.defineProperty() 
    * ES5 부터 모든 프로퍼티는 프로퍼티 서술자로 표현된다.
        * var myObj ={
            a: 2
        }
        * Object.getOwnPropertyDescriptor(myObj, "a"); 
            * 평범한 객체 프로퍼티 a의 프로퍼티 서술자를 조회.
            * 2 말고도 writable, enumerable, configurable의 세 가지 특성이 더 있다.
            * configurable이 true일 때 defineProperty()로 새로운 프로퍼티를 추가하거나 수정할 수 있다.
            * writable을 false로 바꾸면, 수정을 할 수 없으며 엄격모드에서는 에러가 난다.
    * 객체 상수, 확장 금지, 봉인, 동결...

* [[GET]]
    * var myObj = {
        a: 2
    }
    * myObj.a //2
    * myObj.a는 누가 봐도 프로퍼티 접근이지만 보이는 것처럼 a란 이름의 프로퍼티를 myObj에서 찾지 않는다.
    * 명세에 따르면 실제로 이 코드는 myObj에 대해 [[GET]] 연산을 한다. ([[GET]]() 같은 함수 호출)
    * 기본적으로 [[GET]]연산은 주어진 이름의 프로퍼티를 먼저 찾아보고 있으면 그 값을 반환한다.
    * 프로퍼티를 찾아보고 없으면 [[GET]] 연산 알고리즘은 다른 중요한 작업을 하도록 정의되어 있는데, 이 부분은 [[Prototye]] 연쇄 순회 이다.
    * 주어진 값을 어떻게 해도 찾을 수 없으면 [[GET]] 연산은 undefined를 반환한다.
        * var myObj ={
            a: undefined
        }
        * myObj.a
        * myObj.b
        * 이는 둘다 같은 값을 반환하지만, 후자가 전자보다 더 일을 많이 한다고 볼 수 있다.
* [[PUT]]
    * 언뜻 보기에 객체 프로퍼티에 값을 할당하는 일은 그저 [[PUT]]을 호출하여 주어진 객체에 프로퍼티를 셋팅/생성 하는 일이 전부일 듯싶지만 실제로는 복잡하다.
    * [[PUT]] 을 실행하면 주어진 객체에 프로퍼티가 존재하는지(가장 결정적 영향을 미치는 요소) 등 여러 가지 요소에 따라 이후 작동 방식이 달라진다.
    * [[PUT]] 알고리즘은 이미 존재하는 프로퍼티에 대해 대략 다음의 확인 절차를 밟는다.
        * 프로퍼티가 접근 서술자인가? 맞으면 세터를 호출한다.
        * 프로퍼티가 writable:false인 데이터 서술자인가? 맞으면 비엄격 모드에서 조용히 실패하고 엄격 모드에서는 TypeError가 발생한다.
        * 이외에는 프로퍼티에 해당 값을 세팅한다.
    * 객체에 존재하지 않는 프로퍼티라면 [[PUT]] 알고리즘은 훨씬 더 미묘하고 복잡해진다. (프로토타입에서 설명)

* 게터와 세터
    * [[PUT]], [[GET]] 기본 연산은 이미 존재하거나 전혀 새로운 프로퍼티에 값을 세팅하거나 기존 프로퍼티로부터 값을 조회하는 일을 각각 담당한다.
    * ES5 부터는 게터/세터를 통해 (객체 수준이 아닌) 프로퍼티 수준에서 이러한 로직을 오버라이드할 수 있다.
    * 게터/세터는 각각 실제로 값을 가져오는/세팅하는 감춰진 함수를 호출하는 프로퍼티다.
    * 프로퍼티가 게터 또는 세터 어느 한쪽이거나 동시에 게터/세터가 될 수 있게 정의한것을 '접근 서술자' 라고 한다.
    * 접근 서술자에서는 프로퍼티의 값과 writable 속성은 무시되며 대신 프로퍼티의 겟/셋 속성이 중요하다.
    ```
        * var myObj ={
            // 'a'의 게터를 정의한다.
            get a(){
                return 2;
            }
        }
        * Object.defineProperty(
            myObj,
            "b",
            {
                //b의 게터를 정의한다.
                get: function(){return this.a*2},
                //b가 프로퍼티로 확실히 표시되게 한다.
                enumarable: true
            }
        )
        * myObj.a // 2
        * myObj.b // 4
    ```
    * get a(){ } 처럼 리터럴 구문으로 기술하든, defineProperty()로 명시적 정의를 내리든 실제로 값을 가지고 있지 않은 객체에 프로퍼티를 생성하는 건 같지만 프로퍼티에 접근하면 자동으로 게터 함수를 은밀하게 호출하여 어떤 값이라도 게터 함수가 반환한 값이 결괏값이 된다.
    ```
    * var myObj ={
        get a() {
            return 2
        }
    }
    * myObj.a =3
    * myObj.a // 2
    ```
    * a의 게터가 정의되어 있으므로 할당문으로 값을 세팅하려고 하면 에러 없이 조용히 무시된다.
    * 세터가 있어도 커스텀 게터가 2만 반환하게 하드 코딩되어 있어서 세팅은 있으나 마나이다.
    * 짐작하겠지만 프로퍼티 단위로 기본 [[PUT]] 연산을 오버라이드하는 세터가 정의되어야한다.
    * 게터와 세터는 항상 둘 다 선언하는 것이 좋다.
    ```
    * myObj ={
        get a(){
            return thia._a_;
        }

        set a(val){
            this._a_ = val * 2
        }
    };
    * myObj.a =2;
    * myObj.a // 4
    ```
* 존재 확인
    * 객체에 어떤 프로퍼티가 존재하는지는 굳이 프로퍼티 값을 얻지 않고도 확인할 수 있다.
        * ("a" in myObj)
        * myObj.hasOwnProerty('a')
    * in 연산자는 어떤 프로퍼티가 해당 객체에 존재하는지 아니면 이 객체의 [[Prototype]] 연쇄를 따라갔을 때 상위 단계에 존재하는지 확인한다.
        * 언뜻 in 연산자가 내부 값이 존재하는지까지 확인하는 것처럼 보이지만 실은 프로퍼티명이 있는지만 본다.
        * 그러므로 배열에서 4 in [2, 4, 6] 처럼 쓰면 예상대로 실행되지 않는다.
    * 이와 달리 hasOwnProperty()는 단지 프로퍼티가 객체에 있는지만 확인하고 [[Property]] 연쇄는 찾지 않는다.
    * 거의 모든 일반 객체는 Object.prototype 위임을 통해 hasOwnProerty()에 접근할 수 있지만 간혹 Object.create(null) 과 같이 Object.prototype과 연결되지 않은 객체는 myObj.hasOwnProerty() 처럼 사용할 수 없다.
    * 이럴 경우엔 Object.prototype.hasOwnProperty.call(myObj,"a") 처럼 기본 메서드를 빌려와 명시적 바인딩을 하면 된다.

* 열거
    * 프로퍼티 서술자 속성 중 하나인 enumerable
    ```
    * var myObj = {}
    * Object.definedProperty(
        myObj,
        "a",
        {enumerable:true, value: 2}
    )
    * Object.defineProperty(
        myObj,
        "b",
        {enumerable:false, value:3}
    )
    * myObj.b //3
    * ("b" in myObj) // true
    * myObj.hasOwnProperty('b') // true
    * for(var k in myObj){
        console.log(k, myObj[k])
    } // "a" 2
    ```
    * myObj.b는 실제 존재하는 프로퍼티로 그 값에도 접근할 수 있지만, for...in loop에서는 자취를 감춰버린다.
    * 이처럼 '열거 가능하다'는 건 기본적으로 '객체 프로퍼티 순회 리스트에 포함된다'는 뜻이다.
    * for...in loop 를 배열에서 사용하면 배열 인덱스뿐만 아니라 다른 열거 가능한 프로퍼티까지 순회 리스트에 포함되는 원치 않은 결과가 발생할 수 있으므로 객체에만 쓰는 것이 좋다. 
    * 배열은 숫자 인덱스로 순회하는 편이 바람직하다.
    * myObj.propertyIsEnumerable('b') // false
        * 어떤 프로퍼티가 해당 객체의 직속 프로퍼티인 동시에 enumerable:true인지 검사한다.
    * Object.keys(myObj) // ["a"]
    * Object.getOwnPropertyNames(myObj) // ["a", "b"]

* 순회
    * for...in loop 는 열거 가능한 객체 프로퍼티를 차례로 순회한다.
    * var myArr = [1, 2, 3]
    * for(var i = 0 ; i < myArr.length ; i ++){
        console.log(myArr[i])
    }
    * 그러나 이 코드는 인덱스를 순회하면서 해당 값을 사용할 뿐 값 자체를 순회하는 것은 아니다.
    * ES5부터는 forEach(), every(), some() 등의 배열 관련 순회 헬퍼가 도입됐다.
    * 이 함수들은 배열의 각 원소에 적용할 콜백 함수를 인자로 받으며, 원소별로 반환 값을 처리하는 로직만 다르다.
    * forEach()는 배열 전체 값을 순회하지만 콜백 함수의 반환 값은 무시한다.
    * every()는 배열 끝까지 또는 콜백 함수가 false를 반환할 때까지 순회한다.
    * some()은 배열 끝까지 또는 콜백 함수가 true를 반활할 떄까지 순회한다.
    * for...of
        * 순회할 원소의 순회자 객체(iterator object, 명세식으로 말하면 @@iterator 라는 기본 내부 함수)가 있어야 한다.
        * 순회당 한 번씩 이 순회자 객체의 next() 메서드를 호출하여 연속적으로 반환 값을 순회한다.
    * 배열은 @@iterator가 내장된 덕분에 손쉽게 for...of 루프를 사용할 수 있다.
    ```
    * var arr =[1, 2, 3]
    * var it = arr[Sylmbol.iterator]();
    * it.next() // {value:1 , done: false}
    * it.next() // {value:2 , done: false}
    * it.next() // {value:3 , done: false}
    * it.next() // { done: true }
    ```
    * 일반 객체 내부에는 @@iterator가 없다.
    * 순회하려는 객체의 기본 @@iterator를 손수 정의할 수도 있다.
    
    
* delete
    * 자바스크립트에서의 delete는 객체 프로퍼티를 날려버리는 기능이 고작이다.
    * 마지막 프로퍼티를 삭제해도 객체는 여전히 메모리에 할당된 채로 있기 때문이다.

* 정리
    * 
        * 자바스크립트 객체는 리터럴 형식과 생성자 형식 두 가지 형태를 가진다.
        * 대부분 리터럴 형식을 쓰는 편이 좋지만 생성 시 옵션을 더 주기 위해 생성자 형식을 쓰는 경우도 더러 있다.
    * 
        * 많은 사람이 '자바스크립트는 모든 것이 객체이다'라고 말하지만 사실과 다르다.
        * 객체는 6개의 원시 타입 중 하나고 함수를 비롯한 하위 타입이 있다.
    * 
        * 객체는 키/값의 쌍을 모아 놓은 저장소이다.
        * 값은 프로퍼티를 통해 접근할 수 있다.
        * 프로퍼티에 접근하면 엔진 내부에서는 실제로 기본 [[GET]]연산을 호출하는데, 객체 자체에 포함된 프로퍼티뿐만 아니라 필요하면 [[Prototy[e]]] 연쇄를 순회하며 찾아본다.
    * 
        * 프로터티는 프로퍼티 서술자를 통해 제어 가능한 writable, configurable 등의 특정한 속성을 지닌다.
        * 그리고 객체는 Object.preventExtensions(), Object.Seal(), Object.freeze() 등을 이용하여 자신에게, 그리고 자신이 가지고 있는 프로퍼티 까지 여러 단계의 불변성을 적용할 수 있다.
    * 
        * 프로퍼티가 반드시 값을 가져야 하는 것은 아니며 게터/세터로 '접근자 프로퍼티' 형태를 취할 수도 있다.
        * 예를 들어, 열거 가능성을 조정하여 for...in 루프 순회 시 노출 여부를 마음대로 바꿀 수 있다.
    * 
        * ES6 부터는 for...of 구문에서 한 번에 하나씩 다음 데이터값으로 이동하는 next() 메서드를 가진 내장/커스텀 @@iterator 객체를 통해 자료 구조 에서 여러 값을 순회할 수 있다.


## 클래스와 객체 혼합

* 클래스
    * 특정 형태의 코드와 구조를 형성하낟.
    * 실생활 영역의 문제를 소프트웨어로 모델링 하기 위한 방법이다.
    * 데이터는 자신을 기반으로 하는 실행되는 작동과 연관된다.
    * 따라서 데이터와 작동을 함께 잘 감싸는 것이 올바른 설계라고 한다.

* 자바스크립트 클래스
    * ES6부터는 아예 class 라는 키워드가 명세에 정식으로 추가됐다.
    * 하지만, 자바스크립트에 클래스가 있는 것은 아니다.
    * 클래스는 디자인 패턴이므로 적잖이 공을 들이면 고전적인 클래스 기능과 얼추 비슷하게 구현할 수는 있다.
    * 그러나 클래스처럼 보이는 구문일 뿐이며 개발자들이 클래스 디자인 패턴으로 코딩할 수 있도록 자바스크립트 체계를 억지로 고친 것에 불과하다.
    * 어쨌든 클래스는 소프트웨어 디자인 패턴 중 한 가지 옵션일 뿐이니 자바스크립트에서 클래스를 쓸지 말지는 결국 자신이 결정할 문제이다.

* 그래서 일단 안하고 넘어간다.

## 프로토타입
* [[Prototype]]
    * 자바스크립트 객체는 [[Prototye]]이라는 내부 프로퍼티가 있다.
    * 다른 객체를 참조하는 단순 래퍼런스로 사용한다.
    * 거의 모든 객체가 이 프로퍼티에 null 아닌 값이 생성 시점에 할당된다.
    * 드물긴 하지만 [[Prototype]] 링크가 텅 빈 객체도 가능하다.
    ```
    var obj ={a: 2}
    obj.a //2
    ```
    * 3장 객체에서 obj.a 처럼 객체 프로퍼티 참조시 [[Get]] 이 호출된다고 했었다.
    * [[Get]] 은 기본적으로 객체 자체에 해당 프로퍼티가 존재하는지 찾아보고 존재하면 그 프로퍼티를 사용한다.
    * proxy가 개입하면 일반적으로 [[Get]]과 [[Put]]은 작동하지 않는다.
        * [https://hyunseob.github.io/2016/08/17/javascript-proxy/](https://hyunseob.github.io/2016/08/17/javascript-proxy/)
    * 하지만 obj에 a란 프로퍼티가 없으면 다음 관심사는 이 객체의 [[Prototype]] 링크다.
    * [[Get]]은 주어잔 프로퍼티를 객체에서 찾지 못하면 곧바로 [[Prototype]] 링크를 따라가면서 수색 작전을 벌인다.
    ```
    var anotherObj ={
        a:2
    }
    var obj = Object.create(anotherObj);
    obj.a // 2
    ```
    * Object.create() 는 특정 객체의 링크를 가진 객체를 생성한다.
    * obj는 anotherObj와 [[Prototype]]이 링크됐다.
    * 일치하는 프로퍼티명이 나올 때 까지, 아니면 [[Prototype]] 연쇄가 끝날 때까지 같은 과정이 계속된다.
    * 연쇄 끝에 이르러서도 프로퍼티가 발견되지 않으면 [[Get]]은 결괏값으로 undefined를 반환한다.
    * for...in 루프에서 객체를 순회할 때도 [[Prototype]]연쇄의 검색 과정과 비슷한 방식으로 연쇄가 이루어진다.
    * in 연산자로 객체에 프로퍼티 유무를 확인할 때에는 객체의 연쇄를 전부 샅샅이 뒤진다.
    ``` 
    var anotherObj ={
        a:2
    }
    var obj = Object.create(anotherObj);
    for(var k in obj){
        console.log(k + "를 발견")
    }
    ```
    * 어떤 방식이든 객체에서 프로퍼티를 검색할 때 [[Prototype]]연쇄를 한 번에 한 링크씩 계속 찾는다.

* Object.prototype
    * [[Prototype]] 연쇄가 끝나는 지점은 어디일까?
        * 일반 [[Prototype]] 연쇄는 결국 내장 프로토타입 Object.prototype에서 끝난다.
        * 모든 자바스크립트 객체는 Object.prototype객체의 자손이다.
        * 따라서 Object.prototype에는 자바스크립트에서 두루 쓰이는 다수의 공용 유틸리티가 포함되어 있다.
            * .toString()이나 .valueOf(), hasOwnProperty(), isPrototypeOf() 등이 있다.

* 프로퍼티 세팅과 가려짐
    ```
    obj.foo = 'bar'
    ```
    * foo라는 이름의 평범한 데이터 접근 프로퍼티가 obj객체에 직속(해당 프로퍼티에서 곧 바로 찾을 수 있는 경우)된 경우 이 할당문은 기존의 프로퍼티 값을 고치는 단순한 기능을 할 뿐이다.
    * foo가 직속된 프로퍼티가 아니면 [[Get]]처럼 [[Prototype]] 연쇄를 순회하기 시작한다.
    * 그렇게 해도 foo가 발견되지 않으면 그제야 foo라는 프로퍼티를 obj 객체에 추가한 후 주어진 값을 할당한다.
    * foo가 [[Prototype]]의 상위 연쇄 어딘가에 존자하면 obj.foo = 'bar' 할당문의 실행 과정에서 미묘한 일이 벌어진다.
    * shadowing
        * foo라는 프로퍼티명이 obj객체와 이 객체를 기점으로 한 [[Prototype]] 연쇄의 상위 수준 두 곳에서 동시에 별견될 때 이를 가려짐(shadowing)이라 한다.
        * obj에 직속한 foo 때문에 상위 연쇄의 foo가 가려지는 것이다.
        * 이는 obj.foo로 검색하면 언제나 연쇄의 최하위 수준에서 가장 먼저 foo 프로퍼티를 찾기 때문에 그렇다.
    * obj객체에서 foo의 가려짐은 생각보다 그리 간단한 문제가 아니다.
    * obj에 직속한 foo는 없으나 obj [[Prototype]]연쇄의 상위 수준에 foo가 있을때, obj.foo = 'bar' 할당문의 실행 결과는 다 음 세 가지 경우의 수를 따른다.
        * 연쇄의 상위 수준에는 foo 라는 이름의 일반 데이터 접근 프로퍼티가 존재하는데, 읽기 전용이 아닐 경우, myObj의 직속 프로퍼티 foo가 새로 추가되어 결국 shadowing이 된다.
        * 연쇄의 상위 수준에서 발견한 foo가 읽기 전용이면 이 프로퍼티를 세팅하거나 obj 객체에 가려짐 프로퍼티를 생성하는 따위의 일은 일어나지 않는다. 엄격 모드에선 에러가 나며 비엄격 모드에서는 프로퍼티 세팅은 조용히 무시된다. 어쨌든 가려짐 현상은 발생하지 않는다.
        * 연쇄의 상위 단계에서 발견된 foo가 세터일 경우 항상 이 세터가 호출된다. obj에 가려짐 프로퍼티 foo를 추가하지 않으며 foo 세터를 재정의하는 일 또한 없다.
    * [[Prototype]] 연쇄의 상위 수준에 이미 존재하는 프로퍼티에 값을 할당하면 반드시 가려짐이 발생할 것 같지만 보다시피 1번의 경우에만 해당된다.
    * 2, 3 번에서 foo를 가리려면 = 할당 연산자를 쓰면 안 되고 Object.defineProperty() 메서드를 사용하여 obj에 foo를 추가해야 한다.
    ```
    var obj2 = {};
    Object.defineProperty(obj2, 'a', {
        value: 42,
        writable: false
    });
    var obj = Object.create(obj2);
    obj.a = 10;
    obj // {}
    obj.a // 42
    ```
    * 암시적 가려짐이 발생하는 경우
    ```
    var anotherObj={
        a: 2
    }
    var obj = Object.create(anotherObj)
    anotherObj.a // 2
    obj.a // 2

    anotherObj.hasOwnProperty("a"); // true
    obj.hasOwnProperty("a"); // false

    obj.a++; // 암시적인 가려짐이 발생힌다

    anotherObj.a // 2
    obj.a // 3

    obj.hasOwnProperty('a') // true
    ```
    * 겉보기에는 obj.a++ 가 anotherObj.a 프로퍼티를 찾아 1 만큼 값을 증가시킬 것 같지만, 결국 일어나는 일은 obj.a = obj.a + 1 를 의미한다.
    * 따라서 [[Prototype]]을 경유하여 [[Get]] 을 먼저 찾고 anotherObj.a 에서 현재 값 2를 얻은 뒤 1만큼 증가시킨 후, 그 결과값 3을 다시 [[Put]] 으로 obj에 새로운 가려짐 프로퍼티 a를 생성한 뒤 할당한다.
    
* 자바스크립트에서는 어떤 객체에서 다른 객체로 복사하는 것이 아니라 두 객체를 연결한다.
    ```
    function Foo(){
        // ...
    }
    Foo.prototype;
    ```
    * 가장 직관적으로 설명하면 new Foo() 로써 만들어진 모든 객체는 결국 Foo.prototype 객체와 [[Prototype]]링크로 연결된다.
        ```
        function Foo(){
            //
        }
        var a = new Foo();
        Object.getPrototypeOf(a) === Foo.prototype; // true
        ```
        * new Foo()로 a가 생성될 때 일어나는 네 가지 사건 중 하나가 바로 Foo.prototype이 가리키는 객체를 내부 [[Prototye]]과 연결하는 것이다.
        * new Foo() 호출 자체는 새 객체를 다른 객체와 연결하기 위한 간접적인 우회 방법인 셈이다.
        * 더 직접적인 방법으로는 Object.create()가 있다.
    * [[Prototype]] 체계를 다른 말로 프로토타입 상속이라고 하며 흔히 클래스 상속의 동적 언어 버전이라고 말한다.
    * 자바스크립트는객체 프로퍼티를 복사하지 않는다.
    * 대신 두 객체에 링크를 걸어두고 한쪽이 다른 쪽의 프로퍼티, 함수에 접근할 수 있게 위임한다.
    * 위임이야 말로 자바스크립트 객체-연결 체계를 훨씬 더 정확하게 나타낸 용어다.
* 생성자
    ```
    function Foo(){
        // ...
    }
    Foo.prototype.constructor === Foo // true

    var a = new Foo();
    a.constructor === Foo; // true
    ```
    * Foo.prototype 객체에는 기본적으로 열거 불가한 공용 프로퍼티 .constructor가 세팅이 된다.
    * 이는 객체 생성과 관련된 함수(Foo)를 다시 참조하기 위한 레퍼런스다.
    * 마찬가지로 '생성자' 호출 new Foo()로 생성한 객체 a도 .constructor 프로퍼티를 갖고 있어서 '자신을 생성한 함수'를 가리킬 수 있다.
        * 하지만 사실 그렇지 않다.
        * .a는 .constructor 프로퍼티가 없다.
        * 실제로 확인해보면 a.constructor가 Foo 함수에 대응되지만 '생성자' 라는 단어가 글자 그대로 '~에 생성됨' 을 의미하는 것은 아니다.
    * 생성자냐 호출이냐?
        * 앞 예제에서 new를 붙여 Foo 함수를 호출하고 그래서 객체가 '생성' 되는 현장을 목격했으니 Foo를 생성자라고 믿고 싶을 것이다.
        * 하지만 Foo는 생성자가 아닌 그냥 함수일 뿐이다.
        * 함수는 결코 생성자는 아니지만, 그 앞에 new를 붙여 호출하는 순간 이 함수는 '생성자 함수'를 호출한다.
        * 실은 new 키워드는 일반 함수 호출을 도중에 가로채어 원래 수행할 작업 외에 객체 생성이라는 잔업을 더 부과하는 지시자이다.
        ```
        function NothingSpecial(){
            console.log('nothing')
        }
        var a = new NothingSpecial();

        a; //{}
        ```
        * NothingSepcial은 평범한 일반 함수다.
        * 그런데 이 함수를 new로 호출함으로써 객체가 생성되고 부수 효과로 생성된 객체를 변수 a에 할당한다.
        * 이것을 생성자 호출 이라고 보통 말하지만 NothingSepcial 함수 자체는 생성자가 아니다.
        * 즉, 자바스크립트는 앞에 new 를 붙여 호출한 함수를 모두 생성자라 할 수 있다.
        * 함수는 결코 생성자는 아니지만 new 를 사용하여 호출할 때에만 '생성자 호출' 이다.

* 클래스 지향 흉내
    ```
    function Foo(name){
        this.name = name
    }
    Foo.prototype.myName = function (){
        return this.name
    }
    var a = new Foo('a');
    a.myName() // 'a'
    ```
    * 이 예제는 두 가지 '클래스 지향' 꼼수를 썼다.
        * this.name = name 할당 시, .name 프로퍼티가 a 객체에 추가된다.
        * Foo.prototype.myName = ... 부분이 아주 흥미로운 기법으로, 프로퍼티를 Foo.prototype객체에 추가한다. 그래서 놀랍게도 a.myName() 처럼 쓸 수 있다.
    * Foo.prototype 객체의 프로퍼티/함수가 a 생성 시 각각의 객체로 복사될 거라 짐잦하기 쉽지만 절대로 그런 일은 일어나지 않는다.
    * 객체에 직속된 프로퍼티 레퍼런스가 발견되지 않으면 [[Get]]의 기본 알고리즘이 어떻게 작동하는지 생각하면 된다.
    * a는 생성 직후 각자의 내부 [[Prototype]]이 Foo.prototype에 링크된다.
    * myName은 a, b에서 찾을 수 없으므로 위임을 통해 Foo.prototype에서 찾는다.

* .constructor 프로퍼티
    * a.construct === Foo 가 true 임은 a에 Foo를 참조하는 .constructor 라는 프로퍼티가 실재함을 의미하지 않는다.
    * .constructor 역시 Foo.prototype에 위임된 레퍼런스로서 a.constructor는 Foo를 가리킨다.
    * Foo에 의해 생성된 객체 a 가 .constructor 프로퍼티를 통해 Foo에 접근할 수 있으니 언뜻 보면 매우 편리해 보이지만 보안 측면에서는 바람직하지 않다.
    * [[Prototype]] 위임을 거쳐 a.constructor가 우연히 Foo를 올바르게 참조한 건 말하지만 행운이라고 할 수 있다.
    * .constructor가 '~에 의해 생성됨'의 의미라는 불행한 가정이 부메랑 처럼 돌아와 괴롭힐 수 도 있다.
        * Foo.prototype의 .constructor 프로퍼티는 기본적으로 선언된 Foo 함수에 의해 생성된 객체에만 존재한다.
        * 새로운 객체를 생성한 뒤 기본 .prototype 객체 레퍼런서를 변경하면 변경된 레퍼런스에도 마술처럼 .constructor가 따라붙는 일 따윈 일어나지 않는다.
        ```
        function Foo(){}
        Foo.prototype = {}

        var a = new Foo();
        a.constructor === Foo // false
        a.constructor === Object // true
        ```
        * 분명 Object가 a를 생성한 게 아니라 Foo()가 a를 생성한 것으로 보인다.
        * a 에는 .constructor 프로퍼티가 없으므로 [[Prototype]] 연쇄를 따라 올라가 Foo.prototype 객체에 위임한다.
        * 하지만 이 객체에도 .constructor 프로퍼티는 없으므로 계속 상위를 위임하다가 결국 [[Prototype]] 끝자락의 Object.prototype 객체에 이르게 된다.
        * 이 객체는 .constructor 프로퍼티를 당연하 갖고 있으니 결국 내장 Object를 가리키게 된 것이다.
    * .constructor 프로퍼티는 이 객체를 거꾸로 다시 참조하는, 즉 .prototype 래퍼런스를 가진 함수를 임의로 가리킨다.
    * '생성자'와 '프로토타입' 이란 용어 자체의 의미는 기본적으로 느슨하기 짝이 없어서 나중에는 부합하지 않을 가능성도 있다.
    * 또 .constructor는 마법의 불변 프로퍼티가 아니다.
        * 열거 불가지만 값은 쓰기가 가능하다.
        * 게다가 [[Prototype]] 연쇄에 존재하는 'constructor'라는 이름의 프로퍼티를 추가하거나 다른 값으로 덮어쓰는 것도 가능하다.
    * [[Get]] 알고리즘이 [[Prototype]]연쇄를 순회하는 방식 탓에 곳곳에 널려있는 .constructor 프로퍼티가 애초 예상과는 전혀 다른 객체를 가리킬 수도 있다.
    * 결론적으로 a.constructor 같은 임의의 객체 프로퍼티는 실제로 기본 함수를 참조하는 래퍼런스라는 보장이 전혀 없다.
    * a.constructor는 매우 불안정하고 신뢰할 수 없는 래퍼런스이므로 될 수 있는 대로 코드에서 직접 사용하지 않는 게 상책이다.

* 프로토타입 상속
    ```
    function Foo(name){
        this.name = name
    }
    Foo.prototype.myName= function(){
        return this.name
    }
    function Bar(name, label){
        Foo.call(this, name)
        this.label = label
    }

    // Bar.prototype을 Foo.prototype에 연결한다.
    Bar.prototype = Object.create(Foo.prototype)
    Bar.prototype.myLabel = function(){
        return this.label
    }
    var a = new Bar('a', 'obj a')
    a.myName()
    a.myLabel()
    ```
    * Bar.prototype = Object.create(Foo.prototype) 이 중요하다.
    * Object.create() 를 실행하면 난데없이 '새로운' 객체를 만들고 내부 [[Prototype]] 을 지정한 객체에 링크한다.
    * 다른 말로, 'Foo 점 프로토타입과 연결된 새로운 Bar점 프로토타입 객체를 생성하라' 라는 뜻이다.
    * Bar(){} 함수를 선언하면 Bar는 여타 함수처럼 기본으로 .prototype 링크를 자신의 객체에 갖고 있다.
    * 이 객체를 Foo.prototype과 연결하고 싶은데, 현재 그렇게 연결되어 있지는 않다.
    * 따라서 애초 연결된 객체와 헤어지고 Foo.prototype과 연결된 새로운 객체를 생성한 것이다.
    ```
    // 이렇게 하는 건 소용 없다.
    Bar.prototype = Foo.prototype

    // 의도한 대로 동작은 하나 예상치 못한 부수 효과가 발생할 수 있다.
    Bar.prototype = new Foo();
    ```
    * Bar.prototype = Foo.prototype처럼 할당한다고 Bar.prototype이 링크된 새로운 객체가 생성되진 않는다.
    * 단지 Bar.prototype을 Foo.prototype을 가리키는 부가적인 래퍼런스로 만들어 사실상 Foo에 링크된 Foo.prototype 객체와 직접 연결한다.
    * 그래서 Bar.prototype.myLabel = ... 같은 할당문을 실행하면 별도의 객체가 아닌 공유된 Foo.prototype객체 자체를 변경하게 된다.
    * Bar.prototype = new Foo()로 할당하면 Foo.prototype과 링크된 새 객체가 생성되지만 그 과정에서 Foo() 를 생성자 호출한다.
    * 그러므로 개발자는 Foo()의 부수 효과가 일어나지 않도록 Object.create() 를 잘 사용해서 새로운 객체를 적절히 링크하여 생성해야 하는 부담이 있다.
    * 주어진 기존 객체 자신을 수정하는 게 아니라 아예 내던지고 새로운 객체를 만들어 써야 하는 건 아무래도 단점으로 남는다.
    * 이왕이면 기존 객체의 연결 정보를 수정할 수 있는 믿을만한 표준적인 방안이 있으면 좋겠다.
    * ES6부터 Object.setPrototypeOf() 유틸리티가 도입되면서 비로소 예측 가능한 표준 방법을 사용할 수 있게 되었다.
    ```
    // ES6 이전
    Bar.prototype = Object.create(Foo.prototype)

    // ES6 이후
    Object.setPrototypeOf(Bar.prototype, Foo.prototype)
    ```
    * Object.create()를 쓰는 편이 사소하나마 성능은 떨어지지만(던져버린 객체를 나중에 가비지 콜렉트해야 하므로) 코드만 놓고 보면 ES6 이후 기법보다 오히려 더 짧고 가독성은 좋다.

* 클래스 관계 조사
    * a 같은 객체가 어떤 객체로 위임할지는 어떻게 알 수 있을까?
        * a instanceof Foo
            * 왼쪽에 일반 객체, 오른쪽에 함수를 피연산자로 둔 instanceof 연산자는 a 의 [[Prototype]]연쇄를 순회하면서 Foo.prototype가 가리키는 객체가 있는지 조사한다.
            * 이 말은 불행히도 대상 함수에 대해 주어진 객체의 계통만 살펴볼 수 있다.
            * 2개의 객체가 있으면 instanceof만으로는 두 객체가 서로 [[Prototype]]연쇄를 통해 연결되어 있는지는 전혀 알 수 없다.
             * 내장 .bind()
                * 내장 .bind() 유틸리티로 하드 바인딩을 사용하면 .prototype 프로퍼티가 생기지 않는다.
                * 이런 함수에 instanceof를 사용하면 바인딩 함수의 출처인 대상 함수의 .prototype으로 대체된다.
                * 하드 바인딩 함수를 써서 생성자를 호출하는 경우는 극히 드물지만 사용하면 원본 대상 함수를 대신 실행시키는 것과 같은 결과를 가져온다.
                * 즉 하드 바인딩 함수에 instanceof를 사용하면 원래 함수에 따라 작동한다.
        * Foo.prototype.isPrototypeOf(a)
            * isPrototypeOf() 는 'a 전체 [[Prototype]] 연쇄에 Foo.prototype이 있는가' 라는 질문에 대답한다.
            * 똑같은 원리지만 isPrototypeOf를 쓰면 간접적으로 참조할 함수(Foo)의 .prototype 프로퍼티를 거치는 등의 잡다한 과정이 생략되는 장점이 있다.
        * ES5 부터 지원하는 표존 메서드 Object.getPrototypeOf(a) 를 통해 곧바로 조회할 수 있다.
            * Object.getPrototypeOf(a) === Foo.prototype // true
            * 거의 모든 브라우저에서 내부의 [[Prototype]] 을 들여다볼 수 있는 비표준 접근 방법을 과거부터 지원해왔다.
                * a._ _ proto _ _ === Foo.prototype // true
                * _ _ proto _ _ 는 프로퍼티처럼 보이지만 실은 게터/세터에 가깝다.
                ```
                Object.defineProperty(Object.prototype, "_ _ proto _ _", {
                    get: function(){
                        return Object.getPrototypeOf(this)
                    }
                    set: function(o){
                        Object.setPrototypeOf(this ,o);
                        return o
                    }
                })
                ```
                * 따라서 a._ _ proto _ _ 로 접근하는 것은 마치 a._ _ proto _ _ () 를 호출하는 것과 같다.
                * 게터 함수는 Object.prototype 객체에 존재하지만 이 함수를 호출하면 this는 a로 바인딩되며 결국 
                Object.getPrototypeOf(a) 를 실행시키는 것과 비슷하다.
    * 객체의 [[Prototype]] 링크 정보는 읽기 전용으로 다루는 것이 최선이다.

* 객체 링크
    * [[Prototype]] 체계는 다름 아닌 다른 객체를 참조하는 어떤 객체에 존재하는 내부 링크이다.
    * 이 연결 고리는 객체의 프로퍼티/메소드를 참조하려고 하는데, 그런 프로퍼티/메서드가 해당 객체에 존재하지 않을 떄 주로 활용된다.
    * 엔진은 [[Prototype]]에 연결된 객체를 하나씩 따라가면서 프로퍼티/메서드를 찾아보고 발견될 때까지 같은 과정을 되풀이한다.
    * 이렇게 객체 사이에 형성된 일련의 링크를 prototype chain 이라 한다.
    * 링크 생성
    ```
    var foo ={
        somthing: function(){
            console.log('hi')
        }
    }
    var bar = Object.create(foo)
    bar.something() // hi
    ```
    * Object.create()는 먼저 새로운 객체를 생성하고 주어진 객체와 연결한다.
    * 이것만으로도 클래스나 생성자 호출, 헷갈리는 .prototype이나 .constructor 래퍼런스 등을 동원한 함수로 쓸데없이 골치 아프게 하지 않으면서도 [[Prototype]]체계의 진정한 힘(위임)을 실감할 수 있다.
        * Object.create(null)은 [[Prototype]]링크가 빈 객체를 생성하므로 위임할 곳이 없다.
        * 이런 객체는 프로토타입 연쇄 자체가 존재하지 않기 때문에 instaceof 연산 결과는 항상 false다.
        * 일차원적인 데이터 저장소로 제격이다.
        * 순수하게 프로퍼티에 데이터를 저장하는 용도로 사용하며, 이를 보통 딕셔너리라고 한다.
    * Object.create()는 ES5부터 추가되어서 이전 환경까지 고려하면 부분적인 폴리필이 필요하다.
    ```
    if(!Object.create()){
        Object.create = function(o){
            function F(){}
            F.prototype = o
            return new F()
        }
    }
    ```
    * 이 폴리필은 우선 임시 함수 F를 이용하여 F.prototype 프로퍼티가 링크하려는 객체를 가리키도록 오버라이드한다. 그런 다음 new F()로 원하는 연결이 수립된 새 객체를 반환한다.

* 링크는 대비책?
    * 지금까지 설명한 객체 간 연결 시스템을 프로퍼티/메서드를 찾지 못할 경우를 위한 대비책으로 오인하기 쉽다.
    ```
     var foo ={
        somthing: function(){
            console.log('hi')
        }
    }
    var bar = Object.create(foo)
    bar.doSomething = function(){
        this.somthing()
    }
    
    bar.doSomething() // hi
    ```
    * 더 명시적인 API이다.
    * 내부적으로 [[Prototype]] 을 foo.somthing()에 위임한 위임 디자인 패턴의 구현 방식이다.
    * 즉, API 인터페이스 설계 시 구현 상세를 겉으로 노출하지 않고 내부에 감추는 식으로 위임하면 특별히 이상하거나 혼동할 일은 없다.

* 정리하기
    * 
        * 객체에 존재하지 않는 프로퍼티를 접근하려고 시도하면 [[Get]] 은 해당 객체의 내부 [[Prototype]] 링크를 따라 다음 수색 장소를 결정한다.
        * 프로퍼티를 찾아 이 객체에서 저 객체로 줄줄이 순회하기 위한 연결 경로는 prototype chain에 잘 정의되어 있다.
        * 모든 일반 객체의 최상위 프로토타입 연쇄는 내장 Object.prototype이 버터고 있다.
        * 결국 이 지점까지 이르러서도 발견되지 않으면 프로퍼티 수색은 종료된다.
        * toString(), valueOf() 등의 공용 유틸리티들은 바로 Object.prototype에 구현된 덕분에 자바스크립트의 모든 객체가 이용할 수 있다.
    * 
        * 두 객체를 서로 연결 짓는 가장 일반적인 방법은 함수 호출시 new 키워드를 앞에 붙이는 것이다.
    * 
        * 객체는 결국 다른 객체와 내부 [[Prototype]] 연쇄를 통해 연결된다.
    * 
        * 사실 자바스크립트 객체 간의 관계는 복사되는 것이 아니라 위임 연결이 맺어진 것이므로 위임 이라고 해야 더 적절한 표현이다.
    * 
        * [[Prototype]] 체계는 한 객체가 다른 객체를 참조하기 위한 내부 링크다.
<!--  -->
## 작동 위임
* 자바스크립트의 [[Prototype]] 체계는 객체를 다른 객체에 연결한다.
* 클래스 같은 추상화 체계는 없다.
* 부모와 자식 간의 메서드 이름을 다르게 하여 작동 위임해야 한다.
    * [[Prototype]] 연쇄에서 같은 명칭이 뒤섞이는 일은 될 수 있으면 피해야 한다.
    * 같은 이름끼리 충돌하면 래퍼런스를정확히 가려낼 수 없는 부자연스럽고 취약한 구문이 만들어질 수 있다.
* 작동 위임이란, 찾으려는 프로퍼티/메서드 래퍼런스가 객체에 없으면 다른 객체로 수색 작업을 위임하는 것을 의미한다.
* 작동 위임은 정말 강력한 디자인 패턴으로, 부모/자식 클래스, 상속, 다형성 등의 사상과는 완전히 구별된다.
* 이제부터 부모->자식 의 수직적인 클래스 다이어그램 따위는 지우고 객체들이 서로 수평적으로 배열된 상태에서 위임 링크가 체결된 모습을 떠올리길 바란다.
* 위임은 상세한 내부 구현 용도로 적절하다.
    * API 인터페이스에 직접 노출시키지 않는다.
* 상호 위임(허용되지 않음)
    * 복수의 객체가 양방향으로 상호 위임을 하면 발생하는 사이클은 허용되지 않는다.
    * 하지만 모든 래퍼런스가 확실히 존재한다면, 양방향 위임이 가능하다.
    * 이런 조건이 딱 맞아 떨여저 제법 유용한 경우가 있다.
    * 하지만 자바스크립트 엔진 제작사 입장에서는 객체 프로퍼티를 매번 검사할 때마다 가드 체크를 하여 성능 히트를 측정하는 것보다 무한 순환 참조를 확인하는 로직을 구현하는 게 성능 요건에 더 부합한다고 판단되어 결국 상호 위임은 쓸 수 없게 됐다.
* 고전적인 프로토타입 스타일
    ```
    function Foo(who){
        this.me = who
    }
    Foo.prototype.identify = function(){
        return "I'm " + this.me
    }
    function Bar(who){
        Foo.call(this, who)
    }
    Bar.prototype = Object.create(Foo.prototype);
    Bar.prototype.speak = function(){
        alert("Hello, " + this.identify()+ ".");
    }
    var b1 = new Bar("b1")
    ```
    * 자식 클래스 Bar는 부모 클래스 Foo를 상속한 뒤 b1으로 인스턴스화 한다.
    * 그 결과 b1은 Bar.prototype으로 위임되며, 다시 Foo.prototype으로 위임된다.
* OLOO 스타일 (Object Linked to Other Object)
    ```
    Foo = {
        init: function(who){
            this.me = who;
        },
        identify: function(){
            return "I'm " + this.me;
        }
    }

    Bar = Object.create(Foo)
    Bar.speak = function(){
        alert("Hello, " + this.identify() + ".")
    }

    var b1 = Object.create(Bar);
    b1.init('b1');
    b1.speak()
    ```
    * 여기서도 b1 -> Bar -> Foo 로 연결되어 있다.
    * [[Prototype]] 위임을 활용하며, 새 객체는 서로 단단히 연결되어 있다.
    * 여기서 중요한 건 잡다한 것들이 정리되고 단순해졌다는 사실이다.
    * 생성자, 프로토타입, new 호출을 하면서 클래스처럼 보이게 하려고 헷갈리기만 한 장치들을 쓰지 않고 그냥 객체를 서로 연결해 주기만 했다.

* 위젯 클래스
    * OO 디자인 패턴
        ```
        function Widget(width, height) {
            this.width = width || 50;
            this.height = height || 50;
            this.elem = null;
        }

        Widget.prototype.render = function(where) {
            if (this.elem) {
                this.elem.style.width = this.width + 'px';
                this.elem.style.height = this.height + 'px';
                where.appendChild(this.elem);
            }
        };

        function Button(width, height, label) {
            // super 생성자 호출
            Widget.call(this, width, height);
            this.label = label || 'Button';
            this.elem = document.createElement('Button');
        }

        Button.prototype = Object.create(Widget.prototype);

        Button.prototype.render = function(where) {
            Widget.prototype.render.call(this, where);
            this.elem.innerText = this.label;
            this.elem.addEventListener('click', this.onClick.bind(this));
        };
        Button.prototype.onClick = function(e) {
            console.log('click!');
        };

        var content = document.getElementById('content');
        var btn1 = new Button(125, 30, 'Hello');
        btn1.render(content);
        ```
        * OO 디자인 패턴에 따르면 부모 클래스에는 기본 render() 만 선언해두고 자식 클래스가 이를 오버라이드하도록 유도한다.
        * 기본 기능을 완전히 갈아치운다기보단 버튼에만 해당하는 작동을 덧붙여 기본 기능을 증강한다.
        * Widget.call과 Widget.prototype.render.call 호출부를 보면 자식 클래스 메서드에서 부모 클래스의 기본 메서드로 돌아가기 위해 가짜 super 호출까지 동원한 명시적 의사 다형성의 추한 단면을 볼 수 있다.
    * ES6 class 디자인 패턴
        ```
        class Widget {
            constructor(width, height) {
                this.width = width || 50;
                this.height = height || 50;
                this.elem = null;
            }

            render(where) {
                if (this.elem) {
                    this.elem.style.width = this.width + 'px';
                    this.elem.style.height = this.height + 'px';
                    where.appendChild(this.elem);
                }
            }
        }

        class Button extends Widget {
            constructor(width, height, label) {
                super(width, height);
                this.label = label || 'Button';
                this.elem = document.createElement('Button');
            }

            render(where) {
                super.render(where);
                this.elem.innerText = this.label;
                this.elem.addEventListener('click', this.onClick.bind(this));
            }
            onClick(e) {
                console.log('click!');
            }
        }

        var content = document.getElementById('content');
        var btn = new Button(100, 100, 'hello');
        btn.render(content);
        ```
        * 이전 코드랑 비교하면 ES6 class로 보기 흉한 구문들을 상당수 매끄럽게 바꾸었다.
        * 특히, super()가 있다는 점이 좋아보인다...?
        * 구문상으론 나아지긴 했지만 어디까지나 [[Prototype]] 체계 위에서 실행되므로 진짜 클래스는 아니다.
    * OLOO 디자인 패턴
        ```
        var Widget = {
            init: function(width, height) {
                this.width = width || 50;
                this.height = height || 50;
                this.elem = null;
            },
            insert: function(where) {
                if (this.elem) {
                    this.elem.style.width = this.width + 'px';
                    this.elem.style.height = this.height + 'px';
                    where.appendChild(this.elem);
                }
            }
        };

        var Button = Object.create(Widget);
        Button.setup = function(width, height, label) {
            this.init(width, height);
            this.label = label || 'Button';
            this.elem = document.createElement('Button');
        };
        Button.build = function(where) {
            this.insert(where);
            this.elem.innerText = this.label;
            this.elem.addEventListener('click', this.onClick.bind(this));
        };
        Button.onClick = function(e) {
            console.log('click!');
        };

        var content = document.getElementById('content');
        var btn = Object.create(Button);
        btn.setup(150, 50, 'hello');
        btn.build(content);
        ```
        * OLOO 관점에서는 Widget이 부모도, Button이 자식도 아니다.
        * Widget은 보통 객체로, 갖가지 유형의 위젯이 위임하여 사용할 수 있는 유틸리티 창고 역할을 맞는다.
        * 그리고 Button은 그냥 단독으로 존재하는 객체일 뿐이다.
            * 물론 Widget과 위임 링크는 맺어진 상태이다.
        * 디자인 패턴 관점에서 클래스 방식이 고집하는 같은 이름의 render() 메서드 따위는 공유할 필요가 없다.
        * 대신, 각자 수행하는 읨무를 더욱 구체적으로 드러낼 수 있는 다른 이름들을 메서드에 부여한다.
        * 생성하는 코드가 더 많아져서 처음보다 더 나빠진 것처럼 보인다.
            * 원하는 주기에 원하는 기능을 부여하고 싶을때 더 유연하게 대처할 수 있다.
            * 생성 및 초기화 과정을 굳이 한곳에 몰아넣고 실행하지 않아도 되니 OLOO야말로 관심사 분리의 원칙을 더 잘 반영한 패턴이다.
* 비어휘적 식별자
    * 그러나 단축 메서드에더 미묘하지만 간과해서는 안 될 결점이 있다.
    ```
    var Foo = {
        bar() { //... },
        baz: function baz(){ //... }
    }
    ```
    * 이를 일반식으로 표현하면 다음과 같다.
    ```
    var Foo ={
        bar: function(){ //... },
        baz: function baz(){ //... }
    }
    ```
    * 단축 메서드 bar()가 bar 프로터티에 붙여진 '익명 함수 표현식'이 됐다.
    * 이는 함수 객체 자신에 이름 식별자가 없기 때문이다.
    * 익명 함수의 단점
        * 스택 추적을 통해 디버깅 하기가 곤란해진다.
        * 재귀, 이벤트 바인딩 등에서 자기 참조가 어려워진다.
        * 코드 가독성이 조금 더 나빠진다.
    * 1, 3번은 단축 메서드에 해당하지 않는다.
    * 비간편 구문에서는 대개 스택 추적 시 이름이 나오지 않는 익명 함수 표현식을 사용하지만 단축 메서드는 내부 프로퍼티 name에 해당 함수 객체를 정확히 지정하므로 스택 추적에 활용할 수 있다.
    * 하지만 아쉽게도 2번은 단축 메서드의 단점이다.
    * 자기 참조에 사용할 수 있는 어휘적 식별자는 없다.
    ```
    var Foo ={
        bar: function(x){
            if(x<10){
                return Foo.bar(x*2)
            }
            return x;
        },
        baz: function baz(x){
            if(x<10){
                return baz(x*2)
            }
            return x;
        }
    }
    ```
    * Foo.baz(x*2) 처럼 수동으로 참조하는 방법도 있지만 여러 객체가 this 바인딩을 통해 위임을 공유하는 함수처럼 할 수 없는 경우가 꽤 많다.
    * 정말 자기 참조가 필요한 상황에 직면할 가능성이 있을 테니 함수 객체의 이름 식별자는 지정하는 게 좋다.
    * 어쨌든 단축 메서드는 자기 참조 시 방법이 마땅치 않아 이슈가 될 수 있으니 유의하자.
    * 단지 함수 선언이라면 단축 메서드 구문 보다 이름 붙은 함수 표현식 사용을 권한다.

* 타입 인트로스펙션
    * 인스턴스를 조사해 객체 유형을 거꾸로 유추하는 것
    * 클래스 인스턴스에서 타입 인트로스펙션은 주로 인트턴스가 생성된 소스 객체의 구조와 기능을 추론하는 용도로 쓰인다.
    * 다음은 instanceof 연산자로 객체 a의 기능을 추론하는 코드다.
    ```
    function Foo(){

    }
    Foo.prototype.something = function(){
        // ...
    }
    var a = new Foo()
    if(a instanceof Foo){
        a.something()
    }
    ```
    * Foo.prototype은 a의 [[Prototype]] 연쇄에 존재하므로 instaceof 연산자는 (혼란스럽게도) 마치 a가 Foo의 클래스의 인스턴스인 것 같은 결과를 낸다.
    * 그래서 a는 Foo 클래스의 기능을 가진 객체라고 쉽게 단정하게 된다.
    * 물론 애당초 Foo 클래스 같은 건 없었고 Foo는 일반 객체에 불과하며, Foo가 참조한 임의의 객체가 우연히 a에 위임 연결됐을 뿐이다.
    * 구문만 보면 instanceof가 a와 Foo의 관계를 조사하는 듯 보이지만 실제로는 a가 Foo.prototype 사이의 관계를 알려주는 일을 한다.
    * 이렇게 instanceof 에 의존하여 a가 주어진 객체와 연결되는지를 인트로스펙션하는 건 의미상 헷갈리기도 하고 간접적일 수밖에 없다.
    * 해당 객체를 참조하는 래퍼런스가 지닌 함수가 꼭 필요하다.
    ```
    function Foo(){ //.. }
    Foo.prototype.something = function (){ //.. }
    function Bar(){ //.. }
    Bar.prototype = Object.create(Foo)
    var a = new Bar('a')
    ```
    * 이 예제의 개체에 instanceof 연산자와 .prototype을 이용하여 타입 인트로스펙션을 실시하면 다음과 같이 여러 가지를 체크해볼 수 있다.
    ```
    Bar.prototype instanceof Foo // true
    Object.getPrototypeOf(Bar.prototype) === Foo.prototype // true
    Foo.prototype.isPrototypeOf(Bar.prototype) // true

    a instanceof Foo // true
    a instanceof Bar // true
    Object.getPrototypeOf(a) === Bar.prototype true
    Foo.prototype.isPrototypeOf(a) // true
    Bar.prototype.isPrototypeOf(a) // true
    ```
    * Bar instanceof Foo 같은 체크는 의미가 없다.
        * 직관적으로 instanceof가 상속을 의미하는 것 같기 때문에 혼동할 수 있다.
        * Bar.prototype instanceof Foo 라면 모를까
    * 덕 타이핑
        * 오리처럼 보이는 동물이 오리 소리를 낸다면 오리가 분명하다, 라는 속담에서 유래
        * 예를 들면, 위임가능한 something() 함수를 가진 객체와 a의 관계를 애써 조사하는 대신 a.something()을 테스트해보고 여길 통과하면 a는 .something() 을 호출할 자격이 있다고 간주하는 것이다.
        * 이러한 가정 자체는 리스크가 없다.
        ```
        if(a.something()){
            a.something()
        }
        ```
        * 그러나 이 '덕 타이핑'은 원래 테스트 결과 이외에 객체의 다른 기능까지 확장하여 추정하는 경향이 있어 리스크가 더해진다.
        * 가장 유명한 덕 타이핑의 실례가 바로 ES6 프라미스이다.
        * 프라미스
            * 어떤 임의의 객체 래퍼런스가 프라미스이닞를 판단해야 할 경우 해당 객체가 then() 함수를 가졌는지 조사하는 식으로 테스트한다.
            * 즉, 어떤 객체에 then() 메서드가 있으면 ES6 프라미스는 무조건 이 객체는 '데너블 thenable'하다고 단정 짓고 모든 표준 프라미스 작동 로직을 갖추고 있을 거라 예상한다.
    * 본론으로 돌아가서 OLOO 스타일 코드의 타입 인트로스펙션은 훨씬 더 깔끔하다.
    ```
    var Foo = { //.. }
    var Bar = Object.create(Foo)
    var a = Object.create(Bar)
    ```
    * 모든 객체가 [[Prototype]] 위임을 통해 연관된 OLOO 방식에서는 다음과 같이 꽤 단순한 형태로 타입 인트로스펙션을 할 수 있다.
    ```
    Foo.isPrototypeOf(Bar) // true
    Object.getPrototypeOf(Bar) === Foo // true

    Foo.isPrototypeOf(a)
    Bar.isPrototypeOf(a)
    Object.getPrototypeOf(a) === Bar // true
    ```
* 정리하기
    * 클래스와 상속은 소프트웨어 아키텍처를 설계할 때 선택해도, 선택하지 않아도 되는 디자인 패턴의 하나다.
    * 개발자 대부분은 클래스가 코드 체계를 잡는 유일무이한 방법이라고 여기는데, 이 장에서는 '작동 위임' 이라는 실로 강력한 디자인 패턴에 대해 학습했다.
    * 작동 위임 패턴은 객체를 부모/자식 클래스 관계가 아닌 동등한 입장에서 서로 위임하는 형태로 연결되어 있다.
    * 자바스크립트 [[Prototype]]은 태생적으로 작동 위임 체계다.
    * 즉, 자바스크립트에 억지로 클래스 체계를 얹어 코드를 구현하느라 분투할지, 위임 체계로 [[Prototype]] 본연의 기능을 수용하여 활용할지는 얼마든지 선택할 수 있는 문제다.
    * 객체만으로 구성된 코드를 구성한다면 사용 구문도 단순해질뿐더러 실제 코드 아키텍처 또한 더 간단하게 가져갈 수 있다.
    * OLOO는 클래스라는 추상화 장치 없이도 직접 객체를 생성 및 연계한다.
    * OLOO는 [[Prototype]] 기반의 작동 위임을 아주 자연스럽게 구현한다.

## 비동기성
* 요즘의 자바스크립트는
    * 브라우저는 물론 서버 등 거의 모든 가용 장비에서 실행할 수 있어야 한다.
    * 일급 프로그래밍 언어로서의 요건을 충족하기 위해 응용 범위와 복잡도가 날로 증가혹 있다.
    * 이에 따라 비동기 요소를 관리해야 하는 부담이 커지고있다.
* 비동기 콘솔
    * console.* 메서드는 작동 방법이나 요건이 명세에 따로 정해져 있지 않지만 호스팅 환경에 추가된 기능이다.
    * 따라서 브라우저와 자바스크립트 실행 환경에 따라 작동 방식이 다르고 종종 혼동을 유발하기도 한다.
    * 특히 console.log() 메서드는 브라우저 유형과 상황에 따라 출력할 데이터가 마련된 직후에도 콘솔창에 바로 표시되지 않을 수 있다.
    * 많은 프로그램에서 I/O 부분이 가장 느리고 중단이 잦기 때문이다.
    * 브라우저가 콘솔 I/O를 백그라운드에서 비동기적으로 처리해야 성능상 유리하다.
* 이벤트 루프
    * 자바스크립트 엔진은 혼자서는 안 되고 반드시 호스팅에서 실행된다.
    * 지난 수년 동안 자바스크립트는 브라우저를 벗어나 다른 환경으로 그 영역을 확장해왔다.
    * 그러나 환경은 달라도 '스레드'는 공통이다.
    * 여러 프로그램 덩이를 시간에 따라 매 순간 한번씩 엔진을 실행시키는 '이벤트 루프' 라는 장치다.
    * 다시 말해, 자바스크립트 엔진은 애당초 시간이란 관념 따윈 없었고 임의의 자바스크립트 코드 조각을 시시각각 주는 대로 받아 처리하는 실행기일 뿐, '이벤트' 를 스케줄링하는 일은 언제나 엔진을 감싸고 있던 주위 환경의 몫이었다.
    * setTimeout()은 콜백을 이벤트 루프 큐에 넣지 않는다.
    * 타이머가 끝나면 환경이 콜백을 이벤트 루프에 삽입한다.
    * 이 때 이벤트 루프에 많은 이벤트로 가득 차있을 수 있기 때문에, setTimeout() 타이머가 항상 완벽하게 정확한 타이밍으로 작동하지 않을 수 있다.
    * 자바스크립트 프로그램은 수많은 덩이로 잘게 나누어지고 이벤트 루프 큐에서 한 번에 하나씩 차례대로 실행된다.
    * 엄밀히 말해 이 큐엔 개발자가 작성한 프로그램과 직접 상관없는 여타 이벤트들도 중간에 끼어들 가능성도 있다.
    * ES6 부터는 이벤트 루프 큐 관리 방식이 완전히 바뀌었다.
    * 이벤트 루프 큐는 준 공식적인 기술 요건임에도 ES6에 이르러서야 작동 방식이 정확히 규정되어 이제야 호스팅 환경이 아닌 자바스크립트 엔진의 관할이 됐다.
    * 프라미스 도입을 계기로 이러한 변화가 일어났다.
    * 프라미스는 이벤트 루프 큐의 스케줄링을 직접 세밀하게 제어해야 하기 때문이다.
* 병렬 스레딩
    * 비동기와 병령은 아무렇게나 섞어 쓰는 경우가 많지만 그 의미는 완전히 다르다.
    * 비동기는 '지금'과 '나중' 사이의 간극에 관한 용어고 병렬은 동시에 일어나는 일들과 연관된다.
    * 자바스크립트는 절대로 스레드 간에 데이터를 공유하는 법이 없으므로 비결정성의 수준은 문제가 되지 않는다.
    * 하지만 그렇다고 자바스크립트 프로그램이 항상 결정적이란 소리도 아니다.
    * 자바스크립트의 작동 모드는 단일-스레드이다.
* 동시성
    * 사용자가 스크롤바를 아래로 내리면 계속 갱신된 상태 리스트가 화면에 표시되는 웹 페이지를 만들고자 한다.
    * 이런 기능은 2개의 분리된 프로세스를 동시에 실행할 수 있어야 제대로 기능을 구현할 수 있다.
    * 첫 번째 프로세스는 사용자가 페이지를 스크롤바로 내리는 순간 발생하는 onscroll 이벤트에 반응한다.
    * 두 번째 프로세스는 ajax 응답을 받는다.
    * 동시성은 복수의 프로세스가 같은 시간 동안 동시에 실행됨을 의미한다.
    * 각 프로세스 작업들이 병렬로 처리되는지와는 관계 없다.
    * 동시성은 처리 수준 병행성과 상반되는 개념의 프로세스 수준의 병행성이라 할 수 있다.
    * 비상호 작용
        * 어떤 프로그램 내에서 복수의 프로세스가 단계/이벤트를 동시에 인터리빙 할 때 이들 프로세스 사이에 연관된 작업이 없다면 프로세스 간 상호 작용은 사실 의미가 없다.
        * 프로세스 간 상호작용이 일어나지 않는다면 비결정성은 완벽하게 수용 가능하다.
    * 상호 작용
* 잡
    * 잡 큐는 ES6 부터 이벤트 루프 큐에 새롭게 도입된 개념이다.
    * 주로 프라미스의 비동기 작동에서 가장 많이 보게 될 것이다.
    * 아쉽게도 잡 큐는 아직 공개 API 조차 마련되지 않아 구체적인 실례를 들어 설명하긴 곤란하다.
    * 잡 큐는 이벤트 루프 큐에서 '매 틱의 끝나잙에 매달려 있는 큐' 라고 생각하면 가장 알기 쉽다.
    * 이벤트 루프 틱 도중 발생 가능한, 비동기 특성이 내재된 액션으로 인해 전혀 새로운 이벤트 루프 큐에 추가되는 게 아니라 현재 틱의 잡 큐 끝 부분에 원소가 추가된다.
    * 마치 '이건 나중에 처리할 작업인데, 다른 어떤 작업들보다 우선하여 처리해주세요' 같은 느낌이다.
    * 비유하자면 이벤트 루프 큐는 테마파크에서 롤러코스터를 타고나서 한 번 더 타고 싶어 다시 대기열 맨 끝에서 기다리는 것이고 잡 큐는 롤러코스터에서 내린 직후 대기열 맨 앞에서 곧바로 다시 타는 것이다.
    * 잡은 같은 큐 끝에 더 많은 잡을 추가할 수 있는 구조이기 때문에 이론적으로는 잡 루프가 무한 반복되면서 프로그램이 다음 이벤트 루프 틱으로 이동할 기력을 결국 상실할 수도 있다.
    * 개념적으로는 프로그램에서 실행 시간이 긴 코드 또는 무한 루프를 표현한 것과 비슷하다.
    * 잡은 기본적으로 setTimeout(..., 0) 같은 꼼수와 의도는 비슷하지만 처리 순서가 더 잘 정의되어있고 순서가 확실히 보장되는 방향으로 구현되어 있다.
* 정리하기
    * 
        * 자바스크립트 프로그램은 언제나 2 개 이상의 덩이로 쪼개지며 이벤트 응답으로 첫 번째 덩이는 '지금', 다음 덩이는 '나중' 에 실행된다.
        * 한 덩이씩 실행되어도 모든 덩이가 프로그램의 스코프/상태에 똑같이 접근할 수 있으므로 상태 변화는 차례대로 반영된다.
    * 
        * 실행할 이벤트가 있으면 이벤트 루프는 큐를 다 비울 떄까지 실행한다.
        * 이벤트 루프를 한 차례 순회하는 것을 틱 이라 한다.
        * UI, IO 타이머는 이벤트 큐에 이벤트를 넣는다.
    * 
        * 언제나 한 번에 정확히 한 개의 이벤트만 큐에서 꺼내 처리한다.
        * 이벤트 실행 도중, 하나 또는 그 이상의 후속 이벤트를 직/간접적으로 일으킬 수 있다.
    * 
        * 동시성은 복수의 이벤트들이 연쇄적으로 시간에 따라 인터리빙 되면서 고수준의 관점에서 볼 때 꼭 동시에 실행되는 것 처럼 보인다.
    * 
        * 동시 프로세스들은 어떤 형태로든 서로 영향을 미치는 작업을 조정하여 실행 순서를 보장하거나 경합 조건을 예방하는 등의 조치를 해야 한다.

* 콜백
    * 어떤 경우든 함수는 콜백 역할을 한다.
    * 큐에서 대기 중인 코드가 처리되자마자 본 프로그램으로 돌아올 목적지기 때문이다.
    * 의심할 바 없이 아직 콜백은 자바스크립트에서 비동기성을 표현하고 관리하는 가장 일반적인 기법이자, 사실상 자바스크립트 언어에서 가장 기본적인 비동기 패턴이다.
    * 자바스크립트의 충실한 비동기 프로그래밍 역할을 해왔다.
    * 그러나 콜백에도 단점이 있어 많은 개발자는 더 나은 비동기 패턴이 나오리란 기대(프라미스)로 들떠 있다.
    * 지금은 콜백을 깊이 있게 살펴보기 이보다 정교한 비동기 패턴이 나오게 된계기를 설명한다.
* 정리하기
    * 
        * 콜백은 자바스크립트에서 비동기성을 표현하는 기본 단위다.
        * 그러나 자바스크립트와 더불어 점점 진화하는 비동기 프로그래밍 환경에서 콜백만으로는 충분치 않다.
    *   
        * 콜백은 비동기 흐름을 비선형적, 비순차적인 방향으로 나타내므로 구현된 코드를 제대로 이해하기가 어렵다.
        * 추론하기 곤란한 코드는 곧 악성 버그를 품은 나쁜 코드로 이어진다.
    * 
        * 그래서 비동기성을 좀 더 동기적, 순차적, 중단적인 모습으로 우리 두뇌가 사고하는 방식과 유사하게 표현할 방법이 필요하다.
    * 
        * 콜백은 프로그램을 진행하기 위해 제어를 역전, 즉 제어권을 다른 파트(제어가 불가능한 서드파티 유틸리티 등에)에 암시적으로 넘겨줘야 하므로 골치가 아프다.
        * 이렇게 제어권이 넘어가면서 예상보다 더 자주 콜백을 호출하는 등 여러가지 믿음성 문제에 봉착하게 된다.
    * 
        * 믿음성 문제를 해결하고자 임시 로직을 짜 넣으면 당장은 난관을 모면할 순 있지만 생각만큼 구현하기 쉽지 않은 데다 계속 그렇게 하다 보면 거칠고 유지 보수가 어려운 코드로 변질된다.
        * 또 나중에 버그가 발견되어 된통 당해보기 전까진 막대한 피해로부터 보호받을 마땅한 방법 또한 없다.
    * 
        * 관용 코드를 여기저기 흩뿌리지 않고도 생성한 콜백들을 재사용할 수 있는, 그런 일반적인 해결 방안이 강구되어야 믿음성 문제를 해결할 수 있다.

* 프라미스
    * 먼저 제어의 역전, 즉 허술하게 매달려 있다가 쉽게 소실되는 믿음의 문제부터 해결하자.
        * 프로그램의 연속 실행을 감싼 콜백 함수를 다른 곳에 전다하면 별 탈 없이 정상적으로 콜백이 되길 기도할 수 밖에 없다.
        * 그런데 만약 제어의 역전을 되역전 시킬 수 있다면?
        * 그러니까 프로그램의 진행을 다른 파트에 넘겨주지 않고도 개발자가 언제 작업이 끝날지 알 수 있고 그다음에 무슨 일을 해야 할지 스스로 결정할 수 있다면?
        * 이러한 체계가 바로 프라미스이다.
    * 프라미스는 개발자와 명세 작성자가 함꼐 코딩/설계 단계에서 콜백 지옥의 실타래를 풀기 위해 지푸라기 잡는 심정으로 매달리면서 자바스크립트 세상에 혜성처럼 등장했다.
    * 실상 자바스크립트/DOM 플랫폼에 추가된 최신 비동기 API 대부분이 모두 프라미스에 기반을 두고 개발됐다.
    * 이 장에서 프라미스 귀결 액션을 종종 '즉시' 라는 말로 표현한다.
    * 
        * 하지만 사실상 모든 경우에 '즉시'란 잡 큐에서 그렇단 말이지, 엄밀히 말해 동기적인 '지금'은 아니다.
        * 프라미스는 이룸Fulfilment 아닌 버림Rejection으로 귀결될 수 있다.
    * 항상 귀결 값을 프로그램이 결정짓는 이룸 프라미스와는 다르게 버림값은 프로그램 로직에 따라 직접 세팅되거나 런타임 예외에 의해 암시적으로 생겨나기도 한다.
    * 프라미스 then() 함수는 이룸 함수를 첫 번째 인자로, 버림 함수를 두 번째 인자로 각각 넘겨받는다.
    * 프라미스는 시간 의존적인 상태를 외부로부터 캡슐화하기 때문에 프라미스 자체는 시간 독립적이고 그래서 타이밍 또는 내부 결괏값에 상관없이 예측 가능한 방향으로 구성할 수 있다.
    * 불변성 Immutability
        * 또한 프라미스는 일단 귀결된 후에는 상태가 그대로 유지되며 몇 번이든 필요할 때마다 꺼내쓸 수 있다.
        * 프라미스는 귀결되고 나면 외부적으로 불변 상태이므로 사고로 또는 악의적으로 변경되는 일은 없으며 안심하고 다른 파트에 전달할 수 있다.
        * 불변성은 프라미스를 제대로 알려면 반드시 이해해야 할, 강력하고 중요한 개념이다.
    * 프라미스는 미랫값을 캡슐화하고 조합할 수 있게 해주는 손쉬운 반복 장치이다.

* 완료 이밴트
    * 프라미스 각각은 미랫값으로서 작동하지만 프라미스의 귀결은 비동기 작업의 여러 단계를 흐름 제어하기 위한 체계라 볼 수 있다.
    * 완료와 진행
        * 어떤 벌어진 것에 초점을 둘 것인가(진행 이벤트), 아니면 완료가 된 이후에 초점을 둘 것인가(완료 이벤트)의 차이
    * 
        * 리스닝 중인 프라미스 귀결 이벤트는 엄밀히 말해서 이벤트가 아니고 '완료'나 '에러'라고 하지 않는게 보통이다.
        * 대신에 then()을 통한 'then' 이벤트의 등록이며, 더 정확히 말하면 '이룸', '버림' 이벤트를 등록하는 것이다.

* 데너블 덕 타이핑
    * 프라미스 세상에서 중요한 문제는 과연 어떤 값이 진짜 프라미스인지 아닌지 어찌 확신할 수 있는가 하는 점이다.
    * 더 직설적으로 말해 정말 프라미스처럼 작동하는 값이 맞을까?
    * new Promise() 구문으로 생성된 프라미스는 p instanceof Promise로 확인하면 될것 같다.
    * 하지만 불행히도 여러 가지 이유로 이 정도만으로는 불충분하다.
    * 사실 프라미스 값은 주로 다른 브라우저 창에서 넘겨받는데, 현재 윈도우/프레임에 있는 프라미스에 있는 프라미스와는 동떨어진 그들만의 프라미스이므로 프라미스 인스턴스 체크만으로는 제대로 확인할 수 없다.
    * 게다가 외부 라이브러리/프레임워크 중에는 ES6 Promise가 아닌 고유한 방법으로 구현한 프라미스를 사용할 가능성도 무시할 수 없다.
    * 진짜 프라미스는 then() 메서드를 가진, '데너블' 이라는 객체 또는 함수를 정의하여 판별하는 것으로 규정됐다.
    * 데너블에 해당하는 값은 무조건 프라미스 규격에 맞다고 간주하는 것이다.
    * 어떤 값의 타입을 그 형태를 보고 짐작하는 타입 체크를 일반적인 용어로는 덕 타이핑이라고 한다.
    ```
    if(
        p !== null &&
        (
            typeof p === "object" ||
            typeof p === "function 
        ) &&
        typeof p.then === "function
    ){
        // 데너블로 간주한다.
    }
    else{
        // 데너블이 아니다.
    }
    ```
    * 뭔가 문제가 있어 보이는 느낌이 든다.
    * 프라미스를 하필 then() 이라는 함수가 정의된 임의의 객체/함수로 이루고 싶지만 이 객체/함수를 프라미스/데너블로서 다룰 생각은 조금도 없다면?
    * 아쉽게도 그렇게는 안 된다.
    * 엔진이 데너블이라고 자동 인식하여 특별한 규칙을 적용하기 때문이다.
    * then() 이란 함수가 있었다는 사실을 미처 몰랐다고 해도 사정은 달라지지 않는다.

* 프라미스 타임아웃 패턴
    * 아주 흔한 경우이다.
    * 프라미스로 해결할 수 있다.
    * 우선 프라미스 스스로 귀결 사실을 알리지 못하게 막을 방도는 없다.
    * 이룸/버림 콜백이 프라미스에 모두 등록된 상태라면 프라미스 귀결 시 둘 중 하나는 반드시 부른다.
    * 물론 콜백 자체에 자바스크립트 에러가 나면결과가 이상하게 나오겠지만 그래도 어쨌든 콜백은 호출된다.
    * 콜백에서 난 에러가 묻혓너느 안 되므로 잠시 후 콜백에서 발생한 에러를 알림 처리하는 방법을 알아보자
    * 그런데 만일 프라미스 스스로 어느 쪽으로도 귀결되지 않으면?
    * 이럴 상황일지라도 경합이라는 상위 수준의 추상화를 이용하면 프라미스로 해결할 수 있다.
    ```
    function timeoutPromise(delay){
        return new Promise(function(resolve, reject){
            setTimeout(function(){
                reject('timeout')
            }, delay)
        })
    }
    Promise.race([
        foo(),
        timeoutPromise(3000)
    ])
    .then(
        function(){
            // foo()!
        },
        function(err){
            // err
        }
    )
    ```
* 인자/환경 전달 실패
    * 프라미스 귀결 값은 딱 하나뿐이다.
    * 명시적인 값으로 귀결되지 않으면 그 값은 undefined로 세팅된다.
    * 하지만 값이야 어떻든, 지금이든, 나중이든 프라미스는 모든 등록한 콜백으로 반드시 전해진다.
    * 여기서 주의할 점이 있다.
    * resolve(), reject() 함수를 부를 때 인자를 여러 개 넘겨도 두번째 이후 인자는 그대로 무시한다.
    * 앞에서 이야기한 보장 체계를 위반한 것처럼 보이지만 엄밀히 따지면 프라미스 체계를 잘못 사용한 대가일 뿐이다.
    * 값을 여러개 넘기고 싶다면 배열이나 객체로 꼭 감싸야 한다.
    * 자바스크립트 함수는 자신이 정의된 스코프의 클로저를 항상 간직하므로 클로저를 통해 얼마든지 계속 주변 상태에 접근할 수 있다.

* 에러/예외 삼키기
    * 어떤 이유로 프라미스를 버리면 그 값은 버림 콜백으로 전달된다.
    * 하지만 프라미스가 생성 중 또는 귀결을 기다리는 도중 언제라도 TypeError, ReferenceError등의 자바스크립트 에러가 나면 예외를 잡아 주어진 프라미스를 강제로 버린다.
    * 즉시값 또는 프라미스가 아닌 값을 Promise.resolve() 에 건네면 이 값으로 이루어진 프라미스를 손에 넣게 된다.
    ```
    var p1 = new Promise(function(resolve, reject){
        resolve(42)
    })
    var p2 = Promise.resolve(42)
    ```
    * p1과 p2는 같다.
    * Promise.resolve()에 진짜 프라미스가 넘어가도 결과는 마찬가지다.
    ```
    var p1 = Promise.resolve(42)
    var p2 = Promise.resolve(p1)
    p1 === p2 // true
    ```
    * 더욱 중요한 사실은, 프라미스가 아닌 데너블 값을 Promise.resolve()에 주면 일단 그 값을 풀어보고 최종적으로 프라미스 아닌 것 같은 구체적인 값이 나올 때까지 계속 풀어본다는 점이다.
    ```
    var p = {
        then: function(cb){
            cb(42)
        }
    }
    p.then(
        function fulfilled(val){
            console.log(val) // 42
        },
        function rejected(err){
            // 실행되지 않는다.
        }
    )
    ```
    * p는 데너블이지만 진짜 프라미스는 아니다.
    * 다행히 대부분 잘 들어맞는다.
    * 하지만 코드가 다음과 같이 바뀌면 이야기는 달라진다.
    ```
    var p = {
        then: function(cb, errcb){
            cb(42);
            errcb('errrr')
        }
    }
    p.then(
        function fulfilled(val){
            console.log(val) //42
        },
        function rejected(err){
            console.log(err) // errrrr
        }
    )
    ```
    * p 는 데너블이지만 그다지 프라미스처럼 작동하지 않는다.
    * 하지만 어떤 p 라도 Promise.resolve()에 넣으면 정규화하므로 안전한 결과를 기대할 수 있다.
    ```
    Promise.resolve(p).then(
        function fulfilled(val){
            console.log(val) // 42
        },
        function rejected(err){
            // 실행되지 않는다.
        }

    )
    ```
    * Promise.resolve()는 데너블을 인자로 받아 데너블 아닌 값이 발견될 때까지 풀어봐서 믿을만한 진짜 순종 프라미스를 즉석에서 내놓는다.
    * 진짜 프라미스 값을 넘기면 도로 내놓으니까 믿음성을 확보하기 위해 Promise.resolve()를 거친다 해서 단점이 될 만한 요소가 전혀 없다.
    * 만일 foo() 호출 후 반환받은 값이 문제 없이 잘 작동하는 프라미스인지 확신은 못해도 데너블인 것만은 확실하다고 치자.
    * Promise.resolve()는 연쇄되어 나온, 미더운 프라미스 감싸미를 내어줄 것이다.
    * Promise.resolve()로 일반 함수의 반환 값을 감싸면 함수 호출을 정규화하여 비동기 작업을 잘 작동하게 할 수 있다는 부수 효과도 있다.
        * 이를테면 foo(42)가 어떨 때는 즉시값을, 어떨 때는 프라미스일 경우 Promise.resolve(foo(42))로 감싸면 항상 결괏값이 프라미스로 고정된다.

* 연쇄 흐름
    * 프라미스에 then() 을 부를 때마다 생성하여 반환하는 새 프라미스를 계속 연쇄할 수 있다.
    * then()의 이룸 콜백 함수가 반환한 값은 어떤 값이든 자동으로 연쇄된 프라미스의 이룸으로 세팅된다.
    * 프리마스 시퀀스가 각 단계마다 진짜 비동기적으로 작동하게 만드는 핵심은 Promise.resolve()에 넘긴 ㅊ어떤 최종값이 아닌 프라미스/데너블일 때 Promise.resolve()의 작동 로직이다.
        * Promise.resolve()는 진짜 프라미스를 받으면 도로 뱉어낸다.
        * 데너블을 받으면 일단 한번 풀어보고 아니면 원하는 값이 나올 때까지 재귀적으로 계속 풀어본다.
        * 이룸/버림 처리기에서 데너블/프라미스를 반환해도 마찬가지로 풀어본다.
        ```
        var p = Promise.resolve(21)
        p.then(function(v){
            console.log(v) //21
            // 프라미스를 생성하여 반환한다.
            return new Promise(function (resolve, reject){
                resolve(v*2)
            })
        })
        .then(function(v){
            console.log(v) // 42
        })
        ```
    * 귀결 메시지 없이 지연-프라미스 생성을 일반화시켜 단계가 많을 경우에도 재사용할 수 있다.
    ```
    function delay(time){
        return new Promise(function(resolve, reject){
            setTimeout(resolove, time)
        })
    }
    ```

* 용어 정의
    * Promise() 생성자
        * 콜백 2개를 넘긴다.
        * 첫 번째는 프라미스가 이루어졌음을, 두 번째는 프라미스가 버려졌음을 표시하는 용도로 쓴다.
        ```
        var fulfilledPr = Promise.resolve(42)
        var rejectedPr = Promise.reject('errrr')
        ```
        * Promise.resolve()는 주어진 값으로 귀결된 프라미스를 생성한다.
        * 예제의 42는 평범한, 프라미스 아닌 데너블 아닌 값이므로 프라미스 fulfilledPr은 42란 값과 함께 이루어진다.
        * Promise.reject('errrr') 'errrr'라는  버림 사유로 폐기된 프라미스 rejectedPr을 생성한다.
        * fullfied가 아닌 resolve인 이유는, Promise.resolve()에 들어가는 프라미스/데너블 객체가 reject일 수도 있기 때문이다.
        * 대신 then()에 제공할 콜백명은 fulfilled(), rejected()가 어울린다.
  
* 에러 처리
    * 동기적인 try...catch 구문은 개발자들이 대부분 익숙한 가장 일반적인 에러 처리 형태이다.
    * 아쉽게도 try...catch는 동기적으로만 사용 가능하므로 비동기 코드 패턴에서는 무용지물이다.
    * 분산 콜백
        ```
        var p = Promise.reject('errr');

        p.then(
            function fulfilled(){
                
            },
            function rejected(err){
                console.log(err) // errr
            }
        )
        ```
        * 이러한 에러 처리 패턴은 표면적으로는 아주 명쾌한 것 같지만 프라미스 에러 처리는 미묘한 부분이 숨겨져 있다.
        ```
        var p = Promise.reject(42);

        p.then(
            function fulfilled(msg){
                // 숫자에는 문자열 함수가 없으니 에러가 발생한다.
                console.log(msg.toLowerCase())
            },
            function rejected(err){
                console.log(err) // errr
            }
        )
        ```
        * msg.toLowerCase() 에서 문법 오류가 발생했는데, 에러 처리기는 이 사실을 모른다.
        * 에러 처리기의 소속은 프라미스 p 이고 p는 42 값으로 이루어진 상태라서 그렇다.
        * p는 불변값이므로 에러 알림은 오직 p.then()이 반환한 프라미스만이 가능한데 여기서는 이 프라미스를 포착할 방법이 없다.
    * 연쇄의 끝에 catch()를 덧붙인다.
        * p로 유입된 에러 및 p 이후 귀결 중 발생한 에러 모두 잡힌다.
        * 만약 handleErrors() 함수에서 에러가 난다면...?
            * catch()에서 반환한 프라미스도 에러가 발생할 수도 있다.
    * 잡히지 않은 에러 처리
        * 일부 프라미스 라이브러리는 '전역 미처리 버림' 처리기 같은 것을 등록하는 메서드를 추가하여 전역 범위로 에러를 던지는 대신 이 메서드가 대신 호출되도록 해놓았다.
        * 그러나 잡히지 않은 에러인지 식별하기 위해 버림 직후 임의의 시간 동안 타이머를 걸어놓는 식으로 구현한 것이다.

* 프라미스 패턴
    * Promise.all([ ])
        * 비동기 시퀀스는 주어진 시점에 단 한개의 비동기 작업만 가능하다.
        * 그런데 2개 이상의 단계가 동시에 움직일 순 없을까?
        * 복수의 병렬/동시 작업이 끝날 때까지 진행하지 않고 대기하는 관문이라는 장치가 있다.
        * 어느 쪽이 먼저 끝나든지 모든 작업이 다 끝나야 게이트가 열리고 다음으로 넘어간다.
        * 이런 패턴을 프라미스 API는 all([ ])에 담았다.
        * 호출 결과 반환된 이룸 메시지는 배열의 형태로 얻을 수 있다.
        * 배열의 순서는 나열한 순서대로 얻을 수 있다.
        * 단 한 개의 프라미스라도 버려지면 Promise.all([ ]) 프라미스 역시 곧바로 버려지며 다른 프라미스 결과도 덩달아 무효가 된다.
    * Promise.race([ ])
        * 여러 프라미스를 동시에 편성하여 모두 이루어진다는 전제로 작동하는데, 결승선을 통과한 최초의 프라미스만 인정하고 나머지는 무시해야 할 때도 있다.
        * 예전부터 이른봐 걸쇠(Latch) 라는 패턴으로, 프라미스에서는 경합이라고 한다.
        * 빈 배열을 인자로 넘기면 바로 귀결되지 않고 메인 프라미스 race([ ])는 결코 귀결되지 않는다.
        * 프라미스 하나만 승자가 되기에 이룸값은 배열이 아닌 단일 메시지다.
    * 타임아웃 경합
        * Promise.race([ ])를 이용하면 프라미스 타임아웃 패턴을 구현할 수 있다.
        ```
        Promise.race([
            foo(),
            timeoutPromise(3000)
        ]).
        then(
            function(){
                // foo()
            },
            function(err){
                // err
            }        
            )
        ```
    * none([ ])
        * all과 비슷하지만 이룸/버림이 정반대다.
        * 따라서 모든 프라미스는 버려져야 하며, 버림이 이룸값이 되고 이룸이 버림값이 된다.
    * any([ ])
        * all과 유사하나 버림은 모두 무시하며, 하나만 이루어지면 된다.
    * first([ ])
        * any([ ])의 경합과 비슷하다.
        * 일단 최초로 프라미스가 이루어지고 난 이후엔 다른 이룸/버림은 무시한다.
    * last([ ])
        * first([ ])와 거의 같고 최후의 이룸 프라미스 하나만 승자가 된다는 것만 다르다.
    * 손수 만들어보기
    ```
    if(!Promise.first){
        Promise.first = function(prs){
            return new Promise(function(resolve,reject){
                prs.forEach(function(pr){
                    Promise.resolve(pr)
                    .then(resolve)
                })
            })
        }
    }
    ```

* 프라미스 API 복습
    * new Promise() 생성자
        * Promise() 생성자는 항상 new 와 함께 사용하며 동기적으로/즉시 호출할 콜백 함수를 전달해야 한다.
        * 이 함수에는 다시 프라미스를 귀결 처리할 콜백 2개를 넘기는데 resolve()와 reject()라고 명명하는 것이 보통이다.
        ```
        var p = new Promise(function(resolove, reject){
            // resolve() 는 프라미스 귀결/이룬다.
            // reject() 는 프라미스를 버린다
        })
        ```       
        * reject()는 그냥 프라미스를 버리지만 resolve()는 넘어온 값을 보고 이룸/버림 중 한가지로 처리한다.
        * 즉시값, 프라미스가 아닌/데너블이 아닌 값이 resolve()에 흘러오면 이 프라미스는 해당 값으로 이루어진다.
        * 반면, resolve()에 진짜 프라미스/데나블 값이 전달되면 재귀적으로 풀어보고 결국 그 최종 값이 프라미스의 마지막 귀결/상태가 된다.
    * Promise.resolve()와 Promise.reject()
        * Promise.reject()는 이미 버려진 프라미스를 생성하는 지름길이다.
        * 따라서 두 프라미스는 본질적으로 동등하다.
        ```
        var p1 = new Promise(function(resolve, reject){
            reject('errrr')
        })
        var p2 = Promise.reject('errrr);
        ```
        * Promise.reject()와 마찬가지로 Promise.resolve()는 이미 이루어진 프라미스를 생성하는 용도로 쓴다.
        * Promise.resolve()는 데너블 값을 재귀적으로 풀어보고 그 최종 귀결 값이 결국 반환된 프라미스에 해당된다.
        ```
        var fulfilledTh={
            then: function(cb){ cb(42) }
        }
        var rejectedth={
            then:function(cb, errCb){
                errCb('oops')
            }
        }
        var p1 = Promise.resolve(fulfilledTh)
        var p2 = Promise.resolve(rejectedTh)
        ```
        * Promise.resolve()에 진짜 프라미스를 넣으면 아무 일도 하지 않는다는 점을 기억하라.
        * 따라서 종류를 모르는 값을 Promise.resolve()에 넣어봤는데 우연히 진짜 프라미스더라도 오버헤드는 전혀 없다.
    * then()과 catch()
        * 각 프라미스 인스턴스엔 then(), catch() 메서드가 들어있고 프라미스에 이룸/버림 처리기를 등록할 수 있다.
        * 프라미스가 귀결되면 두 처리기중 딱 하나만 언제나 비동기적으로 호출된다.
        * then()은 하나 또는 2개의 인자를 받는데 첫 번쨰는 이룸 콜백, 두 번째는 버림 콜백이다.
        * 어느 한쪽을 누락하거나 함수가 아닌 값으로 지정하면 각각 기본 콜백으로 대체된다.
        * 기본 이룸 콜백은 그냥 메시지를 전달하기만 한다.
        * 기본 버림 콜백은 단순히 전달받은 에러 사유를 도로 던진다.
        * catch()는 버림 콜백 하나만 받고 이룸 콜백은 기본 이룸 콜백으로 대체한다.
        * 쉽게 말하면 then(null, ) 이나 다름 없다.
        ```
        p.then(fulfilled)
        p.then(fulfilled, rejected)
        p.then(null, rejected)
        ```
        * then(), catch()도 새 프라미스를 만들어 반환하므로 프라미스 연쇄 형태로 흐름 제어를 표현할 수 있다.
        * 이룸/버림 콜백에서 예외가 발생하면 반환된 프라미스는 버린다.
        * 콜백 반환 값이 즉시값, 프라미스/데너블이 아닌 값이면 반환된 프라미스의 이룸값으로 지정한다.
        * 이룸 처리기가 특정 프라미스나 데너블 값을 반환하면 이 값을 풀어 반환된 프라미스의 귀결 값이 된다.

* 프라미스 한계
    * 시퀀스 에러 처리
    * 단일값
        * 프라미스는 정의 상 하나의 이룸값, 아니면 하나의 버림 사유를 가진다.
        * 간단한 코드라면 별다른 문제가 없으나 로직이 복잡해지면 발목을 붙들 수 있는 특성이다.
    * 취소 불가
        * 일단 프라미스를 생성하여 이룸/버림 처리기를 등록하면, 도중에 작업 자체를 의미없게 만드는 일이 발생하더라도 외부에서 프라미스 진행을 멈출 방법이 없다.
    * 프라미스 성능
        * 콜백식 비동기 작업 연쇄와 프라미스 연쇄의 움직이는 코드 조각이 얼마나 되는지만 보면 아무래도 프라미스가 처리량이 더 많고 그래서 속도 역시 약간 더 느린게 사실이다.
        * 하지만 같은 수준의 믿음성을 보장하기 위해 프라미스에서 간단히 몇 가지 장치만으로 해결했떤 것과 콜백에서 임기 응변식 코드를 덕지덕지 바르는 것을 비교하면...
* 정리하기
    * 
        * 프라미스는 훌륭하다.
        * 마음껏 사용해라.
        * 제어의 역전 문제를 프라미스가 한방에 해결했다.
    * 
        * 프라미스가 콜백을 완전히 없애는 건 아니다.
        * 다만 기존 콜백 코드를 믿을 만한 중계자 역할을 수행하는 유틸리티를 통해 잘 조정하여 서로 조화롭게 작동할 수 있또록 유도하는 것이다.
    * 
        * 프라미스 연쇄는 비동기 흐름을 순차적으로 표현하는 더 나은 방법이다.
        * 덕분에 우리 두뇌가 비동기 자바스크립트 코드를 좀 더 효율적으로 계획/관리할 수 있다.
        
## 제너레이터
* 어떻게 하면 비동기 흐름 제어를 순차적/동기적 모습으로 나타낼 수 있을까
    * 자바스크립트 개발자 대부분이 당연히 그러리라고 믿겠지만 일단 함수가 실행되기 시작하면 완료될 때까지 계속 실행되며 도중에 다른 코드가 끼어들어 실행되는 법이 없다고 알고 있을것이다.
    * 그런데 ES6부터 완전-실행 법칙을 따르지 않는, 제너레이터라는 전혀 새로운 종류의 함수가 등장했다.
    ```
    var x = 1;
    function foo(){
        x++;
        bar();
        console.log('x: ',x)
    }
    function bar(){
        x++;
    }
    foo(); //3
    ```
    * 틀림 없이 bar()는 x++와 console.log(x)사이에서 실행될 것이다.
    * 자바스크립트는 선점형 멀티스레드 언어가 아니기 때문에 끼어들 수 없다.
    * 하지만 foo()자체가 어떤 코드 부분에서 멈춤 신호를 줄 수 만 있다면 이러한 끼어들기(interruption)를 협동적(cooperative) 형태로 나타낼 수 있다.
        * 협동적이라는 단어를 쓴 이유는 코드의 멈춤 위치를 나타내는 ES6 구문이 yield(양도) 이기 떄문이다.
        * 즉, 제어권을 친절하게 협력적으로 양도한다는 뜻이다.
    ```
    var x = 1;
    function *foo(){
        x++;
        yield;
        console.log("x: ",x);
    }
    function bar(){
        x++;
    }
    ```
    * 자, 그럼 foo()의 yield 지점에서 bar()는 어떻게 실행될까?
        ```
        var it = foo();
        it.next();
        x; //2
        bar();
        x; //3
        it.next(); // x: 3
        ```
        * it = foo() 할당으로 *foo() 제너레이터가 실행되는 건 아니다.
        * 제너레이터 실행을 제어할 이터레이터만 마련한다.
        * 첫 번쨰 it.next()가 *foo() 제너레이터를 시작하고 *foo() 첫쨰 줄의 x++가 실행된다.
        * 첫 번째 it.next()가 완료되는 yield 문에서 *foo()는 멈춘다.
        * *foo()는 실제로 실행 중이지만 멈춰있는 상태다.
        * x값을 출력해보니 2이다.
        * bar()를 호출하면 x++에서 x 값은 하나 증가한다.
        * x 값을 다시 확인하면 지금은 3이다.
        * 마지막 it.next() 호출부에 의해 *foo() 제너레이터는 좀 전에 멈췄던 곳에서 재개되어 console.log() 실행 후 현재 x 값 3을 콘솔창에 표시한다.
    * 분명 foo() 가 시작됐지만 '완전-실행' 되지 않고 yield 문에서 잠깐 멈추었다.
    * 나중에 foo() 를 재개하여 완료하지만 꼭 이렇게 해야 할 필요는 없다.
    * 제너레이터는 1 회 이상 시작/실행을 거듭할 수 있으면서도 반드시 끝까지 실행해야 할 필요는 없는 특별한 함수이다.
    * 비동기 흐름을 제어하는 제너레이터 패턴을 완성하기 위한 가장 기초적인 개념이다.

* 입력과 출력
    * 함수는 함수인지라 기본적인 체계, 즉 인자를 입력 받고 어떤 값을 반환하는 기능은 일반 함수와 같다.
    ```
    function *foo(x,y){
        return x*y
    }
    var it = foo(6,7)
    var res = it.next()
    res.value; // 42
    ```
    * 그런데 여타 함수와 제너레이터의 호출 방법이 다른 걸 알 수 있다.
    * foo(6, 7) 은 당연해 보이는데 제너레이터 *foo()는 일반 함수와는 달라서 이것만으로는 실행되지 않는다.
    * 그래서 제너레이터를 제어하는 '이터레이터' 객체를 만들어 변수 it 에 할당하고 it.next() 해야 *foo() 제너레이터가 현재 위치에서 다음 yield 또는 제너레이터 끝까지 실행할 수 있다.
    * next()의 결괏값은 *foo()가 반환한 값을 value 프로퍼티에 저장한 객체다.
    * 즉, yield는 실행 도중에 제너레이터로부터 값을, 일종의 중간 반환 값 형태로 돌려준다.

* 반복 메세징
    * 인자를 받아 결괏값을 내는 기능 이외에도 제너레이터에는 yield와 next()를 통해 입력/출력 메시지를 주고 받는, 강력하고 감탄스러운 기능이 탑재되어 있다.
        ```
        function *foo(x){
            var y = x * (yield);
            return y
        }
        var it = foo(6)
        it.next();
        var res = it.next(7)
        res.value; //42
        ```
        * 먼저, 인자 x 자리에 6을 넘기고 it.next()를 호출하여 *foo()를 시작한다.
        * *foo()에서 var y=x ... 문이 처리될 즈음 yield 표현식에서 걸린다.
        * 여기서 *foo()는 실행을 멈추고 yield 표현식에 해당하는 결괏값을 달라고 호출부 코드에 요청한다.
        * 그리고 it.next(7)을 호출하면 7 값이 yield 표현식의 결괏값이 되도록 전달한다.
        * 따라서 결과적으로 할당문은 var y = 6 * 7 이 된다.
    * yield... 와 next()는 제너레이터 실행 도중 양방향 메시징 시스템으로 가능하다.
    * 제너레이터 끝에 return 문이 따로 없으면 return 문이 있다고 치고 암시적으로 처리한다.
        * return undefined
    * 이러한 질의 응답 체계는 매우 강력하다.
* 다중 이터레이터
    * 구문 사용법만 놓고 보면 이터레이터로 제너레이터를 제어하는 건 선언된 제너레이터 함수 자체를 제어하는 것처럼 보인다.
    * 그러나 이터레이터를 생성할 때마다 해당 이터레이터가 제어할 제너레이터의 인스턴스 역시 암시적으로 생성된다는 사실을 지나치기 쉽다.
    * 같은 제너레이터의 인스턴스를 동시에 여러 개 실행할 수 있고 인스턴스끼리 상호 작용도 가능하다.
    ```
    function *foo(){
        var x = yield 2;
        z++;
        var y = yield( x * z )
        console.log(x, y, z)
    }
    var z = 1;
    var it1 = foo();
    var it2 = foo();
    var val1 = it1.next().value // 2
    var val2 = it2.next().value // 2
    val1 = it1.next(val2*10).value // 40 
    val2 = it2.next(val1*5).value // 600
    it1.next( val2/2 ); // 20, 300, 3
    it2.next( val1/4 ); // 200, 10, 3
    ```
* 인터리빙
    ```
    var a = 1;
    var b = 2;
    function foo(){
        a++;
        b = b * 1;
        a = b + 3;
    }
    function bar(){
        b--;
        a = 8 + b;
        b = a * 2;
    }
    ```
    * 일반 자바스크립트 함수 foo(), bar() 둘 중 하나는 다른 함수보다 먼저 완전-실행될 것이다.
    * 하지만 foo()의 개별 문을 bar()에 인터리빙 하여 실행하는 건 불가능하다.
    * 따라서 실행 결과는 두 가지만 가능하다.
    * 반면에 제너레이터는 문 사이에서도 인터리빙 할 수 있다.
    ```
    var a = 1;
    var b = 2;
    function* foo(){
        a++;
        yield;
        b=b*a;
        a=(yiled b) + 3;
    }
    function* bar(){
        b--;
        yield;
        a=(yield 8) +b
        b = a * (yield 2);
    }
    ```
    * 이터레이터를 제어하는 step() 헬퍼
    ```
    function step(gen){
        var it = gen();
        var last;

        return function(){
            last = it.next(last).value;
        }
    }
    ```

* 제조기와 이터레이터
    ```
    var gimmeSomething = (function(){
        var nextVal;
        return function(){
            if(nextVal === undefined){
                nextVal = 1;
            }else{
                nextVal = (3 * nextVal) + 6;
            }
            return nextVal;
        }
    })();
    gimmeSomething(); // 1
    gimmeSomething(); // 9
    gimmeSomething(); // 33
    gimmeSomething(); // 105
    ```
    * 이런 작업들은 이터레이터로 해결 가능한, 아주 일반적인 설계 패턴이다.
    * 이터레이터는 생산자로부터 일련의 값들을 받아 하나씩 거치기 위한, 명확한 인터페이스이다.
    ```
    var something = (function(){
        var nextVal;
        return{
            // for...of 루프에서 필요하다
            [Symbol.iterator]: function(){return this},
            // 표준 이터레이터 인터페이스 메서드
            next: function(){
                if(nextVal === undefined){
                    nextVal=1;
                }else{
                    nextVal = (3*nextVal)+6
                }
                return {done: false, value:nextVal}
            }
        }
    })()
    ```
    * next()를 호출하면 프로퍼티가 2개인 객체가 반환된다.
    * done 은 이터레이터 완료 상태를 가리키는 불리언 값이고 value는 순회 값이다.
    * for...of 루프는 매번 자동으로 next()를 호출하다가 done:true를 받으면 그 자리에서 멈춘다.
    
* 이터러블
    * 이전 예제 something 처럼 next() 메서드로 인터페이스하는 객체를 이터레이터라고 한다.
    * 그러나 순회 가능한 이터레이터를 포괄한 객체, 이터러블이 더 밀접한 용어다.
    * ES6 부터 이터러블은 특별한 ES6 심볼값 Symbol.iterator 라는 이름을 가진 함수를 지니고 있어야 이 함수를 호출하여 이터러블에서 이터레이터를 가져올 수 있다.
    * 필수 요건은 아니지만 일반적으로 함수를 호출할 때마다 방금 만든 새 이터레이터를 내어준다.
    * for...of 루프는 자동으로 Symbol.iterator 함수를 호출하여 이터레이터를 생성한다.
    * something 정의부에 이런 코드가 있었다.
    ```
    [Symbol.iterator]: function(){return this}
    ```
    * 약간 헷갈리게 생겼지만 something 값(something의 이터레이터 인터페이스)을 이터러블로 만드는 코드이다.
    * 이제 something은 이터러블이면서 동시에 이터레이터이다.
    * for..of 구문은 something이 이터러블이라는 전제하에 Symbol.iterator 함수가 있는지 찾아보고 호출한다.
    * 이 함수는 그냥 return this 하여 돌려주기 떄문에 for...of 루프는 속사정을 알 길이 없다.

* 제너레이터와 이터레이타ㅓ
    * 제너레이터는 일종의 값을 생산하는 공장이다.
    * 이렇게 만들어진 값들은 이터레이터 인터페이스의 next()를 호출하여 한번에 하나씩 추출할 수 있다.
    * 따라서 엄밀히 말하면 제너레이터 자체는 이터러블이 아니지만 아주 흡사해서 제너레이터를 실행하면 이터레이터를 돌려받게 된다.
    ```
    function* foo(){...}
    var it = foo();
    ```
    * 그러므로 무한 수열 생산기 something은 다음과 같이 구현할 수 있다.
    ```
    function *something(){
        var nextVal;
        while(true){
            if(nextVal === undefined){
                nextVal=1;
            }else{
                nextVal=(3*nextVal) +6;
            }
            yield nextVal
        }
    }
    ```
    * 보통 자바스크립트 프로그램에서 while...true 루프를 break, return 문도 없이 사용하면 자칫 동기적인 무한 루프에 빠져 브라우저 UI를 멈추게 할 수 있다.
    * 그러나 제너레이터는 루프 안에 yield만 있으면 전혀 염려할 일이 없다.
    * 어차피 제너레이터가 순회할 때마다 멈추면서 메인 프로그램 또는 이벤트 루프 큐에 바톤을 넘겨줄 테니 말이다.
    * 제너레이터는 yield를 만나면 일단 멈추기 떄문에 function* something()의 상태는 그대로 유지된다.
    * 다시 말해, 호출할 때마다 변수 상탯값을 보존하기 위해 습관적으로 클로저 구문을 남발할 필요가 없다.
    * 개발자가 직접 이터레이터 인터페이스를 작성할 필요가 없으므로 단순할 뿐만 아니라 사실 내용을 더 분명하게 표현한 추론적인 코드다.
        * while...true 루프 하나만 봐도 이 제너레이터의 목적이 무한 실행 중 언제라도 요청을 하면 값을 만들어 내는 것이라는 사실을 알 수 있다.
    * 화려한 새 제너레이터 *something()을 for...of 루프에 얹어도 기본적으로 작동 방식은 똑같다.
    ```
    for(var v of something()){
        console.log(v)
        if(v >500) break
    }
    ```
    * 전처럼 something을 어떤떤 값으로 참조한 게 아니라 *something() 제너레이터를 호출해서 for...of 루프가 사용할 이터레이터를 얻는다.
    * 그런데 for(var v of something) 처럼 쓴다면?
        * 여기서 something은 제너레이터지 이터러블이 아니다.
    * something() 을 호출하면 이터레이터가 만들어지지만 정작 for...of 루프가 원하는 건 이터러블이 아닌가?
        * 제너레이터의 이터레이터에도 Symbol.iterator 함수가 있고 기본적으로 return this를 한다.
        * 한마디로 제너레이터의 이터레이터도 이터러블이다.
* 제너레이터 멈춤
    * 좀 전에 *something() 제너레이터의 이터레이터 인터페이스는 루프 내에서 break를 호출한 이후에 영원히 정지 상태가 될 것 같아 보인다.
    * 그러나 이 떄 알아서 처리해주는 숨겨진 기능이 하나 있다.
    * 일반적으로 break, return 또는 잡히지 않은 예외로 인해 for...of 루프가 비정상 완료되면 제너레이터의 이터레이터를 중지하도록 신호를 준다.
        * 엄밀히는 루프가 정상 완료될 경우에도 for...of 루프는 이터레이터에게 신호한다.
        * 제너레이터 관점에서 어차피 자신의 이터레이터가 먼저 끝나야 for...of 루프 역시 완료되므로 별 의미는 없다.
    * for...of 루프가 자동으로 전송하는 신호를 수동으로 이터레이터에 보내야 할 경우도 있다.
    * 이럴 떄 return()을 호출한다.
    * 제너레이터가 외부적으로 완료된 다음에도 내부에서 try...finally 절을 사용하면 실행할 수 있다.
    * 이는 자원(DB 접속)을 정리할 떄 유용한 기법이다.
    ```
    function *something(){
        try{
        }
        // 정리 코드
        finally{

        }
    }
    ```
    * for...of 루프에서 break 하면 finally 절로 이동할 것이다.
    * 외부에서 return() 문을 써서 수동으로 제너레이터 인스턴스를 멈추게 할 수 있다.
    ```
    var it = something()
    for(var v of it){
        console.log(v);
        if(v>500){
            console.log(it.return('hello world').value)
        };
    }
    ```
    * it.return() 하면 제너레이터 실행은 즉시 끝나고 finally 절로 옮겨간다.
    * 또 return()에 전달한 인자값이 반환 값이 되어 예제처럼 'hello world'가 바로 출력된다.
    * 제너레이터의 이터레이터는 이미 done:true 이므로 break문을 넣지 않아도 되며 for...of 루프는 다음 순회를 끝으로 막을 내린다.
* 제너레이터를 비동기적으로 순회
    ```
    function *main(){
        try{
            var text = yield foo(11, 31);
            console.log(text)
        }catch(err){
            console.log(err)
        }
    }
    var it = main();
    it.next();
    ```
    * yield foo(11, 31) 에서 foo(11, 31) 호출이 일어나고 반환 값은 없으므로 (yield undefined) data를 요청하기 위해 호출을 했지만 실제로는 yield undefined를 수행한 셈이다.
    * 아직은 이 코드가 뭔가 의미있는 일을 하기 위해 yield된 값이 필요한 상태는 아니니 괜찮다.
    * 여기서 yield는 메시지 전달 수단이 아닌 멈춤/중단을 위한 흐름 제어 수단으로 사용한 것이다.
    * 실제로는 제너레이터가 다시 시작한 이후로 메시지 전달은 단방향으로만 이뤄진다.
    * 따라서 yield를 만나 멈추면서 제너레이터는 "내가 나중에 어떤 값을 갖고 와서 변수 text에 할당해야 하나요?" 라고 묻는다.
        * 누구에게 하는 질문일까?
        * AJAX 요청이 성공하면 다음 한 줄이 실행된다.
        ```
        it.next( data )
        ```
        * 응답 데이터 수신과 동시에 제너레이터는 재개되고 좀 전에 멈췄던 yield 표현식은 이 응답 데이터로 즉시 채워진다.
        * 그 뒤, 제너레이터 코드를 다시 시작하면서 이 값은 지역 변수에 text에 할당된다.
* 동기적 에러 처리
    * it.throw(err)
    * 이러한 제너레이터의 yield-멈춘 기능은 비동기 함수 호출로부터 넘겨받은 값을 동기적인 형태로 return 하게 해줄 뿐만 아니라 비동기 함수 실행 중 발생한 에러를 동기적으로 catch 할수 있게도 해준다.
    ```
    function *main(){
        var x = yield 'Hello World!';
        yield x.toLowerCase();
    }
    var it = main();
    it.next().value; // Hello World

    try{
        it.next(42)
    }
    catch(err){
        console.log(err)
    }
    ```
    * 
* 제너레이터 + 프라미스
    * 이터레이터는 프라미스가 귀결 되기를 리스닝하고 있다가 제너레이터를 이룸 메시지로 재개하든지 아니면 제너레이터로 버림 사유로 채워진 에러를 던진다.
    * 프라미스, 제너레이터를 최대한 활용하는 가장 자연스러운 방법은 우선 프라미스를 yield한 다음 이 프라미스로 제너레이터의 이터레이터를 제어하는 것이다,
    ```
    function foo(x, y){
        return request(
            'http:// x / y '
        );
    }

    function *main(){
        try{
            var text = yield foo(11, 31);
            console.log(text)
        }catch(err){
            console.log(err)
        }
    }

    var it = main();
    var p = it.next().value;
    p.then(
        function(text){
            it.next(text);
        },
        function(err){
            it.throw(err)
        }
    )
    ```
    * 

