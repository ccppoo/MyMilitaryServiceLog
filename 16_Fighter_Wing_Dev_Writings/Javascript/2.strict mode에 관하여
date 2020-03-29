2020/03/27 (목)

개발환경 : Chrome 개발자모드 - 콘솔
책 : 모던 자바스크립트 입문 - 이소 히로시

책 436pg ~

strict mode는 버그가 발생되기 쉬운 문법을 사전에 제거하는 것이다.
이 글은 왜 JS 개발자들이 버그가 발생하기 쉽다고 판단했는지, 기존의 "non" strict mode와 다른 것이 무엇인지
스스로 파악하고 추론하기 위해 작성된 것이다.

★또한 성능에 저해가 되는 것들에 대해 strict mode에서 금지하고 있으므로 나중에 꼭 다시 한번 보도록 하자

★더 자세한 내용은 developer.mozilia.org에 정리된 문서를 보자.


① 변수는 모두 선언후 사용. 선언되지 않은 변수, 함수, 함수의 인자에 값을 대입하면 Referebce Error 발생 -
(이해 Not OK!)

```javascript
a = 10;
var b = 20;
this.a; // 10
this.b  // 20

c // "use strict" -> Error ,  "not strict" -> undefined
```

★"선언되지 않은 함수의 인자 ... "는 어떤 것을 말하는지 이해가 안된다
```javascript

"use strict";
((num)=> { console.log(num);})(10, 20); // 10

(function(a,b) { console.log(a+b); })(1,2,3,4,5); // 3
```
위에서 람다식의 경우는 선언 안된 함수의 인자 '20' 때문에 Error 던져야 하는 것 아닌가?
위의 익명함수의 경우는 인자 '3,4,5' 때문에 Error 던져야 하는 것 아닌가?

////////////////////////////////////////////

② 함수를 직접 호출할 때, 함수 안의 this 값이 undefined가 된다.
non-strict모드에서는 함수 안의 this 값이 전역객체 (window) 참조가 된다.
└ [ bind( ... ) 하기 전 상황을 말하는 듯!! ]

└ function도 객체인데 왜 this참조를 전역객체(window)로 하지?
    └ 중첩함수 or 객체 내부 함수 아니면 전역 환경 (Window)에서 함수 선언한거니깐 thisBinding이
window로 지정되는 것(맞나?)

└ 위와 같은 상황 때문에 객체에 활용하기 위해서 함수 내부에 this를 호출하는 경우, call(), apply(),
bind() - (pg304)를 통해서 thisBinding이 확실하게 되도록 한다.
└ 나중에 addEventListener를 써야하는 경우<Object Name>.prototype.handleEvent -
(pg598) 에 함수를 대입하면, 객체 인스턴스 이름만 매개변수로 넣는 것만으로 작동한다.

== 1) 다양한 경우에서의 this값이 변하는 예시 ==

1. var a = function() { console.log(this) ;}
   a();

   "use strict"; //  -> undefined
   "not strict"   //  -> [object Window] (전역 객체)

2. var b = new a();
    b;

   "use strict"; //  -> [object Object]
   "not strict"   //  -> [object Object]
   (이 때의 this는 생성자 함수로써의 "a"를 지칭하는 것으로 strict모드와 관계없다.

== 2) 람다의 경우 this 값 도출 방법 ==

(()=> {console.log(this); } )();    // 익명함수 정의후 바로 사용

   "use strict"; // -> window
   "not strict"   // -> window

왜 이런 결과가 나오는 걸까?
Step 1) strict mode에서 우선(실행 문맥; Excutional Context 생성 전) 일단은 다른 함수들과
동일하게 this값은 undefined임

Step 2) 호출함과 동시에 람다식 내부의 this값이 람다식 외부를 기준으로 thisBinding됨 (strict모드의
유무 상관없이 람다식의 특징, 그리고 이때 외부는 전역객체(window))

