# 스택(Stack)

![](https://i.imgur.com/2j2MLuo.png)

- **특징**
	\- LIFO(Last In, First Out) 원칙에 따라 작동

- **접근 시간**
	\- O(1) => 가장 최근에 추가된 요소에 접근

- **장점**
	\- 데이터의 삽입, 삭제가 빠름

- **단점**
	\- 중간 요소에 접근 불가능

- **사용 예**
	\- 함수 호출 스택, 수식 계산, 브라우저 뒤로 가기 기능

``` java
import java.util.Stack;

public class StackExample {
	public static void main(String[] args) {
		Stack<Integer> stack = new Stack<>();
		stack.push(1);
		stack.push(2);
		stack.push(3);

		while(!stack.isEmpty()) {
			System.out.println(stack.pop());
		}
	}
}
```
