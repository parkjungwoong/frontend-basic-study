# node package
노드 프로젝트에서 package, package-lock에 관한 내용 정리

## 목차 
- [package.json](#package.json)
- [package-lock.json](#package-lock.json)
- [Semantic Versioning](#Semantic-Versioning)

## package.json
node 프로젝트에 대한 dependency, meta data 정보, 등의 내용들이 json 값으로 저장되어있다.

만약 해당 package를 publish할 계획이 있다면 `name`, `version` 필드는 반드시 작성해야된다. `name`, `version` 필드 값은 결합하여 유니크한 값이여야 한다. 

그 외 내용들은 [package.json 공식문서](https://docs.npmjs.com/files/package.json) 참고

## package-lock.json
npm 명령어로 인해 `node_modules tree`, `package.json`에 변경이 있을 경우 자동적으로 생성된다.

package-lock.json 파일은 다음 4가지 목적으로 소스코드에 포함될 수 있다.

1. 공동 작업자, 또는 환경에서 동일한 dependency를 갖게 하고 싶을 경우
2. node_modules 디렉토리를 커밋하지 않아도 node_modules의 이전 상태로 가져올 수 있도록 할 경우
3. dependency tree의 변경을 diff로 확인 가능하고 싶을 경우
4. npm이 각 package의 dependency를 확인하지 않도록 하여 설치 프로세스를 향상시키고 싶을 경우

[package-lock 공식문서](https://docs.npmjs.com/files/package-lock.json) 참고

### dependency가 갖고 있는 dependency를 특정 버전으로 강제하고 싶을 경우
1번 내용의 연장선으로 package가 갖고 있는 dependency 목록 중 A가 에서 다른 package B에 dependency를 갖고 있을 수 있다.

이 때 A의 package.json에 기술된 B의 버전이 1.3을 1.5로 변경하고 싶을 경우 package-lock.json을 통해 dependency tree 관계를 설정해 줄 수 있다.

[stackOverFlow 답변 내용](https://stackoverflow.com/a/48524488/10702102) 

## Semantic Versioning
package.json의 version 필드에 작성되는 버저닝 규칙

버저닝 방식: `MAJOR`.`MINOR`.`PATCH`

- MAJOR version when you make incompatible API changes <- 기존에 지원하던 기능에 변경이나 삭제가 있을 경우,
- MINOR version when you add functionality in a backwards compatible manner, and <- 새로운 기능이 추가된 경우
- PATCH version when you make backwards compatible bug fixes. <- 버그 픽스

[semver 공식문서](https://semver.org/)

'>', '^', '~' 이런 표기법은 아래 문서 참고
[npm semver 공식문서](https://docs.npmjs.com/misc/semver)
