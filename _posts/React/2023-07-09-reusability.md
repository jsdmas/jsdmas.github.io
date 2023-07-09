---
title: '재사용성이 높은 컴포넌트 만들기'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-09
---

# 재사용성이 높은 컴포넌트

컴포넌트가 재사용성이 높아질수록 중복된 코드를 줄여주고 일관성 있는 뷰를 만드는데 큰 도움이 됩니다.  
재사용하기 쉬운 컴포넌트는 추상성이 높으며 일반적으로 아래와 같은 틀징을 지닙니다.

1. 하나의 역할만 담당한다. (단일 책임 원칙)
2. 순수하다. -> 같은 props 가 주입되면 항상 같은 결과를 렌더링하며 `side Effect`가 없거나 적다.
3. 네이밍에 여러 문맥이 포함되어 있지 않다.
   - 예를 들어, `BankAccountDropdownList` 라는 네이밍을 가진 컴포넌트는 `은행 계좌`라는 문맥이 담긴 컴포넌트에서만 사용할 수 있습니다. 그러나 `DropdownList` 란 네이밍을 가진 컴포넌트는 드롭다운이 필요한 모든 컴포넌트에서 사용할 수 있습니다.

# 추상적인 이벤트 핸들러를 prop 으로 넘기기

**Field.tsx**

```tsx
type Props = {
  name: string;
  value: string;
  setFormValues?: React.Dispatch<React.SetStateAction<Form>>;
};

export default function Field({ name, value, setFormValues }: Props) {
  return (
    <input
      name={name}
      value={value}
      onChange={(event) => {
        setFormValues?.((values) => ({
          ...values,
          [name]: event.target.value,
        }));
      }}
    />
  );
}
```

**Form.tsx**

```tsx
export default function Form() {
  const [formValues, setFormValues] = useState<Form>({
    id: '',
    email: '',
  });

  return (
    <form>
      <Field name='id' value={formValues.id} setFormValues={setFormValues} />
      <Field name='email' value={formValues.email} setFormValues={setFormValues} />
    </form>
  );
}
```

이 Field 컴포넌트는 잘 동작하지만, 다음과 같은 면에서 어색하게 느껴집니다.

- Field 는 Form 이란 문맥과 관련이 없는 컴포넌트입니다. 그러나, 현재는 setFormValues 때문에 Form 이란 문맥과 강하게 결합되어 있습니다.
- 이 때문에 만약 Field 컴포넌트의 change 이벤트를 구독하기 위해선 setFormValues 가 필요합니다. formValues 상태를 사용하지 않는 컴포넌트에선 Field 컴포넌트와 유사한 UI 를 가졌음에도 사용할 수 없습니다.

위의 코드의 문제점을 고치면 아래와 같습니다.

**Field.tsx**

```tsx
type Props = {
  name: string;
  value: string;
  onChange?: React.ChangeEventHandler<HTMLInputElement>;
  // 또는 onChange?: (event: React.ChangeEvent<HTMLInputElement>) => void;
};

export default function Field({ name, value, onChange }: Props) {
  return (
    <input
      name={name}
      value={value}
      onChange={(event) => {
        onChange?.(event);
      }}
    />
  );
}
```

**Form.tsx**

```tsx
export default function Form() {
  const [formValues, setFormValues] = useState<Form>({
    id: '',
    email: '',
  });

  return (
    <form>
      <Field
        name='id'
        value={formValues.id}
        onChange={(event) => {
          setFormValues((prevValues) => ({
            ...prevValues,
            id: event.target.value,
          }));
        }}
      />
      <Field
        name='email'
        value={formValues.email}
        onChange={(event) => {
          setFormValues((prevValues) => ({
            ...prevValues,
            email: event.target.value,
          }));
        }}
      />
    </form>
  );
}
```

전자와 달리 `onChange`라는 콜백 함수를 인자로 받고 있습니다.

이젠 Filed 컴포넌트는 `Form`의 존재를 몰라도 되고, Field 컴포넌트를 사용하는 측에서 change 이벤트를 구독해야 할 필요가 생긴 경우 `onChange`함수를 prop 으로 넘겨주면 됩니다.

> 컴포넌트의 재사용성을 높이려면 이벤트 핸들러로 추상적인 콜백 함술르 받아야 합니다.

# Wrapping 컴포넌트의 props 확장하기

