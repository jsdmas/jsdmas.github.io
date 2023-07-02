---
title: 'Sliding Window & Hash Map algorithm (6)'
excerpt: 'Sliding Window & Hash Map'
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2023-07-02
---

# 최대 매출 (Sliding Window)

현수의 아빠는 제과점을 운영합니다. 현수 아빠는 현수에게 N일 동안의 매출기록을 주고 연속 된 K일 동안의 최대 매출액이 얼마인지 구하라고 했습니다.  
만약 N=10이고 10일 간의 매출기록이 아래와 같습니다.  
이때 K=3이면  
12 15 11 20 25 10 20 19 13 15  
연속된 3일간의 최대 매출액은 11+20+25=56만원입니다. 여러분이 현수를 도와주세요.  
▣ 입력설명  
첫 줄에 N(5<=N<=100,000)과 M(2<=K<=N)가 주어집니다.  
두 번째 줄에 N개의 숫자열이 주어집니다. 각 숫자는 500이하의 음이 아닌 정수입니다.  
▣ 출력설명  
첫 줄에 최대 매출액을 출력합니다.  
▣ 입력예제  
10 3  
12 15 11 20 25 10 20 19 13 15  
▣ 출력예제  
56

```js
function solution(N, K, arr) {
  let max = Number.MIN_SAFE_INTEGER;
  let sum = (left = 0);
  for (let right = 0; right < N; right++) {
    if (right < K) {
      sum += arr[right];
    } else sum += arr[right] - arr[left++];
    max = Math.max(max, sum);
  }
  return max;
}
solution(10, 3, [12, 15, 11, 20, 25, 10, 20, 19, 13, 15]);
```

# 학급 회장 (Hash Map)

학급 회장을 뽑는데 후보로 기호 A, B, C, D, E 후보가 등록을 했습니다.  
투표용지에는 반 학생들이 자기가 선택한 후보의 기호(알파벳)가 쓰여져 있으며 선생님은 그 기호를 발표하고 있습니다.  
선생님의 발표가 끝난 후 어떤 기호의 후보가 학급 회장이 되었는지 출력하는 프로그램을 작 성하세요.  
반드시 한 명의 학급회장이 선출되도록 투표결과가 나왔다고 가정합니다.  
▣ 입력설명  
첫 줄에는 반 학생수 N(5<=N<=50)이 주어집니다.  
두 번째 줄에 N개의 투표용지에 쓰여져 있던 각 후보의 기호가 선생님이 발표한 순서대로 문자열로 입력됩니다.  
▣ 출력설명  
학급 회장으로 선택된 기호를 출력합니다.  
▣ 입력예제  
15  
BACBACCACCBDEDE  
▣ 출력예제  
C

```js
function solution(N, str) {
  const map = new Map();
  let student = null;
  let max = Number.MIN_SAFE_INTEGER;
  for (const vote of str) {
    if (!map.has(vote)) map.set(vote, 1);
    else map.set(vote, map.get(vote) + 1);
  }
  for (const [key, value] of map) {
    if (value > max) {
      max = value;
      student = key;
    }
  }
  return student;
}
solution(15, 'BACBACCACCBDEDE');
```

# 아나그램 (Hash Map)

Anagram이란 두 문자열이 알파벳의 나열 순서를 다르지만 그 구성이 일치하면 두 단어는 아 나그램이라고 합니다.  
예를 들면 AbaAeCe 와 baeeACA 는 알파벳을 나열 순서는 다르지만 그 구성을 살펴보면 A(2), a(1), b(1), C(1), e(2)로 알파벳과 그 개수가 모두 일치합니다. 즉 어느 한 단어를 재 배열하면 상대편 단어가 될 수 있는 것을 아나그램이라 합니다.  
길이가 같은 두 개의 단어가 주어지면 두 단어가 아나그램인지 판별하는 프로그램을 작성하세 요.  
아나그램 판별시 대소문자가 구분됩니다.  
▣ 입력설명  
첫 줄에 첫 번째 단어가 입력되고, 두 번째 줄에 두 번째 단어가 입력됩니다. 단어의 길이는 100을 넘지 않습니다.  
▣ 출력설명  
두 단어가 아나그램이면 “YES"를 출력하고, 아니면 ”NO"를 출력합니다.  
▣ 입력예제 1  
AbaAeCe  
baeeACA  
▣ 출력예제 1  
YES  
▣ 입력예제 2  
abaCC  
Caaab  
▣ 출력예제 2  
NO

