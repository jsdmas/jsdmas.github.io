---
title:  "Two Pointers algorithm (5)"
excerpt: "Two Pointers"
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2023-06-26
---

# 두 배열 합치기

오름차순으로 정렬이 된 두 배열이 주어지면 두 배열을 오름차순으로 합쳐 출력하는 프로그램 을 작성하세요.  

▣ 입력설명  
첫 번째 줄에 첫 번째 배열의 크기 N(1<=N<=100)이 주어집니다. 두 번째 줄에 N개의 배열 원소가 오름차순으로 주어집니다.  
세 번째 줄에 두 번째 배열의 크기 M(1<=M<=100)이 주어집니다. 네 번째 줄에 M개의 배열 원소가 오름차순으로 주어집니다.  
각 리스트의 원소는 int형 변수의 크기를 넘지 않습니다.  
▣ 출력설명  
오름차순으로 정렬된 배열을 출력합니다.  
▣ 입력예제   
3  
1 3 5  
5  
2 3 6 7 9  

▣ 출력예제  
1 2 3 3 5 6 7 9  

```js

function solution(N, arr1, M, arr2){
  const result = [];
  let point1 = point2 = 0;
  // 이미 정렬된 배열 pointer 설정 후 비교 
  while (point1 < N && point2 < M){
    if(arr[point1] < arr2[point2]) result.push(arr1[point1++]);
    else result.push(arr2[point2++]);
  }

  // 남은 수 처리
  while(point1 < N) result.push(arr1[point1++]);
  while(point2 < M) result.push(arr2[point2++]);

  return result;
}

solution(3, [1, 3, 5], 5, [2, 3, 6, 7, 9])
```

# 공통원소 구하기

A, B 두 개의 집합이 주어지면 두 집합의 공통 원소를 추출하여 오름차순으로 출력하는 프로 그램을 작성하세요.  
▣ 입력설명  
첫 번째 줄에 집합 A의 크기 N(1<=N<=30,000)이 주어집니다.  
두 번째 줄에 N개의 원소가 주어집니다. 원소가 중복되어 주어지지 않습니다. 세 번째 줄에 집합 B의 크기 M(1<=M<=30,000)이 주어집니다.  
네 번째 줄에 M개의 원소가 주어집니다. 원소가 중복되어 주어지지 않습니다. 각 집합의 원소는 1,000,000,000이하의 자연수입니다.  
▣ 출력설명  
두 집합의 공통원소를 오름차순 정렬하여 출력합니다.  
▣ 입력예제  
5  
1 3 9 5 2  
5  
3 2 5 7 8  
▣ 출력예제  
2 3 5 
  
```js
function solution(N, arr1, M, arr2) {
    const result = [];
    let point1 = point2 = 0;
    // 배열먼저 오름차순 정렬
    arr1.sort((a, b) => a - b);
    arr2.sort((a, b) => a - b);

    while (point1 < N && point2 < M) {
        // 더 작은쪽의 포인터를 움직여서 비교해야 함 -> 큰쪽이 포인터가 증가되면 더이상 비교 불가능 (알맞는 비교 숫자가 없어서)
        if (arr1[point1] < arr2[point2]) point1++;
        if (arr1[point1] > arr2[point2]) point2++;
        if (arr1[point1] === arr2[point2]) {
            result.push(arr1[point1]);
            point1++;
            point2++;
        }
    }

    return result;

}
solution(5, [1, 3, 9, 5, 2], 5, [3, 2, 5, 7, 8]);
```

# 연속 부분수열 1

N개의 수로 이루어진 수열이 주어집니다.  
이 수열에서 연속부분수열의 합이 특정숫자 M이 되는 경우가 몇 번 있는지 구하는 프로그램을 작성하세요.  
만약 N=8, M=6이고 수열이 다음과 같다면  
1 2 1 3 1 1 1 2  
합이 6이 되는 연속부분수열은 {2, 1, 3}, {1, 3, 1, 1}, {3, 1, 1, 1}로 총 3가지입니다.  
▣ 입력설명  
첫째 줄에 N(1≤N≤100,000), M(1≤M≤100,000,000)이 주어진다. 수열의 원소값은 1,000을 넘지 않는 자연수이다.  
▣ 출력설명  
첫째 줄에 경우의 수를 출력한다.  
▣ 입력예제  
8 6  
1 2 1 3 1 1 1 2  
▣ 출력예제  
3  
  
```js
function solution(N, M, arr) {
    let sum = left = right = result = 0;
    // 오른쪽 으로 가면서 비교
    while (right < N) {
        if (sum === M) {
            result++;
            sum -= arr[left++];
        }
        if (sum > M) {
            sum -= arr[left++];
        } else sum += arr[right++];
    }
    return result;
}
solution(8, 6, [1, 2, 1, 3, 1, 1, 1, 2]);
```

# 연속 부분수열 2

N개의 수로 이루어진 수열이 주어집니다.  
이 수열에서 연속부분수열의 합이 특정숫자 M이하가 되는 경우가 몇 번 있는지 구하는 프로그 램을 작성하세요.  
만약 N=5, M=5이고 수열이 다음과 같다면  
13123  
합이 5이하가 되는 연속부분수열은 {1}, {3}, {1}, {2}, {3}, {1, 3}, {3, 1}, {1, 2}, {2, 3}, {1, 3, 1}로 총 10가지입니다.  
▣ 입력설명  
첫째 줄에 N(1≤N≤100,000), M(1≤M≤100,000,000)이 주어진다. 수열의 원소값은 1,000을 넘지 않는 자연수이다.  
▣ 출력설명  
첫째 줄에 경우의 수를 출력한다.  
▣ 입력예제  
5 5  
1 3 1 2 3  
▣ 출력예제  
10  

  
```js
function solution(N, M, arr) {
    let sum = result = left = 0;
    for (let right = 0; right < N; right++) {
        sum += arr[right];
        while (sum > M) sum -= arr[left++];
        // 배열의 숫자 조합 구하기 (1 -> 3 , 3 1 -> 1, 1 3, 1 3 1 -> ...)
        result += (right - left + 1);
    }
    return result;
}
solution(5, 5, [1, 3, 1, 2, 3]);
```