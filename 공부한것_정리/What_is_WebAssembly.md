# 웹 어셈블리가 대중화되면 브라우저의 가상환경과 로컬머신의 철저한 분리는 보안에 있어 최우선 과제가 될 것이다.

.


## 웹 어셈블리(WebAssembly)는 무엇일까?

아래의 두 영상의 설명을 토대로 정리했다.

[Google Chrome Developers - WebAssembly For Web Developers (Google I/O '19)](https://www.youtube.com/watch?v=njt-Qzw0mVY&list=LLle727HJuGdiXQ_x5CtXlHw&index=6&t=0s)

[JAZOON TECH DAYS - BERN SPRING 2017](https://www.youtube.com/watch?v=cRwUD5SxF4o)

### 웹 어셈블리는 어셈블리어(Assembly)인가요?

아니다, 기계와 직접 대화할 수 있는 그 언어가 아니다.

하나의 추상적인 개념이라고 보는 것이 좋다.

웹 어셈블리는 C,C++,Rust, 등 언어를 더 저수준의 언어로 [트랜스컴파일링](https://en.wikipedia.org/wiki/Source-to-source_compiler)\(TransCompile\)된 소스코드다.
<br>*언어-대-언어 컴파일링(Source-to-Source compile)라고 부르기도 한다.

즉, 사람이 쉽게 알아볼 수 있는 추상적 프로그래밍 언어와 기계가 잘 알아들을 수 있는 어셈블리언어의 **중간** 단계다.


### 웹 어셈블리는 왜 만들어졌나요?

웹 어셈블리의 탄생 배경을 알기 위해선 [Emscripten](https://en.wikipedia.org/wiki/Emscripten)이 왜 생겼으며, 어떤 기능을 가졌는지 알아야한다.

Emscripten(asm.js)은 브라우저 상의 가상환경에서 작동하기 위해서 Native 환경에서 작동하는 C, C++ 같은 언어를 자바스크립트로 트랜스컴파일링해준다.

이렇게 함으로써 얻는 장점은 같은 기능 수행하고 순수 자바스크립트로 작성된 코드에 비해서 성능이 뛰어나다는 것이다.

당연히 자바스크립트보다 저수준의 언어를 통해 직접 제어할 수 있기 때문에 성능상에서의 장점이 뛰어나다.

하지만, 결국에는 자바스크립트를 극한의 수준으로 쓰는 것이기 때문에 Native한 C, C++ 코드가 낼 수 있는 성능의 최대 1/1.5 배로
낮추는 것에 끝났다.
<br>아무리 잘해도 최소 실행속도 1.5배~ 늘어난다는 의미


정리하면 다음과 같다.

Case A : JS Script --> Run on Browser

Case B : C/C++ 코드 --> [Emscripten - Transcompile] --> JS Script --> Run on Browser

유용한 C/C++라이브러리를 웹 환경에서 쓰기위해서 자바스크립트로 작성하지 않고 트랜스컴파일링 한다는 것이다.

-------------

C/C++ 코드를 어떻게든 쓰고 싶어서 이를 다시 자바스크립트로 컴파일링한다는 건 

**아랫돌 빼서 윗돌 괴고 윗돌 빼서 아랫돌 괴기**와 같은 상황이었다.

그래서 아예 어차피 웹상에서 [자바스크립트도 결국 C/C++ 컴파일 된 후 사용되니깐](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)), 애초에 C/C++ 코드를 JS로 컴파일하지말고 

미리 컴파일해서 JS 파일처럼 자바스크립트랑 같이 쓰자!

이렇게 C/C++의 Native한 코드를 바이너리 코드 수준으로 컴파일한게 웹 어셈블리다.

현재 대부분의 웹 브라우저들은 [구글의 V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine))를 사용하기 때문에 절대 사장될 기술은 아니다.

Emscripten도 웹 어셈블리로 컴파일 해준다.

...

### 그럼 자바스크립트 언어 망함?

ㄴㄴ

웹 어셈블리는 JS로 구동하기 버거운 앱들을 바이너리 코드로 돌리는 용도로 쓴다

[유니티 영상](https://www.youtube.com/watch?v=eh-yy7f1bvQ)에서 볼 수 있듯이 [three.js](https://threejs.org/)에서 구현하기 어려운것을 구현해낸다.

일종의 보완재다.

앞으로 게임을 굳이 실행파일로 다운 받지 않고 웹상으로 구동할 수 있다는 것이다.

C, C++, Rust 개발자들은 거들떠도 보지 않은 웹앱을 만들 수 있다.

### 덧, 개인적인 의견

브라우저가 처음 나왔을 때 외부환경(네트워크)과 로컬 머신(컴퓨터)를 중계해주는, 즉 가상환경을 통해 컨텐츠를 제공했다.

웹에서 나쁜짓을 당해도 가상환경에서 이루어짐으로 브라우저를 끄면 제거되므로 안전했다.

윈도우는 컴퓨터의 성능을 최대한 쓰고자 가상환경이 아닌, 컴퓨터의 리소스를 직접 이용하는 액티브X([Active X](https://en.wikipedia.org/wiki/ActiveX))를 제공했다.

로컬 머신과 리소스에 직접 접근할 수 있었기에 보안상의 이유로 현재 많이 없어졌다.

그 동안 JS와 웹 브라우저(특히 크롬)의 비상 덕분에 성능상의 향상은 계속 이루어졌다(Free Lunch는 아니다).

인간의 욕심은 끝이 없다시피 컴퓨터가 날로 좋아지는데 그 성능을 최대한 사용하고 싶었다.

그래서 이제는 가상환경에서 바이너리 코드를 실행시키는 일까지 일어났다 (WebAssembly)

...

브라우저가 **가상환경과 로컬머신의 분리**(보안)을 철저히 하지 않으면

몰래 보내는 것도 아닌, 사용자들이 눈뜨고 코베이듯이 바이러스를 다운 받는것과 같아 보인다.

자바스크립트는 읽을 수라도 있지, 웹어셈블리는 읽기 힘들어서 보안 관계자들의 일거리가 늘어날 것으로 보인다.





