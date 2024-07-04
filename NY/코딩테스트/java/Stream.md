# 1. 생성하기

### Array Stream

```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);

// arr배열의 1~2요소 [b,c]
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
```

### Collection Stream
+ 컬렉션 타입(Collection, List, Set)의 경우 인터페이스에 추가된 디폴트 메소드 stream을 이용

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
Stream<String> parallelStream = list.parallelStream(); // 병렬 처리
```

### 비어있는 Stream

```java
public Stream<String> streamOf(List<String> list) {
	return list == null || list.isEmpty() 
		? Stream.empty()
		: list.stream();
}
```

### 원하는 값으로 생성

1. Stream.builder()
	builder()를 이용해 스트림에 직접 원하는 값을 넣어 생성
	
```java
Stream<String> buildStream = 
	Stream<String>builder()
		.add("Eric").add("Elena").add("Java")
		.build();
```

2. Stream.generate()
	generate의 파라미터로 람다값을 넣어 스트림 생성
	==이때 생성되는 스트림은 크기가 정해져있지 않고 무한(infinite)하기 때문에 특정 사이즈로 최대 크기를 제한해야 함==
	
```java
Stream<String> generatedStream = Stream.generate(() -> "gen").limit(5);
// [el, el, el, el,el]
```

3. Stream.iterate()
	iterate()를 이용해 초기값과 해당값을 다루는 람다를 이용해 스트림 생성
	==이때 생성되는 스트림은 크기가 정해져있지 않고 무한(infinite)하기 때문에 특정 사이즈로 최대 크기를 제한해야 함==
	
```java
Stream<Integer> iteratedStream = Stream.iterate(30, n -> n + 2).limit(5);
// 30이 초기값, 2씩 증가하는 스트림 생성
/ [30, 32, 34, 36, 38]
```

### 기본 타입형 Stream

+ 제네릭을 이용하지 않고 직접적으로 해당 타입의 스트림을 다룰 수 있음
	+  range : 두번째 인자인 종료지점이 포함안됨
	+ rangeClosed : 두번째 인자인 종료지점이 포함됨

```java
IntStream intStream = IntStream.range(1, 5)
// [1, 2, 3, 4]

LongStream longStream = LongStream.rangeClosed(1, 5);
// [1, 2, 3, 4, 5]
```
	
+ 제네릭을 이용하지 않기때문에 불필요한 오토박싱(auto-boxing)이 일어나지 않음
	필요할 경우 boxed() 메소드를 이용해 박싱(boxing).
		오토박싱? 
		 - 원시타입의 데이터를 해당 래퍼 클래스의 객체로 변환하는 것 (ex. int -> Integer)

```java
Stream<Integer> boxedIntStream = IntStream.range(1, 5).boxed();
```

+ random 클래스를 이용해 난수로 3가지 타입(IntStream, LongStream, DoubleStream) 스트림 생성가능
```java
DoubleStream doubles = new Random().doubles(3);
```

### 문자열 String

+  ex 1) 문자열의 각 문자(char)를 int형 스트림으로 변환
```java
IntStream charsStream = "Stream".chars();
// [83, 116, 114, 101, 97, 109]
```

+  ex 2) 정규표현식을 이용해 문자열을 자르고 스트림 생성
```java
Stream<String> stringStream = Pattern.compile(", ").splitAsAtream("Eric, Elene, Java");
// [Eric, Elena, Java]
```

### Parallel Stream
+ parallelStream 메소드를 사용해서 병렬 스트림 생성 가능
```java
// 병렬 스트림 생성
Stream<Product> parallelStream = productList.parallelStream();

// 병렬 여부 확인
boolean isParallel = parallelStream.isParallel();

// 각 작업들은 쓰레드를 이용해 병렬처리됨
boolean isMany = parallelStream
					.map(product -> product.getAmount() * 10)
					.anyMatch(amount -> amount > 200);
```


# 2.가공하기
### Filtering
+ 스트림 내 요소들을 하나씩 평가해서 걸러내는 메소드

```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");

Stream<String> stream = names.stream().filter(name -> name.contains("a"));
// [Elena, Java]
```
### Mapping
1. map() : 스트림 내 요소들을 하나씩 특정 값으로 변환
```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");

