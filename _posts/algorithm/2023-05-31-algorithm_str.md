---
title:  "algorithm 문자열 탐색 (3)"
excerpt: "algorithm basic"
toc: true
toc_sticky: true
categories:
  - algorithm
tags:
  - algorithm
last_modified_at: 2023-05-31
---

# 회문 문자열

앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 회문 문자열이라고 합니다.  
문자열이 입력되면 해당 문자열이 회문 문자열이면 "YES", 회문 문자열이 아니면 “NO"를 출력 하는 프로그램을 작성하세요.  
단 회문을 검사할 때 대소문자를 구분하지 않습니다.  
- 입력설명


첫 줄에 정수 길이 100을 넘지 않는 공백이 없는 문자열이 주어집니다.
- 출력설명


첫 번째 줄에 회문 문자열인지의 결과를 YES 또는 NO로 출력합니다.
- 입력예제

gooG
- 출력예제

YES

```js
function solution(str){
    return str.toLowerCase()[0] === str.toLowerCase()[str.length -1] ? "YES" : "NO";
};
```

# 유효한 팰린드롬
앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 팰린드롬이라고 합니다.  
문자열이 입력되면 해당 문자열이 팰린드롬이면 "YES", 아니면 “NO"를 출력하는 프로그램을 작성하세요.  
단 회문을 검사할 때 알파벳만 가지고 회문을 검사하며, 대소문자를 구분하지 않습니다. 알파벳 이외의 문자들의 무시합니다.
- 입력설명


첫 줄에 정수 길이 100을 넘지 않는 공백이 없는 문자열이 주어집니다.
- 출력설명


첫 번째 줄에 팰린드롬인지의 결과를 YES 또는 NO로 출력합니다.
- 입력예제


"found7, time: study; Yduts; emit, 7Dnuof"
- 출력예제

YES

```js
function solution(str){
    // 소문자 치환 뒤 영어 소문자 외의 문자들은 날려준다.
    const word = str.toLowerCase().replace(/[^a-z]/g, "");
    // 뒤집은 문자와 원래 문자가 같는지 검사
    return word === word.split("").reverse().join("") ? "YES" : "NO";
};
```

# 숫자만 추출

문자와 숫자가 섞여있는 문자열이 주어지면 그 중 숫자만 추출하여 그 순서대로 자연수를 만 듭니다.  
만약 “tge0a1h205er”에서 숫자만 추출하면 0, 1, 2, 0, 5이고 이것을 자연수를 만들면 1205 이 됩니다.  
추출하여 만들어지는 자연수는 100,000,000을 넘지 않습니다.
- 입력설명


첫 줄에 숫자가 섞인 문자열이 주어집니다. 문자열의 길이는 50을 넘지 않습니다.
- 출력설명


첫 줄에 자연수를 출력합니다.
- 입력예제

g0en2T0s8eSoft
- 출력예제

208

```js
function solution(str){
    const number = str.replace(/[^0-9]/g, "");
    return parsInt(number);
};
```
```js
function solution(str){
    let result = "";
    for(const word of str){
        if(!isNaN(word)) result += word;
    };
    return +result;
};
```


# 가장 짧은 문자거리

한 개의 문자열 s와 문자 t가 주어지면 문자열 s의 각 문자가 문자 t와 떨어진 최소거리를 출 력하는 프로그램을 작성하세요.
- 입력설명


첫 번째 줄에 문자열 s와 문자 t가 주어진다. 문자열과 문자는 소문자로만 주어집니다. 문자열의 길이는 100을 넘지 않는다.
- 출력설명


첫 번째 줄에 각 문자열 s의 각 문자가 문자 t와 떨어진 거리를 순서대로 출력한다.
- 입력예제

teachermode e
- 출력예제

10121012210

```js
function solution(s,t){
    const result = [];
    let cnt = 0;
    for(let i = 0; i < s.length; i++){
        if(s[i] !== t) {
            cnt++;
            result.push(cnt);
        }else{
            cnt = 0;
            result.push(cnt);
        }
    };
    cnt = 0;
    for(let i = s.length -1; i >= 0; i--){
       if(result[i] > cnt){
        cnt++;
        result[i] = cnt;
       }else cnt = 0;
    };
    return result;
};
```

# 문자열 압축

알파벳 대문자로 이루어진 문자열을 입력받아 같은 문자가 연속으로 반복되는 경우 반복되는 문자 바로 오른쪽에 반복 횟수를 표기하는 방법으로 문자열을 압축하는 프로그램을 작성하시 오. 단 반복횟수가 1인 경우 생략합니다.

- 입력설명

첫 줄에 문자열이 주어진다. 문자열의 길이는 100을 넘지 않는다.
- 출력설명

첫 줄에 압축된 문자열을 출력한다.
- 입력예제

KKHSSSSSSSE
- 출력예제

K2HS7E

```js
function solution(str){
    const word = str + " ";
    let result = "";
    let cnt = 1;
    for(let i = 0; i < word.length; i++){
        if(word[i] === word[i+1]) cnt++;
        else{
            result += word[i];
            if(cnt > 1) result += cnt + "";
            cnt = 1; 
        }
    };
    return result;
};
```