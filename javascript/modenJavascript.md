# Modern JavaScript
모던 자바스크립트를 위해 알아야될 기본 배경 지식

## 목차
- [자바 스크립트의 동작 환경](#자바-스크립트의-동작-환경)
- [브라우저 환경에서 자바스크립트](#브라우저-환경에서-자바스크립트)
- [서버 환경에서 자바스크립트](#서버-환경에서-자바스크립트)
- [명세서, 메뉴얼](#명세서-메뉴얼)
- [ECMA 명세](#ECMA-명세)
- [엄격 모드 use strict](#엄격-모드-use-strict)
- [폴리필, 트렌스파일러](#폴리필-트렌스파일러)
- [패키지 관리 도구](#패키지-관리-도구)
- [참고문서](#참고문서)

## 자바 스크립트의 동작 환경
- javaScript는 브라우저뿐만 아니라 다양한 플랫폼에서 사용가능한 언어이다.
- 플렛폼은 호스트 환경(Host environmnet)라고 부른다.
- 호스트 환경은 랭귀지 코어(ECMA Script)+호스트 환경에 맞는 객체와 함수를 제공한다.
- 자바스크립트 엔진의 간략 동작 과정
    1. 엔진이 스크립트를 읽어드림(파싱)
    2. 기계어로 변환(컴파일)
    3. 기계어로 변환된 코드 실행
- 엔진은 각 프로세스마다 최적화를 실시, 컴파일일 끝난 시점에서도 실행중인 코드를 감시하며 최적화 진행

### 브라우저
- "자바스크립트 가상 머신"이라 불리는 엔진이 내장되어있음, 각 브라우저와 버전별로 제공하는 랭귀지 코어가 다르다.
    - V8: Chrome, Opera
    - SpiderMonkey: Firefox
    - Trident, Chakra: IE(버전에 따라 다름)
    - ChakraCore: Edge
    - SquirrelFish: Safari
- 브라우저에서는 window라는 객체를 추가적으로 제공한다. window 객체 하위에 DOM,BOM,javascrip 가 있다.
- [DOM(문서 객체 모델)](https://dom.spec.whatwg.org)
    - 웹 페이지의 메인 진입점으로, DOM객체를 이용하여 웹 페이지에 표기되는 항목들을 생성, 수정 가능
    - 호스트 환경이 브라우저가 아니더라도 html파일 다운로드하여 DOM객체를 사용할 수도 있지만 명세의 일부만 사용가능하다.
- [CSSOM](https://www.w3.org/TR/cssom-1/)
    - javaScrip로 CSS 제어를 위한 CSSOM도 있다.
- [BOM(브라우저 객체 모델)](https://html.spec.whatwg.org)
    - 사용자-브라우저간 상호작용을 위한 객체(___alert___, ___promt___,등), ___navigator___ 같은 브라우저가 제공하는 추가적인 객체를 나타냄
    
### 서버 사이드
- nodeJS: V8 자바스크립트 엔진으로 동작

## 브라우저 환경에서 자바스크립트
- 메모리나, CPU같은 Low layer의 조작 허용안함
- 브라우저상에서 동작하는 행위만 가능
    - HTML 조작
    - 사용자의 입력(마우스, 키보드)에 대한 이벤트 핸들링
    - 네트워크 요청
    - 쿠키를 핸들링
    - 로컬스토리지와 같은 영역에서의 클라이언트측 데이터 저장
- 보안을 위한 제약사항
    - 클라이언트 디스크내 파일 접근 불가(사용자가 input태그에 파일을 가져왔을땐 가능)
    - 카메라, 마이크같은 상호작용 불가(사용자의 명시적 허용이 있을경우 가능)
    - 브라우저 내 탭, 창간의 데이터 교류 불가
        - 자바스크립트를 이용한 창 호출시 데이터 교류 가능(단 동일 출처 정책에 한하여)
- 호환성
    - 다양한 브라우저와 브라우저 버전별 자바스크립트의 호환이 다른경우가 많으니 아래 호환표를 참고
    - [caniuse](https://caniuse.com/)에서는 특정 메소드명으로 검색하여 브라우저별 호환표를 볼 수 있음
    
## 서버 환경에서 자바스크립트
- [NodeJS](https://nodejs.org/ko/about/)를 통해 서버사이드에 동작하는 어플리케이션을 javaScript코드로 작성할 수 있다.
- 자세한 내용은 [공식 가이드 문서](https://nodejs.org/ko/docs/guides/)를 참고

## 명세서, 메뉴얼

### 명세서
- 자바스크립트의 언어를 정의하는 명세서는 [ECMA-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm)를 참고하면된다.
- 단 내용이 상당히 어렵고, 상세 스팩을 명시하고 있다.

### 메뉴얼
- 개발을 위한 내용이라면 아래 사이트가 참고하기 편함
- [Mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference)
- 구글 검색에서 ___MDN parseInt___ 식으로 검색해서 사용하면 빠르게 내용을 찾을 수 있음

## ECMA 명세
- ECMAScript, ES 라고 불리는것은 [ECMA-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm)기술 규격에 정의된 표준을 구현한 스크립트로 다양한 호스트 환경을 지원하기 위한 표준
- javaScript는 ___ECMAScript___ 코어와 호스트환경에 따른 추가 객체(브라우저면 DOM, BOM이런것들)로 이루어짐, 즉 javaScript는 ___ECMAScript___ 표준을 구현한 언어라고 할 수 있다.
- ES 버전에 따른 특징
    - [ES3~8까지 아주 간략한 정리](https://medium.com/sjk5766/ecma-script-es-정리와-버전별-특징-77715f696dcb)
    - [ES6](https://seokjun.kim/ecmascript-6-features/)
    - [ES5](https://k39335.tistory.com/81)

## 엄격 모드 use strict
- javaScript 코드를 모던 환경에서 동작하도록한다. (모던 환경이라는 것은 ES5 이상을 말한다.)
- 스크립트 최상단에 ___use strict___ 문자열을 추가하면 브라우저가 해당 javaScript 코드를 모던 모드로 해석한다.

    ``` javascript
    'use strict'

    ...모던한 javasciprt 코드들
    ```

- 특정 함수에서만 엄격 모드를 적용하기 위해서는 함수 내 최상단에 ___use strict___ 문자열을 추가

    ``` javascript
    function() {
        'use strict'
        ...모던한 javasciprt 코드들
    }
    ```
    
- ___class___, ___module___ 같은 몇몇 기능은 자동으로 ___use strict___ 가 적용된다.

## 폴리필, 트렌스파일러
- 모던 자바스크립트 개발을 위해 폴리필과 트렌스파일러는 필수적이다. 모든 호스트 환경에서 javaScript에 새롭게 추가되는 명세를 지원하는 것이 아니기 때문에 폴리필과 트렌스파일러를 이용하여 대응할 수 있도록 지원한다.

### 폴리필 Polyfill
- 호스트 환경의 렝귀지 코어가 지원하는 표준을 준수할 수 있게 기존 함수의 동작 방식을 수정하거나, 새롭게 구현한 함수의 스크립트를 추가하는 작업을 폴리필이라고 한다. poly fill 단어 그대로 부족한 부분을 매꿔주는 것이다.

### 트렌스파일러
- 개발자가 작성한 코드를 재작성해주는 것을 말한다. 이를 수행하면 기존 코드가 타켓의 호스트 환경에 맞게 코드가 변환된다.

## 패키지 관리 도구
- 모듈형태의 라이브러리의 사용과 의존성 관리, 등을 위해 사용하는 관리 도구, 대표적으로 npm, yarn, 등이 있다.

## 참고문서
- [The Modern JavaScript Tutorial](https://javascript.info/)
- [ECMA 위키](https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)
