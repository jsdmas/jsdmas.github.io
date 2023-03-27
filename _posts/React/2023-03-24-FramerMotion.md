---
title: "React-FramerMotion 설명"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-03-24
---
# Setting

[framer-motion GitHub](https://github.com/framer/motion)  
설치방법  
```
npm install framer-motion
```

일반 `div` 같은 tag는 애니메이션을 적용할 수 없다.  
대신 `motion.div`같은 평범한 HTML 태그 앞에 `motion.`을 붙여야 애니메이션이 적용된다.   

# Basic Animation
styled-components와 같이 사용할 때 아래처럼 사용한다.
```jsx
const Box = styled(motion.div)`
    width : 200px;
    height : 200px;
    background-color : red;
`;

const App = () => {
    return(
        <>
        <Box />
        </>
    )
};
```

애니메이션을 시작하는 방식을 명시하려면 `initial`이라는 Prop을 사용한다.  
initial 안에는 원하는 Element의 초기 상태를 써주면 된다.  


<iframe src="https://codesandbox.io/embed/heuristic-hill-gldp63?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="heuristic-hill-gldp63"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

# 2