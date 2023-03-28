---
title: "React-FramerMotion 설명&예제"
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

  
애니메이션이 약간 튀는 이유는 기본값이 Spring으로 되있기 때문입니다.  

# Variants

variants는 기본적으로 애니메이션의 무대(Stage)라고 볼 수 있다.  

<iframe src="https://codesandbox.io/embed/framer-motion2-n1bdnv?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="framer-motion2"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

  
# Drag & Gestures
  
<iframe src="https://codesandbox.io/embed/wonderful-raman-sin7i7?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="wonderful-raman-sin7i7"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


# MotionValues

애니메이션 내의 수치를 트래킹할 때 필요합니다.  
MotionValue는 React Rendering Cycle을 발동시키지 않습니다. 이 말은 ReactJs State로 살아가지 않는다는 뜻입니다.   
따라서 MotionValue가 바뀌어도, 컴포넌트는 다시 랜더링되지 않습니다.  

<iframe src="https://codesandbox.io/embed/cool-dust-6gp3gu?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="cool-dust-6gp3gu"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

  

scroll 예시
  
<iframe src="https://codesandbox.io/embed/twilight-frog-u6c61o?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="twilight-frog-u6c61o"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


# AnimatePresence

- AnimatePresence는 안쪽에 나타나거나 사라지는게 있다면 그것을 animate할 수 있도록 해줍니다.
- exit는 element가 사라질 때, 어떤 animation을 발생시킬지를 정하는 것입니다.
- custom은 variants에 데이터를 보낼수 있게 해주는 property입니다.
  - variants는 원래 여러 object를 가진 object 였지만 custom을 사용하고 싶다면 variant를 object를 return하는 function으로 바꿔야 합니다.
  - motion 규칙에따라 animationPresence 에도 custom을 넣어야 합니다.


<iframe src="https://codesandbox.io/embed/focused-davinci-bp1409?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="focused-davinci-bp1409"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>



# Layout

layout prop를 element에게 주면 layout이 바뀔때 마다 알아서 animate 됩니다.  

<iframe src="https://codesandbox.io/embed/elegant-sound-zo74b5?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="elegant-sound-zo74b5"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


  

<iframe src="https://codesandbox.io/embed/amazing-rosalind-xbmgdt?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="amazing-rosalind-xbmgdt"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>




