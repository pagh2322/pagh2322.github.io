---
title: 타입스크립트 기본 문법 정리
description: Nest JS를 시작하기 위한 준비
categories:
- TypeScript
tags:
- TypeScript
---

# 기본 타입
```typescript
// string
let str: string = 'hi';

// number
let num: number = 10;

// boolean
let isLoggedIn: boolean = false;

// array
let arr: number[] = [1,2,3]; // Array<number>도 가능
let array: (string | number)[] = ['Apple', 1, 2, 'Banana', 'Mango', 3]; // Union

// tuple - 배열 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식 
let arr: [string, number] = ['hi', 10];
// 정의하지 않은 타입, 인덱스로 접근할 경우 오류가 난다
arr[5] = 'hello'; // Error, Property '5' does not exist on type '[string, number]'.

// enum
enum Avengers { Capt, IronMan, Thor }
let hero: Avengers = Avengers.Capt;

// any
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];

// void - 변수에는 undefined와 null만 할당하고, 함수에는 반환 값을 설정할 수 없는 타입
let unuseful: void = undefined;
function notuse(): void {
  console.log('sth');
}

// never - 함수의 끝에 절대 도달하지 않는다는 의미를 지닌 타입
function neverEnd(): never { // 이 함수는 절대 함수의 끝까지 실행되지 않는다는 의미
  while (true) {

  }
}
```



# 함수
```typescript
// 타입을 지정할 수 있음
function sum(a: number, b: number): number {
  return a + b;
}

// optional - 인자를 안 넘기고 싶으면 ?를 사용
function sum(a: number, b?: number): number {
  return a + b;
}
```

> 옵셔널 - `?` = `| undefined`

# 인터페이스
속성에 **`?`**를 사용하면 선택적 속성으로 정의할 수 있다.
```typescript
interface IUser {
  name: string,
  age: number,
  isAdult?: boolean
}

let user: IUser = {
  name: 'Neo',
  age: 123
};
```

# 읽기 전용 읽기
**`readonly`** 키워드를 사용하면 초기화된 값을 유지해야 하는 읽기 전용 속성을 정의할 수 있다.
```typescript
interface IUser {
  readonly name: string,
  age: number
}

let user: IUser = {
  name: 'Neo',
  age: 36
};

user.age = 85;
user.name = 'Evan'; // Error
```

# 오버로드
**이름은 같지**만 **매개변수 타입과 반환 타입이 다른** 여러 함수를 가질 수 있는 것이다.

```typescript
function add(a: any, b: any): any { // 함수 구현
  return a + b;
}
```

`any`를 사용해 오버로드를 구현하는게 다른 언어와 다른 재밌는 점인 듯 하다.

# 클래스
```typescript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}
class Cat extends Animal {
  getName(): string {
    return `Cat name is ${this.name}.`;
  }
}
let cat: Cat;
cat = new Cat('Lucy');
console.log(cat.getName()); // Cat name is Lucy.
```

## 추상 클래스
```typescript
// Abstract Class
abstract class Animal {
  abstract name: string; // 파생된 클래스에서 구현해야 합니다.
  abstract getName(): string; // 파생된 클래스에서 구현해야 합니다.
}
class Cat extends Animal {
  constructor(public name: string) {
    super();
  }
  getName() {
    return this.name;
  }
}
```

추상 클래스가 인터페이스와 다른 점은 속성이나 메소드 멤버에 대한 세부 구현이 가능하다.
```typescript
abstract class Animal {
  abstract name: string;
  abstract getName(): string;
  // Abstract class constructor can be made protected.
  protected constructor(public legs: string) {}
  getLegs() {
    return this.legs
  }
}
```

# 옵셔널
## 옵셔널 체이닝
Swift와 비슷하다. 다른 점은 해당 변수의 타입을 `Union`방식으로 하여 체이닝을 구현할 수 있다.

```typescript
// Error - TS2532: Object is possibly 'undefined'.
function toString(str: string | undefined) {
  return str.toString();
}

// Type Assertion
function toString(str: string | undefined) {
  return (str as string).toString();
}

// Optional Chaining
function toString(str: string | undefined) {
  return str?.toString();
}
```

```typescript
// Before
if (foo && foo.bar && foo.bar.baz) {}

// After-ish
if (foo?.bar?.baz) {}
```

# Nullish 병합 연산자
```typescript
const foo = null ?? 'Hello nullish.';
console.log(foo); // Hello nullish.

const bar = false ?? true;
console.log(bar); // false

const baz = 0 ?? 12;
console.log(baz); // 0
```