Stream<String> stream = names.stream().map(String::toUpperCase)
// [ERIC, ELENA, JAVA]


// Product 객체의 수량을 가져옴
Stream<Integer> stream = productList.stream().map(Product::getAmount);
```

2. flatMap() : 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어줌. 
```java
// [[a], [b]]
List<List<String>> list = Arrays.asList(Arrays.asList("a"), Arrays.asList("b"));

List<String> flatList = list.stream().flatMap(Collection::stream).collect(Collectors.toList());
// [a, b]
```

```java
students.stream()
		.flatMapToInt(student -> IntStream.of(student.getKor(),
											student.getEng(), 
											student.getMath()))
		.average()
		.isPresent(avg -> 
					System.out.println(Math.round(avg * 10)/10.0));
```

### Sorting
+ sorted() 메소드를 이용해 정렬
+ default: 오름차순
+ 파라미터가 없다면 오름차순으로 정렬됨

```java
IntStream.of(14, 11, 20, 39, 23)
	.sorted()
	.boxed()
	.collect(Collectors.toList());
// [11, 14, 20, 23, 39]
```

+ Comparator를 이용해 역순으로 정렬

```java
List<String> lang = Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");

lang.stream().sorted().collect(Collectors.toList());
// [Go, Groovy, Java, Python, Scala, Swift]

lang.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList());
// [Swift, Scala, Python, Java, Groovy, Go]
```

+ compare(o1, o2)  :  두 인자를 비교해서 값 리턴
```java
lang.stream()
	.sorted(Comparator.comparingInt(String::length))
	.collect(Collectors.toList());
// [Go, Java, Scala, Swift, Groovy, Python]

lang.stream()
	.sorted((s1, s2) -> s2.length() - s1.length())
	.collect(Collectors.toList());
// [Groovy, Python, Scala, Swift, Java, Go]	
```

### Iterating
+ peek() : 작업을 처리하는 중간에 결과를 확인해볼때 사용, 결과에 영향을 미치지 않음
```java
int sum = IntStream.of(1, 3, 5, 7, 9)
	.peek(System.out::println)
	.sum();
```

# 3. 결과만들기
### Calculating
+ 최소, 최대, 합,  평균 등 기본형 타입으로 결과생성
```java
long count = IntStream.of(1, 3, 5, 7, 9).count();
long sum = LongStream.of(1, 3, 5, 7, 9).sum();
// 스트림이 비어있는 경우 count, sum은 0 출력

// 평균, 최대, 최소의 경우 표현할 수 없기 때문에 Optional이용해 리턴
OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min();
OptinalInt max = IntStream.of(1, 3, 5, 7, 9).max();

DoubleStream.of(1.1, 2.2, 3.3, 4.4, 5.5).average().ifPresent(System.out::println);
```

### Reduction
+ reduce() : 총 3가지의 파라미터를 받을 수 있음
```java
// 1개 (accumulator)
Optional<T> reduce(BinaryOperator<T> accumulator);

// 2개 (identity)
T reduce(T identity, BinaryOperator<T> accumulator);

// 3개 (combiner)
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
```
1. accumulator  : 각 요소를 처리하는 계산 로직. 각 요소가 올 때마다 중간 결과를 생성하는 로직
```java
OptionalInt reduced = IntStream.range(1, 4).reduce((a, b) -> {
	return Integer.sum(a, b);
})
// 6 -> (1 + 2 + 3)
```

2. identity : 계산을 위한 초기값으로 스트림이 비어서 계산할 내용이 없더라도 이 값은 리턴
```java
int reducedTwoParams = IntStream.range(1, 4).reduce(10, Integer::sum);

//16 -> (10 + 1+ 2+ 3)
```

3.  combiner : ==병렬(parallel) 스트림==에서 나눠 계산한 결과를 하나로 합치는 로직
```java
Integer reducedParams = Arrays.asList(1,2,3).parallelStream().reduce(10, Integer::sum, (a,b) -> {
	System.out.println("combiner was called");
	return a+b;
});

