# 화살표 함수(Arrow Function) 또는 람다식(Lambda)의 특징

## 1.this 값은 함수를 정의할 때 결정된다

람다식의 this는 알아먹기 매우 불편하다.

람다식은 bind, apply, call을 이용해도 this의 값이 변하지 않는다.

함수가 정의 되고나서야 bind,apply,call가 적용되는 것이므로 의미 없는 짓이다.

객체의 필드값으로 람다함수를 넣어도 "함수" 또한 "객체"이기 때문에 함수 객체의 this를 바인딩한다.

그냥 모두가 람다식에 this를 부르지 않는 것이 모두에게 행복하다.

## 2. arguments 변수가 없다.

람다식이 만들어지고 처음 값을 보면 arguments : [TypeError : ... ]와 caller : [TypeError : ... ]를 볼 수 있다
```JavaScript
var a = ()={}
dir(a)  // 여기서 볼 수 있음
```
그리고 값을 대입하거나 읽어오는 것 자체에 에러를 던져버린다.

람다식에는 __proto__이 존재하지만,  prototype이 존재하지 않는다.

즉 함수의 근본을 이루는 **__proto__**에 Function.prototype이 존재하고

Function.prototype의 __proto__에 Object가 존재할 뿐이다.

함수 객체에 대한 prototype이 존재해야 함수객체 내부에 값을 보유하고 이를 통해서 클로저를 만들수 있는데

람다는 prototype이 존재하지 않는다.

</br> </br>


즉 람다함수는 주어진 스코프 ("()=>{}"에서 "{}"부분) 내에서 함수 노릇만 할 수 있는 반쪽짜리 함수객체라고 볼 수 있다.

어떻게 보면 람다함수는 함수형 프로그래밍이 아닌 프로그램언어의 함수의 특징라고 보면 이해할 수 있겠다.


## 3. 생성자로 사용할 수 없다.

2.에서 말 했듯이 객체의 특성인 prototype이 없기 때문에 Object의 함수 자체로서의 속성이 존재하지 않는다.
그래서 복제본의 객체를 만들 수 있는 생성자를 보유하지 못한다.

## 4. yield 키워드를 사용 할 수 없다.

2,3에서 말 했듯이 람다식은 객체가 아닌 함수에 가깝다.

제너레이터는 또 다른 객체 중 하나다. 즉, 생성자 노릇을 못하므로 제너레이터를 암시하는 yield를 못 쓴다.


인터프리터를 어떻게든 속이려고 아스테리크("*")를 사용해서 제너레이터를 만드려고 해도 실패한다.
```JavaScript
var falseGenerator  = *()=>{};    // --> Syntax Error
var falseGenerator2 = *(()=>{}); // --> Syntax Error
var gooodGenerator  = function* () { yield 20; };
```
