# Observable
`observables`은 데이터 컬렉션을 갖고 구독하고 있는 대상에게 lazy Push를 하는 역활을 갖고 있다.

여기서의 `Lazy Push`라 함은 객체의 생성이나 계산결과에 따라 그 즉시 값을 리턴하는것이 아닌 지연시간을 갖는 것을 말한다.

이해를 위해 Push와 Pull의 개념을 정리 하고 넘어가야 한다

### Pull vs Push
데이터의 생산자와 소비자 관점에서 바라봤을때 일반적으로(서버-클라 관계 상황) 생산자(서버)는 요청을 받았을 때 데이터를 생산, 소비자(클라)는 데이터가 필요할 때 요청하는 흐름으로 진행된다. 

이러한 방식이 `Pull`이라고 하며 `Push`의 경우 소비자(클라)는 데이터를 받을 준비(subscribe) 상태이고, 생산자(서버)는 데이터를 생산자가 원하는 시점에 생산하여 소비자에게 보낸다.(push)

Push 방식이라는 것을 정리하자면
> 생산자는 데이터를 언제 보낼지 소비자에게 보낼지 결정권을 갖고, 소비자는 데이터가 언제 도착할지 알지 못한 상태인 것.

> 즉 `observables`는 Push 방식으로 동작하며, `observables`이 특정 시점에 데이터를 Push 할 때 구독(subscribe)하고 있는 소비자는 데이터를 받게 된다.

RxJS는 자바스크립트 푸시 방식의 새로운 구현 방식을 제공하며, 이전 
## Push 방식 구현 방법에서의 Promise와 Observables
Rx를 사용하지 않은 이전 Push 방식은 Promise를 통해 구현 할 수 있었다.

promise를 이용한 방식은 한번 push( resolve 또는 reject 메소드 수행 ) 하면 더 이상 push를 하지 못한다는 단점이 있다. ( 단발성 )

하지만 RxJS의 Observables를 이용할 경우 여러번 push할 수 있다.

- promise를 이용한 push 방식

    ``` javascript
    const producer = new Promise( (resolve => {
        //생산자가 원하는 시점에 데이터를 push 한다.
        setTimeout( resolve(true), 1000)
    }));

    producer.then( r => { 
        //소비자는 생산자가 push해야 데이터를 받는다.
        concumer(r) 
    });
    ```
    
- Obervables를 이용한 push 방식

    ``` javascript
    const observable = new Observable(subscriber => {
        subscriber.next(1);
        setTimeout(() => {
            subscriber.next(2);
        }, 1000);
    });

    observable.subscribe({
        next(x) {
            //observable에서 next가 호출 될 때마다
            console.log('got value ' + x); 
        }
    });
    ```

> Observables는 생성된 시점에서 부터 계속해서 동기 또는 비동기 적으로 데이터를 반환 할 수 있다.

## 일반적인 함수로서의 Observables
일반적인 함수를 생성하고 호출하는 행동과 Obervable을 생성하고 구독하는 행동은 유사하다고 볼 수 있다.

### 호출과 구독

``` javascript
const fn = () => { console.log('t'); return 't2'};
const obs = new Observable( subsc => { 
            console.log('t');
            subsc.next('t2'); 
        });


console.log(fn());

obs.subscribe(x => {
  console.log(x);
});
//fn() 과 obs.subscribe의 수행 결과는 같다.
```

> Observable을 구독(subscribing)한다는 것은 함수를 호출한 것과 유사하다.

### 동기적으로 반한

``` javascript
const obs = new Observable( subsc => { subsc.next('2');subsc.next('3'); });

console.log('1');
obs.subscribe(x => {
  console.log(x);
});
console.log('4');
//1,2,3,4 출력, 순차적으로 수행됨, 즉 동기적으로 수행되었다.
```

> Observable도 완전히 동기적으로 동작할 수 있다.

### 함수와의 차이점
함수는 하나의 `return`만 가능하다. 반면에 `Obsrvable`은 여러 값을 반환할 수 있다.

> 즉 함수의 호출은 하나의 값을 동기적으로 받는다. Observable의 구독은 여러값을 동기적이던, 비동기적이든 받을 준비가 되어있다.

