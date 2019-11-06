# Modern JavaScript Syntax
ES5 이 후 추가된 문법 정리

## 목차
- [변수](#변수)
- [iterator](#iterator)
- [generator](#generator)
- [화살표 함수 표현식](#화살표-함수-표현식)
- [매개변수](#매개변수)
- [map](#map)
- [참고링크](#참고링크)

## 변수

``` javascript
var a = 1;//오래된 변수 선언 키워드
let b = 1;//모던한 변수 선언 키워드
const c = 1;// 상수 선언, 값을 변경할 수 없다.
```

### var VS let
`let`키워드는 es5이후 추가된 변수 선언 방식으로 `let`으로 선언된 변수는 선언된 블록에서만 유효성을 갖습니다.

``` javascript
if(true) {
    let meg = 'hi'; //블록 내에서만 유효하다.
    console.log(meg); //hi
}

console.log(meg);//ReferenceError: meg is not defined
```

`var`키워드는 함수내에서 선언되었다면 함수 내에서만 유효하고, 그 외에는 전역에서 변수로 동작한다.

함수내에서 `var`선언: 함수내에서만 유효
``` javascript
function say() {
    var meg = 'hi';
    console.log(meg); // hi
}

console.log(meg); //ReferenceError: meg is not defined
```

블럭 내에서 `var`선언: 전역 변수로 동작
``` javascript
if(true) {
    var meg = 'hi';
    console.log(meg); //hi
}

console.log(meg); //hi
```

호이스팅으로 인한 `var`선언: 함수 내에서 var로 선언한 변수는 함수 최상단 옮겨짐
``` javascript
function say() {
    meg = 'hi'; //호이스팅으로 인해 선언 위치에 상관없이 사용
    console.log(meg); // hi
    var meg = 'hello'; //호이스팅은 변수 선언에만 영향이 있고 변수 할당엔 영향 없음
}
say();
```

`var`의 이러한 특성 때문에 `let`을 이용하여 변수를 선언하는것이 예상치 못한 버그를 방지하는데 좋음

## iterator
객체의 반복을 핸들링하는 방법으로 [Iterator protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol)을 구현한 객체에서 사용 가능하다.

Iterator 는 **next()** 라는 메소드명으로 *{done: 끝여부, value: 다음값 }* 형식의 값을 반환하는 메소드를 갖고 있으며 next()를 반복적으로 호출하여 객체의 반복을 핸들링 할 수 있다.

javaScript에서는 Array, String, Map, Set, 등과 같은 객체가 기본적으로 iterable를 갖고 있다.

``` javascript
const it = [1,2,3].values();

while (true) {
    const next = it.next();
    if(next.done) break;
    console.log(`value => ${next.value}`);//1,2,3 순차 출력
}
```

### iterator와 iterable
*iterator*: **next()** 라는 메소드명으로 *{done: 끝여부, value: 다음값 }* 형식의 값을 반환하는 메소드를 갖고 있는 객체

*iterable*: 객체의 프로퍼티를 반복할 수 있는 Symbol.iterator 심볼로 *iterator*를 프로퍼티로 갖고 있는 객체

## generator

### 정의
함수는 한번 실행되면 함수의 맨 위부터 끝까지 모두 실행되고 콜스택을 빠져나간다.

이와 다르게 generator는 __실행 -> 보류 -> 실행__ 처럼 함수 내 실행을 제어 가능하다.

*generator*는 **function* 이름() {..}** 형식으로 선언하며 내부에 yield 키워드의 프로퍼티를 갖는다.

반환되는 *generator* 객체는 *iterable*으로 .next()를 통해 yield에 등록된 내용을 순차적으로 실행 가능하다.

내부적으로 *generator* 객체를 실행하였을때 yield 키워드를 만나면 실행 컨텍스트를 복사 후 콜스택에서 빠져나간다. 이후 next()메소드를 콜하였을때 복사해둔 실행 컨텍스트를 다시 콜스택에 넣어 실행을 이어간다.

### 생성 방법
*generator* 객체의 반환값은 *iterator* 이다.
``` javascript
function* generatorFunc() {
    yield 1;
}
const a = generatorFunc();

console.log(a === a[Symbol.iterator]());//true
```

### 사용 방법 
*generator* 객체의 반환값은 *iterator* 임으로 next() 메소드의 반환값으로 yield에 선언한 순서대로 값을 반환한다.
``` javascript
function* generator() {
    yield 'one';
    yield 'two';
    yield 'three.'
}
const gIt = generator();
console.log(gIt.next()); //{ value: 'one', done: false }
console.log(gIt.next()); //{ value: 'two', done: false }
console.log(gIt.next()); //{ value: 'three', done: false }
console.log(gIt.next()); //{ value: undefined, done: true }
```
더 많은 사용 방법은 [여기](https://wonism.github.io/javascript-generator/) 참고

## 화살표 함수 표현식
현재 수행중인 컨텍스트에서 짧은 코드를 위해 사용하기 위해 사용하는 함수 표현식으로 일반 함수와 다른 특성을 갖고 있다.

자세한 내용은 [화살표 함수 정리](https://github.com/parkjungwoong/frontend-basic-study/blob/master/javascript/basic/%ED%95%A8%EC%88%98.md#%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98)참고

## 매개변수


## 참고링크
- [generator정의 및 사용 예시](https://wonism.github.io/javascript-generator/)