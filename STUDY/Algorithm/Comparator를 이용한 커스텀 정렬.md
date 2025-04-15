# Comparator를 이용한 커스텀 정렬

### 📌 개념
- Comparator는 객체의 정렬 기준을 외부에서 정의할 수 있는 인터페이스
- compare(o1, o2) 메소드를 구현해 두 객체를 비교하여 정렬 순서를 결정
- 기본 정렬이 아닌 사용자 정의 정렬 기준이 필요할 때 사용

### 📌 핵심메소드
``` java
compare(T o1, T o2)
```

- 반환값:  음수 -> o1이 먼저 옴
- 반환값: 0 -> 순서 유지
- 반환값: 양수 -> o2가 먼저 옴

### 📌 사용 예시

1) 기본 예시
``` java
Integer[] arr = {5, 2, 9, 1};
Arrays.sort(arr, (a, b) -> a - b); // 오름차순 정렬
```

2) 문자열 길이로 정렬
``` java
String[] arr = {"apple", "pie", "banana"};
Arrays.sort(arr, (a, b) -> a.length() - b.length());
```   

3) 특정 문자 기준 정렬 + 사전순 보조
``` java
String[] arr = {"sun", "bed", "car"};
int n = 1;
Arrays.sort(arr, (a, b) -> {
	if(a.charAt(n) == b.charAt(n)) {
		return a.compareTo(b);
	} 
	return Character.compare(a.charAt(n), b.charAt(n));
});
```

### 📌 팁 & 주의사항
- Arrays.sort() -> 배열 / Collections.sort() -> 리스트
- 람다식 사용시 직관적인 코드 구현이 가능하지만, 복잡한 비교일수록 클래스로 정의해도 좋음
- Comparator는 정렬 기준을 바꾸고 싶을 때 매우 유용
- 여러 조건이 있는 경우 thenComparing()을 사용해 정렬 체인을 만들 수 있음


```` ad-info
title: thenComparing()
color: 255, 255, 255
collapse: close

- 복합 정렬 기준을 만들 때 사용
- Comparator 체이닝을 통해 다단계 정렬 가능

```java
Comparator<T> comp = Comparator.comparing(기준1)
						.thenComparing(기준2)
						.thenComparing(기준3); // 필요시 계속 추가 가능
```

Ex 1) 문자열 길이순 -> 사전순
``` java
String[] arr = {"apple", "pie", "banana", "kiwi"};

Arrays.sort(arr, Comparator.comparing(String::length)
						.themComparing(Comparator.naturalOrder())
);
```

Ex 2) 객체 정렬
``` java
class Person {
	String name;
	int age;
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

List<Person> people = Arrays.asList(
	new Person("Alice", 30),
	new Person("Bob", 25),
	new Person("Alice", 25)
);

people.sort(Comparator
	.comparing((Person p) -> p.name)
	.thenComparingInt(p -> p.age)
);
```
````



### 📌 Comparable vs Comparator
1) Comparable
	구현 위치 : 클래스 내부 (compareTo())
	정렬 기준 수 : 1개
	유연성 : 낮음
	
2) Comparator
	구현 위치 : 클래스 외부 (compare())
	정렬 기준 수 : 여러 개 가능
	유연성 : 높음