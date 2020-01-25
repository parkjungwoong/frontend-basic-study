# First class function (일급함수)
다음과 같은 경우 Firstclass Function 이라고 할 수 있다.

- [변수에 함수가 할당되어 있다](#변수에-함수가-할당되어-있다)
- [함수를 파라미터로 받는다](#함수를-파라미터로-받는다)
- [리턴값이 함수이다](#리턴값이-함수이다)

# examples

## 변수에 함수가 할당되어 있다
``` javascript
const fn = () => { console.log('i am firstclass function'); }

fn();
```

## 함수를 파라미터로 받는다
``` javascript
function (firstClassFn) {
    firstClassFn();
}
```

## 리턴값이 함수이다
``` javascript
const getFirstclassFn = () => {
    return () => { console.log('i am firstclass function'); }
}

const fn = getFirstclassFn();
fn();
```