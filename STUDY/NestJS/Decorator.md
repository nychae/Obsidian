# Decorator
- 클래스, 메소드, 속성, 매개변수에 메타데이터를 추가하는 역할
- TypeScript의 데코레이터 기능을 기반으로 NestJS는 컨트롤러, 서비스, 미들웨어, 파이프, 가드 등의 동작을 정의할 수 있게 해줌

### 📌 자주 사용되는 데코레이터
#### 1. Class Decorator
- 클래스 전체에 대한 설정을 추가하는데 사용

1) @Controller()
	: 컨트롤러 클래스 정의할 때 사용
``` typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users') // '/users' 경로를 담당하는 컨트롤러
export class UserController {
	@Get()
	getUsers() {
		return '유저 목록';
	}
}
```

2) @Injectable()
	: 서비스 클래스를 의존성 주입 가능하게 함
``` typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserService {
	getUsers() {
		return ['유저1', '유저2'];
	}
}
```

#### 2. Method Decorator
- 메소드의 동작을 설정할 때 사용

1) @Get(), @Post(), @Put(), @Delete()
	: HTTP 요청을 처리하는 엔드포인트를 정의할 때 사용
``` Typescript
import { Controller, Get, Post } from '@nestjs/common';

@Controller('users')
export class UserController {
	@Get() // GET 요청을 처리
	getUsers() {
		return '유저 목록';
	}

	@Post() // POST 요청을 처리
	createUser() {
		return '유저 생성';
	}
}
```

2) @useGuards()
	: 메소드에 가드를 적용해서 인증 및 권한을 제어할 때 사용
``` typescript
import { Controller, Get, UseGuard } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Controller('profile')
export class ProfileController {
	@Get()
	@UserGuards(AuthGuard('jwt')) // JWT 인증이 필요한 API
	getProfile() {
		return '내 프로필 정보';
	}
}
```

#### 3. Property Decorator
- 클래스의 속성에 대해 메타데이터를 추가할 때 사용

1) @Inject()
	: 서비스나 다른 의존성을 주입할 때 사용
``` typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class UserService {
	constructor(@Inject('USER_REPOSITORY') private userRepository: any) {}
}
```

#### 4. Parameter Decorator
- 컨트롤러 메소드의 매개변수에 대한 정보를 설정할 때 사용
- 매개변수 데코레이터는 import 없이 바로 사용할 수 있음

1) @Param()
	: URL 경로 파라미터를 가져올 때 사용
``` typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users')
export class UserController {
	@Get(':id')
	getUserById(@Param('id') id: string) {
		return `유저 ID: ${id}`;
	}
}
```

2) @Query()
	: 쿼리스트링 데이터를 받을 때 사용
``` typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users')
export class UserController {
	@Get()
	getUsers(@Query('role') role: string) {
		return `역할: ${role}인 유저 목록`;
	}
}
```

3) @Body()
	: POST 요청에서 JSON 데이터를 받을 때 사용
``` typescript
import { Controller, Post } from '@nestjs/common';

@Controller('users')
export class UserController {
	@Post()
	createUser(@Body() body: any) {
		return `유저 생성: ${JSON.stringify(body)}`;
	}
}
```

4) @Header()
	: HTTP 요청의 헤더 정보를 가져올 때 사용
``` typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users')
export class UserController {
	@Get()
	getUserAgent(@Headers('user-agent') userAgent: string) {
		return `User-Agent: ${userAgent}`;
	}
}
```