## **1. What Is a Hook?**
A **Hook** is a special function in React that allows you to "hook into" React features such as state and lifecycle methods inside functional components.  

### **Why Use Hooks?**
- Eliminates the need for class components.
- Makes components more readable and easier to maintain.
- Encourages the use of functional components.

### **Rules of Hooks**
✅ **Hooks must be called at the top level of a functional component.**  
✅ **Hooks cannot be used inside loops, conditions, or nested functions.**  
✅ **Hooks must be used only inside React functional components or custom Hooks.**  

---

## **2. useState Hook (Managing Component State)**
The `useState` Hook allows components to have local state.  

### **Syntax**
```jsx
const [state, setState] = useState(initialValue);
```
- `state` → Holds the current value.  
- `setState` → Updates the state.  
- `initialValue` → The initial state value.  

---

### **Example 1: Counter with useState**
#### **Scenario: A button increases a counter when clicked.**
```jsx
import { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default Counter;
```
**Explanation:**  
- `useState(0)` initializes `count` to `0`.  
- Clicking the button updates `count` using `setCount`.  

---

### **Example 2: Form Handling with useState**
#### **Scenario: Updating input fields dynamically.**
```jsx
import { useState } from "react";

const FormExample = () => {
  const [name, setName] = useState("");

  return (
    <div>
      <input type="text" value={name} onChange={(e) => setName(e.target.value)} />
      <p>Your name: {name}</p>
    </div>
  );
};

export default FormExample;
```
**Explanation:**  
- The input value updates dynamically with `onChange`.  

---

## **3. useEffect Hook (Handling Side Effects)**
The `useEffect` Hook is used for **side effects** in components, such as:
- Fetching data from an API.
- Listening to events.
- Updating the DOM.

### **Syntax**
```jsx
useEffect(() => {
  // Side effect logic
}, [dependencies]);
```
- **Effect function** runs on component mount/update.
- **Dependencies array** controls when the effect runs.

---

### **Example 3: Running useEffect on Component Mount**
#### **Scenario: Fetching data when the component mounts.**
```jsx
import { useState, useEffect } from "react";

const FetchData = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts/1")
      .then((response) => response.json())
      .then((json) => setData(json));
  }, []);

  return (
    <div>
      {data ? <p>{data.title}</p> : <p>Loading...</p>}
    </div>
  );
};

export default FetchData;
```
**Explanation:**  
- The API call runs **only once** when the component mounts (`[]` dependency array).  
- Data updates with `setData`.  

---

### **Example 4: Using useEffect for Event Listeners**
#### **Scenario: Detecting window resize events.**
```jsx
import { useState, useEffect } from "react";

const WindowSize = () => {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    
    window.addEventListener("resize", handleResize);
    
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <p>Window width: {width}px</p>;
};

export default WindowSize;
```
**Explanation:**  
- The effect runs once (`[]` dependency).  
- The cleanup function **removes** the event listener when the component unmounts.  

---

## **File Structure for a Hooks-Based Project**
```
react-hooks-project/
│── src/
│   │── components/
│   │   │── Counter.jsx
│   │   │── FormExample.jsx
│   │   │── FetchData.jsx
│   │   │── WindowSize.jsx
│   │── App.jsx
│   │── index.js
│── public/
│── package.json
│── README.md
```
**Explanation:**
- `Counter.jsx`: Demonstrates `useState` for a counter.
- `FormExample.jsx`: Demonstrates `useState` for input fields.
- `FetchData.jsx`: Demonstrates `useEffect` for data fetching.
- `WindowSize.jsx`: Demonstrates `useEffect` with event listeners.

---

## **Conclusion**
- **`useState`** manages component-level state.  
- **`useEffect`** handles side effects (API calls, event listeners).  
- **Hooks enable functional components to be powerful and maintainable.**  
