# Effect

- = Side Effect라고도 한다.

- 그 전에 지금까지 배운 것을 돌아보면
  - 특히 리액트 라이브러리 자체는 주요 임무가 하나 있다.
    - 사용자 입력에 반응하여 필요할 때 UI를 렌더링하는 것
    - state나 event 등 주요 임무는 화면에 무언가를 가져오는 것
    - 그래서 사용자가 그 무언가와 상호작용하게 하는 것

```
1. Evaluate & Render JSX
2. Manage State & Props
3. React to (User) Events & Input
4. Re-evaluate Component upon State & Prop Changes
```

- Side Effect는 애플리케이션에서 일어나는 다른 모든 것을 뜻한다. (Anything Else)

```
1. Store Data in Browser Storage
2. Send Http Requests to Backend Servers Set & Manage Timers
...
```

- 일반적인 컴포넌트 평가의 밖에서 일어나야 하는 일이다.

  - 즉, 일반적인 컴포넌트 함수 밖에서

- Side Effect를 처리하는 더 좋은 도구가 필요
  - 특별한 리액트 훅을 사용하면 되는데 바로 = **useEffect 훅**
  - useEffect 훅은 그저 내장 훅
  - 컴포넌트 함수 내부에서 실행할 수 있는 또다른 함수

```
useEffect(() => {...}, [ dependencies ]);
```

- 2개의 매개변수, 2개의 인수와 같이 호출된다.
- 첫 번째 인수는 함수이다.
  - 모든 컴포넌트 평가 후에 실행되어야 하는 함수
    - 지정된 의존성(specified dependencies changed)이 변경된 경우라면
- 두 번째 인수에는 지정된 의존성을 넣어줘야 한다.
- 즉, 의존성으로 구성된 배열이다.
- 의존성이 변경될 때마다 첫 번째 함수가 다시 실행된다.
  - 따라서 첫 번째 함수에 어떠한 Side Effect 코드라도 넣을 수 있다.
    - 그러면 그 코드는 지정한 의존성이 변경된 경우에만 실행될 것이다.
- 컴포넌트가 다시 렌더링될 때는 실행되지 않는다.

## Local storage 저장

```
# src/ App.js

const loginHandler = (email, password) => {
    localStorage.setItem("isLoggedIn", "1");
    setIsLoggedIn(true);
  };

// isLoggedIn (사용자 지정)
// 1 (사용자 지정)
```

## useEffect

```
# src/ App.js

import React, { useState, useEffect } from "react";

useEffect(() => {
    const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

    if (storedUserLoggedInInformation === "1") {
      setIsLoggedIn(true);
    }
  }, []);

// 의존성이 없는 경우 함수는 1회만 실행
```

## Local storage 삭제

```
# src/ App.js

const logoutHandler = () => {
    localStorage.removeItem('IsLoggedIn');
    setIsLoggedIn(false);
  };
```

## 모든 컴포넌 재평가 후에 특정 의존성이 변경되는 경우의 예

```
# src/components/Login/Login.js

import React, { useState, useEffect } from 'react';

useEffect(() => {
    setFormIsValid(
      enteredEmail.includes("@") && enteredPassword.trim().length > 6
    );
  }, [enteredEmail, enteredPassword]);
```

- useEffect는 일반적으로 매우 중요한 훅이다.
- 무언가에 대한 응답으로 실행되는 코드를 다루는데 도움이 됨
  - 그 무언가는 로드되는 컴포넌트일 수도 있고 업데이트되는 이메일 주소일 수 도 있다.
  - 어떤 액션에 대한 응답으로 실행되는 액션이 있다면 그것은 SideEffect이다. 그때 useEffect가 도움이 된다.

## useEffect에서 Cleanup 함수 사용하기

- 사용자의 입력 즉, 키가 입력될 때마다 어느정도 멈췄을 때 그제서야 유효성을 확인하는 것 = **디바운싱**
  - setTimeout() 사용 (브라우저에 내장된 함수)