//10+1, 10+2, 10+3
//11 + 12 + 13 = 36
```
### Collecting
1. Collectors.toList() : 스트림에서 작업한 결과를 담을 리스트 반환
``` java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

List<String> collectorCollection = productList.stream().map(Product::getName).collect(Collectors.toList());

//[potatos, orange, lemon, bread, sugar]
```

2. Collectors.joining() : 스트림에서 작업한 결과를 하나의 스트링으로 이어붙임, 3개의 파라미터를 가질 수 있음
- delimiter : 각 요소 중간에 들어가 요소를 구분시켜주는 구분자
- prefix : 결과 맨 앞에 붙는 문자
- suffix : 결과 맨 뒤에 붙는 문자 
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

String listToString = productList.stream().map(Product::getName).collect(Collectors.joining);

//potatosorangelemonbreajsugar

String listToString2 = productList.stream().map(Product::getName).collect(Collectors.joining(", ", "<", ">"));

//<potatos, orage, lemon, bread, sugar>
```

3. Collectors.averagingInt() : 숫자값의 평균 리턴
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));
                
Double averageAmount = productList.stream().colect(Collectors.averagingInt(Product::getAmount));

// 17.2
```

4. Collectors.summingInt() : 숫자의 합 리턴
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

Integer summingAmount = productList.stream().collect(Collectors.summingInt(Product::getAmount));
// 86

// mapToInt로 더 간단하게 표현 가능
Integer sum = productList.stream().mapToInt(Product::getAmount).sum();
```

5. Collectors.summarizingInt() : 개수, 합계, 평균, 최소, 최대값을 모두 한번에 필요할 때 사용
	IntSummaryStatics 반환 
	- 개수 : getCount()
	- 합계 : getSum()
	- 평균 : getAverage()
	- 최대 : getMax()
	- 최소: getMin()
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

IntSummaryStatics = statitics = productList.stream().collect(Collectors.summarizingInt(Product::getAmount));

// IntSymmaryStatics {count=5, sum=86, min=12, average=17.200000, max=23} 
```

6. Collectors.groupingBy() : 특정 값으로 요소들을 그룹핑
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));
                
Map<Integer, List<Product>> collectorMapOfLists = productList.stream().collect(Collectors.groupingBy(Product::getAmount));

// {23=[Product{amount=23, name="potatos"},
// 	Product{amount=23, name="bread"}],
// 14=[Product{amount=14, name="orange"}],
// 13=[Product{amount=13, name="lemon"},
// Product{amount=13, name="sugar"}]
//} %%
```

7. Collectors.partitioningBy() : 파라미터에 주어진 함수에따라 true, false 값으로 구분지어 리턴.
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

Map<Boolean, List<Product>> mapPartitioned = productList.stream().collect(Collectors.partitioningBy(el -> el.getAmount() > 15));

//{
//false = [
//	Product{amount=14, name="orange"},
//	Product{amount=13, name="lemon"},
//	Product{amount=13, name="sugar"}
//],
//true = [
//	Product{amount=23, name="potatos"},
//	Product{amount=23, name="bread"}
//]
//}
```

8. Collectors.collectingAndThen() : collect이후 추가작업이 필요한 경우 사용
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));

//Set으로 collect한 후 수정불가한 Set으로 변환하는 작업을 추가로 실행
Set<Produt> unmodifiableSet = productList.stream().collect(Collectors.collectingAndThen(Collectors.toSet(), Collections::unmodifiableSet));

```

9. Collector.of() : collector를 직접 필요한 로직으로 만들어서 사용
```java
public static<T, R> Collector<T, R, R> of(  
  Supplier<R> supplier, // new collector 생성  
  BiConsumer<R, T> accumulator, // 두 값을 가지고 계산 
  BinaryOperator<R> combiner, // 계산한 결과를 수집하는 함수.  
  Characteristics... characteristics) { ... }
```

```java
Collector<Product, ?, LinkedList<Product>> toLinkedList = Collectors.of(LinkedList::new),
LinkedList::add,
(first, second) -> {
first.addAll(second);
return first;
});

LinkedList<Product> linkedListOfPersons = productList.stream().collect(toLinkedList);
```
### Matching
- 조건식 람다 Predicate를 받아서 해당 조건을 만족하는 요소가 있는지 체크한 결과를 리턴
	1. anyMatch : 하나라도 조건을 만족하는 요소가 있는지 체크
	2. allMatch :  모두 조건을 만족하는지 체크
	3. nonMatch : 모두 조건을 만족하지 않는지 체크
