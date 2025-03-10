- **Declaration File(선언 파일)** 로 불리며, TypeScript 코드에서 JavaScript 라이브러리를 사용할 때 타입 정보를 제공하는 역할


### 목적
1) JavaScript 라이브러리 타입 정의
	- TypeScript는 정적 타입을 사용하지만, 많은 JavaScript 라이브러리는 타입 정의가 없음 -> d.ts파일을 만들어 해당 라이브러리의 타입 정의 가능
2) 모듈 또는 전역 변수의 타입 제공
	- 외부 모듈을 사용할 때 TypeScript가 해당 모듈의 구조를 알 수 있도록 함
3) 코드 자동 완성 및 문서화 지원
	- 타입 정보를 제공하므로, IDE에서 자동 완성과 문서화 사용 가능


### 기본 구조
1) 인터페이스와 타입 정의
~~~typescript
declare namespace MyLib {
	interface User {
		id: number;
		name: string;
	}

	function getUser(id: number): User;
}
~~~

2) 전역 변수 정의
~~~typescript
declare const API_URL: string;
~~~

3) 외부 모듈 타입 정의
~~~typescript
declare module "lodash" {
	export function shuffle<T>(array: T[]): T[];
}
~~~


### 활용
1) 외부 라이브러리의 타입 설치
	- Typescript에서 많은 외부 라이브러리의 d.ts파일은 @types 패키지에 포함되어있음
~~~sh
// lodash 라이브러리 사용시 이렇게하면 TypeScript가 lodash의 타입 정보 인식 가능
npm install lodash
npm install --save-dev @types/lodash
~~~

2) 수동으로 파일 생성
	- @types 패키지가 존재하지 않는 경우, 직접 d.ts파일을 만들어야 함
~~~typescript
declare module "myModule" {
	export function sayHello(name: string): string;
}


// 사용할 때
import { sayHello } from "myModule";
console.log(sayHello("TypeScript"));
~~~


### 규칙
1. declare 키워드를 사용해 전역변수, 함수, 인터페이스 등을 선언
2. 구현부({}) 없이 타입만 정의
3. .ts 파일과 달리 export 없이 전역에서 사용 가능하지만, 모듈 형식(declare module)으로 정의시 import 필요