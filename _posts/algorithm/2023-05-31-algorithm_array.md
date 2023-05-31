---
title:  "algorithm array 탐색 (2)"
excerpt: "algorithm basic"
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2023-05-31
---

# 큰 수 출력하기
N(1<=N<=100)개의 정수를 입력받아, 자신의 바로 앞 수보다 큰 수만 출력하는 프로그램을 작 성하세요.(첫 번째 수는 무조건 출력한다)
- 입력설명


첫 줄에 자연수 N이 주어지고, 그 다음 줄에 N개의 정수가 입력된다.
- 출력설명


자신의 바로 앞 수보다 큰 수만 한 줄로 출력한다.
- 입력예제

6  
7 3 9 5 6 12  
- 출력예제

7 9 6 12

```js
function solution(n, arr){
    return arr.filter(value => value > n);
};
```

# 보이는 학생
선생님이 N(1<=N<=1000)명의 학생을 일렬로 세웠습니다. 일렬로 서 있는 학생의 키가 앞에 서부터 순서대로 주어질 때, 맨 앞에 서 있는 선생님이 볼 수 있는 학생의 수를 구하는 프로그 램을 작성하세요.  
(앞에 서 있는 사람들보다 크면 보이고, 작거나 같으면 보이지 않습니다.)
- 입력설명


첫 줄에 정수 N이 입력된다. 그 다음줄에 N명의 학생의 키가 앞에서부터 순서대로 주어진다.
- 출력설명


선생님이 볼 수 있는 최대학생수를 출력한다.
- 입력예제


8  
130 135 148 140 145 150 150 153
- 출력예제

5

```js
function solution(n,arr){
    
    let result = 0;
    let max = 0;
    
    for(let student = 0; student < arr.length; student++){
        if(arr[student] > max) {
            max = arr[stundent];
            result++:
        }
    };
    return result;
};
```

# 가위 바위 보
A, B 두 사람이 가위바위보 게임을 합니다. 총 N번의 게임을 하여 A가 이기면 A를 출력하고, B가 이기면 B를 출력합니다. 비길 경우에는 D를 출력합니다.  
가위, 바위, 보의 정보는 1:가위, 2:바위, 3:보로 정하겠습니다.  
예를 들어 N=5이면  

| 회수     | 1   | 2   | 3   | 4   | 5   |
| -------- | --- | --- | --- | --- | --- |
| A의 정보 | 2   | 3   | 3   | 1   | 3   |
| B의 정보 | 1   | 1   | 2   | 2   | 3   |
| 승자     | A   | B   | A   | B   | D   |


두 사람의 각 회의 가위, 바위, 보 정보가 주어지면 각 회를 누가 이겼는지 출력하는 프로그램 을 작성하세요.  
- 입력설명


첫 번째 줄에 게임 횟수인 자연수 N(1<=N<=100)이 주어집니다. 두 번째 줄에는 A가 낸 가위, 바위, 보 정보가 N개 주어집니다. 세 번째 줄에는 B가 낸 가위, 바위, 보 정보가 N개 주어집니다.
- 출력설명


각 줄에 각 회의 승자를 출력합니다. 비겼을 경우는 D를 출력합니다.
- 입력예제

5  
23313  
11223
- 출력예제

A  
B  
A  
B  
D

```js
function solution(n,arrA,arrrB){
    const result = [];
    for(let game = 0; game < n; game++){
        // A가 이기는 경우를 기준으로 삼았습니다.
        if(arrA[game] === 1 && arrB[game] === 3) result.push("A");
        if(arrA[game] === 2 && arrB[game] === 1) result.push("A");
        if(arrA[game] === 3 && arrB[game] === 1) result.push("A");
        if(arrA[game] === arrB[game]) result.push("D");
        else result.push("B");;
    };
    return result;
};
```

# 점수계산
OX 문제는 맞거나 틀린 두 경우의 답을 가지는 문제를 말한다. 여러 개의 OX 문제로 만들어진 시험에서 연속적으로 답을 맞히는 경우에는 가산점을 주기 위해서 다음과 같이 점수 계산을 하기 로 하였다. 1번 문제가 맞는 경우에는 1점으로 계산한다. 앞의 문제에 대해서는 답을 틀리다가 답이 맞는 처음 문제는 1점으로 계산한다. 또한, 연속으로 문제의 답이 맞는 경우에서 두 번째 문제는 2점, 세 번째 문제는 3점, ..., K번째 문제는 K점으로 계산한다. 틀린 문제는 0점으로 계산한다.  
예를 들어, 아래와 같이 10 개의 OX 문제에서 답이 맞은 문제의 경우에는 1로 표시하고, 틀린 경 우에는 0으로 표시하였을 때, 점수 계산은 아래 표와 같이 계산되어, 총 점수는 1+1+2+3+1+2=10 점이다.  
> 1 0 1 1 1 0 0 1 1 0

| 채점 | 1   | 0   | 1   | 1   | 1   | 0   | 0   | 1   | 1   | 0   |
| ---- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 점수 | 1   | 0   | 1   | 2   | 3   | 0   | 0   | 1   | 2   | 0   |

시험문제의 채점 결과가 주어졌을 때, 총 점수를 계산하는 프로그램을 작성하시오.  
- 입력설명


