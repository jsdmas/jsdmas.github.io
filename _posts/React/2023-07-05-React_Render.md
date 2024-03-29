---
title: 'React에서 렌더링을 어떻게 최적화 해야하는가'
execrpt: 'React에서의 렌더링 최적화 방법'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-05
---

# 렌더링(rendering) ?

> 렌더링이란 화면에 특정한 요소를 그려내는 것을 의미합니다.

브라우저에서 렌더링이란 결국 DOM요소를 계산하고 그려내는 것을 의미합니다.  
HTML과 CSS를 통해서 만들어지고 계산된 DOM과 CSSOM은 결합되고, 위치를 계산하고, 최종적으로 브라우저에 그려집니다.

그리고 우리는 브라우저에서 제공하는 DOM API를 JavaScript를 통해 호출하면서 브라우저에 그려진 화면을 변화시킵니다.

하지만 Vanilla JavaScript를 이용해서 DOM에 직접 접근하고 수정하는 것(**명령형**), 그리고 이를 최적화 하는 것은 애플리케이션의 규모가 커지면 커질수록 관리하기 힘들어집니다.

그래서 개발자들은 애플리케이션에서 보여주고 싶은 핵심 UI를 `선언`하기만 하면 실제로 DOM을 조작해서 UI를 그려내고, 변화시키는 일은 라이브러리나 프레임워크가 대신 해주는 방식을 찾게 됩니다.(**선언적 개발**)

이런 니즈에 맞춰서 React, Vue, Angular들의 라이브러리, 프레임워크가 등장하게 되고 그 중에 React가 현재는 가장 많이 사용되고 있는 것입니다.

이처럼, React는 선언형으로 실제 렌더링 과정은 React에서 대신 처리해주고, 개발자는 UI를 설계하는대만 집중하게 해줍니다.

하지만 때로는 React 내부에서 처리해주는 렌더링을 최적화 해야 되는 상황이 발생합니다.

이러한 상황에서는 React 내부에서 렌더링이 언제 발생하는지, 어떤 과정을 거쳐서 이루어지는지를 이해하고 있어야 각 과정에서 렌더링을 최적화 할 수 있게 됩니다.

# 리액트에서 리렌더링이 되는 시점

리액트에서 리렌더링은 언제 발생할까? 좀 더 근본적으로 리액트에서 state를 왜 사용할까?

> 리액트에서 state를 사용하는 이유는 UI와 상태(state)를 연동시키기 위해서입니다.

근본적으로 UI는 어떠한 데이터가 있고 그것을 보기 편한 형태로 표현한 것입니다. 리액트는 이를 이해하고 **UI와 연동되어야 하고, 변할 여지가 있는 데이터들을 state라는 형태로 사용할 수 있게 해주었습니다.**

그리고 데이터가 변경되었을 때 UI가 그에맞춰서 변화하기 위해서 state를 변경시키는 방법을 제한시키고(setState), 이 함수가 호출 될 때 마다 리렌더링이 되도록 설계하였습니다.

이런 이유로 인해서 리액트에서 리렌더링이 발생하는 시점은 state가 변했을 때입니다. 특정 컴포넌트의 state가 변한다면, 해당 컴포넌트와 해당 컴포넌트의 하위에 있는 모든 컴포넌트들은 리렌더링이 발생하게 됩니다.

즉, `state가 변하면 해당 컴포넌트를 포함한 하위 컴포넌트들은 모두 리렌더링 된다.`라는 명확한 멘탈 모델을 이해하고 있는 것이 리액트를 이용해서 애플리케이션을 설계하고, 최적화하는데 가장 기본이 되는 사항입니다.

# 리액트의 렌더링 과정

좀 더 자세히 들어가서, 리액트에서 `렌더링`이란 한 단어로 표현되는 행위 속에서는 어떤 과정이 진행될까요?

앞서, 리액트는 state가 변화했을 때 리렌더링을 발생시킨다고 했습니다. 이 과정을 좀 더 뜯어보면 state가 변화되고 최종적으로 브라우저상의 UI에 반영되기까지 각 컴포넌트에서는 크게 아래의 4단계를 거치게 됩니다.

1. 기존 컴포넌트의 UI를 재사용할 지 확인한다.
2. 함수 컴포넌트 : 컴포넌트 함수를 호출한다 / Class 컴포넌트 : `render`메소드를 호출한다.
3. 2의 결과를 통해 새로운 VirtualDOM을 생성한다.
4. 이전의 VirtualDOM과 새로운 VirtualDOM을 비교해서 실제 변경된 부분만 DOM에 적용한다.