## Obserable의 4가지 관점
Observable는 `new Obseravble` 또는 creation opertor로 생성할 수 있다. 이렇게 생성된것은 Observer에 구독되어진다.

Observer에게 알림을 전달하기 위해 `next/error/complete` 가 수행된다. 그리고 이런 수행결과로 Observable은 폐기될 수 도 있다.

Core Observable의 관점 4가지 (Creating Observables, Subscribing to Observables, Execution the Observable, Disposing Observables)

### 1. Creating Observables
`Observable`의 생성자는 `subscribe` 함수 하나를 인자로 받는다.

아래 예제에서는 1초마다 hi라는 문자열을 subscriber에게 보내는 Observable을 생성한 예이다.
``` javascript
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  const id = setInterval(() => {
    subscriber.next('hi')
  }, 1000);
});
```
> Observable 은 `new Observable`을 통해서 생성할 수 있지만, 일반적으로는 of,from 과 같은 creating function을 이용하여 생성한다.

위 예제에서 중요한 subscribe 함수에 대해서 2번에서 살펴본다.

### 2. Subscribing to Observables
Observable은 아래 예제와 같은 형식으로 구독되어질 수 있다.
``` javascript
observable.subscribe(x => console.log(x));
```
observable을 구독한 구독자들은 각각 독립적으로 발생한 이벤트, 데이터가 주어진다.

예를 들면 하나의 Observable을 3개의 subscriber가 구독하면 3개의 subscriber모두 같은 데이터를 받으며, 각 구독자들은 각자의 컨텍스트를 갖는다. (데이터를 공유하지 않는다라는 뜻)

> Observable을 Subscribing 한다는것은 콜백함수를 리턴하는 함수를 호출한것과 비슷하다.

### 3. Executing the Observable
`new Observable(function subscribe(subscriber) {...})` 이 코드는 Observable을 실행한다라는 표현식이다. 

다만 Observable을 생성한다고 하여 바로 수행되는것이 아닌 `lazy computation`으로 구독 되어질때 수행된다.

Observable은 3가지 타입으로 전달할 수 있다.

1. `Next notification`: 값을 전달한다. 값의 형태는 문자열, 숫자, 등 어떤것도 올 수 있다.
2. `Error notification`: Javascript Error 또는 exception을 보낸다.
3. `Complete notification`: 값을 보내지 않는다.


Next nofitication은 구독자들에게 무한대로 값을 전달 할 수 있으며, Error, Complete는 구독하는 중에 한번만 받거나, 둘 중 하나만 받는다.

만약 Error, Complete을 받으면 이 후 Next를 받을 수 없다.

``` javascript
subscriber.complete();
subscriber.next(4); // Is not delivered because it would violate the contract
```

### 4. Disposing Observables
Observable은 계속 실행 될 수 있기 때문에 (구독자들에게 계속 값을 push할 수 있다.) 컴퓨팅 자원 낭비를 피하기 위해 적절하게 Observable의 실행을 중단 시켜야할 필요가 있다.

createOperator로 Obervable을 생성하였다면, 대부분의 경우 `unsubscribe()`를 호출하여 실행을 중단 시킬수 있다.

만약 커스텀 Observable을 사용한다면, Observable의 생성자의 리턴값으로 실행을 중단 시킬수 있는 `unsubscribe()` 을 반환하여야한다.

``` javascript
const observable = new Observable(function subscribe(subscriber) {
  // Keep track of the interval resource
  const intervalId = setInterval(() => {
    subscriber.next('hi');
  }, 1000);

  // Provide a way of canceling and disposing the interval resource
  return function unsubscribe() {
    clearInterval(intervalId);
  };
});

const unsub = observable.subscribe(x => {
  console.log(x);
});

//2초 뒤에 구독 해지
setTimeout(() => unsub.unsubscribe(),2000);
```


## 결국 RxJS에서의 Observables라는 것은 ..

> 자바스크립트에서 푸시 방식을 개발하기 위한 새로운 방법으로, Observable은 여러값의 생산자이며, Observers(Observable을 구독하는 소비자)에게 그 데이터를 '푸시'하는 것이다.

> (원문) RxJS introduces Observables, a new Push system for JavaScript. An Observable is a Producer of multiple values, "pushing" them to Observers (Consumers).



