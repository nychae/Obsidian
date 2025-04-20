# Spring Batch
- 대용량 처리에 특화된 프레임워크
- ETL(Extract -> Transform -> Load) 처리, 정기적인 대량 작업(ex. 일 배치)에 자주 쓰임
- 트랜잭션, 체크포인트, 재시도, 실패 복구 등을 신회성 있게 처리하는데 최적화

### 📌 주요 특징
1) 대용량 데이터 처리
	- 수십만 ~ 수천만 건의 데이터 처리 가능

2) Chunk 기반 처리
	- 고정 크기 단위로 읽고, 처리하고 쓰는 구조 (read -> process -> write)

3) 트랜잭션 관리
	- 트랜잭션 단위로 Commit/Rollback 제어 가능

4) Job/Step 구조화
	- Job 안에 여러 Step이 있고, Step은 읽기/처리/쓰기 담당

5) Retry/Skip 기능
	- 예외 발생 시 자동 재시도/건너뛰기 설정 가능

6) JobInstance/Execution 구분
	- 어떤 Job이 어떤 파라미터로 실행됐는지 구분 가능

7) 다양한 Reader/Writer 제공
	- JDBC, JPA, Flat File, XML, JSON 등 지원

### 📌 용어 정리
1) Job
	- 하나의 배치 작업 단위 (전체 배치 프로세스)
	- ex) 고객 정보 갱신 배치

2) Step
	- Job 안에 정의되는 하나의 실행 단위 (작업 단계)
	- ex) CSV 읽기 -> DB 저장

3) JobInstance
	- Job 정의에 따라 실행된 인스턴스 (Job + 파라미터 조합)
	- ex) JobA + 파라미터(id=1)

4) JobExecution
	- JobInstance의 실제 실행 시도 (성공/실패 여부 포함)
	- ex) id=1 Job 실행 시도 (1회)

5) StepExecution
	- Step의 실제 실행 시도
	- ex) Step 1 수행 로그

6) JobParameters
	- Job 실행 시 외부에서 전달하는 파라미터
	- ex) 실행 시간, 파일 경로 등

7) JobLauncher
	- Job을 실행하는 트리거 역할의 컴포넌트
	- ex) jobLauncher.run(...)

8) JobRepository
	- Job/Step 실행 결과와 메타데이터를 저장하는 저장소
	- ex) 실행 이력 테이블 관리

9) JobBuilderFactory / StepBuilderFactory
	- Java Config 기반으로 Job/Step을 정의할 때 사용하는 빌더
	- ex) jobBuilderFactory.get("myJob")

10) ItemReader
	- 데이터를 한 건씩 읽는 컴포넌트
	- ex) DB에서 한 row, 파일 한 줄

11) ItemProcessor
	- 읽은 데이터를 가공/검증/필터링하는 컴포넌트 (선택사항)
	- ex) 데이터 유효성 검사

12) ItemWriter
	- 데이터를 한 번에 chunk 단위로 저장하는 컴포넌트
	- ex) DB insert, 파일 쓰기 등

13) Chunk
	- 한 번에 처리할 데이터 묶음 단위
	- ex) chunk(10)이면 10건씩 처리

14) Tasklet
	- 단순한 단일 작업을 수행하는 컴포넌트
	- ex) 파일 삭제, 디렉토리 생성 등

15) ExecutionContext
	- Step간 공유 가능한 상태 정보 저장소
	- ex) 마지막 처리 위치 저장 등


#### 📌 추가 용어(고급)
1) Skip
   - 에러가 나더라도 특정 항목은 무시하고 계속 진행
2) Retry
   - 실패한 항목을 재시도하는 기능
3) Listener
   - Job/Step의 실행 전후 이벤트를 후킹할 수 있는 기능 (ex. 로그 남기기)
4) Partitioning
   - Step을 여러 개로 나누어 병렬로 처리
5) Multi-threaded Step
   - 한 Step 안에서 여러 쓰레드로 parallel 처리
6) JobScope / StepScope
   - Job/Step 실행 시점에만 유효한 Bean 생성 범위


