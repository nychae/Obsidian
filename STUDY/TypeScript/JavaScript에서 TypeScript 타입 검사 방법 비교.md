# JavaScript에서 TypeScript 타입 검사 방법 비교

## 1. allowJs + checkJs
- tsConfig.json의 comipilerOptions에서 allowJs, checkJs 설정 가능
- 프로젝트 전체에서 타입 검사를 활성화할때 사용
- JSDoc과 함께 사용시 더 정밀한 타입체크가 가능

✅ **allowJs**
	- TypeScript 프로젝트에서 js파일을 포함할 수 있게 하는 설정
	- true로 설정시 .js파일을 TypeScript 프로젝트에 포함시켜 함께 컴파일 가능
	- 타입 검사는 활성화 되지 않음

✅ checkJs
	- .js파일에서 TypeScript의 타입 검사 기능을 활성화하는 설정
	- true로 설정시 JavaScript 파일에 대한 타입 검사 가능
	- 타입 검사를 수행은 할 수 있지만, 타입 정보를 명시적으로 제공하지 않으면 정확한 타입 검사에는 한계가 있음
```json
{
	"compilerOptions": {
		"allowJs": true,
		"checkJs": true
	}
}
```


## 2. @ts-check + JSDoc
- 개별 JavaScript 파일에서 타입 검사를 활성화하고 싶을 때 사용

✅ @ts-check
	- TypeScript 타입 검사를 JavaScript 파일에 직접 적용할 수 있는 주석
	- js파일 상단에 @ts-check주석 추가시 TypeScript의 타입 검사 수행 가능
	- 타입 정보를 정확히 제공하려면 JSDoc을 함께 사용하는 것이 좋음

✅ JSDoc
	- JavaScript 코드에 타입을 명시하는 주석 표기법
	- @param, @returns, @typedef 등을 사용해 함수의 매개변수와 반환값의 타입 정의 가능
	- 타입 정보를 명시적으로 지정할 수 있어 ==더 정확한 타입 검사를 받을 수 있음==
	- checkJs와 함께 사용 가능

~~~ js
// @ts-check

/**
* @param {number} a - 첫 번째 숫자
* @param {number} b - 두 번째 숫자
* @returns {number} 두 숫자의 합
*/
function add(a, b) {
	return a + b;
}

console.log(add(1, "2")); // 타입 오류 발생
~~~


📍 상황별 추천 조합
1. 프로젝트 전체에 타입 검사 적용
	-> allowJs + checkJs + JSDoc
2. 개별 파일에서 타입 검사 적용
	-> @ts-check + JSDoc