## DOM Element 를 Wrapping 한 컴포넌트

`@types/react`는 이런 경우 사용하기 좋은 타입을 미리 정의해두었습니다.

바로 `HTMLAttributes`와 `~~~Element`입니다.

- `HTMLAttributes<T>`
  - `HTMLAttributes<T>`는 Element 타입을 받을 수 있는 generic 인자를 받으며, 해당 Element 가 가질 수 있는 속성들을 반환합니다. 아래와 같은 타입들의 조합으로 만들어집니다.
    - `Element 들이 공통적으로 가지는 속성들`
    - `접근성을 위한 ARIA 속성들`
    - `generic 인자로 받은 Element 자신의 속성들`
  - `~~~Element`
    - `types/react`의 global.d.ts 엔 다양한 Element 에 대한 타입이 있습니다.
    - 예를 들어, `<input>`태그의 타입을 원한다면 `HTMLInputElement`를 참조하면 브라우저에서 지원하는 HTMLInputElement 의 속성에 접근할 수 있습니다.
    - 그러나, 이를 단독으로 사용하기엔 React 에서 사용할 수 있는 속성과 약간 거리가 있습니다. 이는 아래에서 설명합니다.
  - `HTMLAttributes<~~~Element>`
    - input 엘리먼트의 change 이벤트를 구독하기 위해선 리액트에서는 `onChange` prop을 이용하지만, `HTMLInputElement`는 브라우저에서 지원하는 `onChange`속성을 담고 있습니다.
    - 이를 해소하기 위해선 HTMLAttributes 로 HTMLInputElement 를 Wrapping 해줘야 합니다.

Button 컴포넌트는 이미 `HTMLAttributes<HTMLButtonElement>`에 정의된 다양한 타입이 있습니다. 따라서, 다음과 같이 타이핑을 할 수 있습니다.

**Button.tsx**

```tsx
type Props = React.HTMLAttributes<HTMLButtonElement>;

export default function Button(props: Props) {
  return <button {...props} />;
}
```

위 코드는 기존에 `<button>`태그가 하는 역할을 완벽히 동일하게 수행합니다.  
이번엔 Button 컴포넌트가 커스텀 한 `shadow` prop 을 받는다고 가정해봅시다. 이런 경우, 다음과 같이 타입을 정의할 수 있습니다.

```tsx
type Props = React.HTMLAttributes<HTMLButtonElement> & CustomProps;

type CustomProps = {
  shadow?: boolean;
};

export default function Button({ shadow, ...props }: Props) {
  return <button {...props} style={shadow ? { filter: 'drop-shadow(5px 5px 10px #000)' } : {}} />;
}
```

이 또한 문제 없이 잘 동작합니다.  
그러나, 만약 CustomProps 내부에 React.HTMLAttributes 와 중복되는 property key 가 있으면 얘기가 조금 달라집니다. 이는 다음 섹션에서 얘기해보겠습니다.

## 이미 존재하는 컴포넌트를 Wrapping 한 컴포넌트

앞선 섹션과 마찬가지로, `@types/react`는 이런 경우에도 사용하기 좋은 타입을 미리 정의해두었습니다.  
`ComponentProps`, `ComponentPropsWithoutRef`, `ComponentPropsWithRef`가 그 주인공입니다.

- `ComponentProps<T>`
  - `ComponentProps<T>`는 컴포넌트의 타입을 Generic 인자로 받으며, 해당 컴포넌트가 갖는 props를 타입으로 갖습니다.
  - 다만, `ComponentProps<T>`는 몇몇 상황에서 예측하지 못한 버그가 있는 것 같습니다.
  - 주석에서도 ComponentProps 보단 ComponentPropsWithRef, ComponentPropsWithoutRef 를 사용하길 권장하고 있습니다.
- `ComponentPropsWithRef<T>`
  - `ComponentPropsWithRef<T>` 는 컴포넌트가 Class 기반 컴포넌트이거나, forwardRef 등으로 Wrapping 된 컴포넌트일 경우 해당 컴포넌트의 Props 를 타입으로 가질 수 있도록 해줍니다.
- `ComponentPropsWithoutRef<T>`
  - `ComponentPropsWithoutRef<T>` 는 컴포넌트가 Ref 를 갖는다면 이를 제외한 Props 를 타입으로 가지며, 만약 Ref 를 갖지 않는다면 해당 컴포넌트가 갖는 Props 를 타입으로 갖습니다.

