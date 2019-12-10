# Node CLI
node CLI 어플리케이션 개발 정리

## package.json 설정
- main: 어플리케이션의 메인 함수
- bin: 어플리케이션 호출시 사용할 이름과 실행할 노드 실행 파일명
```
{
  "name": "my-cli-test",
  "main": "./dist/bundle.js",
  "bin": {
    "my-cli-test": "bin/my-cli-test"
  }
}
```
*name과 bin의 어플리케이션 호출명은 동일해야됨, 위 설정 파일에서는 my-cli-test으로 작성된것은 모두 같아야함*

## 노드 실행 파일
package.json에서 bin에 지정된 파일은 별도  확장자 없이 파일 최 상단에 `#!/usr/bin/env node` 문자열이 있어야함

```
#!/usr/bin/env node
require('../dist/bundle.js');
```

## 배포
- public 저장소에 배포
```
npm login
# 로그인 진행, https://www.npmjs.com/ 가입 필요
npm publish
```

- private 저장소에 배포
`package.json`에 name값에 `@그룹명/어플리케이션명` 설정, 배포 옵션 지정
```
"name": "@private그룹명/어플리케이션명"
"publishConfig": {
    "access": "restricted"
}
```
```
npm login
# 로그인 진행, https://www.npmjs.com/ 가입 필요
npm publish
```

## 파일 경로 관련
어플리케이션 내에서 내부 파일(json 파일,등)을 가져올때 커맨드 실행 경로가 아닌 어플리케이션 설치 경로를 참고하는 방법

1. package.json의 bin에 지정된 파일에 환경 변수값을 등록한다.
```
#!/usr/bin/env node
process.env.__binpath = __dirname + '/'; #환경변수값으로 어플리케이션 설치 경로를 지정
require('../dist/bundle.js');
```

2. 환경 변수값을 불러와 사용한다.
``` javascript
static getRootPath(): string {
    return process.env.__binpath.substring(0, process.env.__binpath.length - 5);
}
```

## 커맨드 라인 입력 받기
`inquirer` 패키지 이용
``` javascript
const yn = (
    await inquirer.prompt([
        {
            type: 'confirm', //type값에 input(값 입력), list(리스트 선택), confirm(예,아니요 선택)으로 변경하여 사용가능
            name: 'yn',
            message: `저장된 계정 정보로 진행하시겠습니까?`,
        },
    ])
).yn;
```

## typescript 설정
```
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "moduleResolution": "node",
    "strict": true,
    "allowJs": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "noImplicitAny": true,
    "noUnusedLocals": true,
    "outDir": "./dist/",
    "baseUrl": ".",
    "paths": {
      "(cli)/*": ["src/cli/*"],
    },
    "typeRoots": ["node_modules/@types", "sources/types"],
    "allowSyntheticDefaultImports": true
  },
  "include": [
    "src"
  ],
  "exclude": [
    "dist",
    "node_modules",
    "tests"
  ]
}

```

## webpack 설정
`target: 'node'` 설정 필수
```
const path = require('path');

module.exports = {
	target: 'node',
	entry: './src/index.ts',
	devtool: 'inline-source-map',
	module: {
		rules: [
			{
				test: /\.tsx?$/,
				use: 'ts-loader',
				exclude: ['/node_modules/', '/tests/'],
			},
		],
	},
	resolve: {
		extensions: ['.tsx', '.ts', '.js'],
		alias: {
			'(cli)': path.resolve('src', 'cli'),
		},
	},
	output: {
		filename: 'bundle.js',
		path: path.resolve(__dirname, 'dist'),
	},
	externals: {
		puppeteer: 'require("puppeteer")',
	},
};
```

## jest 설정
```
// For a detailed explanation regarding each configuration property, visit:
// https://jestjs.io/docs/en/configuration.html

module.exports = {
	transform: {
		'^.+\\.ts$': 'ts-jest',
	},
	clearMocks: true,

	coverageDirectory: 'coverage',

	moduleDirectories: ['.', 'src', 'node_modules'],

	moduleFileExtensions: ['js', 'ts', 'json'],
	testEnvironment: 'node',
	testMatch: ['**/tests/**/*.test.(js|jsx|ts|tsx)'],

	moduleNameMapper: {
		'^\\(cli\\)/(.*)': 'src/cli/$1',
	},
	globals: {
		'ts-jest': {
			diagnostics: true,
			tsConfig: 'tsconfig.json',
		},
	},
};

```