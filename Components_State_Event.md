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

## 양방향 바인딩 추가

```
import React, { useState } from "react";

import "./ExpenseForm.css";

const ExpenseForm = () => {
  const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState("");
  const [enteredDate, setEnteredDate] = useState("");
  // const [userInput, setUserInput] = useState({
  //   enteredTitle: "",
  //   enteredAmount: "",
  //   enteredDate: "",
  // });

  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
    // setUserInput({
    //   ...userInput,
    //   enteredTitle: event.target.value,
    // });
    //   setUserInput((prevState) => {
    //     return { ...prevState, enteredTitle: event.target.value };
    //   });
  };

  const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value);
    // setUserInput({
    //   ...userInput,
    //   enteredAmount: event.target.value,
    // });
  };

  const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value);
    // setUserInput({
    //   ...userInput,
    //   enteredDate: event.target.value,
    // });
  };

  const submitHandler = (event) => {
    event.preventDefault();

    const expenseData = {
      title: enteredTitle,
      amount: enteredAmount,
      date: new Date(enteredDate),
    };

    console.log(expenseData);
    setEnteredTitle("");
    setEnteredAmount("");
    setEnteredDate("");
  };

  return (
    <form onSubmit={submitHandler}>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input
            type="text"
            value={enteredTitle}
            onChange={titleChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            value={enteredAmount}
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            value={enteredDate}
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

```
// submit 후 입력된 값들 즉, 폼이 깔끔히 지워짐
setEnteredTitle("");
setEnteredAmount("");
setEnteredDate("");

// 추가
value={enteredTitle}
value={enteredAmount}
value={enteredDate}
```

- 변경되는 입력값만 수신하는 것이 아니라 입력에 새로운 값을 다시 전달할 수도 있다.
- 그래서 프로그램에 따라 입력값을 재설정하거나 입력할 수 있음

  - How? : 기본 속성인 value를 입력 요소에 추가하면 됨

- 이것은 모든 입력 요소들이 갖는 내부 값의 '프로퍼티'를 설정함

  - 그리고 그것을 새로운 값으로 설정할 수 있음

- 이제 이것은 양방향 바인딩이 되었는데 상태를 업데이트하기 위해 입력에서 변경사항을 수신하는 것뿐만 아니라 입력에 상태를 다시 보내주기도 한다.
- 그래서 상태를 변경하면 입력도 변함

## 자식 대 부모 컴포넌트 통신(상향식)

※ 속성은 오로지 부모에서 자식으로만 전달될 수 있다. <br>
※ 중간 컴포넌트를 생략할 수는 없다 <br>

```
<input
  type="text"
  value={enteredTitle}
  onChange={titleChangeHandler}
/>
```

- 자체 이벤트 속성을 생성해서 만약 이런 식으로 호출하고 싶으면 값으로 함수를 가질 수 있고 부모 컴포넌트로부터 자식 컴포넌트로 함수를 전달할 수 있게 해줄 것이고 자식 컴포넌트에서 그 함수를 호출할 수 있음
- 그리고 함수를 호출했을 때 그 함수에 매개변수로 데이터를 전달할 수 있음
- 이런 식으로 자식에서 부모로 정보를 전달할 수 있음

```
# src/components/NewExpense/NewExpense.js

const NewExpense = (props) => {
  const saveExpenseDataHandler = (enteredExpenseData) => {
    const expenseData = {
      ...enteredExpenseData,
      id: Math.random().toString()
    };
    props.onAddExpense(expenseData);
  };

  return (
    <div className="new-expense">
      <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
    </div>
  );
};

export default NewExpense;
```

```
# src/components/NewExpense/ExpenseForm.js

const ExpenseForm = (props) => {
  const [enteredTitle, setEnteredTitle] = useState("");
  const [enteredAmount, setEnteredAmount] = useState("");
  const [enteredDate, setEnteredDate] = useState("");

  ...

  const submitHandler = (event) => {
    event.preventDefault();

    const expenseData = {
      title: enteredTitle,
      amount: enteredAmount,
      date: new Date(enteredDate),
    };

    props.onSaveExpenseData();
    setEnteredTitle("");
    setEnteredAmount("");
    setEnteredDate("");
  };
```

- onSaveExpenseData 함수는 NewExpense 컴포넌트에서 정의되었는데 그 함수를 다른 컴포넌트(ExpenseForm)에서 실행
- ExpenseForm 안에서 정의되지 않았어도 그 함수를 실행할 수 있음
- 왜냐하면 onSaveExpenseData를 통해 **포인터**를 전달하기 때문
  - 이것은 컴포넌트끼리, 자식에서 부모 컴포넌트로 소통하는 방법

```
# src/components/NewExpense/ExpenseForm.js

...

const expenseData = {
      title: enteredTitle,
      amount: enteredAmount,
      date: new Date(enteredDate),
    };

    props.onSaveExpenseData(expenseData);
    setEnteredTitle("");
    setEnteredAmount("");
    setEnteredDate("");
  };
```

- NewExpense 컴포넌트에서 함수를 호출할 수 있고 매개변수로 데이터를 전달할 수 있음 (매개변수명은 사용자 마음)
- 그래서 ExpenseForm에서 onSaveExpenseData를 호출할 때 여기서 생성한 expenseData를 인자로 전달할 수 있음
- 그리고 그 값이 NewExpense에서 매개변수로 받는 값임
  - 여기서 핵심은 함수에서 포인터를 전달하는 것임

```
# src/App.js

  ...

  const addExpenseHandler = (expense) => {
    console.log("In App.js");
    console.log(expenses);
  };

  return (
    <div>
      <NewExpense onAddExpense= {addExpenseHandler} />
      <Expenses items={expenses} />
    </div>
  );
};

