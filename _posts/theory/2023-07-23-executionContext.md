---
title: 'JavaScript Execution Context'
execrpt: '실행 컨텍스트'
toc: true
toc_sticky: true
categories:
  - theory
tags:
  - theory
last_modified_at: 2023-07-23
---

# 실행 컨텍스트

스코프와 this, 호이스팅, 클로저 이 개념들의 공통점은 무엇일까요?  
자바스크립트에서 위의 개념들은 모두 실행 컨텍스트라는 개념에 근간을 두고 있습니다. 즉, 위 개념들이 어떻게 구현되었는지 이해하기 위해서는 실행 컨텍스트에 대해서 이해해야 합니다.

그렇다면, 실행 컨텍스트란 무엇일까요? 실행 컨텍스트란 단어 그대로 자바스크립트가 실행되는 환경을 정의합니다. 여기서 환경이란 this, 변수, 객체, 함수 등 코드의 실행에 필요한 기반들을 의미합니다.

좀 더 구체적으로 말하자면 실행 컨텍스트는 코드를 실행하는데 필요한 환경을 제공하고, 관리합니다. 이 중 식별자, 스코프, this 등은 실행 컨텍스트의 렉시컬 환경을 기반으로 관리되며, 코드의 실행순서는 콜스택(실행 컨텍스트 스택)을 통해서 관리됩니다.

## 실행 컨텍스트의 종류

1. Global Execution Context : Global Execution Context는 처음으로 자바스크립트 코드가 실행될 때 생성되는 실행 컨텍스트입니다. 이 실행 컨텍스트에서는 전역에서 관리되는 값들(함수나, 별도 모듈로 분리되어 있지 않은 값)을 관리합니다.
2. Function Execution Context : Function Execution Context는 함수가 호출될 때 마다 생성되는 실행 컨텍스트입니다. 각각의 함수 호출은 모두 각자의 실행 컨텍스트를 생성하고 가집니다.
3. 그 외에도 Eval Execution Context와 Module Execution Context가 존재합니다.

## 실행 컨텍스트 스택(콜스택)

앞서, 실행 컨텍스트는 코드의 실행 순서를 관리하며 이는 실행 컨텍스트 스택을 통해서 관리된다고 했습니다. 스택은 LIFO(Last In, First Out)의 특징을 가진 자료구조로서, 흔히 접시를 쌓아놓은 모양을 생각하면 됩니다. 자바스크립트는 스택에 실행 컨텍스트를 순차적으로 추가하고 제거하면서 코드의 순서를 관리하면서 실행합니다.

```js
const a = 'a';

function foo() {
  const b = 'b';
  bar();

  function bar() {
    const c = 'c';
    if(true){
      console.log("Hello, World")
    }

}

foo();
```

위에서 실행 컨텍스트 스택의 흐름을 알아보겠습니다.

- 처음에는 실행 컨텍스트 스택은 비어있습니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |

<hr />

1. 먼저 자바스크립트 코드가 실행되면서 전역 실행 컨텍스트가 생성됩니다. 여기서 전역 스코프에 선언된 변수와, 함수등의 정의가 진행됩니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| global                            |

<hr />

2. 그 다음, foo 함수가 호출되면서 foo 함수의 함수 실행 컨텍스트가 생성됩니다. 이 안에서 b와 bar등의 평가와 실행이 진행됩니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| foo                               |
| global                            |

<hr />

3. foo 함수 안에서 bar를 호출하였으므로 bar 함수의 실행 컨텍스트가 생성됩니다.

| bar    |
| ------ |
| foo    |
| global |

<hr />

4. bar 함수의 동작이 완료되었으므로 bar 함수의 실행 컨텍스트가 스택에서 제거됩니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| foo                               |
| global                            |

<hr />

5. foo 함수의 동작이 완료되었으므로 foo 함수의 실행 컨텍스트가 스택에서 제거됩니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| global                            |

<hr />

6. 모든 코드의 실행이 종료되었으므로 전역 실행 컨텍스트가 스택에서 제거됩니다.

| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| --------------------------------- |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |
| &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; |

이처럼 자바스크립트는 스택 구조안에 실행 컨텍스트를 추가하고 제거하면서 현재 무슨 코드가 실행되어야 하는지 파악하고, 개별 코드의 실행이 종료되면 다시 어느 코드로 돌아가서 동작을 이어가야 하는지를 관리합니다.

