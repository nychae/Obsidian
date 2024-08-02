# 링크드리스트(LinkedList)

![](https://i.imgur.com/SUhqWsC.png)


- **특징**
	\- 각 요소는 노드 형태로 저장되며, 각 노드는 데이터와 다음 노드를 가리키는 포인터를 가짐
	
- **접근 시간**
	\- O(n) => 처음부터 차례로 접근해야 함

- **장점**
	\- 크기가 동적이며, 삽입과 삭제가 용이함

- **단점**
	\- 인덱스를 통한 직접 접근이 불가능함
	\- 추가적인 메모리 공간(pointer)이 필요

- **사용 예**
	\- 빈번한 삽입/삭제 작업, 동적 메모리 할당이 필요한 경우

``` java
class Node {
	int data;
	Node next;
	
	Node(int data) {
		this.data = data;
		this.next = null;
	}
}

public class LinkedListExample {
	Node head;

	public void append(int data) {
		if(head == null) {
			head = new Node(data);
			return;
		}

		Node current = head;
		
		while(current.next != null) {
			current = next;
		}
		
		current.next = new Node(data);
	}
}

public void printList() {
	Node current = head;

	while(current != null) {
		System.out.print(current.data + " ");
		current = current.next;
	}
}

public static void main(String[] args) {
	LinkedListExample list = new LinkedListExample();

	list.append(1);
	list.append(2);
	list.append(3);

	list.printList();
}
```