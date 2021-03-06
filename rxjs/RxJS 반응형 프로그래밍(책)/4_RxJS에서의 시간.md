# 4.RxJS에서의 시간
4장에서는 비동기 코드에서 시간의 개념으로 인한 문제점을 해결하기 위해서 사용하는 연산자들을 소개한다.

비동기 코드에서 각 동작간에 지연시간을 추측하여 코드에 명시적으로 작성하는 방법 보다는 이전 작업의 결과에 따라 다음 작업이 반응적으로 동작 하도록 한느 방식으로 권장하고 있다.

## 4.1 왜 시간을 신경 써야 할까
- 어플리케이션 사용자는 시간에 굉장히 민감하게 반응한다는 점을 상기시킴. 사용자 상호 작용 후 1초 이내에 다음 동작이 수행되지 않으면 사용자의 이탈률이 높아질 수 밖에 없다.

- 시간이라는 외부 변수가 계속적으로 변화하고 있으므로 시간 기반함수는 외부 상태에 직간접적으로 의존할수 밖에 없는 구조이다.

## 4.2 자바스크립트의 비동기 타이미 이해하기
비동기 코드의 실행 시간은 통제할 수 없는 영역(네트워크, 사용자, 시스템 등)을 포함하기 때문에 수행 시간을 정확하게 예측할 수 없다.

명확하게 알 수 없는 시간에 대해 개발자가 대응할 수 있는 자바스크립트에서 지원하는 함수와 이와 비슷한 RxJS에서 지원하는 연산자를 소개한다.

- 비동기 이벤트의 두 가지 주요 문제
    1. 미래에 발생하지 않을수도, 발생 할 수도 있다는 점에서 모호하다.
    2. 이전 작업의 결과에 따라 실행에 결과가 좌우된다는 조건부적인 성향이 있다.
    
RxJS에서는 시간에 대한 추상화 계층을 만들어서 비동기 작업을 마치 동기 작업처럼 코드로 작성 할 수 있는 방식을 지원한다.

이는 옵저븝ㄹ의 오케스트레이션 계층을 통해 암시적, 명시적으로 시간을 처리 할 수 있다.

### 암시적 타이밍
이 책에서는 암시적 타이밍을 작업이 끝난 경우 결과값과 이 결과값을 갖고 수행할 다음 동작을 콜백함수로 수행하는 형식을 설명한다.

즉 작업을 순차적으로 실행한다. 이 작업은 이전 작업이 끝난 후 다음 작업이 수행

코드에 명시적으로 이작업 다음에 이 함수를 수행하라는 식으로 쓴다는 말인데 왜 암시적이라는 단어를 사용했을까 했는데

구체적으로 1초,2초 소요된다는 방식으로 시간에 대한 명시적 선언이 없기 때문에 `암시적`이라는 표현을 사용한거 같다.

### 명시적 타이밍
코드를 실행할 때 명시적으로 시간에 대한 값을 지정하여 실행을 제어 할 수 있다.

명시적으로 시간을 지정할 경우 3가지 특성을 갖는다.
1. 구체적(concrete): 정해진 시간에 이벤트가 발생한다.
2. 명시적(explicit): 이벤트 발생 시간을 개발자가 명시적으로 지정한다. 그리고 그 시점은 실행 할 때 발생한다.(예를 들면 클릭 후 1초 후 특정 이벤트 발생)
3. 무조건적(unconditional): 에러가 발생하지 않거나 스트림이 취소되지 않는 한 항상 이벤트를 발생한다.

명시적 타이밍을 지정해야될 경우는 2가지 경우가 있다.
1. 사용자 중심: 인간의 눈으로 보여지는 부분, 클릭 후 동작이 완료되었을때 2초후 로딩 화면이 없어져야 한다거나..
2. 리소스 중심: 여러번 조회 버튼을 클릭 하더라도, 처음 클릭 한번만 수행되도록 해서 서버 요청 리소스를 절약 한다거나..

### 자바스크립트의 타이밍 인터페이스
1회성 이벤트 발생, setTimeout, timer

- setTimeout: 특정 시간 이후 이벤트를 발생시킴

    ``` javascript
    setTimeout( () => {
        console.log('after 1s');
    }, 1000)
    ```