앞선 `<Button>` 컴포넌트를 Wrapping 하는 `<WrappedButton>` 컴포넌트를 만들어봅시다.

```tsx
type Props = React.ComponentPropsWithoutRef<typeof Button>;

function WrappedButton(props: Props) {
  return <Button {...props} />;
}
```

위 컴포넌트도 기존의 `<Button>` 컴포넌트가 하는 역할을 완벽히 동일하게 수행합니다!

여기서 `<WrappedButton>` 의 onClick 이벤트 핸들러가 기존과 다른 인자를 받는다고 가정해봅시다.

React 의 onClick 함수는 `React.MouseEvent<T>` 타입의 이벤트를 받아 void 를 반환하는 함수입니다. 이 함수를 `string` 을 받아 void 를 받도록 수정해보죠. 아까와 비슷하게 다음과 같이 수정하면 되지 않을까요?

```tsx
type Props = React.ComponentPropsWithoutRef<typeof Button> & {
  onClick: (data: string) => void;
};

function WrappedButton(props: Props) {
  return <Button {...props} />;
}
```

![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/eceaa541-d8f6-403e-a9e4-2066d502882c)

onClick 의 data 가 string 으로 추론되어야 할 것 같은데, `string | React.MouseEvent<HTMLButtonElement, MouseEvent> `로 추론됩니다!

이는 `& (intersection)` 의 성질 때문인데요. 함수와 함수를 intersection 하는 경우, 더 좁은 타입으로 만드는 것이 불가능하므로 overload 하게 되는 것입니다.

이를 해소하기 위해선 아래와 같이 `Omit` 연산자를 사용해 기존 타입에서 제거를 한 후, intersection 해주어야 합니다.

```tsx
// - type Props = React.ComponentPropsWithoutRef<typeof Button> & {
type Props = Omit<React.ComponentPropsWithoutRef<typeof Button>, 'onClick'> & {
  onClick: (data: string) => void;
};

function WrappedButton(props: Props) {
  return <Button {...props} />;
}
```

Wrapping 하는 최상위 DOM, Component 의 이미 존재하는 props 를 함께 받아 보다 일반적이고 재사용성이 높은 컴포넌트를 만들 수 있습니다.

- DOM Element 를 Wrapping 한 컴포넌트
  - HTMLAttributes, ~~~Element 를 사용합니다.
  - FYI) ComponentPropsWithoutRef<'input'> 으로 같은 기능을 수행할 수 있습니다.
- 이미 존재하는 컴포넌트를 Wrapping 한 컴포넌트
  - `ComponentPropsWithoutRef`, `ComponentPropsWithRef` 를 사용합니다.
- 두 케이스 모두 함수를 재정의해야 하는 경우, `Omit` Utility Type 으로 정의된 타입을 제거하고 다시 intersection 해야 합니다.

# ReactNode 를 porp 으로 받기

## childern prop 을 사용하기

다음과 같은 요구조건이 있다고 가정 해보겠습니다.

- Main, Project 페이지를 구현해야 합니다.
- 각 페이지들은 공통적으로 Header, Footer 를 사용합니다.

아마 아래와 같은 구조의 컴포넌트가 만들어질 것입니다.

**MainPage.tsx**

```tsx
export default function MainPage() {
  return (
    <Background>
      <Header />

      {/* Main Page Logic */}

      <Footer />
    </Background>
  );
}
```

**ProjectPage.tsx**

```tsx
export default function ProjectPage() {
  return (
    <Background>
      <Header />

      {/* Project Page Logic */}

      <Footer />
    </Background>
  );
}
```

`Background, Header, Footer` 컴포넌트가 공통적으로 사용되고 있습니다. 이를 하나로 묶어주는 `Layout` 이란 컴포넌트로 추상화 할 수 있습니다.

**Layout.tsx**

```tsx
type Props = { childern?: React.ReacNode };
export default function Layout({ childern }: Props) {
  return (
    <Background>
      <Header />

      {childern}

      <Footer />
    </Background>
  );
}
```

**MainPage.tsx**

```tsx
export default function MainPage() {
  return <Layout>{/* Main Page Logic */}</Layout>;
}
```

**ProjectPage.tsx**

```tsx
export default function ProjectPage() {
  return <Layout>{/* Project Page Logic */}</Layout>;
}
```