```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");

boolean anyMatch = names.stream().anyMatch(name -> name.contains("a")); //true
boolean allMatch = names.stream().allMatch(name -> name.length() > 3); //true
boolean noneMatch = names.stream().noneMatch(name -> name.endsWith("S")); //true

```
### Iterating
- forEach () : 요소를 돌면서 실행되는 최종 작업, peek()은 중간작업인 반면 forEach()는 최종작업임.
```java
names.stream().forEach(System.out::println);
```

# 4. 동작 순서
- 한 요소가 모든 파이프라인을 거쳐서 결과를 만들어내고, 다음 요소로 넘어가는 순서로 동작됨
```java
List<String> list = Arrays.asList("Eric", "Elena", "Java");

list.stream()
	.filter(el -> {
		System.out.println("filter() was called");
		return el.contains("a");
	})
	.map(el -> {
		System.out.println("map() was called);
		return el.toUpperCase();
	})
	.findFirst();
	
// 출력결과
// filter() was called
// filter() was called
// map() was called
```
	1. Eric은 a를 포함하지 않기 때문에 다음 연산으로 넘어가지 않음 -> filter() was called만 출력
	2. 다음 요소인 Elena는 a를 포함하기 때문에 다음 연산으로 넘어감. 다음 연산인 findFirst()는 첫번째 요소만은 반환하는 연산이기 때문에 다음 요소를 수행할 필요가 없어서 연산 종료됨 -> filter() was called, map() was called 출력
	
# 5. 성능 향상
 + 스트림은 한 요소씩 수직적으로(vertically) 실행되기때문에 요소의 범위를 줄이는 작업을 먼저 실행하면 불필요한 연산을 막을 수 있어 성능을 향상 시킬 수 있음
	 1. skip(n) : Stream의 처음 n개의 요소를 버리는 작업 
``` java
List<String> list = Arrays.asList("Eric", "Elena", "Java");
	
List<String> collect = list.stream()
	.skip(2)
	.map(el -> {
		wasCalled();
		return el.substring(0, 3);
	})
	.collect(Collectors.toList()); 

System.out.println(collect); //[Jav]
	```
	 
	 2. filter() : Stream 내의 요소에 대해서 필터링 하는 작업
``` java
List<String> list = Arrays.asList("Eric", "Elena", "Java");

List<String> collect = list.stream()
	.filter(el -> {
		return el.contains("a");
	})
	.collect(Collectors.toList()); 
	
System.out.println(collect); // [Elena, Java]
```
	
	 3. distinct(): Stream에서 중복되는 요소들을 모두 제거하고 새로운 스트림을 반환하는 작업
``` java
List<String> list = Arrays.asList("Eric", "Elena", "Java", "Eric", "Elena");

List<String> collect = list.stream()  
.distinct()  
.collect(Collectors.toList()); 

System.out.println(collect); // [Eric, Elena, Java]
```

# 6. 스트림 재사용
+ 종료 작업을 하지 않는 한 하나의 인스턴스로서 계속해서 사용 가능
+ 종료 작업을 하는 순간 스트림이 닫히기 때문에 재사용 불가
+ 종료연산 종류
	1. forEach(), forEachOrdered() : 각 요소에 지정된 작업 수행
	2. count() : 개수 반환
	3. max() : 최대값 반환
	4. min() : 최소값 반환
	5. findAny(), findFirst() : 아무거나 / 첫번째 요소 반환
	6. allMatch() : 모두 만족시키는지
	7. anyMatch() : 하나라도 만족시키는지
	8. noneMatch() : 모두 만족하지 않는지
	9. toArray() : 배열로 반환
	10. reduce() : Stream 요소를 하나씩 줄여가며 계산
	11. collect() : Stream 요소를 수집
