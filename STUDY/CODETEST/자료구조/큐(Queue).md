# 큐(Queue)

![](https://i.imgur.com/gMWAiFJ.png)

- **특징**
	\- FIFO(First In, First Out) 원칙에 따라 작동

- **접근 시간**
	\- O(1) => 가장 먼저 추가된 요소에 접근

- **장점**
	\- 데이터의 삽입/삭제가 빠름

- **단점**
	\- 중간 요소에 접근 불가

- **사용 예**
	\- 작업 스케줄링, 인쇄 대기열 관리

``` java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
	public static void main(String[] args) {
		Queue<Integer> queue = new LinkedList<>();
		queue.add(1);
		queue.add(2);
		queue.add(3);

		while(!queue.isEmpty()) {
			System.out.println(queue.poll());
		}
	}
}
```