### children?: React.ReactNode 의미

`@types/react`는 React element 의 타이핑을 위한 `JSX.Element`, `ReactElement`, `ReactNode`타입을 지원합니다.  
각 타입의 정의는 다음과 같습니다.

```tsx
// JSX.Element
namespace JSX {
  interface Element extends React.ReactElement<any, any> {}
  // ...
}

// ReactElement
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
  type: T;
  props: P;
  key: Key | null;
}

// ReactNode
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;
type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```

- `JSX.Element`: JSX 표현식을 위한 타입입니다.
- `ReactElement`: 사실상 JSX.Element 와 동일합니다. 컴포넌트, createElement, cloneElement 등에 의해 렌더링 되어지는 값의 타입이지만 null 을 포함하고 있진 않습니다.
- `ReactNode`: JSX 내부에서 렌더링 할 수 있는 모든 대상입니다.

컴포넌트를 더 추상적으로 만들기 위해선 더 많은 타입을 수용할 수 있는 `ReactNode` 를 사용해야 합니다.

예를 들어, `children: React.ReactElement` 로 타입을 정의했다면 string 을 받을 수 없고, 오로지 JSX 형식으로 children prop 에 넘겨줘야 합니다. ReactNode 는 ReactText 를 포함하는 타입이므로, 이를 수용할 수 있습니다.

한편, `children` prop 이 `undefined` 인 경우 넘어오는 property 자체가 없으므로 optional 하게 타입을 정의합니다.

### children prop 만 ReactNode 를 받을 수 있는 것은 아니다

Layout 컴포넌트를 조금 더 추상화시켜 보겠습니다.

만약, 디자이너가 Project 페이지에선 500px 이상 스크롤 할 시에 배경색이 변하도록 요청했다고 가정해봅시다.

다음과 같이 수정할 수 있어 보입니다.

**Layout.tsx**

```tsx
type Props = {
  shouldChangeHeaderBgColor: boolean;
  children?: React.ReactNode;
};

export default function Layout({ shouldChangeHeaderBgColor, children }: Props) {
  return (
    <Background>
      <Header style={shouldChangeHeaderBgColor ? { backgroundColor: 'red' } : {}} />
      {children}
      <Footer />
    </Background>
  );
}
```

**ProjectPage.tsx**

```tsx
export default function ProjectPage() {
  const { isPassed } = useScrollMonitor({ y: 500 });

  return <Layout shouldChangeHeaderBgColor={isPassed}>{/* Project Page Logic */}</Layout>;
}
```

갑자기 또 요구조건이 변경되었다고 해봅시다.

디자이너분께서 특정 좌표를 넘어가면 Header 가 보이지 않도록 부탁하였습니다. 같은 방식으로 또 새로운 props 를 추가하여 해결할 수도 있겠습니다만, 점점 prop 네이밍, 분기 로직이 복잡해집니다.

그리고 이상함이 느껴지지 않나요? 새로운 요구조건은 Header 와 관련이 있는데, Layout 컴포넌트에 새로운 prop 을 추가해주어야 합니다. 수정은 Header 컴포넌트에 해야 하는데, Layout 컴포넌트를 수정해야 합니다!

이렇게 된 이유는 현재 Layout 이 담당하는 역할 때문입니다. Layout 의 관심사는 공통 요소의 배치 뿐이어야 하는데, 지금은 이와 동시에 Header 와 관련한 기능도 관심사로 두고 있습니다.

Layout 의 재사용성을 높이려면 Header 관심사를 분리해줘야 합니다. 어떻게 해야 할까요?

Header 또한 ReactNode 를 값으로 받는 prop 으로 받으면 문제가 해결됩니다!

**Layout.tsx**

```tsx
type Props = {
  header?: React.ReactNode;
  children?: React.ReactNode;
};

export default function Layout({ header, children }: Props) {
  return (
    <Background>
      {header}

      {children}

      <Footer />
    </Background>
  );
}
```

**Header.tsx**

```tsx
export default function Header(props: HTMLAttributes<HTMLDivElement>) {
  return <div {...props}>{children}</div>;
}
```

**ProjectPage.tsx**

```tsx
export default function ProjectPage() {
  const { isPassed: passed500 } = useScrollMonitor({ y: 500 });
  const { isPassed: passed1000 } = useScrollMonitor({ y: 1000 });

  return <Layout header={passed1000 ? null : <Header style={passed500 ? { backgroundColor: 'red' } : {}} />}>{/* Project Page Logic */}</Layout>;
}
```