실행 컨텍스트는 종류별로 세부적인 구성요소와 동작이 다릅니다. 하지만 모든 실행 컨텍스트는 아래의 과정을 거쳐서 생성되고 관리됩니다.

1. **평가**
2. **실행**

### 평가

**평가**란, 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문을 파악하여서 현재 스코프내에서 사용할 수 있는 변수, 함수의 식별자를 실행 컨텍스트에 등록하는 과정입니다.

여기서 호이스팅이란 개념이 나옵니다. 실제 자바스크립트 스펙에는 호이스팅이란 용어는 없습니다.  
다만, 식별자가 마치 코드블락의 제일 상위 부분으로 끌어올려지는것 같다는 것을 표현하기 위해 호이스팅이란 용어를 설명을 위해서 사용하였지만, 현재에는 이 용어가 유명해져서 일반적으로 자바스크립트의 동작을 표현하는 정식 개념처럼 사용되고 있습니다.  
하지만, 사실 호이스팅이란 단언는 스펙에는 존재하지 않으며 다만 평가과정에서 식별자들에 대한 정보가 먼저 실행 컨텍스트에 기록되는 것 뿐입니다.

### 실행

실행이란 선언문을 제외한 소스코드를 실행하는 과정을 의미합니다. 이 과정에서는 소스코드 실행에 필요한 정보, 즉 변수나 함수의 참조를 실행 컨텍스트에서 찾고 실행 과정에서 일어나는 변수값의 변경 등은 다시 실행 컨텍스트에 등록됩니다.

# 실행 컨텍스트의 내부

모든 실행 컨텍스트의 객체와 같이 내부적으로 여러 프로퍼티들을 가지고 있으며 각 프로퍼티들은 각기 고유한 정보를 표현합니다.

실행 컨텍스트의 내부에는 LexicalEnvironment(어휘 환경)라는 객체가 존재합니다.  
LexicalEnvironment는 스코프를 관리하는 역할을 수행하며, 이 안에 변수, 함수의 정의와 값들이 저장되게 됩니다.

LexicalEnvironment는 실행 컨텍스트의 종류에 따라 조금씩 다르지만 크게 두가지 영역으로 구성되어 있습니다.

1. **EnvironmentRecord** : 식별자를 등록하고 관리하는 역할
2. **OuterLexicalEnvironmentReference** : 상위 스코프를 참조하는 역할, 이를 통해 상위 영역에 있는 식별자에도 접근할 수 있게 된다.

그리고 LexicalEnvironment는 평가과정에서 일반적으로 크게 3가지의 동작을 수행합니다.

1. 식별자 생성
2. this binding
3. 참조할 외부 환경 결정

그 다음 실행과정과 평가과정에서 생성된 정보들을 기반으로 값들을 변경하는 방식으로 진행됩니다.

## 전역 실행 컨텍스트

먼저, 자바스크립트 코드가 실행되면서 전역 객체가 생성됩니다. 전역 객체는 자바스크립트에 기본적으로 포함되어 있는 빌트인 객체, 함수, 객체를 포함합니다. 이는 자바스크립트가 실행되는 환경(node, web)등에 따라서 일부 달라집니다.

코드 형태로 보자면 먼저, 전역 객체가 생성됩니다.

```js
const global = {
  // 전역 객체
  console: {
    log() {},
  },
};
```

그 다음, 전역 코드가 평가되며 GlobalExecutionContext가 생성되며 그 내부에 GlobalLexicalEnvironment가 생성됩니다.

```js
const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {},
};
```

GlobalLexicalEnvironment는

1. let, const로 선언한 변수를 관리하는 **Declarative Environment Record**
2. 그 외의 영역(var, 전역 함수, 빌트인 프로퍼티 등)을 관리하는 **ObjectEnvironmentRecord**

두 프로퍼티를 가지고 있습니다.

```js
const global = {
  // 전역 객체
  console : {
    log(){}
  }
};

const GlobalExecutionContext = {
  GlobalLexicalEnvironmentL {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        // let, const로 선언한 키워드를 저장
        // 선언과 초기화가 분리되서 실행, 초기화전에는 TDZ에 빠지게 됨
      }
    }
  }
}
```

ObjectEnvironmentRecord안에는 BincingObject 프로퍼티에서 전역객체를 참조합니다. 이제 var로 선언된 변수, 전역에 선언된 함수 선언은 BindingObject를 통해서 전역객체의 프로퍼티에 등록되게 됩니다.

