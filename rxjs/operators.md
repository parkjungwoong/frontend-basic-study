# Operators
`Operators` 는 함수로 두가지 종류의 Operators가 있다.

### 1.Pipeable Operators
> A Pipeable Operator is a function that takes an Observable as its input and returns another Observable. It is a pure operation: the previous Observable stays unmodified.

`observerableInstance.pipe(operator())` 형태의 문법으로 다른 Observables를 연결(piped) 할 수 있다.

`Pipeable Operators`가 호출되면 기존 Observable 인스턴스를 변경하지 않고 새로운 Obervable을 반환하는데, 반환되는 Observable은 첫번째 Observable을 베이스(subscription logic)로 만들어진 인스턴스이다.

즉 `Pipeable Operators`는 순수 함수인 하나의 Obervable을 input으로 받고 다른 새로운 Obervable을 output으로 생성한다.

output으로 나온 Obervable을 구독한다는 것은 input Obervable도 같이 구독한다는것이다.

### 2.Creation Operators
독립 함수(부가적인 생성작업없이)로 해당 함수를 호출하면 새로운 Obervable을 생성하여 리턴해주는 Operators이다.

예를 들면, `of`와 같은 Create Operators가 있는데, `of(1,2,3)`이라고 `of` 함수를 호출하면 1,2,3을 순차적으로 반환하는 Observable이 생성된것이다.

## filter

## delay

## debounceTime

## take

## takeUntil

## distinct

## distinctUnillCanged