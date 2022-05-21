---
title: Controller란
description: 들어오는 요청(request)를 받고 처리된 결과를 응답(response)으로 돌려주는 인터페이스 역할
categories:
- NestJS
tags:
- NestJS
- TypeScript
---

# Controller
Controller는 **엔드포인트 라우팅(routing)** 메커니즘을 통해 각 Controller가 받을 수 있는 요청을 분류한다.

## 생성
```
$ nest g controller [name]
```

위 명령어를 실행 후, 자동 생성된 코드에서 `AppController`는 `AppModule`에 선언되어 있고, `AppService`를 import해서 사용하고 있다.

## CRUD 보일러 플레이트 코드 생성
```
$ nest g resource [name]
```

해당 명령어를 실행하여 어떠한 리소스를 생성하면, `module`, `controller`, `service`, `entity`, `dto`와 `테스트` 코드가 자동으로 생성이 된다.

# Routing
## 생성 후 (app.controller.ts)
**`@Controller`** 데코레이터를 클래스에 선언하는 것으로 해당 클래스는 Controller의 역할을 하게 된다.

`getHello` 함수는 **`@Get`** 데코레이터를 가지고 있으며, 루트 경로('/' 생략)로 들어오는 요청을 처리할 수 있다. 라우팅 경로를 **`@Get`** 데코레이터의 인자로 관리할 수 있다.

**`@Controller`** 데코레이터에도 인자를 전달할 수 있고, 이를 통해 라우팅 경로의 `prefix`를 지정한다. 예를 들어, **`@Controller('app')`** 이라고 했다면 이제 `http://localhost:3000/app/hello` 라는 경로로 접근해야 한다. prefix는 보통 Controller가 맡은 리소스의 이름을 지정하는 경우가 많다.

# 와일드카드
라우팅 패스는 **와일드카드**를 이용하여 작성할 수 있다. **`*`**, **`?`**, **`+`**, **`()`** 문자를 사용하면 문자열 가운데 어떤 문자가 와도 상관없이 라우링 패스를 구성하겠다는 뜻이다.

```typescript
@Get('he*lo')
getHello(): string {
  return this.appService.getHello();
}
```