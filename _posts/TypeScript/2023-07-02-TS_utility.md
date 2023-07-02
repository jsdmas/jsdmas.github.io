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
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}
// Partial - 상품의 정보를 업데이트 (put) 함수 -> id, name 등등 어떤 것이든 인자로 들어올수있다
// 인자에 type으로 Product를 넣으면 모든 정보를 다 넣어야함
// 그게 싫으면
interface UpdateProduct {
  id?: number;
  name?: string;
  price?: number;
  brand?: string;
  stock?: number;
}
// 위와 같이 정의한다.
// 그러나 같은 인터페이스를 또 정의하는 멍청한 짓을 피하기 위해서 우리는 Partial을 쓴다.
function updateProductItem(prodictItem: Partial<Product>) {
  // Partial<Product>이 타입은 UpdateProduct 타입과 동일하다
}
```

# Pick

픽 타입은 특정 타입에서 몇 개의 속성을 선택하여 타입을 정의합니다.

```ts
Pick<T, K>;
```

- T는 타입입니다. Pick이 속성을 선택할 대상 타입입니다.
- K는 선택하려는 속성의 이름을 담은 유니온 타입입니다.

```ts
type shoppingItem = Pick<Product, 'id' | 'name' | 'price'>;

// 상품의 상세정보 (Product의 일부 속성만 가져온다)
function displayProductDetail(shoppingItem: shoppingItem) {
  // id, name, price의 일부만 사용 or 별도의 속성이 추가되는 경우가 있음
  // 인터페이스의 모양이 달라질 수 있음
}
```

# Omit

pick의 반대로, 특정 속성만 제거한 타입을 정의합니다.

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}

type shoppingItem = Omit<Product, 'stock'>;

const apple: Omit<Product, 'stock'> = {
  id: 1,
  name: 'red apple',
  price: 1000,
  brand: 'del',
};
```

여러개 타입 제외할떄 (multiple key) `|` 를 씁니다.

```ts
const apple: Omit<Product, 'stock' | 'brand'> = {
  id: 1,
  name: 'red apple',
  price: 1000,
};
```