먼저 4번의 과정을 왜 하는지, 근본적으로 VirtualDOM을 왜 사용하는지에 대해 알아봐야 합니다.

브라우저는 근본적으로 화면을 보여주기 위해서 HTML, CSS, JavaScript를 다운로드 받고 그를 처리해서 화면에 픽셀 형태로 그려냅니다.  
그리고 이 과정을 CRP(Critical Rendering Path)라고 부릅니다.

Critical Rendering Path는 기본적으로 아래의 과정을 수행합니다.

1. HTML을 파싱해서 DOM을 만든다.
2. CSS를 파싱해서 CSSOM을 만든다.
3. DOM과 CSSOM을 결합해서 Render Tree를 만든다.
4. Render Tree와 Viewport의 width를 통해서 각 요소들의 위치와 크기를 계산한다.(**Layout**)
5. 지금까지 계산된 정보를 이용해 Render Tree상의 요소들을 실제 Pixel로 그려낸다. (**Paint**)

이후 DOM 또는 CSSOM이 수정될 때 마다 위의 과정을 반복합니다. 따라서 이 과정을 최적화 하는 것이 퍼포먼스상에 중요한 포인트입니다. 그런데 위 과정중에서 Layout, Paint 과정은 특히나 많은 계산을 필요로하는 부분입니다.

따라서 리액트는 이 CRP이 수행되는 횟수를 최적화 하기 위해서 VirtualDOM을 사용하는 것입니다.

UI를 변화하기 위해서는 많은 DOM 조작이 필요합니다. 하나하나의 DOM조작마다 CRP가 수행될 것이고 이는 곧 브라우저에게 많은 연산을 요구하게 되고, 퍼포먼스를 저하시키는 요인이 될 수 있습니다.

리액트는 이를 해결하고자 VirtualDOM이란 개념을 도입한 것입니다.

리액트에서는 UI의 변화가 발생하면 변화에 필요한 DOM조작들을 매번 바로 실제DOM에 적용하는 것이 아니라, VirtualDOM이란 리액트가 관리하고 있는 DOM과 유사한 객체형태로 만들어냅니다.

그리고 이전의 VirtualDOM과 새로운 VirtualDOM을 비교해서 실제로 변화가 필요한 DOM요소들을 찾아냅니다. 그 다음에 한번에 해당 DOM요소들을 조작합니다.

이 처리를 통해 브라우저에서 수행되는 CRP의 빈도를 줄일 수 있고 이게 VirtualDOM을 이용해서 리액트가 수행하는 최적화입니다.

즉, `4. 이전의 VirtualDOM과 새로운 VirtualDOM을 비교해서 실제 변경된 부분만 DOM에 적용한다.`에 해당하는 최적화는 리액트 내부적으로 수행하고 있다는 의미이고 이 부분은 리액트를 사용하는 개발자 입장에서는 따로 최적화를 수행할 여지가 없습니다.

리액트를 사용하는 개발자가 할 수 있는 최적화는

`1. 기존 컴포넌트의 UI를 재사용할 지 확인한다.`  
`3. 2의 결과를 통해서 새로운 VirtualDOM을 생성한다.`

위 두부분에 해당하는 최적화입니다.  
좀 더 자세히 말하자면 `1. 기존 컴포넌트의 UI를 재사용할 지 확인한다.`의 경우에는 만약 리렌더링 될 컴포넌트의 UI가 이전의 UI와 동일하다고 판단되는 경우 새롭게 컴포넌트 함수를 호출하지 않고 이전의 결과값을 그대로 사용하도록 함으로서 최적화를 수행할 수 있습니다.

또한 `3. 2의 결과를 통해서 새로운 VirtualDOM을 생성한다.`의 경우는 컴포넌트 함수가 호출되면서 만들어질 VirtualDOM의 형태를 비교적 차이가 적은 형태로 만들어지도록 하는 것입니다.

예를들어 UI를 바꾸기 위해서 `<div>`tag를 `<span>`태그로 변환시키는 것 보다는 `<div className="block" />`을 `<div className="inline" />`으로 변환시키는 것이 virtualDOM끼리 비교했을 때 차이가 적은 형태로 만들어지도록 하는 것입니다.

이 중 `기존 컴포넌트의 UI를 재사용할 지 확인한다`에 해당하는 부분은 다른 포스팅에서 다루도록 하겠습니다. (memo, useCallback 등의 memoization)
