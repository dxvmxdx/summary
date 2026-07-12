# Contents

1. [setup](#1-setup)  
   1-1. [프로젝트 생성 with vite](#1-1-프로젝트-생성-with-vite)  
   1-2. [snippet](#1-2-snippet)
2. [state](#2-state)  
   2-1. [useState](#2-1-usestate)  
   2-2. [useReducer](#2-2-usereducer)
3. [component lifecycle](#3-component-lifecycle)  
   3-1. [useEffect](#3-1-useeffect)
4. [context](#4-context)  
   4-1. [useContext](#4-1-usecontext)

<br>
<br>
<br>
<br>
<br>

# 1. setup

## 1-1. 프로젝트 생성 with vite

```shell
brew update
brew upgrade nvm
nvm -v
nvm install --lts
nvm use v24.18.0
npm install -g npm@latest
node -v
npm -v
npm install -g corepack
yarn -v
yarn set version stable
yarn -v


yarn create vite <app name>
cd <app name>
git init
git add .gitignore *
git commit -m "initial project setup"

yarn install
yarn dev
```

① App.jsx 정리  
② index.css, App.css 파일 안의 코드 삭제

<br>
<br>
<br>

## 1-2. snippet

global snippet에 등록

```json
{
  "reactFunction": {
    "prefix": "rfc",
    "body": "import React from 'react';\n\nexport default function ${1:${TM_FILENAME_BASE}}() {\n\treturn (\n\t\t<>\n\t\t\t\n\t\t</>\n\t);\n}\n\n",
    "description": "Creates a React Function component"
  },
  "reactStatelessImplicitReturn": {
    "prefix": "rsi",
    "body": "import React from 'react';\n\nexport const ${1:${TM_FILENAME_BASE}} = (props) => (\n\t\t\t$0\n\t);",
    "description": "Creates a React Function component"
  },
  "Import Module CSS": {
    "prefix": "si",
    "body": ["import styles from './$TM_FILENAME_BASE.module.css'"],
    "description": "Import PostCSS"
  },
  "ClassName": {
    "prefix": "cn",
    "body": ["className={styles.$1}"],
    "description": "Adding className"
  }
}
```

<br>
<br>
<br>
<br>
<br>

# 2. state

컴포넌트 상태

<br>

## 2-1. useState

```jsx
const [state, setState] = useState(initialValue);
```

- useState 훅은 `state`와 `setState`를 배열 형태로 반환한다.  
  `state`: 현재 상태값.  
  `setState`: state를 업데이트하는 함수.

- setState()를 통해 state를 업데이트할 때마다 해당 컴포넌트는 re-rendering 된다.

- 초기값을 가져올 때 무거운 작업을 해야한다면 콜백함수로 가져와야 한다.  
  그러면 맨 처음 렌더링 될 때만 실행됨.

- 이전 `state`를 기반으로 `state`를 업데이트하는 경우, `setState()` 함수의 argument로 새로운 state를 반환하는 콜백함수를 넣어준다.

  ```jsx
  function handleClick() {
    setAge((prevAge) => prevAge + 1);
  }
  ```

<br>
<br>
<br>

## 2-2. useReducer

복잡한 상태 관리.

```jsx
const [state, dispatch] = useReducer(reducer, initialState, init?)
```

> `state`를 업데이트 시키기 위해서는 `dispatch` 함수의 argument로 `action`을 넣어서 `reducer`에게 전달해준다. 그러면 `reducer`가 컴포넌트의 `state`를 `action`에 있는 내용대로 업데이트해준다.

```jsx
// App.jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'add': {
      const name = action.name;
      const newStudent = {
        id: Date.now(),
        name,
        isHere: false,
      };
      return {
        count: state.count + 1,
        students: [...state.students, newStudent],
      };
    }

    case 'remove': {
      const id = action.id;

      return {
        count: state.count - 1,
        students: state.students.filter((student) => student.id != id),
      };
    }

    case 'mark': {
      return {
        count: state.count,
        students: state.students.map((student) => {
          if (student.id === action.id) {
            return { ...student, isHere: !student.isHere };
          } else {
            return student;
          }
        }),
      };
    }

    default:
      return state;
  }
};

const initialState = {
  count: 0,
  students: [],
};

function App() {
  const [name, setName] = useState('');
  const [studentsInfo, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <h1>출석부</h1>
      <p>총 학생수: {studentsInfo.count}</p>
      <input
        type="text"
        placeholder="이름을 입력해주세요."
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <button onClick={() => dispatch({ type: 'add', name })}>추가</button>

      {studentsInfo.students.map((student) => {
        return (
          <Student key={student.id} student={student} dispatch={dispatch} />
        );
      })}
    </>
  );
}

export default App;
```

```jsx
// Student.jsx
export default function Student({ student: { id, name, isHere }, dispatch }) {
  return (
    <div>
      <span
        style={{ textDecoration: isHere ? 'line-through' : 'none' }}
        onClick={() => {
          dispatch({ type: 'mark', id });
        }}
      >
        {name}
      </span>
      <button
        onClick={() => {
          dispatch({ type: 'remove', id });
        }}
      >
        삭제
      </button>
    </div>
  );
}
```

<br>
<br>
<br>
<br>
<br>

# 3. component lifecycle

- mount: 화면에 첫 렌더링
- update: 리렌더링
- unmount: 화면에서 사라짐

<br>

## 3-1. useEffect

어떤 컴포넌트가 mount/update/unmount 되었을 때, 특정 작업을 처리할 코드를 실행시키고 싶을 때 `useEffect` 훅을 사용한다.

```jsx
// 1. 컴포넌트가 렌더링 될 때마다 실행
useEffect(() => {
  // 작업...
});

// 2. 컴포넌트가 처음 렌더링 될 때, value 값이 바뀔 때마다 실행
useEffect(() => {
  // 작업...
}, [value]);
```

콜백함수의 return 값으로 clean up 함수를 설정할 수 있는데, 설정한 경우 해당 컴포넌트가 의존성 변화에 따라 리렌더링되서 콜백함수가 실행되기 이전에 또는 unmount 되었을 때 실행된다.

```jsx
useEffect(() => {
  // 작업...
  return () => {
    // 정리 작업...
  };
}, [value]);
```

<br>
<br>
<br>
<br>
<br>

# 4. context

app 안에서 전역적으로 사용되는 데이터들을 여러 컴포넌트끼리 쉽게 공유할 수 있는 방법을 제공해준다.

context를 사용하면 props로 데이터를 일일히 전달해주지 않아도 하위 컴포넌트들은 `useContext` 훅을 사용해 자신이 어느 위치에 있든 상관없이 해당 데이터를 받아올 수 있다.

<br>

## 4-1. useContext

### context 생성

```jsx
// src/context/DarkModeContext.jsx
import { createContext, useState } from 'react';

export const DarkModeContext = createContext();

export function DarkModeProvider({ children }) {
  const [mode, setMode] = useState(false);
  const toggleMode = () => setMode((mode) => !mode);

  return (
    <DarkModeContext.Provider value={{ mode, toggleMode }}>
      {children}
    </DarkModeContext.Provider>
  );
}
```

> 🔖 children  
> 부모 컴포넌트에서 호출하는 자식 컴포넌트 태그 사이에 전달한 내용이, 자식 컴포넌트의 children props로 전달된다.
>
> ```jsx
> export default function App() {
>   return (
>     <>
>       <Card>
>         <p>Card1</p>
>       </Card>
>
>       <Card>
>         <h2>Card2</h2>
>         <p>설명</p>
>       </Card>
>     </>
>   );
> }
>
> function Card({ children }) {
>   return <div style={{}}>{children}</div>;
> }
> ```

<br>

### context 사용

- 상위 컴포넌트를 생성한 context의 Provider로 감싸줌.

  ```jsx
  import { DarkModeProvider } from './context/DarkModeContext';

  function App() {
    return (
      <DarkModeProvider>
        <Header />
        <Main />
        <Footer />
      </DarkModeProvider>
    );
  }
  ```

- 데이터를 사용하고자 하는 하위 컴포넌트에서 `useContext`로 필요한 데이터를 가져온다.

  ```jsx
  import { useContext } from 'react';
  import { DarkModeContext } from './context/DarkModeContext';

  export default function ProductDetail() {
    const { mode, toggleMode } = useContext(DarkModeContext);
    return (
      <div>
        <p>
          darkMode: <span>{mode.toString()}</span>
        </p>
        <button onClick={() => toggleMode()}>toggle</button>
      </div>
    );
  }
  ```

<br>
<br>
<br>
<br>
<br>