export default App;
```

- addExpenseHandler 함수는 NewExpense 컴포넌트에 **포인터**를 전달한다.
- 그래서 NewExpense 컴포넌트 안에서 이 함수를 호출할 수 있고, App 컴포넌트까지 expense 데이터를 전달할 수 있음
  - onAddExpense(속성명은 사용자 마음)

```
# src/components/NewExpense/NewExpense.js

import React from "react";

import ExpenseForm from "./ExpenseForm";
import "./NewExpense.css";

const NewExpense = (props) => {
  const saveExpenseDataHandler = (enteredExpenseData) => {
    const expenseData = {
      ...enteredExpenseData,
      id: Math.random().toString(),
    };
    props.onAddExpense(expenseData);
  };

  return (
    <div className="new-expense">
      <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
    </div>
  );
};

export default NewExpense;
```

### State 끌어올리기 연습

```
# src/components/Expenses/ExpenseFilter.js

import React from 'react';

import './ExpensesFilter.css';

const ExpensesFilter = () => {
  return (
    <div className='expenses-filter'>
      <div className='expenses-filter__control'>
        <label>Filter by year</label>
        <select>
          <option value='2022'>2022</option>
          <option value='2021'>2021</option>
          <option value='2020'>2020</option>
          <option value='2019'>2019</option>
        </select>
      </div>
    </div>
  );
};

export default ExpensesFilter;
```

### [1. 함수 선언]

```
# src/components/Expenses/ExpenseFilter.js

import React from 'react';

import './ExpensesFilter.css';
const ExpensesFilter = () => {
  const dropdownChangeHandler = () => {};

  return (
    ...
  );
```

- 이 함수에서 포인터를 onChange에 전달하는데, 값이 선택될 때마다 이 함수가 작동되도록 해야 한다.

### [2.포인터 전달 ]

```
# src/components/Expenses/ExpenseFilter.js

...

const ExpensesFilter = () => {
  const dropdownChangeHandler = () => {};

  return (
    <div className='expenses-filter'>
      <div className='expenses-filter__control'>
        <label>Filter by year</label>
        <select onChange={dropdownChangeHandler}>
        <option value='2022'>2022</option>
          <option value='2021'>2021</option>
          <option value='2020'>2020</option>
          <option value='2019'>2019</option>
        </select>
      </div>
    </div>
  );
};

export default ExpensesFilter;
```

### [3. 이벤트 객체 얻기]

```
const ExpensesFilter = () => {
  const dropdownChangeHandler = (event) => {
    console.log(event.target.value)
  };
```

- 이제 Expenses 컴포넌트에서 div 태그를 추가하고 ExpenseFilter 컴포넌트를 추가

```
import ExpenseItem from "./ExpenseItem";
import Card from "../UI/Card";
import "./Expenses.css";
import ExpensesFilter from "./ExpenseFilter";

const Expenses = (props) => {
  return (
    <div>
      <Card className="expenses">
        <ExpensesFilter />
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
      </Card>
    </div>
  );
};
export default Expenses;
```

- 여기까지가 첫 번째 단계
- 두 번째 부분은 선택한 값을 Expenses.js 컴포넌트로 보내는 것
  - 그런 다음, 그 값을 state로 관리

```
# src/components/Expenses/Expenses.js

...

const Expenses = (props) => {
  const filterChangeHandler = (selectedYear) => {
    console.log("Expenses.js");
    console.log(selectedYear);
  };

  return (
    <div>
      <Card className="expenses">
        <ExpensesFilter onChangeFilter={filterChangeHandler} />
        
        ...

      </Card>
    </div>
  );
};
export default Expenses;
```

```
# src/components/Expenses/ExpensesFilter.js

...

const ExpensesFilter = (props) => {
  const dropdownChangeHandler = (event) => {
    props.onChangeFilter(event.target.value)
  };

  return (
    
    ...

  );
};

export default ExpensesFilter;
```

- 이제 값이 Expenses.js로 보내졌음
- 중요한 것은 이것을 state로 저장해야 함
- Expenses.js에서 useState를 import

```
# src/components/Expenses/Expenses.js

import React, { useState } from "react";

import ExpenseItem from "./ExpenseItem";
import Card from "../UI/Card";
import "./Expenses.css";
import ExpensesFilter from "./ExpensesFilter";

const Expenses = (props) => {
  const [filteredYear, setFilteredYear] = useState('2020');

  const filterChangeHandler = (selectedYear) => {
    setFilteredYear(selectedYear);
  };

  return (
    <div>
      <Card className="expenses">
        <ExpensesFilter selected={filteredYear} onChangeFilter={filterChangeHandler} />
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
      </Card>
    </div>
  );
};
export default Expenses;
```

```
# src/components/Expenses/ExpensesFilter.js

import React from 'react';

import './ExpensesFilter.css';

const ExpensesFilter = (props) => {
  const dropdownChangeHandler = (event) => {
    props.onChangeFilter(event.target.value)
  };

  return (
    <div className='expenses-filter'>
      <div className='expenses-filter__control'>
        <label>Filter by year</label>
        <select value={props.selected} onChange={dropdownChangeHandler}>
          <option value='2022'>2022</option>
          <option value='2021'>2021</option>
          <option value='2020'>2020</option>
          <option value='2019'>2019</option>
        </select>
      </div>
    </div>
  );
};

export default ExpensesFilter;
```

- 여기까지가 이벤트를 수신하고 자식에서 부모 컴포넌트로 데이터를 전달하고 state로 작업하는 연습