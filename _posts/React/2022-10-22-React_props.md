---
title: "React props"
execrpt: "props 사용예시"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-11-04
---
## props
- 컴포넌트를 사용하는 부모로부터 전달받는 변수값이 포함되어 있는 객체
- 흔히 컴포넌트에게 HTML 속성 같은 형태로 전달된다.
[React_Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)  
- Props는 일종의 방식이다. 부모 컴포넌트로부터 자식 컴포넌트에 데이터를 보낼 수 있게 해주는 방법.
- prop을 전달할 떄의 이름과 받아서 사용할 떄의 이름은 동일해야 한다.
  
**간단한 예시**
```jsx
function Btn(props){
          //{banana} --> property를 오브젝트로부터 꺼내서 간단히 사용 가능


    console.log(props);
    //banana='Hello' x={8} , banana='potato' 라고 오브젝트를  출력해준다.
    return(
        <button
            style={{
                backgroundColor:'tomato',
                color:'white',
                border:0,
                borderRadius:10,
            }}
        >{props.banana} </button>
        //{banana} --> 주석에 있는데로 썻다면 props 생략가능.(대부분 이렇게 쓴다.)
    );
}

function App(){
    return(
        <div>
            <Btn banana='Hello' x={8}/>
            <Btn banana='potato'>
        </div>
    );
}

```


## Components 예시

### GradeItem

```jsx
import PropTypes from "prop-types";

const GradeItem = ({name, level, sex, kor, eng, math, sinc}) => {
    const sum = parseInt(kor + eng + math + sinc);
    const avg = parseInt(sum/4);
    return(
        <tr align="center">
            <td>{name}</td>
            <td>{level}</td>
            <td>{sex}</td>
            <td>{kor}</td>
            <td>{eng}</td>
            <td>{math}</td>
            <td>{name}</td>
            <td>{sinc}</td>
            <td>{sum}</td>
            <td>{avg}</td>
        </tr>
    );
};

// 속성들에 대한 타입 정의
GradeItem.propTypes = {
    name: PropTypes.string.isRequired,
    level: PropTypes.number.isRequired,
    sex: PropTypes.string.isRequired
};

// 속성값이 전달되지 않을 경우에 대비하여 기본값을 JSON으로 정의해 둘 수 있다.
// (defaultProps 객체이름 고정)
GradeItem.defaultProps = {
    kor:0,
    eng:0,
    math:0,
    sinc:0
};
```

## Page

### GradeTable

```jsx
import GradeItem from "../components/GradeItem";
import GradeData from "../GradeDate";

const GradeTable = () =>{
    return(
        <div>
            <h2>성적표</h2>
            <table border="1" cellpadding="7">
                <thead>
                    <tr align="center">
                        <th>이름</th>
                        <th>학년</th>
                        <th>성별</th>
                        <th>국어</th>
                        <th>영어</th>
                        <th>수학</th>
                        <th>과학</th>
                        <th>총점</th>
                        <th>평균</th>
                    </tr>
                </thead>
                <tbody>
                    {GradeDate.map((v,i)=>{
                        return(
                            <GradeItem 
                            key={i}
                            name={v.이름}
                            level={v.학년}
                            sex={v.성별}
                            kor={v.국어}
                            eng={v.영어}
                            math={v.수학}
                            sinc={v.과학}
                            />
                        );
                    })}
                </tbody>
            </table>
        </div>
    );
};
```