```
# src/components/Login/Login.js

useEffect(() => {
    const identifier = setTimeout(() => {
      console.log("Checking form validity!");
      setFormIsValid(
        enteredEmail.includes("@") && enteredPassword.trim().length > 6
      );
    }, 500);

    return () => {
      console.log('CELANUP');
      clearTimeout(identifier);
    };
  }, [enteredEmail, enteredPassword]);
```

# useReducer

- 내장된 훅으로 state 관리를 도와줌, 그래서 useState와 비슷
- 하지만 기능이 더 많고 더 복잡한 state에 특히 유용

```
# src/components/Login/Login.js

import React, { useState, useEffect, useReducer } from "react";

import Card from "../UI/Card/Card";
import classes from "./Login.module.css";
import Button from "../UI/Button/Button";

const emailReducer = (state, action) => {
  if (action.type === "USER_INPUT") {
    return { vaule: action.val, isValid: action.val.inclueds('@') };
  }
  if (action.type === 'INPUT_BLUR') {
    return { value: state.value, isValid: state.value.inclueds('@') };
  }
  return { value: '', isValid: false};
};

const Login = (props) => {
  // const [enteredEmail, setEnteredEmail] = useState("");
  // const [emailIsValid, setEmailIsValid] = useState();
  const [enteredPassword, setEnteredPassword] = useState("");
  const [passwordIsValid, setPasswordIsValid] = useState();
  const [formIsValid, setFormIsValid] = useState(false);

  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: null,
  });

  useEffect(() => {
    console.log("EFFECT RUNNING");

    return () => {
      console.log("EFFECT CLEANUP");
    };
  }, []);

  // useEffect(() => {
  //   const identifier = setTimeout(() => {
  //     console.log("Checking form validity!");
  //     setFormIsValid(
  //       enteredEmail.includes("@") && enteredPassword.trim().length > 6
  //     );
  //   }, 500);

  //   return () => {
  //     console.log("CELANUP");
  //     clearTimeout(identifier);
  //   };
  // }, [enteredEmail, enteredPassword]);

  const emailChangeHandler = (event) => {
    dispatchEmail({ type: "USER_INPUT", val: event.target.value });

    setFormIsValid(
      event.target.value.includes("@") && enteredPassword.trim().length > 6
    );
  };

  const passwordChangeHandler = (event) => {
    setEnteredPassword(event.target.value);

    setFormIsValid(
      emailState.isValid && event.target.value.trim().length > 6
    );
  };

  const validateEmailHandler = () => {
    dispatchEmail({type: 'INPUT_BLUR'});
  };

  const validatePasswordHandler = () => {
    setPasswordIsValid(enteredPassword.trim().length > 6);
  };

  const submitHandler = (event) => {
    event.preventDefault();
    props.onLogin(emailState.value, enteredPassword);
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <div
          className={`${classes.control} ${
            emailState.isValid === false ? classes.invalid : ""
          }`}
        >
          <label htmlFor="email">E-Mail</label>
          <input
            type="email"
            id="email"
            value={emailState.value}
            onChange={emailChangeHandler}
            onBlur={validateEmailHandler}
          />
        </div>
        <div
          className={`${classes.control} ${
            passwordIsValid === false ? classes.invalid : ""
          }`}
        >
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            value={enteredPassword}
            onChange={passwordChangeHandler}
            onBlur={validatePasswordHandler}
          />
        </div>
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn} disabled={!formIsValid}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};

export default Login;
```

## State 관리를 위한 useReducer VS useState

- useState를 사용하면 너무 번거로운 경우 useReducer 사용

  - 즉, 너무 많은 일을 처리해야 하는 경우

### useState()

```
1. The main state management "tool"
2. Great for independent pieces of state/data
3. Greate if state updates are easy and limited to a few kinds of updates
```

### useReducer()

```
1. Great if you need "more power"
2. Should be considered if you have related pieces of state/data
3. Can be helpful if you have more complex state updates
```

# 리액트 Context(Context API) 소개

## 리액트 Context API 사용

