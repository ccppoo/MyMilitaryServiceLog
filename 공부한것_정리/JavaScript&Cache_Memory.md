[Memory in JavaScript - Beyond Leaks](https://medium.com/walkme-engineering/memory-in-javascript-beyond-leaks-8c1d697c655c)
읽고 정리한

검색어 : 자바스크립트 튜닝, 성능향상, 성능 튜닝, Data Locality, Data Oriented Design, **최종병기**

# 너무나 빠른 CPU

현대 컴퓨터는 [폰 노이만 구조](https://ko.wikipedia.org/wiki/%ED%8F%B0_%EB%85%B8%EC%9D%B4%EB%A7%8C_%EA%B5%AC%EC%A1%B0)를
따르기 때문에 메모리에서 부터 실행파일을 가져와야 한다.

그러나 현대의 CPU 속도가 너무 빨라 주/보조 기억장치에서 가져오는 메모리를 순식간에 처리하기 때문에

처리해야 하는 일거리를 가져오는 속도보다 일 처리 속도가 앞지르는 현상이 일어난다.

즉, **CPU는 메모리가 일거리를 가져오는 시간동안 놀고 있는 일이 벌어진다**

이 현상은 CPU의 성능을 간접적으로 저하시키는 것과 같다(일 안하고 대기만 하는 중...)

이를 보완하기 위해서 캐시 메모리가 존재한다.

------

# 캐시 메모리(Cache Memory)

[캐시 메모리](https://namu.wiki/w/%EC%BA%90%EC%8B%9C%20%EB%A9%94%EB%AA%A8%EB%A6%AC)는
CPU에 비해 상대적으로 느린 메모리(기억장치)에  

<ins>프로그램상에서 현재 요구되는 메모리</ins> + <ins>나중에 쓰일 것으로 예상되는 범위의 메모리</ins> 를 미리 꺼내놓은 것이다.
<br>*캐시는 종류 상관없이 CPU부터 SSD/HDD까지 거의 모든 메모리에서 빠른 접근성을 위해서 쓰인다*

나중에 사용이 될거라고 추정되는 기준 3가지가 있다.

1) for, while, do-while과 같은 Loop에서 반복적으로 쓰이는 조건변수 [최근에 사용 - 시간지역성 (Temporal Locality)]

2) 100개의 정수 배열에서 34번 원소에 접근시 주위에 해당되는 배열원소 **[주변 메모리 - 공간 지역성(Spatial Locality)]**
<br>*(예:20...33번 + 35...60번)*

3) 100줄짜리 코드의 경우 if와 같은 분기가 발생이 없으면 미리 100번째 줄의 코드까지 [순차적 지역성(Sequential Locality)]

### 캐시 메모리 정리

* 뭔가 나중에 필요할거 같은 메모리를 미리 꺼내오는 것

* 캐시 메모리에 다음 코드(명령)에...

  1) 필요한 메모리가 있다 -> 캐시 적중(실행 속도 ↑, 시간 소요 ↓)

  2) 필요한 메모리가 없다 -> 캐시 미스(miss) (실행 속도 ↓, 시간 소요 ↑)

-------

### 다시 주제로 돌아가서...
## 그래서 캐시 메모리와 자바스크립트가 어떤 관련이 있냐?

나도 처음에는 자바스크립트와 같은 [고급언어(High Level)](https://ko.wikipedia.org/wiki/%EA%B3%A0%EA%B8%89_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)
가
기계어, 어셈블리와 같은 [저급언어(Low Level)](https://ko.wikipedia.org/wiki/%EC%A0%80%EA%B8%89_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)
언어처럼 '심오'하게 하드웨어와 친분이 닿을 줄을 몰랐다.

하지만 앞서 언급한 2번재 캐시 메모리의 선정 방법(공간 지역성, Spatial Locality)을 완벽히 파괴하고

고의로 느리게 하는 코드를 예시와 함께 알아보자

```JS
class data {
  constructor(id){
    this.num;
  }
  x;  y;
}

let row = 1000;
let col = 1000;
let arr = new Array(row * col).fill(0).map((cb, i) => new data(i));
```

1000 * 1000 배열에 data 클래스를 할당하고

```JS
function localAccess(array){
for(let i = 0 ; i < row; i++){
for(let j = 0; j < col; j++){
array[i * row + j].x = 0;
        }
    }
}

function nonLocalAccess(array){
for(let i = 0; i <row; i++){
for(let j = 0; j < col; j ++){
array[j * row + i].x = 0;
        }
    }
}
```
두가지 경우로 Data Array에 접근하자.

**localAccess**는 배열을 0,1,2, ... ,999, 1000, 1001,... 와 같이 순차적으로 접근한다.

**nonLocalAccess**는 0, 1000, 2000, ... 와 같이 1000개씩 건너 뛰며 접근한다.

```JS
function repeat(array, access, type) {
  console.log(`%c Started data ${type}`, 'color: red');
  const start = performance.now();
  for (let i = 0; i < 100; i++) {
    access(array);
  }
  const end = performance.now();
  console.log('Finished data locality test run in ', ((end- start) / 1000).toFixed(4), ' seconds');
  return end- start;
}
repeat(arr,localAccess, 'Local');
repeat(arr, nonLocalAccess, 'non Local');

// Started data Local
// Finished data locality test run in  0.7517  seconds
// Started data non Local
// Finished data locality test run in  4.6501  seconds
```

위와 같이 시간을 측정하는 코드를 작성하면 7~8배의 속도차이가 나는 것을 알수 있다.

만약 위와 같이 1000, 2000, ... 9000 같이 비연속적인 데이터를 접근해야되는 경우

**연속적으로 접근할 수 있게끔** 새로운 배열을 만들어서 사용하자

```JS
let s = performance.now();
let diffArr = new Array(row * col).fill(0);
for (let i = 0; i < col; i++) {
  for (let j = 0; j < row; j++) {
    diffArr[i * row + j] = arr[j * col + i];
  }
}
repeat(diffArr, localAccess, 'diffArr Local');
let e = performance.now();
console.log('Copy and repeat costed : ', ((e- s) / 1000).toFixed(4), ' seconds');

// Copy and repeat costed : 1.3460  seconds
```

## 결론

인접하지 않은 값을 읽고 쓰는 경우 새로운 배열을 만들어서 '인접'하게끔 만들자.

JS를 쓰면서 대량의 배열을 쓸 일은 흔하지 않지만, 다른 프로그래밍 언어 또한 똑같으므로

'인접한 데이터 쓰기' 튜닝을 한번쯤은 고려해보도록 하자.

'인접한'데이터란 함수, local, global 데이터 접근과도 상관있는 것이다.

참고할 사이트 : https://gameprogrammingpatterns.com/data-locality.html

참고할 개념 :  Data-Oriented Design(데이터 지향 디자인, AST - Abstract Syntax Tree)


