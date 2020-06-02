### 참고한 영상 

1. [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ&list=WL&index=3&t=0s)
| Philip Roberts | JSConf EU

2. [In The Loop](https://www.youtube.com/watch?v=cCOL7MC4Pl0&list=WL&index=4&t=6s)
| Jake Archibald | JSConf.Asia


! 대략적인 개념을 정리한 내용으로 심오한 내용은 다루지 못함

### 서문

자바스크립트는 Singal Process, Singal Thread다.

여담이지만, 똑같은 Single Process지만 multi Thread인 파이썬에 비해서 '느리다'라는 말은 못 들어본 것 같다.

파이썬에 비해 JS에 대한 기대치가 다르기 때문인걸까 아니면 사용 용도가 다르기 때문에 그런걸까, 이런 생각이 많이 들었다.

...

아무튼 JS가 특히 Singal Thread이기 때문에 나는 자바스크립트가 비동기를 어떻게 처리하는지 궁금해했다.

Singal Thread면 하나의 Call stack이 존재하는 것이고, stack은 LIFO(Last in First out)인데 

**비동기인 작업이 Stack에 적재된 이후 다른 작업이 스택에 계속 쌓이는 동안 비동기 작업이 완료되면 스택 중간에서 꺼내쓰나??**

라는 질문으로부터 공부를 시작했다.

파이썬, 자바, C++를 공부하고 있을 당시에는 Multi Threading이 가능하니깐 이 의문에 대해서 크게 고민해보지 못했다.

JS 덕분에 접할 수 있는 기회를 갖게되었다!

-------

### Callstack, 프로세스

![Event Loop Image in Web](./Javascript/callstack.png)

JS는 위와 같은 구조로 이루어져있다.

위 그림과 같은 구조에서 Web Api를 빼면 하나의 프로세스(Process)며, call stack이 여러개 있으면 멀티 스레드다.

JS는 설계 처음부터 싱글 프로세스 싱글 스레드를 지원하고 있다.<br><br>

call stack은 변수 선언, 함수 선언, 함수 평가, 등 '실행'된다는 것들을 모두 담는 곳이다.

일단 무언가를 실행하기 전에 담아두는 바구니라고 생각할 수 있다.

스택의 구조상 맨위에 있는, 즉 가장 최근에 적재한 작업을 먼저 실행한다.

![stackImage](./Javascript/stack.png)

JS는 싱글 스레드라고 말하지만, 웹상으로는 많은 작업들이 빠른시간 내 요구되기 때문에 Web Api라고 불리는 스레드가 또 존재한다.

DOM과 같이 중요한 파일을 직접 제어하는 일 같은 경우 모두 메인 스레드가 처리하도록 되어있다.

JS에는 모든 것들이 call stack에 쌓이고 바로바로 실행되어 스택에서 제거되지 않는다.

대표적으로 http 연결과 같은 빠른 시간내 반응을 완료를 보장할 수 없는 작업의 경우 비동기(async)적으로 실행된다.

근데 만약 다운로드가 4~5초 넘게 걸리는데 call stack에 계속 쌓인 상태로 웹이 작동한다면

다운르도 작업만 하느라 4~5초 동안 JS 작업을 독점하게 된다 (다운로드가 완료되고 메서드가 끝날 떄까지)

...

그러면 화면을 1초당 60번 업데이트를 해야할 것을(화면 주사율 60Hz) 못하고

서비스 이용자들은 랙걸린다고 사이트를 꺼버린다. 안된다...

```JS
// 1)
while(i<1000000000){
  //...
}

// 2)
var req = new XMLHttpRequest();
req.open("GET", "..super big item");
req.send()
```
그래서 비동기적인 작업과 비동기적으로 작성된 메서드들, 처리하는데 오래걸리는 작업들이

리소스를 계속 잡아먹는 것을 막기위해 non-blocking, async, lazy evaluation으로 평가된 메소드를

call stack에서 Web Api로 넘겨버린다.

...

Web Api에서 처리된(처리할) 작업들 (또는 리턴값들)은 완성된 후 Callback Queue로 넘겨진다.

Callstack이 어느정도 비워지면 Event Loop가 Callback Queue에서 결과물들을 call Stack에 넣어준다.

일 처리의 우선순위는 Callstack > callback queue, render queue 이므로

Queue에 있는 작업 또는 결과물을 callStack으로 Event Loop가 넘기는 행위를 **A**,

callStack에 있는 작업 또는 결과물을 처리하는 행위를  **B**라고 하면

callstack에 있는 일들이 비워진 후에 다음과 같이 진행된다(대략적으로).

```txt
A -> B -> A -> B -> A ...
```

------

### Render Queue

! 해당 내용은 영상을 볼 것(영상으로 이해하자) --> 2. [In The Loop](https://www.youtube.com/watch?v=cCOL7MC4Pl0&list=WL&index=4&t=6s)
| Jake Archibald | JSConf.Asia

JS를 이용하는 웹은 사람이 보기위한 화면이 존재한다.

사람이 단절된 이미지의 연속에서 자연스러운 영상이라고 인지하는 주사율은 평균적으로 60Hz다.

즉 자연스러운 화면을 보여주기 위해 초당 60번 Update해야된다는 의미다.

Render Queue에 초당 60번의 작업이 push 된다는 의미다.

에니메이션을 업데이트하기 위해 DOM을 직접 제어하는 명령을 통해 업데이트를 하면 이벤트가 초당 60번 이상 발생할 수 있다. 

예를 들면 반짝이는 모션을 보여주기 위해서 

```JS
while(++tick){
  show();
  hide();
}
```
와 같이 코딩할 수 있다.

Render Queue는 callback queue보다 우선순위는 높지만, 즉각 실행되지 않는다.

그래서 너무 빨리 show(), hide()가 반복되면 렌더링이 업데이트 될 때 

show(),hide(),show() --> hide()안됨와 같은 일이 발생할 수 있다.

사람은 보지도 못하고, 제대로 구현도 안되고 리소스만 낭비되는 짓을 하는 것이다.

별도로 [requestanimationframe](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)
을 쓰는 방법도 있다

-------------------

추가로 나중에 정리할 것 (코드)

1. setTimeout( ... , ... ) return value : microTask number

2. 
```JS

// async func
async function call(name){
  await console.log("async...", name);
}

// microtask with 0.000 sec --> to check run after blocking code
setTimeout( () => {
  console.log('time out... 0 mcs');
}, 0); // 

// microtask with 0.001 sec --> to check run after blocking code
setTimeout( () => {
  console.log('time out... 1 mcs');
}, 1); // 

let i = 0;

// microTask?
(async () => {
  setTimeout(() => {
    console.log('time out in lambda async');
  }, 1)
  }
)();

call("before");

// blocking function 
while(i++ <1000) {
  console.log("sync");
}

call("after");
console.log("================================");
```

#### 실행 결과

```txt
async... before
sync (repeats 1000 times) 
async... after
================================
undefined // --> end of code (no return statements)
time out... 0 mcs
time out... 1 mcs
time out in lambda async
```

### 정리

  1. call stack에 쌓인것이 모두 끝난뒤 call back queue에 담긴 setTimeout의 micro task가 끝난 것을 볼수 있다 ("time out...")
  2. 하지만, async before이 "sync" 이전에 나온다... async function <-> async lambda function <-> microtask(setTimeout) 다른가?
  3. async function을 즉시실행으로 바꾼뒤 실행했을 경우에도 위와 동일했다... **왜?**

* 참고
async function call(name){
  await setTimeout(console.log("async...", name), 12);
}
이렇게 바꿔도 동일한 결과가 나왔다.

* 3번에서...

```JS
(async function call(name){
  await setTimeout(console.log("async...", name), 0);
})("before");

// blocking function
while(i++ <1000) {
  console.log("sync");
}

(async function call(name){
  await setTimeout(console.log("async...", name), 0);
})("after");
```

이렇게 바꿔도 동일한 결과가 나왔다... 어떻게 돌아간걸까