Step 3) 위의 2가지의 과정을 걸쳐 위와 같은 예시에서 this의 값이 전역객체다. Step 1이 없는 경우는 "not
strict"모드와 동일하다


== 3) ★나중에 해결할 문제 및 궁금한 것★ 질문!  ==

var a = function(){
                console.log("a : " + this);
                this.func = function() { console.log("this.func : " + this); };
                this.func();
           };

* not strict mode *
a(); // -> a : [object window], this.func : [ object window ]

* use "strict mode" *
a(); / -> a:undefined,  and then throws ... "func" is undefined! Error

└─> Why is this happed???

////////////////////////////////////////////

③ with 문은 사용할 수 없다.

JS의 with문은 파이썬의 with 문과 달리 C#의 using <Class Name> { ... } 과 동일하다.

with( Math ){
    console.log(cos(20));
    ....
}

본인은 파이썬을 많이 사용했기 때문에 라이브러리를 참고하는 경우,
많은 메소드를 사용하지 않는 이상 import문을 from ... import ... 처럼 필요한 것만 사용했기 때문에
with문을 사용할 수 없다는 것에 딱히 큰 상관은 없다.

Mozila MDN에 with문은 다른 "name space + 같은 메소드 이름의 경우"와
with( Math) { ... }  블럭 내부에 Math에 없는 메소드가 있는경우 인터프리터가 찾으러 다니는 시간 때문에
strict모드에서 사용을 금지했다고 한다. 인정!

////////////////////////////////////////////

④ 함수 정의문에 같은 이름의 인수가 있으면 문법 오류가 발생한다.

일반적인 경우에 같은 이름의 함수를 또 사용하고 싶은 마음에 재정의해서 사용할 수 있겠으나 미래의 나, 또는 협업자를 위해
이런 짓은 하지 말자.

////////////////////////////////////////////

⑤ 객체에 같은 이름의 프로퍼티가 있으면 문법 오류가 발생한다.

4번 항과 동일한 이유로 인정하는 부분이다.

////////////////////////////////////////////

⑥ NaN, Infinity, undefined 표기시 TypeError Throw (이해 not OK)

"user strict";
var a = NaN; /// -> OK

실전에서 저 위의 3가지 값을 사용하는 경우는 문자열 합연산('+') 말고 보편적인 예시가 있는가??

////////////////////////////////////////////

⑦ arguments[i]는 호출되었을 때의 인수 값을 유지한다(Read Only).
    not strict모드에서는 arguments[i]는 읽기, 쓰기, 수정이 가능하다

매소드의 매개변수를 하나의 선언된 값으로 재사용하는건 자주 했었다.
함수자체를 만들 때 나중에 햇갈리지 않도록 나름 이름을 신중히 만들기 때문이다.

재사용이 꼭 필요하거나, 확장가능한 프로젝트의 경우 함수또한 객체니깐
매개변수를 예를 들어
function sayTo(listener_){
    var listener = listener_;
    ....
}
처럼 해서 재사용 원본은 냅두도록 하자

////////////////////////////////////////////

⑧ arguments.callee를 읽을 수 없다. 읽기 시도하면 TypeError가 발생한다.

arguments.callee는 (익명)함수를 다시 쓰거나(재귀), 객체, this 값을 참조하기 위해 다시 쓸 수 있는 요긴한 객체다.
인라인 함수와 같은 최적화를 상당히 방해한다는 이유로 금지되었다.

⑨ eval로 실행한 코드는 호출자의 유효범위안에 새로운 변수나 함수를 선언 할 수 없다.

파이썬에서도 eval, expr와 같은 함수는 프로그래밍 상에서 위험한 것들이다.
편하지도 않고, input()이라는 대체 가능한 것들이 있듯이 상당히 "raw"한 것들과 작용하는 함수들은 최대한 사용하지 않는 것이 좋다.

eval("var b = 10") 같은 경우 정의되는 평가되지만, 정작 쓰지 못하는 쓰래기만 만든다.
마법스러운 것들은 strict 모드를 쓰나 안쓰나 기피하자.


