다룰 내용 : 클로저의 간단한 이해와 은폐가 되는 방법(간략히)

----------

클로저는 은폐다.

함수가 객체다 보니, 사실상 **외부 접근자를 제공하지 않는 private 맴버들이 존재하는 것과 같다.**

단지 함수의 성격과 같이 만드는 기능만을 위해서 존재하는 점에서 객체와 다르다.


많은 예시로 나오는 카운터(counter)를 예시로 들겠다.

```JS
function counter(){
  var count = 0;
  return function () {
    return count++;
  }
}

let a = counter()
let b = counter()

a() // --> 0
a() // --> 1
a() // --> 2

b() // --> 0
b() // --> 1
b() // --> 2
```

counter 함수는 리턴값으로 함수를 반환한다.

그리고 리턴하는 함수(이하 리턴함수)는 counter의 *count*변수를 알고 있다.

알고 있다는 의미는 리턴함수 내부에서 쓰이고 이는 즉 메모리 주소 값을 안다고 할 수 있다. 

**하지만** 정작 *count*를 알고 있는 counter 함수는 리턴함수를 return 하고 실행을 마친상태다.

즉, *count* 변수를 갖고 있는 counter 함수가 스텍에서 제거된 상태다.

하지만, *count*는 리턴함수(a, b)가 각자 참조하고 있으므로 메모리에서 해제되지 않는다. 

...


**이 때 count 변수에 접근할 수 있는 방법은 무엇이 있을까?**

리턴함수 a,b를 통해서 밖에 접근 할 수 밖에 없다.

마치 private 필드에 있는 객체 **인스턴스**와 같다고 볼 수 있다.

...


사실 
```JS
let a = counter()
let b = counter()
```
부분에서 counter이라는게 생성자처럼 쓰인다고 볼 수 있다는 점에서 행동을 유추할 수 있다.


----------

사실상 일반 객체, 클래스와 같이 쓰임이 같아서 다음과 같이 사용할 수도 있다.

```JS
function man(name, age){
  var _name = name;
  var _age = age;
  return {
    // getName : (()=> _name)
    getName : function () { return _name; },
    getAge : function () { return _age; },
    setAge : function(age) { _age = age; }
  }
}

var someone = man("Tim", 20);

someone.getName() // --> Tim
someone._name = "I'm not Tim"
someone.getName() // --> Tim
```

이와 같이 private 필드에 있는 생성자처럼 사용이 가능하다.

*someone*의 _name은 *someone*의 객체 필드에 새로운 *_name*을 말하는 것이라 클로저 내에 정의된 var name과 다른 메모리 주소를 가르킨다.

**getName이 가르키는 _name** : function man의 필드에 있었던 _name

**someone._name이 가르키는 _name** : someone 인스턴스 필드에서의 **위와 다른** _name