그리고 let과 const로 선언된 키워드들은 DeclarativeEnvironmentRecord(선언 환경 레코드)에 저장되며, 여기서 저장된 식별자들은 선언과 초기화 과정이 분리되서 실행됩니다. 따라서 var로 선언한 변수와 함수선언과는 다르게 초기화전까지는 접근할 수 없게 됩니다.

```js
// 👈👈👈(실행중인 코드)

var x = 'var'; // var 키워드로 전역변수 x 선언 후 값 "var" 할당
const a = 'a'; // const 키워드로 전역변수 a 선언 후 값 "a" 할당

function foo() {
  // 전역함수 foo 선언
  const b = 'b';
  bar();

  function bar() {
    const c = 'c';
    console.log('Hello, World');
  }
}

foo(); // 전역함수 foo 호출
```

```js
const global = {
  // 전역 객체
  console: {
    log() {},
  },
  x: undefined, // var로 선언한 전역변수 x, undefined로 초기화
  foo: '<function object>', // 전역함수 foo
};

const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: '<uninitialized>', // const로 선언한 전역변수 a, 초기화 되지 않음
      },
    },
  },
};
```

그리고 이제 this값에 대한 참조가 결정됩니다. this값에 대한 참조는 GlobalThisValue라는 프로퍼티에 저장됩니다. 전역 코드의 this는 전역 객체를 가리키므로, GlobalThisValue의 값은 global이 됩니다.

```js
const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: '<uninitialized>',
      },
      GlobalThisValue: global, // this 값에 대한 참조를 전역객체 global로 설정
    },
  },
};
```

그리고 마지막으로 외부 환경에 대한 참조가 결정됩니다. 전역 실행 컨텍스트는 그 자체가 최상위 스코프이므로 외부 환경에 대한 참조값은 null이 됩니다.

```js
const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: '<uninitialized>',
      },
      GlobalThisValue: global,
    },
    // 외부환경에 대한 참조를 null로 결정
    OuterLexicalEnvironmentReference: null,
  },
};
```

이제, 평가과정에서 수행할 식별자 정의, this binding, 외부 환경 참조 결정 과정이 모두 완료되고 실행과정에서 실제 값들이 할당되기 시작합니다.

```js
var x = 'var';
const a = 'a'; // 👈👈👈(실행중인 코드)

function foo() {
  const b = 'b';
  bar();

  function bar() {
    const c = 'c';
    console.log('Hello, World');
  }
}

foo();
```

```js
const global = {
  console: {
    log() {},
  },
  x: 'var', // x에 "var" 할당
  foo: '<function object>',
};

const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: 'a', // a에 "a" 할당
      },
      GlobalThisValue: global,
    },
    OuterLexicalEnvironmentReference: null,
  },
};
```

전역 실행 컨텍스트를 통해서 코드를 실행하던 중, foo 함수가 호출됩니다. 자바스크립트는 **함수가 호출되면 함수 실행 컨텍스트를 생성하고 스택에 푸쉬합니다.**

함수 실행컨텍스트도 마찬가지로 내부에 LexicalEnvironment가 생성됩니다.  
그리고 LexicalEnvironment안에는 FunctionEnvironmentRecord가 생성됩니다. 이 안에는 매개변수와 함수 내부에서 생성된 지역변수등이 저장됩니다.

```js
var x = 'var';
const a = 'a';

function foo() {
  console.log(x);
  const b = 'b';
  bar();

  function bar() {
    const c = 'c';
    console.log('Hello, World');
  }
}

foo(); // 👈👈👈(실행중인 코드)
```

```js
const FunctionExecutionContext = {
  LexicalEnvironment: {
    FunctionEnvironmentRecord: {
      // 매개변수, arguments, 지역 변수, 함수 등록
      b: '<uninitialized>',
      bar: '<function object>',
    },
  },
};
```

그 다음은 함수의 this 참조값을 결정짓습니다. 함수의 this 참조값은 함수가 호출되는 상황에 따라서 결정되는데 여기서 foo 함수는 일반 함수로 호출되었으므로 this값은 전역객체가 됩니다.

```js
const FunctionExecutionContext = {
  LexicalEnvironment: {
    FunctionEnvironmentRecord: {
      b: '<uninitialized>',
      bar: '<function object>',
      ThisValue: global,
    },
  },
};
```

