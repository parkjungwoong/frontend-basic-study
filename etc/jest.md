# Jest

## 목차
- [match테스트](#match 테스트)
- [mock]()
- [spy]()
- [async함수 테스트]()
- [전후처리]()
- [리포팅]()
- [설정]()
- []()
- [참고링크](#참고링크)

## match 테스트
- 값, 객체 동등 판단
    - toBe: `Object.is` 를 통해 같은지 검사
    - toEqual: 값이 같은지 검사 (객체의 값이 같은지 검사할 경우 사용하면 좋을듯) 
``` javascript
test('공통 메쳐 테스트', () => {
        //exepct(실제값).toBe(기대값);
        expect(1+3).toBe(4);
        expect(1+3).not.toBe(5);
        
        //exepct(실제값).toEqual(기대값);
        expect({one: 1, two: 2}).toEqual({one: 1, two: 2});
        expect({one: 1, two: 2}).not.toEqual({one: 1, two: 2, three:3});
    });
```

- 참거짓 판단
    - null, undefined, falsly, turely 를 구분하여 판단
``` javascript
test('참거짓 판단', () => {
    //null 체크
    expect(null).toBeNull();
    expect('null').not.toBeNull();
    expect(null).toBeDefined(); //toBeUndefined의 반대
    expect(null).not.toBeUndefined();
    expect(null).not.toBeTruthy();
    expect(null).toBeFalsy();

    //undefined 체크
    expect(undefined).toBeUndefined();
    expect('undefined').not.toBeUndefined();
    expect(undefined).not.toBeNull();
    expect(undefined).not.toBeTruthy();
    expect(undefined).toBeFalsy();

    //false 값 체크, toBeFalsy <-> toBeTruthy
    expect(false).toBeFalsy();
    expect(0).toBeFalsy();
    expect('').toBeFalsy();
    expect(undefined).toBeFalsy();
    expect(null).toBeFalsy();
    expect(1).not.toBeFalsy();
});
```

- 숫자판단
    - 범위, 부동소수점에 주의해서 사용
``` javascript
test('숫자 범위', () => {
    const value = 2 + 2;
    expect(value).toBeGreaterThan(3);//실제값 > 기대값
    expect(value).toBeGreaterThanOrEqual(3.5);//실제값 >= 기대값
    expect(value).toBeGreaterThanOrEqual(4);//실제값 >= 기대값
    expect(value).toBeLessThan(5);//실제값 < 기대값
    expect(value).toBeLessThanOrEqual(4.5);//실제값 <= 기대값

    //테스트의 결과 값이 정수값일 경우 toBe, toEqual로 검사
    expect(value).toBe(4);
    expect(value).toEqual(4);

    //정수가 아니라면 toBeCloseTo로 검사
    expect(0.1 + 0.2).toBeCloseTo(0.3); //Yes
    // expect(0.1 + 0.2).toBe(0.3); //No
});
```

- 문자열 판단
    - 문자열중에 특정 단어가 있는지 판단
``` javascript
test('문자열 판단', () => {
    expect('에러가 발생하였습니다.').toMatch('에러');
    expect('성공하였습니다.').not.toMatch('에러');
});
```

- 배열 요소 판단
    - array, iterable 의 요소에 특정 값이 있는지 판단
``` javascript
test('배열 요소 판단', () => {
        const shoppingList = [
            'diapers',
            'kleenex',
            'beer',
        ];
        expect(shoppingList).toContain('beer');//array
        expect(new Set(shoppingList)).toContain('beer');//iterable
    });
```

- Exception 판단
    - exception 발생여부, throw 객체 종류, message의 문자열 판단
``` javascript
test('Exception 판단', () => {
        function throwExceptionCode() {
            throw new Error('숫자만 입력');
        }

        expect(throwExceptionCode).toThrow(); //throw 발생했는지
        expect(throwExceptionCode).toThrow(Error); //throw 의 객체

        expect(throwExceptionCode).toThrow('숫자만 입력');
        expect(throwExceptionCode).toThrow(/숫자만/);
    });
```

- expect의 전체 메소드
    - [jest 공식 홈페이지](https://jestjs.io/docs/en/expect)

## mock
## spy
## async함수 테스트
## 전후처리
## 리포팅
## 설정

## 참고링크
- [jest공홈문서](https://jestjs.io/docs/en/getting-started)