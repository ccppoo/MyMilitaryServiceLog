## 클로저의 탄생

1. 변수는 어떤 방식으로 작동되는가

2. 메모리는 어떻게 관리되는가 - GC

3. 함수에서 쓰인 변수는 어떻게 GC에서 살아남는가 - 클로저

※ 메모리와 포인터에 관한 내용은 미래의 나를 위한 단지 상기용으로 간략히 작성한 것으로 불편한 분들은 읽는 것을 삼가하길 바란다.

### 1. 변수는 어떤 방식으로 작동되는가

C에서 변수를 선언할 때 자료형(int, long, char, 등)과 이름과 함께 선언한다.

변수 그 자체만 선언 할 수도 있고, 변수 선언과 동시에 값을 대입할 수 있다.
```C
int myData;
int yourData = 10;
```
변수를 선언하면 데이터를 저장할 메모리를 제공한다.

그럼 당연히 데이터를 저장할 메모리 주소를 C에서 준다.

포인터(Pointer)의 개념을 빌리면 다음과 같은 문장으로 나타 낼 수 있다.

```C
myData   pointing 0x111111 containing data void and read as int
yourData pointing 0x222222 containing data 10   and read as int
```

하지만 자바스크립트는 C와 달리 메모리 주소와 데이터 형식을 자동으로 결정하기 때문에 표면상으로 다음과 같이 보인다.

```JS
myData   (인지 x : pointing 0x111111) containing data undefined (인지 x : and read as undefined)
yourData (인지 x : pointing 0x222222) containing data `10`      (인지 x : and read as Number)
```

당연하듯이 JS 또한 C 처럼 메모리상에서 관리되고 있다는 점을 알아야한다.

<br>

-------

<br>

### 2. 메모리는 어떻게 관리되는가 - GC

메모리를 직접 관리하는 C와 C++와 달리 JS를 포함한 대부분의 언어는 자동으로 사용하지않는 메모리를 직접 관리해준다.

JS는 두 가지 방식으로 메모리를 정리해준다.

##### (1) 레퍼렌스 카운팅 (Reference Counting)

레퍼렌스(Reference)란 간단히 말하면 변수 혹은 데이터를 **읽는 것**과 같다.

다음과 같이 변수를 부르기만 해도 레퍼렌스 된 것이라고 볼 수 있다.

```JS
function data(num){
  this.num = num
}

var myData = new data(20);
{
  myData
}
```
**data(20)**이라는 데이터가 있는 메모리 주소(0x333333)를 가르키는 변수 data가 있다

하나의 데이터에 가르키는 변수가 여러개일 수 있다.

```JS
var yourData = myData;

myData   pointing 0x333333 containing class "data" with `20`
yourData pointing 0x333333 containing class "data" with `20`
```
즉, 메모리 주소 0x333333에 두개의 변수가 가르키는 걸 말한다.

이 떄 참조(pointing)을 직접 제거하거나 (delete 연산자) 간접적으로 제거할 수 있다 (값 대입)
```JS
간접 제거 : delete myData
직접 제거 : myData = new data(40);
```
위와 같이 참조를 제거하다가 데이터의 참조가 없어져면 GC에서 메모리를 해제시킨다.

다만 doubled linked list와 같이 서로 참조하는 자료형의 경우 head나 tail 데이터를 해제해도

중간에 있는 list data가 서로 참조하고 있기 때문에 (참조 횟수 줄어들지 않음) GC에서 메모리를 해제하지 않는 일이 일어나기도 한다.

이와 같은 순환참조된 데이터의 경우 사용자가 직접 메모리를 제어하지 않는이상 메모리가 해제되지 않기 때문에

메모리가 낭비가 일어난다 (Memory Leak)

##### (2) 마크 앤 스윕 (Mark and Sweep) ★

레퍼렌스 카운팅의 문제를 보완하기 위해서 나온 방식이 마크 앤 스윕이다.

사용자 및 프로그램에서 사용되는 모든 자료형이 참조하고 있는 메모리를 추적해서

사용하고 있는(**읽을 수 있는**) 메모리를 확인하고(Mark)

모든 메모리를 추적한 뒤 확인이 안된 메모리를 해제(Sweep)하는 방법이다.

스크립트 문맥상 **사람이 읽을 수 있는 변수명 상태**가 아니어도 된다

```JS
{
  var num = 1;
}
var num = 2;
```
num 변수는 스코프가 끝난 뒤에 사용되지 않기 때문에 해제 될 것이다.

그리고 스코프 아래의 num은 위의 num과 다른 num이다.

가르키는 메모리 주소가 다르다. **사람이 붙여준 이름만 같은 메모리**일 뿐이다.

동명이인 변수라고 보면 된다.

<br>

-------

<br>


### 3. 함수에서 쓰인 변수는 어떻게 GC에서 살아남는가 - 클로저

```JS
function closure() {
  var data = 20;
  return function() {
    return data
  }
}
var closureFunction = closure()
closureFunction() // 20
```
위에서 var data는 function closure이 종료된 후 (return) 메모리에서 없어져야 해야하지만

반환되는 함수(익명)에서 data를 참조하고 있기 때문에

겉으로 사람이 보기에 data는 없어지지만 data가 참조하고 있는 메모리 주소는 해제가 안된다.

이를 이용한 것이 closure이다.

그리고 매번 함수 closure()를 사용해도 var data는 새로 선언되기 때문에

기존에 만들어진 closureFunction의 data와 다른 데이터를 말하는 것이다.

이를 이용해서 함수 팩토리, private 필드를 가진 객체를 만들 수 있다.

--------


### 예시 

이런식으로 오브젝트 생성자로 만들거나
```JS
function User(name, age) {
	return {
		get name(){
			return name;
    	},
		get age(){
			return age;
        }
    }
}
var ccppoo = User("ccppoo", 99)
ccppoo.age // 99
```

일반 객체처럼 만들 수도 있다
```JS
function User(name, age) {
	_name = name;
	_age = age
	return {
		getName : function(){
			return _name;
    	},
		getAge : function(){
			return _age;
        }
    }
}
var ccppoo = User("ccppoo", 99)
ccppoo.getAge() // 99
```

이 코드에서 name을 직접 제어한답시고 아래와 같이 해도...
```JS
ccppoo._name = "Grass"
ccppoo.getName() // ccppoo
```
ccppoo 객체에 새로운 "name"이라는 필드를 만드는 것에 지나지 않다.

왜냐하면 User 객체를 만드는 시점에서 존재한 _name의 메모리 주소(컴퓨터의 관점)를 참조하기 때문이다.