- 리액트 내부적으로 state를 관리할 수 있도록 해주는 것
- 앱의 어떤 컴포넌트에서라도 직접 변경하여, 앱의 다른 컴포넌트에 직접 전달할 수 있게 해준다.
  - props 체인을 구축하지 않고도

```
# src/store/auth-context.js

import React from "react";

const AuthContext = React.createContext({
  isLoggedIn: false
});

export default AuthContext;
```

- '공급한다는 것'은 JSX 코드로 감싸는 것을 뜻함

```
# src/App.js


... {

return (
    <AuthContext.Provider>
      <MainHeader isAuthenticated={isLoggedIn} onLogout={logoutHandler} />
      <main>
        {!isLoggedIn && <Login onLogin={loginHandler} />}
        {isLoggedIn && <Home onLogout={logoutHandler} />}
      </main>
    </AuthContext.Provider>
  );
}

export default App;
```

- MainHeader 뿐만 아니라 Login 및 Home 컴포넌트 등 모든 자식은 AuthContext에 접근할 수 있게 됨

- 즉, 이것을 공급하는 거
- 이제 두번째 부분은 '리스닝 부분'

```
# src/App.js

import React, { useState, useEffect } from "react";

import Login from "./components/Login/Login";
import Home from "./components/Home/Home";
import MainHeader from "./components/MainHeader/MainHeader";
import AuthContext from "./store/auth-context";

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const loginHandler = (email, password) => {
    // We should of course check email and password
    // But it's just a dummy/ demo anyways
    localStorage.setItem("isLoggedIn", "1");
    setIsLoggedIn(true);
  };

  useEffect(() => {
    const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

    if (storedUserLoggedInInformation === "1") {
      setIsLoggedIn(true);
    }
  }, []);

  const logoutHandler = () => {
    localStorage.removeItem("IsLoggedIn");
    setIsLoggedIn(false);
  };

  return (
    <AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
      }}
    >
      <MainHeader isAuthenticated={isLoggedIn} onLogout={logoutHandler} />
      <main>
        {!isLoggedIn && <Login onLogin={loginHandler} />}
        {isLoggedIn && <Home onLogout={logoutHandler} />}
      </main>
    </AuthContext.Provider>
  );
}

export default App;
```

```
# src/components/MainHeader/Navigations.js

import React from "react";

import AuthContext from "../../store/auth-context";
import classes from "./Navigation.module.css";

const Navigation = (props) => {
  return (
    <AuthContext.Consumer>
      {(ctx) => {
        return (
          <nav className={classes.nav}>
            <ul>
              {ctx.isLoggedIn && (
                <li>
                  <a href="/">Users</a>
                </li>
              )}
              {ctx.isLoggedIn && (
                <li>
                  <a href="/">Admin</a>
                </li>
              )}
              {ctx.isLoggedIn && (
                <li>
                  <button onClick={props.onLogout}>Logout</button>
                </li>
              )}
            </ul>
          </nav>
        );
      }}
    </AuthContext.Consumer>
  );
};

export default Navigation;
```

# Rules of Hooks

1.  Only call React Hooks in **React Functions**

  - React Component Functions
  - Custom Hooks <br>

> 리액트 훅은 리액트 함수 컴포넌트에서만 호출해야 한다. 또는 사용자 정의 리액트 훅 함수 안에서도 호출할 수 있다.

> 이 2가지 경우에 리액트 훅을 사용할 수있다.

<hr>

2. Only call React Hooks at the Top Level
  - Don't call them in nested functions
  - Don't call them in any block statements<br>
  
> 리액트 훅은 리액트 컴포넌트 함수 또는 사용자 정의 훅 함수의 최상위 수준에서만 호출해야 한다.

> 중첩 함수에서 훅을 호출하면 안된다.

> block 문에서도 호출하면 안된다.

<hr>

3. 추가) unofficial Rule for useEffect(): ALWAYS add everything you refer to inside of useEffect() as a dependency!<br>
(모든 훅이 아닌 특정 훅과 관련된 규칙 - useEffect())

> 항상 참조하는 모든 항목을 의존성으로 useEffect 내부에 추가해야 한다.