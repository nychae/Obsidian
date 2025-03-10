# Ts config
- tsconfig.json은 TypeScript 프로젝트의 설정 파일로, 프로젝트 컴파일러 옵션 및 파일 포함/제외 규칙을 정의함

### 1. compilerOptions
- TypeScript 컴파일러의 동작을 제어하는 옵션

1) target
	- 생성될 JavaScript의 ECMAScript 타겟 버전 설정
	~~~ json
	{
		"compilerOptions": {
			"target": "ES5" | "ES6"
		}
	}
	~~~
 
2) module
	- 생성할 모듈 시스템 설정
~~~ json
{
	"compilerOptions": {
		"module": "commonjs" | "esnext" | "umd"
	}
}
~~~
 
3) strict
	 - 엄격한 타입 검사를 활성화
	 - true: 대부분의 엄격한 검사 옵션이 활성화 됨

4) outDir
	- 컴파일된 JavaScript 파일을 저장할 디렉터리 지정

5) rootDir
	- TypeScript 소스 파일의 루트 디렉터리 지정

6) esModuleInterop
	- ES 모듈과 CommonJS 모듈의 상호 운용성을 향상 시킴
	- 주로 import 구문을 사용할 때 유용

7) skipLibCheck
	- 라이브러리 파일의 타입 검사 건너뛰기
	- true로 설정시 성능이 개선될 수 있음

8) resolveJsonModule
	- JSON 파일을 모듈처럼 임포트할 수 있도록 설정

### 2. include
- 컴파일에 포함할 파일이나 폴더를 지정함
- 기본적으로 TypeScript는 tsconfig.json 파일이 위치한 폴더 내의 모든 파일을 포함하지만, include 옵션 사용시 특정 파일만 포함시킬 수 있음
~~~json
{
	"include": ["src/**/*"]
}
~~~

### 3. exclude
- 컴파일러에서 제외할 파일이나 폴더 지정
- node_modules 폴더는 기본적으로 제외됨
~~~json
{
	"exclude": ["node_modules", "dist"]
}
~~~

### 4. files
- 포함할 파일을 명시적으로 지정 가능
- include와 유사하지만 더 세밀한 제어 가능
~~~json
{
	"files": ["src/index.ts", "src/util.ts"]
}
~~~

### 5. extends
- tsconfig.json 파일을 확장할 때 사용됨
- 다른 설정 파일을 기반으로 추가 설정을 덧붙일 수 있음
~~~json
{
	"extends": "./base-tsconfig.json"
}
~~~

### 6. references
- 프로젝트 참조를 정의할 때 사용됨
- 여러 TypeScript 프로젝트를 관리할때 유용함
- 다른 프로젝트를 참조하고 해당 프로젝트를 함께 빌드할 수 있도록 함
~~~json
{
	"references": [{ "path": "./lib" }]
}
~~~

### 7. typeRoots
- 타입 정의 파일의 위치를 지정할 때 사용함
- 기본적으로 TypeScript는 node_modules/@types 디렉터리에서 타입 정의를 찾음
- 해당 옵션 사용시 다른 디렉터리에서도 타입을 찾을 수 있음
~~~json
{
	"typeRoots": ["./custom_types", "./node_modules/@types"]
}
~~~

### 8. types
- TypeScript에서 사용할 타입 정의를 지정함
- 특정 타입 정의만 포함시킬 때 유용
~~~json
{
	"types": ["node", "jest"]
}
~~~

### 9. lib
- 프로젝트에서 사용할 ECMAScript 라이브러리의 버전 설정
- 예를 들어 브라우저 API나 Node.js API 사용 가능
~~~json
{
	"lib": ["ES6", "DOM"]
}
~~~

### 10. noEmit
- true로 설정시 TypeScript는 컴파일을 수행하지만 실제로 JavaScript 파일을 출력하지 않음
- 주로 타입 체크만 하고 싶은 경우 유용함
 

