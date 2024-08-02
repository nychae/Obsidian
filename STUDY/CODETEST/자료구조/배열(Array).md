# 배열(Array)

![](https://i.imgur.com/YhXyALT.png)

- **특징**
	\- 동일한 데이터 타입의 요소들이 연속된 메모리 공간에 저장됨

- **접근 시간**
	\- O(1) => 인덱스를 통해 직접 접근 가능

- **장점**
	\- 빠른 데이터 접근 속도

- **단점**
	\- 크기가 고정되어 있어 배열 크기 변경 불가

- **사용 예**
	\- 정적 데이터 저장, 읽기 작업이 빈번한 경우

``` java
public class ArrayExample {
	public static void main(String[] args) {
		int[] array = {1, 2, 3, 4, 5};

		for(int i = 0; i < array.length; i++) {
			System.out.println("Element at index " + i + " : " + array[i]);
		}
	}
}
```
