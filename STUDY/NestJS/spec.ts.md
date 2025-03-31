# spec.ts
- NestJS에서 테스트 코드를 작성할 때 사용하는 테스트 파일 확장자
- 일반적으로 서비스(.service.ts), 컨트롤러(.controller.ts), 레포지토리(.repository.ts)와 같은 주요 모듈을 검증하는데 사용
- Jest를 기본 테스트 프레임워크로 사용
	`Jest: Facebook(메타)에서 개발한 Javascript 테스트 프레임 워크`
- \*.spec.ts파일이 테스트 파일임을 의미
- 테스트 코드 작성을 통해 애플리케이션의 안정성을 높이고, 코드 변경시 기존 기능이 정상 동작하는지 확인 가능

#### 📌 역할
1. 단위 테스트 (Unit Test)
	- 특정 서비스나 모듈이 올바르게 동작하는지 검증
	- @Injectable()이 붙은 서비스의 메소드 테스트
	  
2. 통합 테스트 (Intergration Test)
   - 전체 애플리케이션을 실행하여 실제 데이터베이스, 모듈 간의 동작을 확인
   - Test.createTestingModule()을 사용하여 NestJS의 DI 컨테이너를 활용

#### 📌 자주 사용하는 테스트 메소드
1. 기본 메소드
	(1) describe()
	   - 테스트 그룹을 정의하는 메소드
	   - 동일한 개념`(ex) 하나의 서비스, 컨트롤러)`에 대한 테스트를 그룹화할 때 사용
	(2) it()
		- 개별 테스트 케이스를 정의하는 메소드
		- 첫 번째 파라미터: 테스트 설명, 두 번째 파라미터: 테스트 함수
	(3) expect()
		- 테스트 결과를 검증하는 메소드
		- 예상한 값과 실제 값이 일치하는지 확인
	(4) beforeEach()
		- 각 테스트 케이스 실행 전마다 실행 -> it() 실행마다 실행됨
	(5) beforeAll()
		- 모든 테스트 실행 전에 한 번 실행 -> describe() 별로 한 번만 실행됨
		  
```typescript
describe('SampleService', () => {
	// 테스트 코드 작성
	beforeAll(async () => {
		const module: TestingModule = await Test.createTestingModule({
			providers: [SampleService],
		}).compile();
		service = module.get<SampleService>(SampleService);
	});
	
	beforeEach(() => {
		// ex) 일부 상태를 초기화하는 경우 사용
		jest.clearAllMocks(); // 모든 Mock 초기화 (필요한 경우)
	})

	it('should return "Hello, World!"', () => {
		expect(1 + 1).toBe(2);
		expect(service.getHello()).toEqual('Hello, World!');
	})
})
```
 
2. NestJS 테스트 관련 메소드
	(1) Test.createTestingModule()
		- NestJS의 테스트 환경을 구성하기 위한 테스트 모듈 생성
		- 서비스, 컨트롤러, 레포지토리를 등록하여 테스트 가능
	(2) module.get()
		- NestJS의 TestingModule에서 특정 클래스(서비스, 컨트롤러 등)를 가져옴
	(3) jest.fn()
		- Jest에서 Mock 함수를 생성하는 메소드
		- 테스트 시 실제 로직을 실행하지 않고, 미리 정의한 값을 반환하도록 설정 가능
```typescript
describe('SampleService', () => {
	let service: SampleService;
	
	beforeEach(async () => {
		const module: TestingModule = await Test.createTestingModule({
			providers: [SampleService],
		}).compile();

		service = module.get<SampleService>(SampleService);
	})

	it('should call getData()', () => {
		const mockFn = jest.fn(); // Mock 함수 생성
		mockFn(); // 호출
		
		expect(mockFn).toHaveBeenCalled(); // 호출되었는지 검증
	})
})
```
   
3. Jest Matcher (비교 연산)
	(1) toBe()
		- 값이 정확히 일치하는지 확인
	(2) toEqual()
		- 객체나 배열의 값을 비교할 때 사용
	(3) toBeDefined()
		- 값이 undefined가 아닌지 확인
	(4) toHaveBeenCalled()
		- 특정 함수가 호출되었는지 확인
	(5) toHaveBeenCalledWith()
		- 특정 파라미터를 사용하여 호출되었는지 확인
```typescript
describe('SampleService', () => {
	it('Matcher Test', () => {
		expect(1 + 1).toBe(2);
		
		expect({ name: 'NestJS' }).toEqual({ name: 'NestJS' });

		expect(service).toBeDefined();
		
		const mockFn = jest.fn();
		mockFn();
		expect(mockFn).toHaveBeenCalled();

		const mockFn = jest.fn();
		mockFn('NestJS');
		expect(mockFn).toHaveBeenCalledWith('NestJS');
	})
})
```

#### 📌 테스트 코드 작성 예시
1. 서비스 테스트 예제 (sample.service.spec.ts)
``` typescript
import { Test, TestingModule } from '@nestjs/testing';
import { SampleService } from './sample.service';

describe('SimpleService', () => {
	let service: SampleService;

	beforeEach(async () => {
		const module: TestingModule = await Test.createTestingModule({
			providers: [SampleService]
		}).compile();

		service = module.get<SampleService>(SampleService);
	});

	it('should be defined', () => {
		expect(service).toBeDefined();
	});

	it('should return "Hello, World!"', () => {
		expect(service.getHello())
	})
})
```
