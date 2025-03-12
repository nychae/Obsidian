# DefinitelyTyped

- npm에 존재하는 거의 모든 패키지들에 대해 TypeScript의 타입 정의 파일(.d.ts)을 모아놓은 레포지토리
- JavaScript 라이브러리를 TypeScript에서 사용할 수 있도록 타입 정보를 제공
	=> typesript로 만들어지지 않은 패키지를 타입 정의를 직접하지 않고 사용할 수 있게 해줌
	=> JavaScript 라이브러리를 완전한 타입 안전성과 함께 사용 가능 


✅ 사용 방법
1. @types 패키지 설치
	- ==@types/네임스페이스==를 통해 npm 패키지로 제공됨
~~~sh
// ex
npm i -D @types/lodash
~~~
~~~ typescript
import _ from "lodash";

const arr = [1, 2, 3];
const firstElement: number = _.first(arr) // 정상적으로 타입 추론됨
~~~