그리고, 함수의 외부 환경에 대한 참조를 결정합니다. 이떄 중요한 특징이 있습니다.  
바로, 함수의 정의는 함수를 평가 할 때 결정됩니다. 따라서 평가될 시점에 실행중인 실행 컨텍스트가 바로 함수의 OuterEnvironmentRecordReference가 됩니다.

여기서 foo 함수는 전역 실행 컨텍스트에서 평가되었으므로 foo 함수의 외부 참조값은 전역 실행 컨텍스트의 LexicalEnvironement입니다. 따라서, FunctionExecutionContext안에서는 GlobalLexicalEnvironment에 정의된 값들을 접근하고 사용할 수 있습니다.

```js
const FunctionExecutionContext = {
  LexicalEnvironment: {
    FunctionEnvironmentRecord: {
      b: '<uninitialized>',
      bar: '<function object>',
      ThisValue: global,
    },
    // 함수가 정의된 시점에 실행중인 실행 컨텍스트의 LexicalEnvironment를 참조
    OuterEnvironmentReference: GlobalExecutionContext.GlobalLexicalEnvironment,
  },
};
```

즉, 함수의 실행 컨텍스트는 함수의 호출 시점에 생성되지만, 함수의 외부 환경에 대한 참조는 어디서 호출했는지가 아니라 어디서 정의되었는지에 따라서 결정됩니다.  
좀 더 상세하게 설명하자면 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 실행중인 실행 컨텍스트의 렉시컬 환경을 함수 객체의 내부 슬롯 \[\[Environment]] 에 저장하고, 함수의 OuterEnvironmentRecord Reference에 할당되는 값은 함수 객체의 내부슬롯 \[\[Environment]]에 저장된 렉시컬 환경에 대한 참조로 저장합니다.

```js
const global = {
  console: {
    log() {},
  },
  x: undefined,
  // 전역 함수 foo
  foo: {
    [[Environment]]: GlobalExecutionContext.GlobalLexicalEnvironment,
  },
};

const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: '<uninitialized>',
      },
    },
  },
};

const FooFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctionEnvironmentRecord: {
      b: '<uninitialized>',
      bar: '<function object>',
      ThisValue: global,
    },
    // 함수 객체의 [[Environment]] 값 참조
    OuterEnvironmentReference: global.foo[[Environment]],
  },
};
```

이러한 동작으로 인해 **클로저**라는 개념이 성립되게 됩니다. 클로저는 외부 환경을 기억하고 있는 함수입니다.

클로저를 이해하기 위해서는 실행 컨텍스트와 렉시컬 환경이 별개의 존재라는 것을 인식해야 합니다.  
실행 컨텍스트는 해당 컨텍스트안의 모든 코드를 실행하고 나면 콜스택에서 제거되지만, 실행 컨텍스트가 제거된다고 반드시 렉시컬 환경이 제거되는 것은 아닙니다.

렉시컬 환경은 실행 컨텍스트에 의해서 참조되기는 하지만, 독립적인 객체로서 그 누구에게도 참조되지 않을 떄 비로소 **가바지 컬렉팅**대상이 되어서 메모리에서 소멸됩니다.

따라서 외부 함수의 실행 컨텍스트 내에서 정의된 내부함수가 외부 함수의 LexicalEnvironment를 계속해서 참조하고 있다면 외부 함수의 LexicalEnvironment는 소멸하지 않습니다.

<hr />

이제 foo 함수 실행 컨텍스트의 평가 과정이 종료되었으니 실행과정으로 넘어갑니다. 그리고 foo함수의 실행 과정에서 bar함수가 호출되었으므로 bar 함수의 실행 컨텍스트가 생성됩니다.

```js
var x = 'var';
const a = 'a';

function foo() {
  const b = 'b';
  bar(); // 👈👈👈(실행중인 코드)

  function bar() {
    const c = 'c';
    console.log('Hello, World');
  }
}

foo();
```

```js
const FooFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctonEnvironmentRecord: {
      b: 'b',
      bar: '<function object>',
      ThisValue: global,
    },
    OuterEnvironmentReference: GlobalExecutionContext.GlobalLexicalEnvironment,
  },
};
```

bar 함수의 실행 컨텍스트도 마찬가지로 생성되고 똑같이 평가와 실행과정을 거칩니다.

