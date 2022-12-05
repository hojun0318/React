# React

### 사용자 인터페이스를 구축하는 자바스크립트 라이브러리

```
# pubilc/index.html

<title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
```

```
# src/index.js

import ReactDOM from 'react-dom/client';

import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

```
# src/App.js

function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
    </div>
  );
}

export default App;
```

## JSX코드 이해하기

- JavaScript 안에 있는 HTML 코드
  - 사실, JSX는 자바스크릅티 XML을 의미함
  - 결국 HTML은 XML이라고 할 수 있기 때문

## 사용자 지정 컴포넌트 만들기

```
# src/components/ExpenseItem.js

function ExpenseItem() {
    return <h2>Expense item!</h2>
}

export default ExpenseItem
```

- HTML 코드를 리턴하는 함수인 컴포넌트를 생성해서 내보내면 된다.

```
# src/App.js

import ExpenseItem from "./components/ExpenseItem";

function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem></ExpenseItem>
    </div>
  );
}

export default App;
```

- 그런 다음 사용하고 싶은 파일에서 Import 하는데 그 JSX 코드에서 그냥 대문자로 시작하는 HTML 요소처럼 사용하면 된다.

## CSS 적용

```
# src/components/ExpenseItem.js

import './ExpenseItem.css'

function ExpenseItem() {
    const expenseDate = new Date(2021, 2, 28)
    const expenseTitle = 'Car Insurance'
    const expenseAmount = 294.67

  return (
    <div className="expense-item">
      <div>{expenseDate.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{expenseTitle}</h2>
        <div className="expense-item__price">${expenseAmount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## props를 통해 데이터 전달하기

- 리액트는 기본적으로 동일한 개념이 내장되어 있어서 재사용할 수 있는 컴포넌트를 만들 수 있다.

  - 매개변수를 사용하거나 리액트의 props라는 개념을 사용해서

- 사용자 지정 요소에서 우리가 얻는 속성에 대한 모든 값을 갖고 있는 객체이다. 좀 더 자세히 말하면 여기 있는 props 객체에서 Key와 Value로 이루어진 파일 모맷을 얻는데 그것은 리액트에 의해 자동으로 전달된다.
  - 여기서 Key는 정의된 속성 이름인 title, amount, date가 된다.
  - Value는 설정된 값이 된다.

```
# src/App.js

import ExpenseItem from "./components/ExpenseItem";

function App() {
  const expenses = [
    {
      id: 'e1',
      title: 'Toilet Paper',
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { id: 'e2', title: 'New TV', amount: 799.49, date: new Date(2021, 2, 12) },
    {
      id: 'e3',
      title: 'Car Insurance',
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: 'e4',
      title: 'New Desk (Wooden)',
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];

  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem
        # title="Toilet Paper"
        title={expenses[0].title}
        amount={expenses[0].amount}
        date={expenses[0].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[1].title}
        amount={expenses[1].amount}
        date={expenses[1].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[2].title}
        amount={expenses[2].amount}
        date={expenses[2].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[3].title}
        amount={expenses[3].amount}
        date={expenses[3].date}
      ></ExpenseItem>
    </div>
  );
}

export default App;
```

- 동적인 props를 위해 값을 설정할 때 이런 중괄호 구문을 사용한다.
- title="Toilet Paper"와 같이 고정 코드화된 값을 가질 수도 있다.
- 즉, props는 동적으로 설정된 값에만 국한되지 않는다.

```
# src/components/ExpenseItem.js

import "./ExpenseItem.css";

function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <div>{props.date.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## 컴포넌트에 "일반" JavaSript 논리 추가하기

```
# src/components/ExpenseItem.js

import "./ExpenseItem.css";

function ExpenseItem(props) {
  const month = props.date.toLocaleString("en-US", { month: "long" });
  const day = props.date.toLocaleString("en-US", { day: "2-digit" });
  const year = props.date.getFullYear();

  return (
    <div className="expense-item">
      <div>
        <div>{month}</div>
        <div>{year}</div>
        <div>{day}</div>
      </div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

## 컴포넌트를 여러 컴포넌트로 분할하기

```
# src/Appj.js

import ExpenseItem from "./components/ExpenseItem";

function App() {
  const expenses = [
    {
      id: "e1",
      title: "Toilet Paper",
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { id: "e2", title: "New TV", amount: 799.49, date: new Date(2021, 2, 12) },
    {
      id: "e3",
      title: "Car Insurance",
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: "e4",
      title: "New Desk (Wooden)",
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];

  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem
        title={expenses[0].title}
        amount={expenses[0].amount}
        date={expenses[0].date}
      />
      <ExpenseItem
        title={expenses[1].title}
        amount={expenses[1].amount}
        date={expenses[1].date}
      />
      <ExpenseItem
        title={expenses[2].title}
        amount={expenses[2].amount}
        date={expenses[2].date}
      />
      <ExpenseItem
        title={expenses[3].title}
        amount={expenses[3].amount}
        date={expenses[3].date}
      />
    </div>
  );
}

export default App;
```

```
# src/components/ExpenseItem.js

import ExpenseDate from "./ExpenseDate";
import "./ExpenseItem.css";

function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

```
# src/components/ExpenseDate.js

import './ExpenseDate.css';

function ExpenseDate(props) {
  const month = props.date.toLocaleString("en-US", { month: "long" });
  const day = props.date.toLocaleString("en-US", { day: "2-digit" });
  const year = props.date.getFullYear();

  return (
    <div className="expense-date">
      <div className="expense-date__month">{month}</div>
      <div className="expense-date__year">{year}</div>
      <div className="expense-date__day">{day}</div>
    </div>
  );
}
export default ExpenseDate;
```

## 연습하기[App.js에서 새로운 component로]

```
# src/components/Expenses.js

import ExpenseItem from "./ExpenseItem";
import "./Expenses.css"

function Expenses(props) {
  return (
    <div className="expenses">
      <ExpenseItem
        title={props.items[0].title}
        amount={props.items[0].amount}
        date={props.items[0].date}
      />
      <ExpenseItem
        title={props.items[1].title}
        amount={props.items[1].amount}
        date={props.items[1].date}
      />
      <ExpenseItem
        title={props.items[2].title}
        amount={props.items[2].amount}
        date={props.items[2].date}
      />
      <ExpenseItem
        title={props.items[3].title}
        amount={props.items[3].amount}
        date={props.items[3].date}
      />
    </div>
  );
}
export default Expenses;
```

```
# src/App.js

import Expenses from "./components/Expenses";

function App() {
  const expenses = [
    {
      id: "e1",
      title: "Toilet Paper",
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { id: "e2", title: "New TV", amount: 799.49, date: new Date(2021, 2, 12) },
    {
      id: "e3",
      title: "Car Insurance",
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: "e4",
      title: "New Desk (Wooden)",
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];

  return (
    <div>
      <h2>Let's get started!</h2>
      <Expenses items={expenses}/>
    </div>
  );
}

export default App;
```

## State

```
# src/components/Expenses/ExpenseItem.js

const ExpenseItem = (props) => {
  // function clickHandler() {}
  const [title, setTitle] = useState(props.title);
```

- useState를 사용해서 상태를 등록하면 항상 두 개의 값을 얻는데 현재 상태값과 업데이트하는 함수이다.

```
  const clickHandler = () => {
    setTitle('Updated!');
  };
```

- 그리고 state가 변할 때마다 업데이트 함수를 호출한다.

```
  return (
  <Card className="expense-item">
  <ExpenseDate date={props.date} />
  <div className="expense-item__description">
  <h2>{title}</h2>
  <div className="expense-item__price">${props.amount}</div>
  </div>
  <button onClick={clickHandler}>Change Title</button>
  </Card>
  );
  }

export default ExpenseItem;
```

```
<h2>{title}</h2>
```

- JSX 코드에서 그것을 출력하기 위해 여기서 처럼 상태값을 사용하고 싶을 때마다 첫 번째 요소를 사용한다.

- 이제 리액트가 나머지 작업을 하고 상태가 변할 때마다 컴포넌트형 함수를 다시 실행하고 JSX 코드를 다시 평가한다.
- 그것이 바로 state이고 중요한 개념이다.
  - 왜냐하면 응용프로그램에게 반응성을 추가하는 것이 state이기 때문이다.

## 1. 여러 State 다루기

```
# src/components/NewExpense/ExpenseForm.js

import React, { useState } from "react";

import "./ExpenseForm.css";

const ExpenseForm = () => {
  const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState('');
  const [enteredDate, setEnteredDate] = useState('');

  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
  };

  const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value)
  };

  const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value)
  };

  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;
```

## 2. State 대신 사용하기(그리고 더 나은 방법)

```
import React, { useState } from "react";

import "./ExpenseForm.css";

const ExpenseForm = () => {
  // const [enteredTitle, setEnteredTitle] = useState("");
  // const [enteredAmount, setEnteredAmount] = useState('');
  // const [enteredDate, setEnteredDate] = useState('');
  const [userInput, setUserInput] = useState({
    enteredTitle: '',
    enteredAmount: '',
    enteredDate: ''
  });

  const titleChangeHandler = (event) => {
    // setEnteredTitle(event.target.value);
    setUserInput({
      ...userInput,
      enteredTitle: event.target.value,
    })
  };

  const amountChangeHandler = (event) => {
    // setEnteredAmount(event.target.value)
    setUserInput({
      ...userInput,
      enteredAmount: event.target.value,
    })
  };

  const dateChangeHandler = (event) => {
    // setEnteredDate(event.target.value)
    setUserInput({
      ...userInput,
      enteredDate: event.target.value,
    })
  };

  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;
```

## 3. 이전 State에 의존하는 State 업데이트

```

```
