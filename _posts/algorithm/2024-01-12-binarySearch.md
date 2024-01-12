---
title: '이진 탐색 알고리즘'
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2024-01-12
---

- 순차 탐색 : 리스트 안에 있는 특정한 데이터를 찾기 위해 **앞에서부터 데이터를 하나씩 확인**하는 방법.
- 이진 탐색 : **정렬되어 있는 리스트**에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법.
  - 이진 탐색은 시작점, 끝점, 중간점을 이용하여 탐색 범위를 설정합니다.

# 이진 탐색의 시간 복잡도

- 단계마다 탐색 범위를 2로 나누는 것과 동일하므로 연산 횟수는 log2N에 비례합니다.
- 예를 들어 초기 데이터 개수가 32일 때, 이상적으로 1단계를 거치면 16개가량의 데이터만 남습니다.
  - 2단계를 거치면 8개 가량의 데이터만 남습니다.
  - 3단계를 거치면 4개 가량의 데이터만 남습니다.
- 다시 말해 이진 탐색은 탐색 범위를 절반씩 줄이며, 시간 복잡도는 O(logN)을 보장합니다.

```js
const binarySearch = (arr, target, start, end) => {
  while (start <= end) {
    const mid = Math.floor((start + end) / 2);
    if (target === arr[mid]) return mid;
    else if (target < arr[mid]) end = mid - 1;
    else start = mid + 1;
  }

  return -1;
};

const n = 10;
const target = 7;
const arr = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19];

const start = 0,
  end = n - 1;
console.log(binarySearch(arr, target, start, end) + 1);
```

- 처리해야 할 데이터의 개수가 `1,000만`단위 이상으로 넘어가면, 이진 탐색과 같이 `I(logN)`의 속도를 내야 하는 알고리즘으로 문제를 풀어야 하는 경우가 많다.
- 데이터의 개수가 `1,000만(10^7)개`를 넘어가거나, 탐색 범위의 크기가 `1,000억(10^11)`이상이라면, 이진 탐색 알고리즘을 의심해 보자.

# 파라메트릭 서치

- 파라메트릭 서치란 최적화 문제를 결정 문제(예 or 아니오)로 바꾸어 해결하는 기법.
  - 예시 : 특정한 조건을 만족하는 가장 알맞은 값을 빠르게 찾는 최적화 문제
- 일반적으로 코딩 테스트에서 파라메트릭 서치 문제는 이진 탐색을 이용하여 해결 할 수 있습니다.
