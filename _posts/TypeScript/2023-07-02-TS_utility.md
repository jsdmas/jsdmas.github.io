---
title: 'TS 유틸리티 타입'
toc: true
toc_sticky: true
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-07-02
---

- 제네릭 타입이라고도 불립니다.
- 꼭 쓰지는 않아도 되지만, 쓰면 짧게 쓸 수 있습니다.

# Partial

파셜 타입은 특정 타입의 부분 집합을 만족하는 타입을 정의할 수 있습니다.

**예시1**

```ts
interface Adderss {
  email: string;
  address: string;
}
type MyEmail = Partial<Adress>;
// 아래의 3가지 모두 가능
const me: MyEamil = {};
const you: MyEamil = { email: 'hello@gmail.com' };
const all: MyEamil = { email: 'hello@gmail.com', address: 'jinho' };
```

**예시2**

```ts

```