``` java
Stream<String> stream = Stream.of("Eric", "Elena", "Java").filter(name -> name.contains("a"));

Optional<String> firstElement = stream.findFirst(); //스트림 닫힘
Optional<String> anyElement = stream.findAny(); // IllegalStateException: stream has already been operated upon or closed
```

``` java
// 아래와 같이 변경 가능
List<String> names = Stream.of("Eric", "Elena", "Java").filter(name -> name.contains("a")).collect(Collectors.toList());

Optional<String> findFirst = names.stream().findFist();
Optional<String> findAny = names.stream().findAny();
```

# 7. 지연 처리(Lazy Invocation)
+ Stream에서 최종 결과는 최종 작업이 이루어질 때 계산됨
```java
private wasCalled() {
	counter++;
}

List<String> list = Arrays.asList("Eric", "Elena", "Java");
long counter = 0;

Stream<String> stream1 = list.stream().filter(el -> {
	wasCalled();
	return el.contains("a");
})

System.out.println(counter); // 0 -> 최종작업이 실행되지않아서 실제로 스트림연산이 실행되지않았기 때문.

counter = 0;

List<String> stream2 = list.stream().filter(el -> {
	wasCalled();
	return el.contains("a");
}).collect(Collectors.toList());

System.out.println(counter); // 3 -> collect메소드로 최종작업이 실행되어 실제로 스트림연산이 실행됨
```
# 8. Null-safe 스트림 생성
+ 개발 시 흔히 발생하는 예외 상황인 NullPointerException에 대비하여 Optional을 이용해 null-safe한 스트림 생성
```java
public <T> Stream<T> collectionToStream(Collection<T> collection) {
	return Optional.ofNullable(collection) // collection 객체로 Optional 객체 생성
		.map(Collection::stream) // 스트림 생성
		.orElseGet(Stream::empty); // 컬렉션이 비어있는경우 빈 스트림 리턴
}

List<Integer> intList = Arrays.asList(1, 2, 3);
List<String> strList = Arrays.asList("a", "b", "c");

Stream<Integer> intStream = collectionToStream(intList); // [1, 2, 3]
Stream<String> strStream = collectionToStream(strList); // [a, b, c]

List<String> nullList = null;

nullList.stream()
	.filter(str -> str.contains("a"))
	.map(String::length)
	.forEach(System.out::println); // NullPointerException

collectionToStream(nullList)
	.filter(str -> str.contains("a"))
	.map(String::length)
	.forEach(System.out::println); // []
```
# 9. 줄여쓰기(Simplified)
+ Stream 사용시 같은 내용을 좀 더 간결하게 줄여쓰는 몇가지 경우가 있음
``` java
collection.stream().forEach() -> collection.forEach()

collection.stream().toArray() -> collection.toArray()

Arrays.asList().stream() -> Arrays.stream() / Arrays.streamOf()

Collections.emptyList().stream() -> Stream.empty()

stream.filter().findFirst().isPresent() -> stream.anyMatch()

stream.collect(counting()) -> stream.count()

stream.collect(maxBy()) -> stream.max()

stream.collect(mapping()) -> stream.map().collect()

stream.collect(reducing()) -> stream.reduce()

stream.collect(summingInt()) -> stream.mapToInt().sum()

stream.map(x -> {...; return x;}) -> stream.peek(x -> ...)

!stream.anyMatch() -> stream.noneMatch()

!stream.anyMatch(x -> !(...)) -> stream.allMatch()

stream.map().anyMatch(Boolean::booleanValue) -> stream.anyMatch()

IntStream.range(expr1, expr2).mapToObj(x -> array[x]) 
	-> Arrays.stream(array, expr1, expr2)

Collection.nCopies(count, ...) -> Stream.generate().limit(count)

stream.sorted(comparator).findFirst() -> Stream.min(comparator)
```

+ 주의 사항 1 : 동기화(synchronized) 차이 
```java
// not synchronized
Collections.synchronizedList(...).stream().forEach()

// synchronized
Collections.synchronizedList().forEach()
```

+ 주의 사항 2:  NullPointerException
``` java
collect(Collectors.maxBy()) // Optional 객체리턴 => Null-Safe함

Stream.max() // NullPointerException 발생 가능
```