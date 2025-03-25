# Pipes
- 요청 데이터를 변환하고 유효성 검사를 처리하는 데 사용되는 기능
- 주로 컨트롤러의 핸들러로 전달되는 데이터가 유효하고 올바른 형식인지 확인하고, 필요에 따라 변환하거나 정리함

#### 📌 주요기능
1. 유효성 검사 (Validation)
	`ex) 클라이언트로부터 전달된 데이터가 특정 조건을 만족하는지 검사하고, 유효하지 않으면 오류 발생`

2. 데이터 변환 (Transformation)
	`ex) 문자열 -> 숫자 변환, 날짜 문자열 -> Date 객체 변환 등`

3. 예외 처리 (Exception handling)
	- 유효성 검사 또는 데이터 변환 중 오류가 발생하면, 파이프는 예외를 발생시켜 클라이언트에게 오류 메시지 전달

#### 📌 대표적인 파이프
- 기본적으로 몇 가지의 내장 파이프를 제공하며, 대표적으로는 **ValidationPipe, ParseIntPipe**가 있음

1) ValidationPipe
	
	📎 주요 옵션
	1. transform (boolean)
		- 기본값: false
		- true -> 요청 데이터가 DTO로 변환될 때 자동으로 타입 변환 시도(요청 데이터의 값을 DTO 클래스에 맞는 타입으로 변환)
	2. whitelist (boolean)
		- 기본값: false
		- true -> 요청 데이터에서 DTO에 정의되지 않은 프로퍼티를 자동으로 제거하여 불필요한 데이터 차단 가능
	3. forbidNonWhitelisted (boolean)
		- 기본값: false
		- true -> 요청 데이터에 DTO에 정의되지 않은 필드가 포함되어 있을 경우 예외 발생
		- whitelist 옵션과 함께 사용하여 추가적인 필드가 포함되었을 때 오류를 발생시키는 방식
	4. disableErrorMessages (boolean)
		- 기본값: false
		- true -> 유효성 검사 실패 시 오류 메시지가 반환되지 않음
		- 오류 메시지를 숨기고 싶을 떄 유용하게 사용됨
	5. skipMissingProperties (boolean)
		- 기본값: false
		- true -> DTO에서 정의된 프로퍼티가 요청 데이터에 포함되지 않아도 검증을 건너뛰고 진행(프로퍼티가 없어도 검증 안함)
	6. validationError ({target: boolean, value: boolean })
		- 기본값: { target: true, value: true }
		- 검증 오류가 발생할 때, 오류메시지에 포함될 정도를 세부적으로 제어 가능
			- target: 검증 실패한 객체를 오류 메시지에 포함할 지 여부 (true -> 객체정보 포함)
			- value: 검증 실패한 값 자체를 오류 메시지에 포함할 지 여부 (true -> 값 포함)
	7. exceptionFactory (Function)
		- 기본값: undefined
		- 커스텀 예외를 생성할 수 있는 팩토리 함수로, 검증 실패시 기본 BadRequestException 대신 다른 예외를 발생시킬 수 있음
	8. stopAtFirstError (boolean)
		- 기본값: false
		- true -> 첫 번째 검증 오류가 발생했을 때 검증을 즉시 중지
		- 여러 오류가 있을 때 모두 검증하지 않고 첫 번째 오류에서 멈추고 싶을 때 유용
	
``` typescript
// CreateUserDto클래스에 정의된 유효성 검사 규칙을 따름
// 클라이언트에서 잘못된 데이터를 보내면, 자동으로 오류 응답 반환
import { Controller, Post, Body } from '@nestjs/common';
import { IsString, IsInt } from 'class-validator';
import { ValidationPipe } from '@nestjs/common';

class CreateUserDto {
	@IsString()
	name: string;

	@IsInt()
	age: number;
}

@Controller('users')
export class UsersController {
	@Post()
	create(@Body(new ValidationPipe()) createUserDto: CreateUserDto) {
		return `User created: ${createUserDto.name}, Age: ${createUserDto.age}`;
	}
}
```



2) ParseIntPipe
``` typescript
// id 파라미터를 문자열에서 정수로 반환하며, 만약 id가 숫자가 아니면 예외 발생
import { Controller, Get, Param } from '@nestjs/common';
import { ParseIntPipe } from '@nestjs/common';

@Controller('users')
export class UsersController {
	@Get(':id')
	getUsers(@Param('id' ParsedIntPipe) id: number) {
		return `User ID is ${id}`;
	}
}
```


#### 📌 커스텀 파이프 생성
- PipeTransform 인텊페이스를 구현하여 커스텀 파이프 생성 가능
``` typescript
// 문자열을 대문자로 변환하는 커스텀 파이프 생성
import { Injectable, PipeTransform, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class CustomPipe implements PipeTransform {
	transform(value: any, metadata: ArgumentMetadata) {
		// 데이터 변환 로직
		return value.toUpperCase();
	}
}

// 사용 예제
@Controller('users')
export class UsersController {
	@Post()
	create(@Body(new CustomPipe()) name: string) {
		return `User name: ${name}`;
	}
}
```