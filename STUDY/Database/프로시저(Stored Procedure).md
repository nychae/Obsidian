# 프로시저(Stored Procedure)
- 데이터베이스에 저장된 일종의 서브루틴
- 일련의 SQL문 (SELECT, INSERT, UPDATE, DELETE 등)과 제어 흐름을 논리적으로 묶어서, 이름으로 저장해 두고 필요할 때마다 호출해서 실행할 수 있도록 하는 기능

##### 📌 특징
- 반복적으로 수행되는 로직을 모듈화하여 재사용 가능
- 트랜잭션 제어 가능 (COMMIT, ROLLBACK 등)
- IN, OUT, IN OUT 파라미터 지원
- SQL Developer, PL/SQL 블록, 애플리케이션 등에서 호출 가능

##### 📌 사용 목적
- 비즈니스 로직을 DB 레벨에서 처리
- 성능 향상 (한 번 컴파일된 후 캐시됨)
- 보안 강화 (직접 쿼리 노출 방지)

##### 📌 장점
- SQL 로직 재사용
- 비즈니스 로직의 캡슐화
- 클라이언트-서버 간 통신 비용 절감 (로직을 DB에서 처리)
- 보안 및 권한 제어 용이


##### 📌 기본 구조
``` SQL
CREATE OR REPLACE PROCEDURE 프로시저명 (
	파라미터명1 IN 데이터타입,
	파라미터명2 OUT 데이터타입
)
IS
	-- 변수 선언부
	변수명 데이터타입;
BEGIN
	-- 실행 로직
	-- ex: SELECT INTO, INSERT, UPDATE 등
EXCEPTION
	WHEN OTHERS THEN
		-- 예외 처리부
		DBMS_OUTPUT.PUT_LINE('에러 발생: ' || SQLERRM);
END 프로시저명;

```
- IN 파라미터: 호출 시 입력되는 값
- OUT 파라미터: 프로시저 실행 결과로 반환되는 값
- IN OUT 파라미터: 입력되 받고 변경된 값을 반환도 가능
- 변수 선언부: 내부 로직에서 사용하는 지역변수
- 예외 처리부: PL/SQL 예외 발생 시 처리 로직

##### 📌 사용 예제
- 사원 정보를 조회하고 급여를 OUT 파라미터로 반환하는 저장 프로시저 예제
``` sql
-- EMPLOYEE 테이블 생성 (샘플 데이터 테이블)
CREATE TABLE EMPLOYEE (
	EMP_ID NUMBER PRIMARY KEY,
	EMP_NAME VARCHAR2(100),
	SALARY NUMBER
)

-- 샘플 데이터 삽입
INSERT INTO EMPLOYEE (EMP_ID, EMP_NAME, SALARY)
VALUES (1001, 'KIM', 4000);

COMMIT;

-- GET_SALARY 프로시저 생성
CREATE OR REPLACE PROCEDURE GET_SALARY (
	P_EMP_ID IN NUMBER,
	P_SALARY OUT NUMBER
)
IS
BEGIN
	SELECT SALARY
	INTO P_SALARY
	FROM EMPLOYEE
	WHERE EMP_ID = P_EMP_ID;
EXCEPTION
	WHEN NO_DATA_FOUND THEN
		P_SALARY := 0;
	WHEN OTHERS THEN
		P_SALARY := -1;
END GET_SALARY;
```

🔹 자주 사용하는 예외처리
	1) NO_DATA_FOUND: SELECT INTO에서 반환된 행이 없을 때 발생
	2) TOO_MANY_ROWS: SELECT INTO에서 2건 이상 반환될 때
	3) OTHERS: 그 외의 예외 처리