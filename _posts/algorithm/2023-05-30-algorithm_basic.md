---
title:  "algorithm 기본 문제 풀이"
excerpt: "algorithm basic"
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2023-05-30
---

# 세 수 중 최솟값

100이하의 자연수 A, B, C를 입력받아 세 수 중 가장 작은 값을 출력하는 프로그램을 작성하 세요.(정렬을 사용하면 안됩니다)
  
- 입력설명  
첫 번째 줄에 100이하의 세 자연수가 입력된다.
  
- 출력설명  
첫 번째 줄에 가장 작은 수를 출력한다.
  
- 입력예제  
6 5 11
  
- 출력예제  
5

```js
function solution(a,b,c){

  let result = 0;

  if(a > b) result = b;
  else result = a;
  if(result > c) result = c;

  return result;
};
```

# 삼각형 판별하기

길이가 서로 다른 A, B, C 세 개의 막대 길이가 주어지면 이 세 막대로 삼각형을 만들 수 있 으면 “YES"를 출력하고, 만들 수 없으면 ”NO"를 출력한다.  

- 입력설명  

첫 번째 줄에 100이하의 서로 다른 A, B, C 막대의 길이가 주어진다.  

- 출력설명  
첫 번째 줄에 “YES", "NO"를 출력한다.  

- 입력예제  
6 7 11  
  
- 출력예제   
YES  

- 입력예제   
13 33 17  

- 출력예제   
NO  

```js
function solution(a,b,c){

  const sum = a + b + c;  
  let max = 0;
  
  if(a> b) max =a;
  else max = b;
  if (c > max) max = c;
  
  return max < max - sum ? "YES" : "NO";

};
```

# 연필 개수

연필 1 다스는 12자루입니다. 학생 1인당 연필을 1자루씩 나누어 준다고 할 때 N명이 학생수 를 입력하면 필요한 연필의 다스 수를 계산하는 프로그램을 작성하세요.
  
- 입력설명  

첫 번째 줄에 1000 이하의 자연수 N이 입력된다.  
- 출력설명  

첫 번째 줄에 필요한 다스 수를 출력합니다.  

- 입력예제  
25  
- 출력예제  
3  
- 입력예제  
178  
- 출력예제  
15  

```js
function solution(n){
  const studentNum = n;
  const dozen = 12;

  return Math.ceil(studentNum/dozen);

}
```

# 1부터 N까지 합 출력하기

자연수 N이 입력되면 1부터 N까지의 합을 출력하는 프로그램을 작성하세요.  

- 입력설명  
  
첫 번째 줄에 20이하의 자연수 N이 입력된다..  
- 출력설명  
  
첫 번째 줄에 1부터 N까지의 합을 출력한다.  
- 입력예제  
6  
- 출력예제  
21  
- 입력예제  
10  
- 출력예제  
55  

```js
function solution(n){
  let sum = 0;
  for(let i =1; i <= n; i++){
    sum += i;
  };
  return sum;
};
```

# 최솟값 구하기

7개의 수가 주어지면 그 숫자 중 가장 작은 수를 출력하는 프로그램을 작성하세요.  
- 입력설명  

첫 번째 줄에 7개의 수가 주어진다.  
- 출력설명  

첫 번째 줄에 가장 작은 값을 출력한다.  
- 입력예제  

5 3 7 11 2 15 17  
- 출력예제  
2

```js
function solution(arr){
  return Math.min(...arr);
};
```

# 홀수
7개의 자연수가 주어질 때, 이들 중 홀수인 자연수들을 모두 골라 그 합을 구하고, 고른 홀수들 중 최소값을 찾는 프로그램을 작성하세요.  

예를 들어, 7개의 자연수 12, 77, 38, 41, 53, 92, 85가 주어지면 이들 중 홀수는 77, 41, 53, 85이므로 그 합은
> 77 + 41 + 53 + 85 = 256

이 되고,
> 41 < 53 < 77 < 85 

이므로 홀수들 중 최소값은 41이 된다.
- 입력설명

첫 번째 줄에 자연수 7개가 주어진다. 주어지는 자연수는 100보다 작다. 홀수가 한 개 이상 반드시 존재한다.
- 출력설명


첫째 줄에 홀수들의 합을 출력하고, 둘째 줄에 홀수들 중 최소값을 출력한다.
- 입력예제 


12 77 38 41 53 92 85
- 출력예제 


256  
41

```js
function solution(arr){
  const oddArr = arr.filter(value => value % 2 !== 0);
  const sum = oddArr.reduce((acc,cur) => acc+ cur,0);
  const min = Math.min(...oddArr);
  return sum, min;
};
```

