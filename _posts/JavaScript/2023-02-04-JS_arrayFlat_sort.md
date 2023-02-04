---
title:  "JS Array flat & sort"
excerpt: "JS Array flat & sort 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

# Array flat
[MDN-ArrayFlat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)  

flat() 메서드는 모든 하위 배열 요소를 지정한 깊이까지 재귀적으로 이어붙인 새로운 배열을 생성합니다.  

```js
const arr1 = [1, 2, [3, 4]];
arr1.flat();
// [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

# Array sort 
[MDN-ArraySort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.  
참고로 **sort 는 배열에 변화를 준다.**  

## 구문  
```js
arr.sort([compareFunction])
```

## 매개변수
compareFunction : Optional  
정렬 순서를 정의하는 함수. 생략하면 배열은 각 요소의 문자열 변환에 따라 각 문자의 유니 코드 코드 포인트 값에 따라 정렬됩니다.

## 반환 값
정렬한 배열. 원 배열이 정렬되는 것에 유의하세요. 복사본이 만들어지는 것이 아닙니다.


## 설명

compareFunction이 제공되지 않으면 요소를 문자열로 변환하고 유니 코드 코드 포인트 순서로 문자열을 비교하여 정렬됩니다.  
예를 들어 "바나나"는 "체리"앞에옵니다. 숫자 정렬에서는 9가 80보다 앞에 오지만 숫자는 문자열로 변환되기 때문에 "80"은 유니 코드 순서에서 "9"앞에옵니다.  

compareFunction이 제공되면 배열 요소는 compare 함수의 반환 값에 따라 정렬됩니다. a와 b가 비교되는 두 요소라면  

- compareFunction(a, b)이 0보다 작은 경우 a를 b보다 낮은 색인으로 정렬합니다. 즉, a가 먼저옵니다. : 오름차순
- compareFunction(a, b)이 0을 반환하면 a와 b를 서로에 대해 변경하지 않고 모든 다른 요소에 대해 정렬합니다. 
- compareFunction(a, b)이 0보다 큰 경우, b를 a보다 낮은 인덱스로 소트합니다. : 내림차순
- compareFunction(a, b)은 요소 a와 b의 특정 쌍이 두 개의 인수로 주어질 때 항상 동일한 값을 반환해야합니다. 일치하지 않는 결과가 반환되면 정렬 순서는 정의되지 않습니다.

```js
const people = [
    {
        name: "hello1",
        age:13,
    },
    {
        name: "hello2",
        age:19,
    },
    {
        name: "hello3",
        age:8,
    },
    {
        name: "hello4",
        age:76,
    }
]

const orderPeopleByAge = (personA, personB)=> {
    console.log(personA, personB);
    return personA.age - personB.age;
}
console.log(people.sort(orderPeopleByAge));
```
[](https://user-images.githubusercontent.com/105098581/216742604-527b4044-6970-4058-a892-89f819bbe533.png)