- timer: setTimeout와 동일하게 일회성으로 특정 시간 이후 일회성 push하는 옵저버블 생성

    ``` javascript
    import * as Rx from 'rxjs';

    Rx.timer(1000)
    .subscribe( () => {
      console.log('after 1s');
    });
    ```
    
시간 주기를 갖고 이벤트 발생, setInterval, 
- setInterval: 특정 시간 주기로 발생

    ``` javascript
    setInterval( () => {
        console.log('1s');
    }, 1000)
    ```
    
- interval: setInterval과 동일하게 특정 주기를 갖고 push하는 옵저버블 생성
    
    ``` javascript
    Rx.interval(1000)
    .subscribe( () => {
      console.log('1s');
    });
    ```

## 4.3 RxJS로 시간 다루기
앞의 타이밍 연산자(timer, inteval 같은)는 데이터를 생성하는 옵저버블 스트림과 결합하여 사용한다.
- 이 조합을 통해 데이터를 소비하는 빈도를 타이머로 동기화 시킬수 있다. (특정 시간동안 한번만 발생시킨다던가..)
- 시간을 다루는 연산자는 이벤트 전파에만 영향력을 갖는다. 이벤트 생성 시간에 영향이 없다. (이벤트는 생성되었지만 push는 1초뒤에 한다거나..)
- 시간 연산자는 함수체이닝으로 연결된 순서대로 동작한다.

### 전파
여기서는 시간관련 연산자가 전체 파이프라인 중에서 어디까지 전파가 그리고 어떠한 순서로 전파되는가에 대한 설명을 하고 있다.

*delay()*연산자의 경우 전체 옵저버블 파이프라인에 영향을 미치는 것이 아니라 delpay가 지정된 하위 시퀀스 전체를 일정 시간 만큼 지연시킨다 라고 생각해야된다.

``` javascript
Rx.of(1,2,3,4,5)
.pipe(tap(x => console.log(`do1:${x}`))) //여기 까지는 delpay의 영향이 없다.
.pipe(delay(2000))
.pipe(tap(x => console.log(`do2:${x}`))) //여기 수행이 시점이 이전 시퀀스 종료시점으로 부터 2초만큼 지연된것이라고 생각해야된다.
.pipe(delay(1000)) //여기서 1초의 딜레이를 추가한다.
.pipe(tap(x => console.log(`do3:${x}`))) //여기 기준으로는 3초가 딜레이 되었다고 생각하기 보다는, 어차피 이전 시퀀스가 끝난 다음 수행되는 것이니 1초가 딜레이 되었다라고 생각하는게 맞다. 즉 순차적으로 동작한다 라는 것을 설명
.subscribe( x => console.log(`recive:${x}`));
```

## 4.4 사용자 입력 처리하기
interval, imter, delay와 같은 함수들을 수행할 작업을 알고 있는 상태에서 스케쥴링하려는 목적으로 적합하다. 즉 특정 시간 후 동작할 작업을 미리 알고 있는 상태를 말한다.

짧은 시간내에 많은 이벤트가 발생하는 환경에서는 앞서 설명한 함수들을 사용하기 보다는 debounceTime, throttleTime 을 사용하는것이 좋다.

- debounceTime: 일정 시간 범위에서 발생하는 이벤트에 대해 모두 무시하며, 일정 시간 경과 후 마지막 하나의 이벤트만 push한다. (1초동안 마우스를 연속 클릭해도 1초동안 1번의 클릭만 push)

- thottleTime: 일정 시간 동안 처음 발생한 이벤트와 다른 모든 이벤트를 무시하고 처음 발생한 이벤트를 push한다.

! 이부분에 대한 예시를 좀더 찾아서 명확하게 개념을 잡아야할거 같다.

## 4.5 RxJS에서 버퍼링
- 버퍼링 연산자는 과거에 발생한 이벤트를 일시적으로 저장하는 구조를 제공한다.
- 버퍼링 연산자에서 push한 데이터를 수신하는 쪽은 옵저버블 배열을 받는다.
- 하나의 이벤트 처리에 큰 오버헤드가 발생하는 작업에 유용하다.(마우스나 스크롤 관련 이벤트 처리)

buffer, bufferCount, bufferWhen, bufferTime 연산자들이 있다.