# 10부제
서울시는 6월 1일부터 교통 혼잡을 막기 위해서 자동차 10부제를 시행한다. 자동차 10부제는 자동차 번호의 일의 자리 숫자와 날짜의 일의 자리 숫자가 일치하면 해당 자동차의 운행을 금 지하는 것이다.  

예를 들어, 자동차 번호의 일의 자리 숫자가 7이면 7일, 17일, 27일에 운행하 지 못한다. 또한, 자동차 번호의 일의 자리 숫자가 0이면 10일, 20일, 30일에 운행하지 못한다.  

여러분들은 일일 경찰관이 되어 10부제를 위반하는 자동차의 대수를 세는 봉사활동을 하려고 한다. 날짜의 일의 자리 숫자가 주어지고 7대의 자동차 번호의 끝 두 자리 수가 주어졌을 때 위반하는 자동차의 대수를 출력하는 프로그램을 작성하세요.  
- 입력설명


첫 줄에는 날짜의 일의 자리 숫자가 주어지고 두 번째 줄에는 7대의 자동차 번호의 끝 두 자 리 숫자가 주어진다.

- 출력설명


주어진 날짜와 자동차의 일의 자리 숫자를 보고 10부제를 위반하는 차량의 대수를 출력합니 다.
- 입력예제

3  
25 23 11 47 53 17 33
- 출력예제

3
- 입력예제

0  
12 20 54 30 87 91 30
- 출력예제

3

```js
function solution(n,arr){
  const day = n;
  let cnt = 0;
  for(const number of arr){
    if(number % 10 === day) cnt++;
  }
  return cnt;
};
```

# 일곱 난쟁이
왕비를 피해 일곱 난쟁이들과 함께 평화롭게 생활하고 있던 백설공주에게 위기가 찾아왔다. 일과를 마치고 돌아온 난쟁이가 일곱 명이 아닌 아홉 명이었던 것이다.  
아홉 명의 난쟁이는 모두 자신이 "백설 공주와 일곱 난쟁이"의 주인공이라고 주장했다. 뛰어난 수학적 직관력을 가지고 있던 백설공주는, 다행스럽게도 일곱 난쟁이의 키의 합이 100이 됨을 기억해 냈다.  
아홉 난쟁이의 키가 주어졌을 때, 백설공주를 도와 일곱 난쟁이를 찾는 프로그램을 작성하시오.
  
- 입력설명

아홉 개의 줄에 걸쳐 난쟁이들의 키가 주어진다. 주어지는 키는 100을 넘지 않는 자연수이며, 아홉 난쟁이의 키는 모두 다르며, 가능한 정답이 여러 가지인 경우에는 아무거나 출력한다.
- 출력설명

입력된 순서대로 일곱 난쟁이의 키를 출력한다.
- 입력예제

20 7 23 19 10 15 25 8 13
- 출력예제

20 7 23 19 10 8 13

```js
function solution(arr){
  const result = [...arr];
  const total = result.reduce((acc, cur) => acc + cur, 0);
  let flag = 0;
  for(let person1 = 0; person1 < result.length; person1++){
    for(let person2 = person1 + 1; person2 < result.length; person2++){
      if(total - result[person1] + result[person2] === 100){
        result.splice(person1, 1);
        result.splice(person2, 1);
        flag = 1;
        break;
      }
    };
    if(flag) break;
  };
  return result;
};
```

# A를 #으로
대문자로 이루어진 영어단어가 입력되면 단어에 포함된 ‘A'를 모두 ’#‘으로 바꾸어 출력하는 프로그램을 작성하세요.
- 입력설명


첫 번째 줄에 문자열이 입력된다.
- 출력설명


첫 번째 줄에 바뀐 단어를 출력한다.
- 입력예제

BANANA
- 출력예제 

B#N#N#

```js
function solution(str){
  return str.replace(/A/g, "#");
};
```

# 문자 찾기
한 개의 문자열을 입력받고, 특정 문자를 입력받아 해당 특정문자가 입력받은 문자열에 몇 개 존재하는지 알아내는 프로그램을 작성하세요.  
문자열의 길이는 100을 넘지 않습니다.
- 입력설명


첫 줄에 문자열이 주어지고, 두 번째 줄에 문자가 주어진다.
- 출력설명


첫 줄에 해당 문자의 개수를 출력한다.
- 입력예제

COMPUTERPROGRAMMING  
R
- 출력예제

3

```js
function solution(str, keyword){
  let result = 0;
  for(const word of str){
    if(word === keyword) result++;
  }
  return result;
};
```

# 대문자 찾기
한 개의 문자열을 입력받아 해당 문자열에 알파벳 대문자가 몇 개 있는지 알아내는 프로그램 을 작성하세요.
- 입력설명