```js
const FooFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctonEnvironmentRecord: {
      b: 'b',
      bar: '<function object>',
      ThisValue: global,
    },
    OuterEnvironmentReference: GlobalExecutionContext.GlobalLexicalEnvironment,
  },
};

const BarFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctonEnvironmentRecord: {
      c: 'c',
      ThisValue: global,
    },
    OuterEnvironmentReference: FooFunctionExecutionContext.LexicalEnvironment,
  },
};
```

bar 함수를 실행하는 과정에서 `console.log("Hello, World")`라는 코드가 실행됩니다. 이 코드를 실행하기 위해서는 console 객체의 log 메서드를 찾아야합니다. 하지만 BarContext안에서는 console 식별자를 찾을 수 없습니다. 자바스크립트에서는 실행 컨텍스트 안에서 원하는 식별자를 찾을 수 없으면 OuterEncvironmentReferece에 접근해서 원하는 식별자가 있는지 검색하는 동작을 수행합니다. BarContext의 외부 참조는 FooContext의 LexicalEnvrionment이므로 해당 객체에 접근해서 탐색하는 과정을 수행합니다.

하지만, FooContext에도 원하는 값이 없습니다. 이런 경우에는 원하는 값을 찾을떄 까지 계속해서 참조를 거슬러서 올라갑니다. Foo의 참조는 GlobalLexicalEnvironement이므로 해당 객체에 접근합니다.

```js
var x = 'var';
const a = 'a';

function foo() {
  const b = 'b';
  bar();

  function bar() {
    const c = 'c';
    console.log('Hello, World'); // 👈👈👈(실행중인 코드)
  }
}

foo();
```

```js
const global = {
  console: {
    log() {},
  },
  x: 'var',
  foo: '<function object>',
};

const GlobalExecutionContext = {
  GlobalLexicalEnvironment: {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord: {
        BindingObject: global,
      },
      DeclarativeEnvironmentRecord: {
        a: 'a',
      },
      GlobalThisValue: global,
    },
    OuterLexicalEnvironmentReference: null,
  },
};

const FooFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctonEnvironmentRecord: {
      b: 'b',
      bar: '<function object>',
      ThisValue: global,
    },
    OuterEnvironmentReference: GlobalExecutionContext.GlobalLexicalEnvironment,
  },
};

const BarFunctionExecutionContext = {
  LexicalEnvironment: {
    FunctonEnvironmentRecord: {
      c: 'c',
      ThisValue: global,
    },
    OuterEnvironmentReference: FooFunctionExecutionContext.LexicalEnvironment,
  },
};
```

GlobalLexicalEnvironment 내부에서는 BindingObject를 통해서 global의 `console`객체를 찾을 수 있으므로 이제 해당 `console`객체의 `log`메서드를 최종적으로 실행하게 됩니다.  
이러한 원리를 통해서 자바스크립트의 스코프가 형성되며, 만약 외부 환경에 대한 참조가`null`이 될때까지 원하는 값을 찾지 못한다면, 그때는 `ReferenceError : {indetifier} is not defined`가 발생하게 됩니다.

### 클로저의 예시

```js
function outer() {
  let count = 0;

  return function counter() {
    // counter는 counter 함수의 평가시점인 outer함수 실행컨텍스트의 LexicalEnvironment를 참조하고 있음.
    return ++count;
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
```

# 정리

> 실행 컨텍스트는 소스 코드를 실행하는데 필요한 환경을 제공하고, 코드의 실행결과를 실제로 관리하는 영역입니다.

이 실행 컨텍스트를 통해서 스코프, 클로저등의 동작과 코드의 실행 순서 관리가 구현됩니다. 실행 컨텍스트는 소스코드의 타입에 따라서 실행 컨텍스트를 생성하는 과정과 컨텍스트 내부에서 관리하는 내용이 달라집니다.

**또한, 실행 컨텍스트와 렉시컬 환경은 별도의 객체입니다.** 실행 컨텍스트에서 렉시컬 환경을 참조하고는 있지만, 실행 컨텍스트가 종료된 후에도 해당 렉시컬 환경이 어딘가에서 참조되고 있다면 렉시컬 환경은 가비지 컬렉팅 대상에서 제되됩니다. 이러한 동작으로 인해 클로저라는 개념이 성립하게 됩니다.
