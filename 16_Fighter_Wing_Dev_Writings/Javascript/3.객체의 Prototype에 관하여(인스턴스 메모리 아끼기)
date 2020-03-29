2020/03/27 (금)

개발환경 : Chrome 개발자모드 - 콘솔
책 : 모던 자바스크립트 입문 - 이소 히로시

책 351pg ~ 객체의 프로토타입 수정 및 교체하기



객체를 만들 때 다음과 같이 한다.

var Circle = function(center, radius) {
                    this.center = center;
                    this.radius = radius;
                    this.getArea = function() { return Math.PI *
this.radius * this.radius; };
}

너무나도 당연하기 때문에 뭐가 문제인지를 처음에 자각하기 어렵다.
이때 이 함수(생성자)도 하나의 객체라는 점을 인지하면 차근차근 알아갈 수 있다.

Circle 객체 내 this.center는 하나의 변수다.
각 인스턴스가 각자 갖고 있는 데이터다.
그래서 다른 인스턴스와 혼용되지 않도록 각자 값을 복사해야한다.
this.center가 인스턴스의 데이터이듯이 this.getArea 또한 함수이자 데이터다.

★ this.getArea라는 함수는 다른 인스턴스가 어떤 값을 가지던 간에 똑같이 쓰여도 문제가 없는 함수임에도 불구하고
Circle 인스턴스를 만들 때 마다 똑같이 복사를 한다는 것이다!

var Circle = function(center, radius) {
                    this.center = center;
                    this.radius = radius;
}

Circle.prototype.getArea =  function() { return Math.PI * this.radius
* this.radius; };

객체 모두 동일하게 사용하는 매소드의 경우 복사할 필요가 없다.
그래서 위와 같이 객체의 핵심부(core)에 해당하는 __prototype__ (이것 또한 객체)에 함수 프로퍼티를 추가하면 된다.
모든 인스턴스는 동일한(바꾸지 않는 이상) prototype를 참조하고 있으므로 함수를 복사할 필요없이 똑같은 함수를 사용할 수 있게된다!