첫 줄에 문자열이 입력된다. 문자열의 길이는 100을 넘지 않습니다.
- 출력설명


첫 줄에 대문자의 개수를 출력한다.
- 입력예제

KoreaTimeGood
- 출력예제

3

```js
function solution(str){
  return str.match(/[A-Z]/g).length;
};
```

# 대문자로 통일

대문자와 소문자가 같이 존재하는 문자열을 입력받아 대문자로 모두 통일하여 문자열을 출력 하는 프로그램을 작성하세요.
- 입력설명


첫 줄에 문자열이 입력된다. 문자열의 길이는 100을 넘지 않습니다.
- 출력설명


첫 줄에 대문자로 통일된 문자열이 출력된다.
- 입력예제

ItisTimeToStudy
- 출력예제

ITISTIMETOSTUDY

```js
function solution(str){
  return str.toUpperCase();
}
```

# 대소문자 변환

대문자와 소문자가 같이 존재하는 문자열을 입력받아 대문자는 소문자로 소문자는 대문자로 변환하여 출력하는 프로그램을 작성하세요.
- 입력설명


첫 줄에 문자열이 입력된다. 문자열의 길이는 100을 넘지 않습니다.
- 출력설명


첫 줄에 대문자는 소문자로, 소문자는 대문자로 변환된 문자열을 출력합니다.
- 입력예제

StuDY
- 출력예제

sTUdy

```js
function solution(str){
  let result = "";
  for(const word of str){
    if(word === word.toUpperCase()) result += word.toLowerCase();
    else result += word.toUpperCase();
  }
  return result;
};
```

```js
function solution(str){
  const result = str.replace(/[a-zA-Z]/g, (value) => {
        if (value === value.toUpperCase()) {
            return value.toLowerCase();
        } else {
            return value.toUpperCase();
        }
    });
    return result;
};
```

# 가장 긴 문자열

N개의 문자열이 입력되면 그 중 가장 긴 문자열을 출력하는 프로그램을 작성하세요.
- 입력설명


첫 줄에 자연수 N이 주어진다.(3<=N<=30)  
두 번째 줄부터 N개의 문자열이 주어진다. 문자열의 길이는 100을 넘지 않습니다. 각 문자열의 길이는 서로 다릅니다.  
- 출력설명


첫 줄에 가장 긴 문자열을 출력한다.
- 입력예제

5  
teacher  
time  
student  
beautiful  
good
- 출력예제

beautiful

```js
function solution(n, arr){
  let result = "";
  for(const word of arr){
    if (word.length > result.length) result = word;
  }
  return result;
};
```

# 가운데 문자 출력

소문자로 된 단어(문자열)가 입력되면 그 단어의 가운데 문자를 출력하는 프로그램을 작성하세 요. 단 단어의 길이가 짝수일 경우 가운데 2개의 문자를 출력합니다.
- 입력설명


첫 줄에 문자열이 입력된다. 문자열의 길이는 100을 넘지 않습니다.
- 출력설명


첫 줄에 가운데 문자를 출력합니다.
- 입력예제

study
- 출력예제

u
- 입력예제

good
- 출력예제

oo

```js
function solution(str) {
  if (str.length % 2) {
      // 홀수
        const index = Math.floor(str.length / 2);
        return str[index];
    } else {
      // 짝수
        const index = Math.floor(str.length / 2);
        return str[index] + str[index - 1];
    }
};
```

# 중복문자제거

소문자로 된 한개의 문자열이 입력되면 중복된 문자를 제거하고 출력하는 프로그램을 작성하 세요.
제거된 문자열의 각 문자는 원래 문자열의 순서를 유지합니다.
- 입력설명


첫 줄에 문자열이 입력됩니다.
- 출력설명


첫 줄에 중복문자가 제거된 문자열을 출력합니다.
- 입력예제

ksekkset
- 출력예제

kset

```js
function solution(str){
  let result = "";
  for(let i = 0; i < str.length; i++){
    if(i === str.indexOf(str[i])) result += str[i];
  };
  return result;
};
```

# 중복단어제거
N개의 문자열이 입력되면 중복된 문자열은 제거하고 출력하는 프로그램을 작성하세요. 출력하는 문자열은 원래의 입력순서를 유지합니다.
- 입력설명


첫 줄에 자연수 N이 주어진다.(3<=N<=30)  
두 번째 줄부터 N개의 문자열이 주어진다. 문자열의 길이는 100을 넘지 않습니다.  
- 출력설명


첫 줄부터 중복이 제거된 문자열을 차례로 출력한다.
- 입력예제

5  
good  
time  
good  
time  
student  
- 출력예제

good  
time  
student

```js
function solution(n,arr){
  return arr.filter((value,index) => index === arr.indexOf(value));
};
```