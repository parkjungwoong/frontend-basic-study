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
var a = 1;//
let b = 1;//
const c = 1;//
```

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
간결한 함수선언식, 그리고 어떤 기타 이유에 의해서 쓰는데 찾아봐야함

함수 선언식과 화살표 함수 표현식 비교
``` javascript
function fn(a,b) { ... } //함수 선언식
(a,b) => {} //화살표 함수 표현식
```

매개변수 표현 방법
``` javascript
(매개변수1,매개변수2,..) => { 함수 내용 }//괄호 안에 매개변수명 작성
const sum = (a, b) => { return a+b }

매개변수 => { 함수 내용 }// 매개변수가 하나일 경우 괄호 생략 가능
const sayHi = name => { return `hi ${name}`};

() => { } //매개변수가 없을 경우 괄호 생략 불가
const noParam = () => { console.log('no param')};
```

함수 내용 표현 방법
``` javascript
() => { 함수 내용 }// 중괄호 안에 함수 내용 작성
const noParam = () => { console.log('no param')};

() => 함수 내용 //중괄호 생략시 { return 내용 } 과 같이 동작함
const sum = (a, b) => a+b // { return a+b } 와 같음
```


## 매개변수


## 참고링크
- [generator정의 및 사용 예시](https://wonism.github.io/javascript-generator/)