```js
function solution(str1, str2) {
  const map = new Map();
  // 초기값 세팅
  for (const keyword of str1) {
    if (!map.has(keyword)) map.set(keyword, 1);
    else map.set(keyword, map.get(keyword) + 1);
  }
  // str2와 str1map 비교하기
  for (const keyword of str2) {
    // 단어를 가지고있지 않거나 가진 단어의 개수가 0인경우
    if (!map.has(keyword) || map.get(keyword) === 0) return 'NO';
    else map.set(keyword, map.get(keyword) - 1);
  }
  return 'YES';
}

solution('AbaAeCe', 'baeeACA');
```

# 모든 아나그램 찾기 (Two Pointer & Sliding Window, Hash Map)

S문자열에서 T문자열과 아나그램이 되는 S의 부분문자열의 개수를 구하는 프로그램을 작성하 세요.  
아나그램 판별시 대소문자가 구분됩니다. 부분문자열은 연속된 문자열이어야 합니다.

▣ 입력설명  
첫 줄에 첫 번째 S문자열이 입력되고, 두 번째 줄에 T문자열이 입력됩니다.  
S문자열의 길이는 10,000을 넘지 않으며, T문자열은 S문자열보다 길이가 작거나 같습니다.  
▣ 출력설명  
S단어에 T문자열과 아나그램이 되는 부분문자열의 개수를 출력합니다.  
▣ 입력예제 1  
bacaAacba  
abc  
▣ 출력예제 1  
3

출력설명: {bac}, {acb}, {cba} 3개의 부분문자열이 "abc"문자열과 아나그램입니다.

```js
function solution(S, T) {
  let answer = 0;
  let tH = new Map();
  let sH = new Map();
  // 아나그램 판별 문자열 map 자료구조에 담기
  for (const word of T) {
    // 이미 존재하는 단어의 경우 개수를 +1 해준다
    if (tH.has(word)) tH.set(word, tH.get(word) + 1);
    tH.set(word, 1);
  }

  // 검사할 문자열(S)에서 판별문자열(T)의 길이를 비교하기위해 초기값 세팅
  const startLength = T.length - 1;
  for (let i = 0; i < startLength; i++) {
    if (sH.has(S[i])) sH.set(S[i], sH.get(S[i]) + 1);
    else sH.set(S[i], 1);
  }

  let left = 0;
  for (let right = startLength; right < S.length; right++) {
    if (sH.has(S[right])) sH.set(S[right], sH.get(S[right]) + 1);
    else sH.set(S[right], 1);
    if (compareMaps(sH, tH)) answer++;
    // 투포인터와 슬라이딩 윈도우로 문자열을 오른쪽으로 이동합니다
    // 왼쪽 값을 하나 지우고 left값 증가
    sH.set(S[left], sH.get(S[left]) - 1);
    // 만약 key값이 0일경우 다른값이 와야할 공간을 만들어야 하기에 delete 합니다.
    if (sH.get(S[left]) === 0) sH.delete(S[left]);
    left++;
  }
  return answer;
}

function compareMaps(sHashMap, tHashMap) {
  // hashMap의 key 비교
  if (sHashMap.size !== tHashMap.size) return false;
  for (let [key, value] of sHashMap) {
    // 키의 값이 존재하는지 판별, value값이 같은지 판별
    if (!tHashMap.has(key) || tHashMap.get(key) !== value) return false;
  }

  return true;
}

solution('bacaAacba', 'abc');
```