첫째 줄에 문제의 개수 N (1 ≤ N ≤ 100)이 주어진다. 둘째 줄에는 N개 문제의 채점 결과를 나 타내는 0 혹은 1이 빈 칸을 사이에 두고 주어진다. 0은 문제의 답이 틀린 경우이고, 1은 문제의 답이 맞는 경우이다.
- 출력설명


첫째 줄에 입력에서 주어진 채점 결과에 대하여 가산점을 고려한 총 점수를 출력한다.
- 입력예제


10  
1011100110
- 출력예제

10

```js
function solution(n, arr){
    let result = 0;
    let cnt = 0;
    for(const number of arr){
        if(number) cnt++;
        else {
            result += cnt;
            cnt = 0;
        };
    };
    return result;
};
```

# 등수구하기

N(1<=N<=100)명의 학생의 국어점수가 입력되면 각 학생의 등수를 입력된 순서대로 출력하는 프로그램을 작성하세요.

- 입력설명

첫 줄에 N(3<=N<=1000)이 입력되고, 두 번째 줄에 국어점수를 의미하는 N개의 정수가 입력 된다. 같은 점수가 입력될 경우 높은 등수로 동일 처리한다. 즉 가장 높은 점수가 92점인데 92점이 3명 존재하면 1등이 3명이고 그 다음 학생은 4등이 된다.
- 출력설명

입력된 순서대로 등수를 출력한다.
- 입력예제

5  
87 89 92 100 76
- 출력예제

4 3 2 1 5

```js
function solution(n, arr){
    const rankArr = Array.from({length:n}, ()=> 1);
    for(let testScore1 = 0; testScore1 < arr.length, testScore1++){
        for(let testScore2 = 0; testScore2 < arr.length, testScore2++){
            // 자기 자신을 제외하고 전부 조회
            if(arr[testScore1] < arr[testScore2]) {
                rankArr[testScore1]++;
            }
        }
    };
    return rankArr;
};
```

# 격자판 최대합
5*5 격자판에 아래롸 같이 숫자가 적혀있습니다.
![img](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/0d96a774-873e-471a-9166-0a45a7ca2ee0)  

N*N의 격자판이 주어지면 각 행의 합, 각 열의 합, 두 대각선의 합 중 가 장 큰 합을 출력합 니다.  
- 입력설명


첫 줄에 자연수 N이 주어진다.(1<=N<=50)  
두 번째 줄부터 N줄에 걸쳐 각 줄에 N개의 자연수가 주어진다. 각 자연수는 100을 넘지 않는 다.
- 출력설명

최대합을 출력합니다.
- 입력예제


5  
10 13 10 12 15  
12 39 30 23 11  
11 25 50 53 15  
19 27 29 37 27  
19 13 30 13 19

- 출력예제

155

```js
function solution(n, arr){
    
    let max = 0;
    let row = col = 0;

    for(let i = 0; i < arr.length ; i++){
        for(let j = 0; j < arr.length; j++){
            row += arr[i][j];
            col += arr[j][i];
        };
        max = Math.max(max, row, col);
    };

    row = col = 0;

    for(let i = 0; i < arr.length; i++){
        row += arr[i][i];
        col += arr[i][arr.length -1 - i];
    };
    max = Math.max(max, row, col);
    
    return max;

};
```

# 봉우리
지도 정보가 N*N 격자판에 주어집니다. 각 격자에는 그 지역의 높이가 쓰여있습니다. 각 격자 판의 숫자 중 자신의 상하좌우 숫자보다 큰 숫자는 봉우리 지역입니다. 봉우리 지역이 몇 개 있는 지 알아내는 프로그램을 작성하세요.  
격자의 가장자리는 0으로 초기화 되었다고 가정한다.  
만약 N=5 이고, 격자판의 숫자가 다음과 같다면 봉우리의 개수는 10개입니다.  
![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/87c3fe29-2f0d-404d-8f0b-c900d49269e9)  

- 입력설명


첫 줄에 자연수 N이 주어진다.(1<=N<=50)  
두 번째 줄부터 N줄에 걸쳐 각 줄에 N개의 자연수가 주어진다. 각 자연수는 100을 넘지 않는 다.
- 출력설명


봉우리의 개수를 출력하세요.
- 입력예제

5  
5 3 7 2 3  
3 7 1 6 1   
7 2 5 3 4  
4 3 6 4 1  
8 7 3 5 2
- 출력예제

10


```js
function solution(n, arr){
    let result = 0;
    // 좌표상 한 위치를 기준으로 비교하는 값들을 생각하고 정합니다.
    const compareX = [-1, 0, 1, 0];
    const compareY = [0, 1, 0, -1];

    // 배열탐색
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length; j++) {
            let flag = 1;
            // 봉우리 비교 시작
            for(let k = 0; k < 4; k++){
                const nx = i + compareX[k];
                const ny = j + compareY[k];
                // 주변에 더 큰 값이 있다면 거짓
                // 비교값이 배열을 벗어나는 경우도 거짓
                if(arr[nx][ny] >= arr[i][j] && nx >=0 && ny >= 0 nx < arr.length && ny < arr.length){
                    flag = 0;
                    break;
                }
            };
            if(flag) result++;
        };
    };
    return result;
};
```
