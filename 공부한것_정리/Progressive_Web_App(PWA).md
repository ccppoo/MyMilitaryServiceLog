1. [Predicting the Future of the Web Development (2020 and 2025)](https://www.youtube.com/watch?v=24tQRwIRP_w)

2. [The modern PWA Cheat Sheet by Maximiliano Firtman | JSConf EU 2019](https://www.youtube.com/watch?v=cybhV88KLfI&t=1256s)

### 서문

2025년 까지의 웹 프로그래밍의 미래를 예상하는 영상과 JSConf에서 발표된 PWA 영상을 보고 작성하게된 글이다.

[Predicting the Future of the Web Development (2020 and 2025)](https://www.youtube.com/watch?v=24tQRwIRP_w)
는 웹 생태계를 지탱하고 있는 JavaScript과 확장된 언어들(TypeScript)의 생태계 지속성

Web Assembly, Package Installers(NPM, ...)의 성장과 보안위협, 그리고 JS의 미래를 다룬다.

프로그램의 언어 흥망성쇄를 직접 본 경우는 겨우 Python2.x 버전말고는 없고, 직접적으로 영향을 받아본 적이 없어

많이 실감하지 못했고, JavaScript도 공부만했지 직접 사용한 경험은 극히 드물어서 영상의 내용을 보고 정리하지 않았다.

아무튼 웹 기술의 발전 흐름에 대한 내용이고, PWA 개발도 Web Assembly 앱처럼 보편화 될 것으로 생각하기 때문에

간단히 언급만 하고, PWA에 대해 간단히 정리하고자 이 글을 쓴다.


## Progressive Web App (PWA)

간단히 말하면 웹 페이지를 앱으로 만든것이다.

웹 엔진을 기반으로 한 앱이라고 할 수 있다.

보여주고, 상호작용할 수 있는 웹의 특성을 그대로 이용해서 Native App화(化)한거라고 간단히 말할 수 있다.

[Android Developer](https://developer.android.com/) ,[The Hacker News](https://thehackernews.com/)를 들어가면

PC/Mobile 상관없이 설치 후 앱처럼 사용할 수 있다.

JVM위에 자바 앱을 만들고 플렛폼 상관없이 컴파일 할 수 있듯이

웹이라는 잘 만들어진 플렛폼 위에 설치형 앱을 베포하는 것과 같다.

## PWA vs AMP(Accelerated Mobile Pages)

두 기술 모두 웹 서비스의 사용자 경험을 높이기 위해 만들어졌다.

빠른면 빠를수록 무조건 좋기 때문이다.

AMP는 앱을 네트워크에 좀더 초점을 맞춘 것으로 보인다.

AMP의 발표 영상들은 안좋은 네트워크 인프라를 갖춘 환경과 Low-end 모바일 기기를 위해

적은 용량으로 웹 앱의 서비스를 전달하는 것에 초점을 둔다.

AMP의 초점은 '똑같은 html, js, css 파일을 계속 다운 받지말고 미리 저장한 뒤에 빠르게 로딩해버리자'에 맞춰져있다.

...

PWA와 AMP는 제한된 파이를 두고 경쟁하는 기술이 아닌

앱처럼 만들고 싶은 웹, 빠르게 보여주는 웹의 필요함을 각자 충족시킨 기술이다.

AMP의 빠르게 로딩되는 기술과 웹을 앱(Native App)처럼 사용할 수 있는 PWA가 합쳐지는 것이 궁극적인 목표다.

여기서 가끔 사용하는 앱인데 용량은 크고(>10MB) 네트워크 환경이 안좋은 사용자를 위해 (빠르고 쾌적한 서비스를 위해)

사용되는것이 Web Assembly다.

[PWA vs. AMP: Which is Better & How are They Different from Each Other?
](https://instapage.com/blog/pwa-vs-amp)

[PWA vs AMP : Choosing the best for me?](https://codeburst.io/pwa-vs-amp-choosing-the-best-for-me-91c8c48ff152)

같은 내용의 글이지만, 둘다 동시에 읽기 좋다.