Layout 에서 Header 의 역할을 분리함으로써, 이젠 Header 와 관련한 요구사항은 Header 에서 수정할 수 있게 되었습니다.

```
ReactNode 를 prop 으로 받으면 더 추상적인 컴포넌트를 작성할 수 있습니다. 이미 정의되어 있는 children prop 뿐만 아니라, 사용자가 정의한 타입 또한 ReactNode 를 값으로 받을 수 있습니다.
```

# Render Props

Render Props 는 ReactNode 를 반환하는 함수를 prop 으로 받는 테크닉입니다.

- Render Props 를 사용하면 여러 컴포넌트 간의 공통 관심사(횡단 관심사) 를 분리할 수 있습니다.
- Render Props 라고 해서 prop 의 이름이 render 여야 하는 것은 아닙니다. 어떤 이름이든 간에, 인자를 받아 ReactNode 를 반환하는 함수를 prop 으로 받으면 Render Props 입니다.

앞서 소개하듯, Render Props 의 가장 큰 장점은 여러 컴포넌트가 비슷한 관심사를 공유하게 될 경우 이를 분리할 수 있다는 점입니다.

**Grid.tsx**

```tsx
type Props<T> = Omit<React.HTMLAttributes<HTMLDivElement>, keyof OwnProps<T>> & OwnProps<T>;

type OwnProps<T> = {
  items: T[];
  label?: React.ReactNode;
  children?: (item: T) => React.ReactNode;
} & GridStyleProps;

export default function Grid<T>({ items, label, children, className, ...props }: Props<T>) {
  return (
    <>
      {label}
      <Container className={className} {...props}>
        {items.map((item) => children?.(item))}
      </Container>
    </>
  );
}
```

Grid 컴포넌트는 `display: grid;` 를 좀 더 편하게 사용하기 위해 만들어진 컴포넌트입니다.

`OwnProps` 부분을 확인해보면 `children` prop 에 `(item: T) => React.ReactNode` 로 함수 타입을 받고, `items.map((item) => children?.(item))` 로 렌더링하는 것을 확인할 수 있습니다.

사용하는 쪽의 코드를 보면 다음과 같습니다.
**Member.tsx**

```tsx
<Grid
  items={people}
  columns={{ mobile: 2, desktop: 4 }}
  gap={{ mobile: '20px 16px', desktop: '31px 47px' }}
  label={
    <Grid.Label>
      {str}hello <sub>{people.length}</sub>
    </Grid.Label>
  }
>
  {(person) => <Person key={person.name} {...person} />}
</Grid>
```

items prop 으로 넘겨주는 people 은 `Person[]` 타입이고, person 은 `Person` 타입으로 추론됩니다.

Grid 사이에 있는 children 에서, person 이란 인자와 `Person` 컴포넌트를 반환하는 콜백 함수를 `Grid` 컴포넌트에게 넘겨줍니다. Grid 컴포넌트는 `items` prop 을 순회하며 `children` prop 으로 받은 콜백 함수를 호출하며 렌더링하게 됩니다.

> 이전의 ReactNode 섹션에선 렌더링 제어권을 컴포넌트 사용자가 가지고 있었지만, Render
> Props 는 제어권을 컴포넌트에게 넘겨주게 됩니다.

만약, Render Props 로 작성하지 않았다면 렌더링 하는 요소와 Grid 가 강결합 되므로 유사한 UI 를 가졌다 하더라도 컴포넌트를 재사용하기 어려웠을 것입니다. 그러나, 현 컴포넌트는 items 와 children prop 을 바꿔주면 재사용할 수 있습니다.

이처럼 Render Props 를 사용하면 횡단 관심사를 깔끔하게 분리할 수 있으며 재사용 하기에도 용이합니다.

# Ref

[https://www.pumpkiinbell.com/blog/react/reusable-components#reactnode-%EB%A5%BC-prop-%EC%9C%BC%EB%A1%9C-%EB%B0%9B%EA%B8%B0](https://www.pumpkiinbell.com/blog/react/reusable-components#reactnode-%EB%A5%BC-prop-%EC%9C%BC%EB%A1%9C-%EB%B0%9B%EA%B8%B0)
