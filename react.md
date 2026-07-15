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
5. [CSS](#5-css)  
   5-1. [PostCSS](#5-1-postcss)  
   5-2. [tailwindcss](#5-2-tailwindcss)
6. [react router](#6-react-router)  
   6-1. [Routing](#6-1-routing)  
   6-2. [Navigating](#6-2-navigating)
7. [TanStack 쿼리](#7-tanstack-쿼리)  
   7-1. [Queries](#7-1-queries)
8. [axios](#8-axios)

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


npm create vite@latest
cd <app name>
git init
git add .gitignore *
git commit -m "initial project setup"

npm i
npm run dev
```

① App.jsx 정리  
② index.css, App.css 파일 안의 코드 삭제

<br>

### 환경변수

vite에서 환경변수는 `.env` 파일에 작성하면 된다.

```
VITE_<key-name>=값
```

가져올 때는 `import.meta.env.VITE_<key-name>` 방식으로 가져오면 된다.

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

# 5. CSS

## 5-1. PostCSS

css를 모듈별로 관리해준다.

`파일명.module.css`로 파일을 만들고, 사용하고자 하는 컴포넌트 파일에서 import해 사용한다.  
import 한 스타일은 해당 컴포넌트 내에서만 유효하게 동작한다.

```jsx
import styles from './Header.module.css';

export default function Header() {
  return <header className={styles.header}>...</header>;
}
```

<br>
<br>
<br>

## 5-2. tailwindcss

### 설치

```shell
yarn add -D tailwindcss @tailwindcss/vite
```

vite.config.js 파일에 plugin 추가

```js
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  plugins: [tailwindcss()],
});
```

index.css 파일 최상단에 import

```css
@import 'tailwindcss';
```

vscode 확장프로그램으로 'Tailwind CSS IntelliSense' 설치

<br>
<br>
<br>
<br>
<br>

# 6. react router

### 설치

```shell
npx create-react-router@latest <프로젝트 이름>
cd <프로젝트 이름>
npm i
npm run dev
```

<br>

## 6-1. Routing

① app/routes 경로에 페이지 컴포넌트 생성.  
② app/routes.ts 파일에서 경로를 설정해준다.

```ts
import { type RouteConfig, index, route } from '@react-router/dev/routes';

export default [
  index('routes/home.tsx'),
  route('about', 'routes/about.tsx'), // 추가
] satisfies RouteConfig;
```

<br>

### Nested Routes

app/routes.ts 파일에서 경로를 설정해준다.

```ts
import { type RouteConfig, index, route } from '@react-router/dev/routes';

export default [
  route('/', 'routes/home.tsx', [
    index('routes/videos.tsx', { id: 'videos-popular1' }),
    route('videos', 'routes/videos.tsx', { id: 'videos-popular2' }),
    route('videos/:search', 'routes/videos.tsx', { id: 'videos-search' }),
    route('videos/watch/:videoId', 'routes/video-detail.tsx'),
  ]),
] satisfies RouteConfig;
```

#### Outlet

부모 페이지에서 자식 페이지가 렌더링 될 부분을 `<Outlet />`으로 넣어줌.

```tsx
// home.tsx
export default function Home() {
  return (
    <div>
      <Navbar />
      <Outlet />
    </div>
  );
}
```

<br>

### Dynamic Segments

① app/routes.ts 파일에서 경로를 설정해준다.

```ts
import { type RouteConfig, index, route } from '@react-router/dev/routes';

export default [
  route('/', 'routes/home.tsx', [
    index('dashbord/dashbord.tsx'),
    route('teams', 'routes/teams.tsx'),
    route('teams/:teamId', 'routes/team.tsx'), // teamId
  ]),
] satisfies RouteConfig;
```

② `params`로 path segment를 가져올 수 있다. (또는 `useParams()` 훅으로 가져올 수 있다.)

```tsx
import type { Route } from './+types/team';

export default function Team({ params }: Route.ComponentProps) {
  return <div>{params.teamId}</div>;
}
```

<br>
<br>
<br>

## 6-2. Navigating

### 정적

`<Link>`를 이용해 이동할 페이지 연결

```tsx
import { Link } from 'react-router';

export function Navbar() {
  return (
    <nav>
      <Link to="/">Welcome</Link>
      <Link to="/about">ABOUT</Link>
    </nav>
  );
}
```

<br>

### 동적

`useNavigate()` 훅을 이용.

```tsx
// routes/team.tsx
import React, { useState } from 'react';
import { useNavigate } from 'react-router';

export default function Teams() {
  const [text, setText] = useState('');
  const navigate = useNavigate();

  const handleChange = (e) => {
    setText(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setText('');
    navigate(`/teams/${text}`); // 이동
  };

  return (
    <div>
      Teams
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="team id?"
          value={text}
          onChange={handleChange}
        />
      </form>
    </div>
  );
}
```

#### 데이터 전달

이동할 때 데이터를 같이 전달해주고 싶다면 `state` 옵션에 전달해주면 된다.

```tsx
export default function VideoCard({ video }) {
  const navigate = useNavigate();
  const handleClick = () => {
    navigate(`/videos/watch/${video.id}`, {
      state: { video },
    });
  };

  return <>...</>;
}
```

받을 때는 `useLocation()` 훅으로 데이터를 가져온다.

```tsx
import { useLocation } from 'react-router';

export default function VideoDetail() {
  const {
    state: { video },
  } = useLocation();

  return <>...</>;
}
```

<br>
<br>
<br>
<br>
<br>

# 7. TanStack 쿼리

비동기 상태 관리 라이브러리

### 설치

```shell
npm i @tanstack/react-query
```

<br>

### Usage

사용하고자 하는 컴포넌트의 최상위 컴포넌트에 `QueryClientProvider`로 감싸준다.

```jsx
// App.js
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

// ① Create a client
const queryClient = new QueryClient();

function App() {
  return (
    // ② Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  );
}

export default App;
```

<br>
<br>
<br>

## 7-1. Queries

### useQuery

```js
const result = useQuery({ queryKey, queryFn });
```

- queryKey  
  쿼리를 식별하는 고유한 값으로, 배열 형태로 지정한다.
- queryFn  
  데이터를 가져오는 비동기 함수로 반드시 데이터를 반환하거나 오류를 던져야 한다.
- staleTime (Optional)  
   캐시 데이터 상태를 관리한다. (fresh-stale)

  🗣️ 만약, 쿼리키와 일치하는 캐시된 데이터가 있는 경우 바로 UI 업데이트되고, (캐시된 데이터가 fresh 상태면 여기서 끝) 캐시된 데이터가 stale 상태면 백그라운드에서 네트워크 통신을 통해 데이터를 받아온 다음 캐시 데이터를 업데이트해주고 UI도 업데이트 해준다. (그 전과 데이터와 비교해서 다른 부분만 업데이트되고, 동일하다면 UI 업데이트 되지 않음)

<br>

```jsx
import { useQuery } from '@tanstack/react-query';
import { getTodos } from '../my-api';

function Todos() {
  // Queries
  const {
    isLoading,
    error,
    data: todos,
  } = useQuery({ queryKey: ['todos'], queryFn: getTodos });

  return (
    <div>
      {isLoading && <p>Loading...</p>}
      {error && <p>Something is wrong...</p>}
      {todos && (
        <ul>
          {todos.map((todo) => (
            <Todo key={todo.id} todo={todo} />
          ))}
        </ul>
      )}
    </div>
  );
}
```

<br>

### 캐시 데이터 갱신

```jsx
import { useQueryClient, useQuery } from '@tanstack/react-query';
import { getTodos } from '../my-api';

function Todos() {
  // ① Access the client
  const queryClient = useQueryClient();

  const query = useQuery({ queryKey: ['todos'], queryFn: getTodos });

  return (
    <div>
      <ul>
        {query.data?.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
      <button
        onClick={() => {
          // ② ['todos'] 쿼리 키를 가진 모든 캐시 무효화시킴
          queryClient.invalidateQueries({ queryKey: ['todos'] });
        }}
      >
        업데이트
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

# 8. axios

### 설치

```shell
npm i axios
```

<br>

### usage

```jsx
import { getSearch } from '../api/youtube.js';

export default function Videos() {
  const { search } = useParams();
  const {
    isLoading,
    error,
    data: videos,
  } = useQuery({
    queryKey: ['videos', search],
    queryFn: () => getSearch(search), // 컴포넌트 내에서 네트워크 로직을 보여주지 않도록 함.
    staleTime: 1000 * 60 * 1,
  });

  return <>...</>;
}
```

```js
// src/api/youtube.js
import axios from 'axios';

const apiClient = axios.create({
  baseURL: 'https://youtube.googleapis.com/youtube/v3',
  params: {
    key: import.meta.env.VITE_YOUTUBE_API_KEY,
  },
});

export async function getSearch(search: string) {
  return search ? getSearchByKeyword(search) : getPopular();
}

async function getSearchByKeyword(search) {
  return apiClient
    .get('/search', {
      params: {
        part: 'snippet',
        type: 'video',
        maxResults: 25,
        q: search,
      },
    })
    .then((res) =>
      res.data.items.map((item) => ({ ...item, id: item.id.videoId })),
    )
    .catch(console.log);
}

async function getPopular() {
  return apiClient
    .get('/videos', {
      params: {
        part: 'snippet',
        chart: 'mostPopular',
        regionCode: 'KR',
        maxResults: 25,
      },
    })
    .then((res) => res.data.items)
    .catch(console.log);
}
```
