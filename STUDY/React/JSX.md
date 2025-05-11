# JSX
- JavaScript의 확장 문법
- HTML과 유사한 태그 문법을 사용해 UI를 구성할 수 있게 해줌
- Babel같은 트랜스파일러가 JSX를 JavaScript로 변환시켜줌
- 예시
```jsx
const element = <h1>Hello, world!</h1>;
```
```javascript
// 위의 jsx문법은 내부적으로 아래 코드와 같이 변환됨
const element = React.createElement('h1', null, 'Hello, world!');
```

##### 📌 특징
1. HTML과 유사문법
	- HTML처럼 태그를 작성하지만 실제로는 JavaScript코드임
2. 표현식 포함 가능
	- 중괄호 {}를 사용해 변수나 함수 결과 삽입 가능
3. 컴포넌트 표현
	- React 컴포넌트를 태그 형태로 작성 가능
4. 1개의 부모 태그
	- JSX에서 반환시 반드시 하나의 부모 태그만 반환 해야함
5. claaaName 사용
	- HTML의 class대신 className사용 (JS의 예약어와 충돌 방지를 위해)

##### 📌 사용 예
``` jsx
function Greeting(props) {
	return <h1>Hello, {props.name}<h1>
}

const App = () => (
	<div>
		<Greeting name="Alice" />
		<Greeting name="Bob" />
	</div>;
)
```

