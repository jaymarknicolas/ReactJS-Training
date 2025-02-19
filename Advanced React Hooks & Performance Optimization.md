## **Advanced React Hooks: useContext, useRef, useReducer, useCallback, useMemo, Custom Hooks, and React.memo**  

React provides several advanced hooks to handle global state, optimize performance, and manage complex component logic.  

---

## **📁 Folder Structure**
```
/react-advanced-hooks
│── /src
│   │── /components
│   │   │── Counter.jsx
│   │   │── ExpensiveComputation.jsx
│   │   │── FocusInput.jsx
│   │   │── MemoizedComponent.jsx
│   │   │── ThemeToggle.jsx
│   │── /context
│   │   │── ThemeContext.jsx
│   │── /hooks
│   │   │── useFetchData.js
│   │── App.jsx
│   │── index.js
│── package.json
│── README.md
```

---

## **1. useContext (Global State Management)**
The `useContext` Hook allows components to **share state globally** without prop drilling.  

### **Example: Theme Context**
#### **📄 File: `/src/context/ThemeContext.jsx`**
```jsx
import { createContext, useState } from "react";

export const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

#### **📄 File: `/src/components/ThemeToggle.jsx`**
```jsx
import { useContext } from "react";
import { ThemeContext } from "../context/ThemeContext";

const ThemeToggle = () => {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>
      Current Theme: {theme}
    </button>
  );
};

export default ThemeToggle;
```

#### **📄 File: `/src/App.jsx`**
```jsx
import { ThemeProvider } from "./context/ThemeContext";
import ThemeToggle from "./components/ThemeToggle";

function App() {
  return (
    <ThemeProvider>
      <ThemeToggle />
    </ThemeProvider>
  );
}

export default App;
```

---

## **2. useRef (Handling DOM Elements & Persisting Values)**
The `useRef` Hook is used to **access DOM elements** or **persist values** across renders.

#### **📄 File: `/src/components/FocusInput.jsx`**
```jsx
import { useRef } from "react";

const FocusInput = () => {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
};

export default FocusInput;
```

---

## **3. useReducer (Managing Complex State)**
The `useReducer` Hook is useful for managing **complex state logic**.

#### **📄 File: `/src/components/Counter.jsx`**
```jsx
import { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
};

export default Counter;
```

---

## **4. useCallback (Optimizing Function References)**
The `useCallback` Hook **memoizes functions** to prevent unnecessary re-creation.

#### **📄 File: `/src/components/MemoizedComponent.jsx`**
```jsx
import { useState, useCallback } from "react";

const ChildComponent = ({ handleClick }) => {
  console.log("Child re-rendered!");
  return <button onClick={handleClick}>Click me</button>;
};

const Parent = () => {
  const [count, setCount] = useState(0);
  
  const memoizedHandleClick = useCallback(() => {
    console.log("Button clicked!");
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <ChildComponent handleClick={memoizedHandleClick} />
    </div>
  );
};

export default Parent;
```

---

## **5. useMemo (Optimizing Computed Values)**
The `useMemo` Hook **memoizes expensive computations**.

#### **📄 File: `/src/components/ExpensiveComputation.jsx`**
```jsx
import { useState, useMemo } from "react";

const ExpensiveComputation = () => {
  const [number, setNumber] = useState(0);
  const [counter, setCounter] = useState(0);

  const computeSquare = (num) => {
    console.log("Computing square...");
    return num * num;
  };

  const squaredNumber = useMemo(() => computeSquare(number), [number]);

  return (
    <div>
      <input
        type="number"
        value={number}
        onChange={(e) => setNumber(Number(e.target.value))}
      />
      <p>Squared: {squaredNumber}</p>
      <button onClick={() => setCounter(counter + 1)}>Re-render ({counter})</button>
    </div>
  );
};

export default ExpensiveComputation;
```

---

## **6. Custom Hooks (Reusing Logic)**
Custom Hooks allow us to **extract reusable logic**.

#### **📄 File: `/src/hooks/useFetchData.js`**
```jsx
import { useState, useEffect } from "react";

const useFetchData = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
};

export default useFetchData;
```

#### **📄 File: `/src/components/FetchComponent.jsx`**
```jsx
import useFetchData from "../hooks/useFetchData";

const FetchComponent = () => {
  const { data, loading } = useFetchData("https://jsonplaceholder.typicode.com/posts");

  if (loading) return <p>Loading...</p>;

  return (
    <div>
      {data.slice(0, 5).map((post) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  );
};

export default FetchComponent;
```

---

## **7. React.memo (Preventing Unnecessary Re-renders)**
`React.memo` prevents re-renders when **props don’t change**.

#### **📄 File: `/src/components/MemoizedComponent.jsx`**
```jsx
import React from "react";

const ChildComponent = React.memo(({ value }) => {
  console.log("Child rendered!");
  return <p>Value: {value}</p>;
});

export default ChildComponent;
```

---

This setup ensures **organized code**, **reusable components**, and **optimized performance**. 🚀