### 📌 구조 예시
```
Job (고객 배치)
	Step 1: CSV 읽기 -> DB 저장 (Chunk 기반)
	Step 2: 결과 통계 메일 전송 (Tasklet 기반)
```


### 📌 실행 흐름
1. Job 실행 시작
2. JobLauncher가 Job을 실행
3. JobRepository에 JobExecution 정보 저장
4. Job 내부의 여러 개로 구성된 Step 실행 시작
5. Step 안에서 반복적으로 Chunk 단위 처리
	- ItemReader: 데이터 읽음
	- ItemProcessor: 데이터 가공 (선택적)
	- ItemWriter: 데이터 출력 (ex. DB 저장)
6. StepExecution 결과 저장
7. 모든 Step 완료되면 Job 완료
8. JobExecution 결과 저장


### 📌 간단 코드 예제
``` java
@Bean
public Step step1() {
	return stepBuilderFactory.get("step1")
		.<String, String>chunk(10)
		.reader(myReader())
		.processor(myProcessor())
		.writer(myWriter())
		.build();
}

@Bean
public Job myJob() {
	return jobBuilderFactory.get("myJob")
		.start(step1())
		.build();
}
```


### 📌 실무 사용 예
1. 전날 주문 데이터 집계 및 통계 저장
2. DB -> 파일 내보내기 (Export)
3. 파일 -> DB 대량 로딩
4. 다건 SMS / 이메일 발송
5. 백그라운드 데이터 정합성 검사 및 정제


### 📌 Batch 처리의 장점
1. 신뢰성: 실패한 데이터만 재처리 가능
2. 성능 최적화: Chunk, 멀티스레드 등으로 병렬 처리 가능
3. 확장성: 분산 처리 (Partitioning, Remote Chunk) 가능
4. Spiring 연동성: Spring 환경과 자연스럽게 통합됨


### 📌 처리 방식
##### 1) Chunk 기반 처리	
✔️ 개념
	- 데이터를 묶음(chunk) 단위로 처리하는 방식
	- 읽기(Read) -> 처리(Process) -> 쓰기(Write) 과정을 반복하며 대량 데이터를 효율적으로 처리
	
✔️ 구성 요소
	- ItemReader: 데이터를 읽음 (ex. DB, 파일 등)
	- ItemProcessor: 데이터를 가공/변환 (선택적)
	- ItemWriter: 데이터를 출력 또는 저장

✔️ 예제
``` java
stepBuilderFactory. get("chunkStep")
	.<InputType, OutputType>chunk(100) // 100개씩 묶어서 처리
	.reader(myReader())
	.processor(myProcessor())
	.writer(myWriter())
	.build();
```
	
✔️ 특징
	- 트랜잭션이 chunk 단위로 commit됨 (rollback도 chunk 단위)
	- 대량 데이터 처리에 유의
	- 병렬 처리, skip/retry 처리에 적합


##### 2) Tasklet 기반 처리
✔️ 개념
	- Step을 단순한 작업 단위로 정의해서 한 번에 실행하는 방식
	- 반복 없이 한 번에 동작하며 주로 간단한 로직에 적합

✔️ 예제
``` java
stepBuilderFactory.get("taskletStep")
	.tasklet((contribution, chunkContext) -> {
		System.out.println("단일 작업 수행 중...");
		return RepeatStatus.FINISHED;
	})
	.build();

// 또는 클래스로 분리
public class MyTasklet implements Tasklet {
	public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) {
		// 로직 작성
		return RepeatStatus.FINISHED;
	}
}
```

✔️ 특징
- 한 번만 실행되는 로직에 적합
- ex) 디렉토리 정리, 로그 초기화, 파라미터 검증 등
- chunk와 달리 Reader/Processor/Writer 개념 없음

##### 상황별 사용되는 방식
1. 대량 데이터 반복 처리 -> Chunk 기반
   - 효율적, 성능 최적화 가능
2. 단순 파일 복사, 디렉토리 삭제 등 -> Tasklet 기반
	- 반복 없이 1회성 처리
3. 복잡한 트랜잭션 제어 -> Chunk 기반
	- 트랜잭션 범위 관리 유리
4. 배치 초기 설정, 후처리 -> Tasklet 기반
	- 전처리/후처리에 적합
	