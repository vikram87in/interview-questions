# 📚 React Interview Questions Bank

---

## 🔹 **Basics**

---

### 🎯 **What is React and why use it**

#### **📋 Beginner:**
- **Q1:** What is React and what problem does it solve?
<details>
<summary>Answer</summary>
React is a JavaScript library for building user interfaces, particularly web applications. It solves the problem of efficiently updating and managing the UI when data changes. React uses a component-based architecture and virtual DOM to make UI updates predictable and performant.
</details>

- **Q2:** What are the main advantages of using React over vanilla JavaScript?
<details>
<summary>Answer</summary>
- **Reusable Components**: Write once, use anywhere
- **Virtual DOM**: Efficient updates and better performance
- **Declarative**: Describe what UI should look like, not how to achieve it
- **Large Ecosystem**: Rich community and third-party libraries
- **Developer Tools**: Excellent debugging and development experience
</details>

- **Q3:** What is the difference between a library and a framework? Where does React fit?
<details>
<summary>Answer</summary>
- **Library**: You call its functions when you need them (you control the flow)
- **Framework**: It calls your code when needed (framework controls the flow)
React is a **library** because you import and use React functions in your code, maintaining control over the application structure and flow.
</details>

#### **🚀 Intermediate:**
- **Q1:** How does React's virtual DOM improve performance compared to direct DOM manipulation?
<details>
<summary>Answer</summary>
The Virtual DOM is a JavaScript representation of the real DOM. React compares (diffs) the new virtual DOM with the previous version, calculates the minimum changes needed, and updates only those specific DOM nodes. This batch updating is much faster than frequent direct DOM manipulation, which triggers layout recalculations and repaints.
</details>

- **Q2:** What are the trade-offs of using React in a project?
<details>
<summary>Answer</summary>
**Pros**: Component reusability, large ecosystem, good performance, strong community
**Cons**: Steep learning curve, frequent updates, additional build complexity, larger bundle size for simple projects, JSX learning curve
</details>

---

### 🎯 **JSX**

#### **📋 Beginner:**
- **Q1:** What is JSX and why is it used in React?
<details>
<summary>Answer</summary>
JSX (JavaScript XML) is a syntax extension that allows you to write HTML-like code in JavaScript. It makes React code more readable and allows you to write UI components in a familiar HTML-like syntax while maintaining the full power of JavaScript.
</details>

- **Q2:** Can browsers understand JSX directly? If not, how is it processed?
<details>
<summary>Answer</summary>
No, browsers cannot understand JSX directly. JSX is transpiled (converted) to regular JavaScript function calls using tools like Babel. For example, `<h1>Hello</h1>` becomes `React.createElement('h1', null, 'Hello')`.
</details>

- **Q3:** What are the basic rules for writing JSX?
<details>
<summary>Answer</summary>
- Must return a single parent element (or use React.Fragment)
- Use `className` instead of `class`
- Use `htmlFor` instead of `for`
- Close all tags (including self-closing tags like `<br />`)
- Use camelCase for event handlers (`onClick`, not `onclick`)
</details>

#### **🚀 Intermediate:**
- **Q1:** What does this JSX code compile to?

<details>
<summary>Answer</summary>


```jsx
const element = <h1 className="greeting">Hello, world!</h1>;
```


```javascript
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, world!'
);
```
</details>

- **Q2:** How do you handle events in JSX and what are synthetic events?
<details>
<summary>Answer</summary>
Events in JSX are handled using camelCase event handlers like `onClick`. Synthetic events are React's wrapper around native events that provide consistent behavior across different browsers. They have the same interface as native events but are cross-browser compatible.


```jsx
<button onClick={(e) => console.log(e)}>Click me</button>
```
</details>

---

### 🎯 **Components (functional vs class)**

#### **📋 Beginner:**
- **Q1:** What is a React component and what are the two main types?
<details>
<summary>Answer</summary>
A React component is a reusable piece of UI that accepts inputs (props) and returns JSX describing what should appear on screen. The two main types are:
- **Functional components**: JavaScript functions that return JSX
- **Class components**: ES6 classes that extend React.Component
</details>

- **Q2:** How do you create a simple functional component?
<details>
<summary>Answer</summary>


```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Or as an arrow function
const Welcome = (props) => {
  return <h1>Hello, {props.name}!</h1>;
};
```
</details>

- **Q3:** What is the basic structure of a class component?
<details>
<summary>Answer</summary>


```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What are the advantages of functional components over class components?
<details>
<summary>Answer</summary>
- **Simpler syntax**: Less boilerplate code
- **Hooks support**: Can use state and lifecycle features with hooks
- **Better performance**: Slightly better optimization in React
- **Easier testing**: Functions are easier to test than classes
- **Future-proof**: React team focuses on functional components
</details>

- **Q2:** Convert this class component to a functional component:

<details>
<summary>Answer</summary>


```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```


```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

// Or as arrow function
const Welcome = ({ name }) => {
  return <h1>Hello, {name}</h1>;
};
```
</details>

---

### 🎯 **Props vs state**

#### **📋 Beginner:**
- **Q1:** What are props in React and how do you pass them to components?
<details>
<summary>Answer</summary>
Props (properties) are read-only inputs passed from parent to child components. They are passed as attributes:


```jsx
// Parent component
<Welcome name="John" age={25} />

// Child component receives them
function Welcome(props) {
  return <h1>Hello, {props.name}! You are {props.age} years old.</h1>;
}
```
</details>

- **Q2:** What is state and how is it different from props?
<details>
<summary>Answer</summary>
State is mutable data that belongs to a component and can change over time. 
- **Props**: Immutable, passed from parent, read-only
- **State**: Mutable, owned by component, can be updated


```jsx
const [count, setCount] = useState(0); // State
<Counter initialValue={10} /> // Props
```
</details>

- **Q3:** Can you modify props inside a component? Why or why not?
<details>
<summary>Answer</summary>
No, props are **read-only** and should never be modified. This ensures:
- **Predictable data flow**: Data flows down from parent to child
- **Pure components**: Same props always produce same output
- **Debugging**: Easier to track where data changes occur
</details>

#### **🚀 Intermediate:**
- **Q1:** What happens when you try to modify props directly in a component?
<details>
<summary>Answer</summary>
React will throw an error in development mode: "Cannot assign to read only property". In production, it may silently fail or cause unexpected behavior. Props are frozen in development to enforce immutability.
</details>

- **Q2:** Explain the concept of "lifting state up" with an example scenario.
<details>
<summary>Answer</summary>
When multiple child components need to share state, move the state to their closest common parent. The parent manages the state and passes it down as props.


```jsx
// Parent manages shared state
function App() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Counter count={count} setCount={setCount} />
      <Display count={count} />
    </>
  );
}
```
</details>

---

### 🎯 **Rendering elements**

#### **📋 Beginner:**
- **Q1:** How do you render a React element to the DOM?
<details>
<summary>Answer</summary>


```jsx
// React 18+
import { createRoot } from 'react-dom/client';
const root = createRoot(document.getElementById('root'));
root.render(<App />);

// React 17 and earlier
import ReactDOM from 'react-dom';
ReactDOM.render(<App />, document.getElementById('root'));
```
</details>

- **Q2:** What is the root element in a React application?
<details>
<summary>Answer</summary>
The root element is the DOM element where React mounts and manages the entire application. It's typically a div with id "root" in the HTML file:
```html
<div id="root"></div>
```
</details>

- **Q3:** Can you have multiple React apps on the same page?
<details>
<summary>Answer</summary>
Yes, you can have multiple React roots on the same page by creating multiple root elements and rendering different applications to each:


```jsx
const root1 = createRoot(document.getElementById('app1'));
const root2 = createRoot(document.getElementById('app2'));
root1.render(<App1 />);
root2.render(<App2 />);
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What is the difference between ReactDOM.render() and ReactDOM.createRoot()?
<details>
<summary>Answer</summary>
- **ReactDOM.render()**: Legacy API (React 17 and earlier), synchronous rendering
- **createRoot()**: New API (React 18+), enables concurrent features like automatic batching, transitions, and Suspense


```jsx
// Old way
ReactDOM.render(<App />, container);

// New way
const root = createRoot(container);
root.render(<App />);
```
</details>

- **Q2:** How does React decide when to re-render components?
<details>
<summary>Answer</summary>
React re-renders components when:
- **State changes** (via setState or hooks)
- **Props change** from parent
- **Parent re-renders** (child components re-render by default)
- **Context value changes** (for components consuming context)
- **Force update** is called (not recommended)
</details>

---

### 🎯 **Conditional rendering**

#### **📋 Beginner:**
- **Q1:** How do you conditionally render elements in React?
<details>
<summary>Answer</summary>
You can conditionally render elements using JavaScript expressions:


```jsx
// Using && operator
{isLoggedIn && <WelcomeMessage />}

// Using ternary operator
{isLoggedIn ? <WelcomeMessage /> : <LoginButton />}

// Using if-else (outside JSX)
if (isLoggedIn) {
  return <WelcomeMessage />;
} else {
  return <LoginButton />;
}
```
</details>

- **Q2:** What are three different ways to implement conditional rendering?
<details>
<summary>Answer</summary>
1. **Logical AND (&&)**: `{condition && <Component />}`
2. **Ternary operator**: `{condition ? <ComponentA /> : <ComponentB />}`
3. **If-else statements**: Outside JSX, return different components
</details>

- **Q3:** What will this code render if `isLoggedIn` is false?

<details>
<summary>Answer</summary>


```jsx
{isLoggedIn && <WelcomeMessage />}
```

It will render nothing (no element). The logical AND operator returns `false` when the condition is false, and React doesn't render boolean values.
</details>

#### **🚀 Intermediate:**
- **Q1:** What's the difference between using `&&` operator and ternary operator for conditional rendering?
<details>
<summary>Answer</summary>
- **&& operator**: Renders component or nothing
  

```jsx
  {isTrue && <Component />} // Renders Component or nothing
  ```
- **Ternary operator**: Always renders something
  

```jsx
  {isTrue ? <ComponentA /> : <ComponentB />} // Always renders one component
  ```
</details>

- **Q2:** How do you handle multiple conditions in JSX efficiently?
<details>
<summary>Answer</summary>
Use helper functions or switch statements for complex conditions:


```jsx
const renderContent = () => {
  if (loading) return <Spinner />;
  if (error) return <ErrorMessage />;
  if (data.length === 0) return <EmptyState />;
  return <DataList />;
};

return <div>{renderContent()}</div>;
```
</details>

---

### 🎯 **Lists & keys**

#### **📋 Beginner:**
- **Q1:** How do you render a list of items in React?
<details>
<summary>Answer</summary>
Use the `map()` method to transform an array into JSX elements:


```jsx
const items = ['apple', 'banana', 'orange'];
const listItems = items.map((item, index) => 
  <li key={index}>{item}</li>
);
return <ul>{listItems}</ul>;
```
</details>

- **Q2:** What are keys in React and why are they important?
<details>
<summary>Answer</summary>
Keys are unique identifiers for list elements that help React identify which items have changed, been added, or removed. They improve performance by enabling efficient list updates and maintaining component state correctly during re-renders.


```jsx
{users.map(user => <User key={user.id} data={user} />)}
```
</details>

- **Q3:** What happens if you don't provide keys when rendering lists?
<details>
<summary>Answer</summary>
React will show a warning in the console. Without keys, React can't efficiently update the DOM when list items change, potentially causing:
- Poor performance
- Incorrect component state preservation
- UI bugs when items are reordered
</details>

#### **🚀 Intermediate:**
- **Q1:** Why should you avoid using array indices as keys?
<details>
<summary>Answer</summary>
Using array indices as keys can cause issues when the list order changes:
- React might incorrectly preserve component state
- Performance degradation during reordering
- Input focus and scroll position bugs


```jsx
// Bad - can cause issues
{items.map((item, index) => <Item key={index} />)}

// Good - stable unique identifier
{items.map(item => <Item key={item.id} />)}
```
</details>

- **Q2:** What makes a good key for list items?
<details>
<summary>Answer</summary>
A good key should be:
- **Unique**: Among siblings in the list
- **Stable**: Doesn't change between renders
- **Predictable**: Same item always has same key
Examples: database IDs, UUIDs, or stable combinations of data fields.
</details>

---

## 🔹 **Component Lifecycle**

---

### 🎯 **Class component lifecycle methods**
#### **📋 Beginner:**
- **Q1:** What are the three main phases of a component lifecycle?
<details>
<summary>Answer</summary>
1. **Mounting**: Component is being created and inserted into the DOM
2. **Updating**: Component is being re-rendered as a result of changes to props or state
3. **Unmounting**: Component is being removed from the DOM
</details>

- **Q2:** When does componentDidMount() execute and what is it commonly used for?
<details>
<summary>Answer</summary>
`componentDidMount()` executes immediately after a component is mounted (inserted into the DOM tree). It's commonly used for:
- Making API calls
- Setting up subscriptions
- Initializing timers
- Accessing DOM elements
</details>

- **Q3:** What is componentWillUnmount() used for?
<details>
<summary>Answer</summary>
`componentWillUnmount()` is called just before a component is unmounted and destroyed. It's used for cleanup:
- Cancelling network requests
- Removing event listeners
- Clearing timers/intervals
- Unsubscribing from subscriptions
</details>

#### **🚀 Intermediate:**
- **Q1:** What's the difference between componentDidUpdate() and componentDidMount()?
<details>
<summary>Answer</summary>
- **componentDidMount()**: Called once after initial render (mounting phase)
- **componentDidUpdate()**: Called after every re-render (updating phase), receives prevProps and prevState as parameters


```jsx
componentDidUpdate(prevProps, prevState) {
  if (prevProps.userId !== this.props.userId) {
    this.fetchUserData(this.props.userId);
  }
}
```
</details>

- **Q2:** How do you prevent unnecessary re-renders using lifecycle methods?
<details>
<summary>Answer</summary>
Use `shouldComponentUpdate()` or `React.PureComponent`:


```jsx
shouldComponentUpdate(nextProps, nextState) {
  return nextProps.id !== this.props.id;
}

// Or extend PureComponent for shallow comparison
class MyComponent extends React.PureComponent {
  // Automatically prevents re-renders if props/state haven't changed
}
```
</details>

---

## 🔹 **Hooks**

---

### 🎯 **useState**

#### **📋 Beginner:**
- **Q1:** What is the useState hook and how do you use it?
<details>
<summary>Answer</summary>
`useState` is a Hook that lets you add state to functional components. It returns an array with the current state value and a setter function.


```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```
</details>

- **Q2:** What does useState return?
<details>
<summary>Answer</summary>
`useState` returns an array with exactly two elements:
1. **Current state value**: The current value of the state
2. **Setter function**: A function to update the state value


```jsx
const [state, setState] = useState(initialValue);
```
</details>

- **Q3:** How do you update state using useState?
<details>
<summary>Answer</summary>
Call the setter function with the new value:


```jsx
const [name, setName] = useState('');
const [count, setCount] = useState(0);

// Direct value
setName('John');
setCount(42);

// Function update (for previous state)
setCount(prevCount => prevCount + 1);
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What will happen when this button is clicked?

<details>
<summary>Answer</summary>


```jsx
const [count, setCount] = useState(0);
const handleClick = () => {
  setCount(count + 1);
  setCount(count + 1);
  console.log(count);
};
```

- `count` will only increase by 1 (not 2)
- Console will log `0` (current value, not updated value)
- Both `setCount` calls use the same `count` value (0), so both set it to 1
- State updates are batched and asynchronous
</details>

- **Q2:** How do you update state based on previous state correctly?
</details>

- **Q2:** How do you update state based on previous state correctly?
<details>
<summary>Answer</summary>
Use the functional update pattern:


```jsx
// Correct way
setCount(prevCount => prevCount + 1);
setCount(prevCount => prevCount + 1); // Now count increases by 2

// For objects
setUser(prevUser => ({ ...prevUser, name: 'John' }));
```
</details>

---

### 🎯 **useEffect (dependencies, cleanup)**

#### **📋 Beginner:**
- **Q1:** What is useEffect and when does it run?
<details>
<summary>Answer</summary>
`useEffect` is a Hook that lets you perform side effects in functional components. It runs:
- After every render by default
- After initial render (like componentDidMount)
- After updates (like componentDidUpdate)


```jsx
useEffect(() => {
  console.log('Component rendered');
});
```
</details>

- **Q2:** How do you make useEffect run only once?
<details>
<summary>Answer</summary>
Pass an empty dependency array `[]` as the second argument:

```jsx
useEffect(() => {
  console.log('Runs only once after initial render');
}, []); // Empty dependency array
```
</details>

- **Q3:** What is the purpose of the dependency array in useEffect?
<details>
<summary>Answer</summary>
The dependency array controls when the effect runs:
- **No array**: Runs after every render
- **Empty array `[]`**: Runs only once after initial render
- **With dependencies `[dep1, dep2]`**: Runs when any dependency changes

```jsx
useEffect(() => {
  fetchUser(userId);
}, [userId]); // Runs when userId changes
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you clean up effects (like event listeners or subscriptions)?
<details>
<summary>Answer</summary>
Return a cleanup function from the effect:

```jsx
useEffect(() => {
  const handleScroll = () => console.log('scrolling');
  window.addEventListener('scroll', handleScroll);
  
  // Cleanup function
  return () => {
    window.removeEventListener('scroll', handleScroll);
  };
}, []);
```
</details>

- **Q2:** What will happen in this useEffect?

<details>
<summary>Answer</summary>


```jsx
useEffect(() => {
  console.log('Effect runs');
  setCount(count + 1);
}, [count]);
```

This creates an **infinite loop**:
1. Effect runs and updates `count`
2. `count` change triggers effect again
3. Effect runs and updates `count` again
4. Process repeats infinitely

**Fix**: Use functional update or different dependency.
</details>

---

### 🎯 **useContext**
1. Effect runs and updates `count`
2. `count` change triggers effect again
3. Effect runs and updates `count` again
4. Process repeats infinitely

**Fix**: Use functional update or different dependency.
</details>

---

---

### 🎯 **useContext**

#### **📋 Beginner:**
- **Q1:** What is useContext and how do you use it?
<details>
<summary>Answer</summary>
`useContext` is a Hook that lets you consume context values in functional components without nesting:

```jsx
const ThemeContext = React.createContext();

function MyComponent() {
  const theme = useContext(ThemeContext);
  return <div className={theme}>Content</div>;
}
```
</details>

- **Q2:** How do you create a context in React?
<details>
<summary>Answer</summary>
Use `React.createContext()`:

```jsx
// Create context
const UserContext = React.createContext();

// Provide context
function App() {
  const user = { name: 'John', id: 1 };
  return (
    <UserContext.Provider value={user}>
      <ChildComponent />
    </UserContext.Provider>
  );
}
```
</details>

- **Q3:** What is a Context Provider?
<details>
<summary>Answer</summary>
A Context Provider is a component that supplies context values to all its descendant components. It accepts a `value` prop that will be available to all consuming components:

```jsx
<ThemeContext.Provider value="dark">
  <App /> {/* All children can access "dark" theme */}
</ThemeContext.Provider>
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you avoid unnecessary re-renders when using Context?
<details>
<summary>Answer</summary>
- **Split contexts**: Separate frequently changing data
- **Memoize context value**: Use `useMemo` for object values
- **Memoize components**: Use `React.memo` for consumers

```jsx
const value = useMemo(() => ({ user, settings }), [user, settings]);
<UserContext.Provider value={value}>
```
</details>

- **Q2:** When should you split contexts instead of using one large context?
<details>
<summary>Answer</summary>
Split contexts when:
- Different parts of data change at different frequencies
- Different components need different subsets of data
- You want to avoid unnecessary re-renders

```jsx
// Instead of one large context
<UserContext.Provider> // Split into
<ThemeContext.Provider> // multiple focused contexts
```
</details>

---

### 🎯 **useReducer**

#### **📋 Beginner:**
- **Q1:** What is useReducer and when would you use it instead of useState?
<details>
<summary>Answer</summary>
`useReducer` is a Hook for managing complex state logic. Use it instead of `useState` when:
- State has multiple sub-values
- Next state depends on the previous one
- You want predictable state transitions

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```
</details>

- **Q2:** What are the arguments that useReducer takes?
<details>
<summary>Answer</summary>
`useReducer` takes two required arguments and one optional:
1. **reducer function**: `(state, action) => newState`
2. **initial state**: The initial state value
3. **init function** (optional): For lazy initialization

```jsx
const [state, dispatch] = useReducer(reducer, initialState, init);
```
</details>

- **Q3:** What is a reducer function?
<details>
<summary>Answer</summary>
A reducer is a pure function that takes current state and an action, then returns a new state:

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement useReducer for a counter with increment, decrement, and reset actions?
<details>
<summary>Answer</summary>

```jsx
const initialState = { count: 0 };

function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return initialState;
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```
</details>

- **Q2:** What are the benefits of using useReducer for complex state logic?
<details>
<summary>Answer</summary>
- **Predictable**: All state changes go through the reducer
- **Testable**: Reducer is a pure function, easy to test
- **Scalable**: Better for complex state with multiple actions
- **Debugging**: Easier to track state changes
- **Performance**: Can optimize by avoiding inline object creation
</details>

---

### 🎯 **useRef**

#### **📋 Beginner:**
- **Q1:** What is useRef and what are its common use cases?
<details>
<summary>Answer</summary>
`useRef` is a Hook that returns a mutable ref object. Common use cases:
- **Accessing DOM elements**: Focus inputs, scroll to elements
- **Storing mutable values**: Previous values, timers, any mutable data
- **Persisting values**: Across renders without causing re-renders

```jsx
const inputRef = useRef(null);
const countRef = useRef(0);
```
</details>

- **Q2:** How do you access DOM elements using useRef?
<details>
<summary>Answer</summary>
Assign the ref to an element's `ref` attribute:

```jsx
function MyComponent() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```
</details>

- **Q3:** Does changing a ref's current value cause a re-render?
<details>
<summary>Answer</summary>
**No**, changing `ref.current` does not cause a re-render. This is the key difference between refs and state:

```jsx
const countRef = useRef(0);
countRef.current = 5; // No re-render triggered
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How can you use useRef to store previous values?
<details>
<summary>Answer</summary>
Use `useRef` with `useEffect` to track previous values:

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}

function MyComponent({ count }) {
  const prevCount = usePrevious(count);
  return <div>Current: {count}, Previous: {prevCount}</div>;
}
```
</details>

- **Q2:** What's the difference between useRef and useState for storing values?
<details>
<summary>Answer</summary>
| useRef | useState |
|--------|----------|
| Mutable | Immutable |
| No re-render on change | Triggers re-render |
| `.current` property | Direct value |
| Persists across renders | Persists across renders |
| Synchronous updates | Asynchronous updates |
</details>

---

### 🎯 **useMemo**

#### **📋 Beginner:**
- **Q1:** What is useMemo and when should you use it?
<details>
<summary>Answer</summary>
`useMemo` is a Hook that memoizes (caches) the result of expensive calculations. Use it when:
- You have expensive computations
- You want to avoid recalculating on every render
- The calculation depends on specific values

```jsx
const expensiveValue = useMemo(() => {
  return someExpensiveCalculation(a, b);
}, [a, b]);
```
</details>

- **Q2:** How does useMemo help with performance?
<details>
<summary>Answer</summary>
`useMemo` helps performance by:
- **Skipping expensive calculations** when dependencies haven't changed
- **Preventing unnecessary object creation** that could trigger child re-renders
- **Caching results** between renders
- Only recalculating when dependencies in the array change
</details>

- **Q3:** What happens if you don't provide a dependency array to useMemo?
<details>
<summary>Answer</summary>
**Error**: `useMemo` requires a dependency array as the second argument. Without it, React will throw an error. The dependency array determines when to recalculate the memoized value.
</details>

#### **🚀 Intermediate:**
- **Q1:** When can useMemo be an anti-pattern?
<details>
<summary>Answer</summary>
`useMemo` can hurt performance when:
- **Overused on cheap calculations**: The memoization overhead exceeds the benefit
- **Dependencies change frequently**: Defeats the purpose of memoization
- **Used everywhere**: Creates unnecessary complexity and memory usage

```jsx
// Anti-pattern - too simple
const sum = useMemo(() => a + b, [a, b]);
```
</details>

- **Q2:** How do you decide what to include in useMemo's dependency array?
<details>
<summary>Answer</summary>
Include all values from component scope that are used inside `useMemo`:
- **Props** used in the calculation
- **State variables** used in the calculation
- **Other computed values** used in the calculation

```jsx
const result = useMemo(() => {
  return calculate(prop1, state1, localVar);
}, [prop1, state1, localVar]); // All used variables
```
</details>

---

### 🎯 **useCallback**

#### **📋 Beginner:**
- **Q1:** What is useCallback and how is it related to useMemo?
<details>
<summary>Answer</summary>
`useCallback` is a Hook that memoizes functions. It's related to `useMemo` but specifically for functions:
- **useCallback**: Returns a memoized function
- **useMemo**: Returns a memoized value

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

// Equivalent to:
const memoizedCallback = useMemo(() => {
  return () => doSomething(a, b);
}, [a, b]);
```
</details>

- **Q2:** When should you use useCallback?
<details>
<summary>Answer</summary>
Use `useCallback` when:
- Passing functions to memoized child components
- Functions are dependencies of other hooks
- Preventing unnecessary re-renders of child components

```jsx
const Child = React.memo(({ onClick }) => <button onClick={onClick} />);

function Parent() {
  const handleClick = useCallback(() => {
    console.log('clicked');
  }, []);
  
  return <Child onClick={handleClick} />;
}
```
</details>

- **Q3:** What does useCallback return?
<details>
<summary>Answer</summary>
`useCallback` returns a **memoized version of the function** that only changes if one of the dependencies has changed. It returns the same function reference between renders when dependencies don't change.
</details>

#### **🚀 Intermediate:**
- **Q1:** Why doesn't this optimization work?

<details>
<summary>Answer</summary>


```jsx
const Child = React.memo(({ onClick, data }) => {
  return <button onClick={onClick}>{data.name}</button>;
});

const Parent = () => {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => setCount(c => c + 1), []);
  const data = { name: 'Button' };
  return <Child onClick={handleClick} data={data} />;
};
```

The optimization doesn't work because `data` is a **new object on every render**. Even though `onClick` is memoized, the `data` prop changes, causing `Child` to re-render.

**Fix**: Memoize the data object too:

```jsx
const data = useMemo(() => ({ name: 'Button' }), []);
```
</details>

- **Q2:** How can overusing useCallback hurt performance?
<details>
<summary>Answer</summary>
Overusing `useCallback` can hurt performance by:
- **Creating unnecessary closures**: Memory overhead for each memoized function
- **Comparison overhead**: React must compare dependencies on each render
- **Complexity without benefit**: When child components aren't memoized
- **Stale closures**: Capturing outdated values in dependencies
</details>

---

### 🎯 **useLayoutEffect**

#### **📋 Beginner:**
- **Q1:** What's the difference between useEffect and useLayoutEffect?
<details>
<summary>Answer</summary>
- **useEffect**: Runs **asynchronously** after DOM mutations and browser paint
- **useLayoutEffect**: Runs **synchronously** after DOM mutations but before browser paint

```jsx
useEffect(() => {
  // Runs after paint - won't block visual updates
});

useLayoutEffect(() => {
  // Runs before paint - can block visual updates
});
```
</details>

- **Q2:** When should you use useLayoutEffect?
<details>
<summary>Answer</summary>
Use `useLayoutEffect` when you need to:
- **Measure DOM elements** before the browser paints
- **Make DOM mutations** that users should not see
- **Prevent visual flicker** during DOM updates
- **Synchronously update** based on DOM measurements
</details>

- **Q3:** What are the performance implications of useLayoutEffect?
<details>
<summary>Answer</summary>
`useLayoutEffect` can hurt performance because:
- **Blocks browser painting** until effect completes
- **Synchronous execution** can delay visual updates
- **Should be used sparingly** - prefer `useEffect` when possible
- Can cause **jank** if heavy computations are performed
</details>

#### **🚀 Intermediate:**
- **Q1:** How would you measure DOM elements before the browser paints?
<details>
<summary>Answer</summary>

```jsx
function MyComponent() {
  const [height, setHeight] = useState(0);
  const elementRef = useRef(null);
  
  useLayoutEffect(() => {
    if (elementRef.current) {
      const rect = elementRef.current.getBoundingClientRect();
      setHeight(rect.height);
    }
  });
  
  return (
    <div ref={elementRef}>
      <p>Element height: {height}px</p>
    </div>
  );
}
```
</details>

- **Q2:** Why might useLayoutEffect cause issues in SSR?
<details>
<summary>Answer</summary>
`useLayoutEffect` can cause SSR hydration mismatches because:
- **Server doesn't have DOM**: No layout information available
- **Client-server differences**: Different measurements between server and client
- **Hydration warnings**: React detects content differences
**Solution**: Use `useEffect` for SSR or conditionally render based on `typeof window !== 'undefined'`
</details>

---

### 🎯 **useImperativeHandle**

#### **📋 Beginner:**
- **Q1:** What is useImperativeHandle and with which hook is it typically used?
<details>
<summary>Answer</summary>
`useImperativeHandle` is a Hook that customizes the instance value exposed to parent components when using `ref`. It's typically used with `forwardRef`:

```jsx
const MyInput = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => inputRef.current.value = ''
  }));
  
  return <input ref={inputRef} />;
});
```
</details>

- **Q2:** When would you need to use useImperativeHandle?
<details>
<summary>Answer</summary>
Use `useImperativeHandle` when you need to:
- **Expose specific methods** from child to parent component
- **Create reusable components** with imperative APIs
- **Control what gets exposed** through refs
- **Provide custom functionality** beyond basic DOM access
</details>

- **Q3:** How do you expose methods from a child component to its parent?
<details>
<summary>Answer</summary>

```jsx
// Child component
const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef();
  
  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    },
    getValue() {
      return inputRef.current.value;
    }
  }));
  
  return <input ref={inputRef} {...props} />;
});

// Parent component
function Parent() {
  const childRef = useRef();
  
  return (
    <>
      <CustomInput ref={childRef} />
      <button onClick={() => childRef.current.focus()}>Focus</button>
    </>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** Why should useImperativeHandle be used sparingly?
<details>
<summary>Answer</summary>
`useImperativeHandle` should be used sparingly because:
- **Breaks React's declarative paradigm** - makes code more imperative
- **Tight coupling** between parent and child components
- **Harder to test and reason about** than props-based communication
- **Alternative solutions** often exist using props and callbacks
- **Can make components less reusable**
</details>

- **Q2:** How do you create a custom input component that exposes a focus method?
<details>
<summary>Answer</summary>

```jsx
const FocusableInput = forwardRef(({ placeholder, ...props }, ref) => {
  const inputRef = useRef();
  
  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    },
    blur() {
      inputRef.current.blur();
    },
    getValue() {
      return inputRef.current.value;
    },
    clear() {
      inputRef.current.value = '';
    }
  }), []); // Empty dependency array since methods don't change
  
  return (
    <input 
      ref={inputRef} 
      placeholder={placeholder} 
      {...props} 
    />
  );
});
```
</details>

---

### 🎯 **Custom hooks**

#### **📋 Beginner:**
- **Q1:** What are custom hooks and what naming convention must they follow?
<details>
<summary>Answer</summary>
Custom hooks are JavaScript functions that:
- **Start with "use"** (naming convention)
- **Can call other hooks** inside them
- **Allow sharing stateful logic** between components

```jsx
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  const increment = () => setCount(c => c + 1);
  return { count, increment };
}
```
</details>

- **Q2:** Why would you create a custom hook?
<details>
<summary>Answer</summary>
Create custom hooks to:
- **Share stateful logic** between multiple components
- **Abstract complex logic** into reusable functions
- **Keep components clean** by extracting side effects
- **Create reusable patterns** for common use cases
- **Separate concerns** and improve code organization
</details>

- **Q3:** Can custom hooks use other hooks?
<details>
<summary>Answer</summary>
**Yes**, custom hooks can use any built-in hooks or other custom hooks:

```jsx
function useApi(url) {
  const [data, setData] = useState(null); // useState
  const [loading, setLoading] = useState(true); // useState
  
  useEffect(() => { // useEffect
    fetch(url).then(res => res.json()).then(setData);
  }, [url]);
  
  return { data, loading };
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** Create a custom hook called useLocalStorage that manages localStorage state.
<details>
<summary>Answer</summary>

```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });
  
  const setValue = (value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error saving to localStorage:', error);
    }
  };
  
  return [storedValue, setValue];
}

// Usage
function App() {
  const [name, setName] = useLocalStorage('name', '');
  return <input value={name} onChange={e => setName(e.target.value)} />;
}
```
</details>

- **Q2:** How do you test custom hooks?
<details>
<summary>Answer</summary>
Use React Testing Library's `renderHook`:

```jsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

test('should increment counter', () => {
  const { result } = renderHook(() => useCounter(0));
  
  expect(result.current.count).toBe(0);
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```
</details>

---

## 🔹 **Advanced Hooks Patterns**

---

### 🎯 **Rules of hooks**

#### **📋 Beginner:**
- **Q1:** What are the two main Rules of Hooks?
<details>
<summary>Answer</summary>
1. **Only call hooks at the top level** - Don't call hooks inside loops, conditions, or nested functions
2. **Only call hooks from React functions** - Call them from React function components or custom hooks, not regular JavaScript functions
</details>

- **Q2:** Why can't you call hooks inside loops or conditions?
<details>
<summary>Answer</summary>
React relies on the **order of hook calls** to maintain state between renders. Calling hooks conditionally or in loops can change this order, causing:
- **State to be associated with wrong variables**
- **Unexpected behavior and bugs**
- **Loss of state between renders**

```jsx
// ❌ Wrong - conditional hook call
if (condition) {
  const [state, setState] = useState();
}

// ✅ Correct - hook at top level
const [state, setState] = useState();
if (condition) {
  // Use state here
}
```
</details>

- **Q3:** Where can hooks be called?
<details>
<summary>Answer</summary>
Hooks can only be called from:
- **React function components**
- **Custom hooks** (functions starting with "use")
- **Top level** of these functions (not inside loops, conditions, or nested functions)
</details>

#### **🚀 Intermediate:**
- **Q1:** How does React keep track of hook state between renders?
<details>
<summary>Answer</summary>
React uses a **linked list** to track hooks:
- Each component instance has a **hooks list**
- Hooks are called in the **same order** every render
- React **matches hooks by position** in the list, not by name
- This is why hook order must remain consistent
</details>

- **Q2:** What tools help enforce the Rules of Hooks?
<details>
<summary>Answer</summary>
- **ESLint plugin**: `eslint-plugin-react-hooks` catches violations
- **React DevTools**: Shows hook violations in development
- **TypeScript**: Can help with some hook-related type safety

```json
// .eslintrc.js
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```
</details>

---

### 🎯 **Sharing logic with custom hooks**

#### **📋 Beginner:**
- **Q1:** How do custom hooks help with code reuse?
<details>
<summary>Answer</summary>
Custom hooks enable code reuse by:
- **Extracting stateful logic** into reusable functions
- **Sharing common patterns** across multiple components
- **Avoiding code duplication** for similar functionality
- **Creating component-agnostic logic** that can be used anywhere

```jsx
// Reusable across multiple components
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  const toggle = useCallback(() => setValue(v => !v), []);
  return [value, toggle];
}
```
</details>

- **Q2:** What's the difference between custom hooks and HOCs for sharing logic?
<details>
<summary>Answer</summary>
| Custom Hooks | HOCs |
|--------------|------|
| Function-based | Component-based |
| Share stateful logic | Share component logic |
| No wrapper components | Creates wrapper components |
| Easier composition | Can cause wrapper hell |
| Better with modern React | Legacy pattern |
| Direct state access | Props-based communication |
</details>

- **Q3:** Can custom hooks return JSX?
<details>
<summary>Answer</summary>
**No**, custom hooks should **not return JSX**. They should return:
- **State values and setters**
- **Computed values**
- **Event handlers**
- **Plain JavaScript values**
If you need to share JSX, use **render props** or **compound components** instead.
</details>

#### **🚀 Intermediate:**
- **Q1:** Design a custom hook for handling form input state.
<details>
<summary>Answer</summary>

```jsx
function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  
  const handleChange = (name, value) => {
    setValues(prev => ({ ...prev, [name]: value }));
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };
  
  const setError = (name, error) => {
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  const reset = () => {
    setValues(initialValues);
    setErrors({});
  };
  
  const getInputProps = (name) => ({
    value: values[name] || '',
    onChange: (e) => handleChange(name, e.target.value),
    error: errors[name]
  });
  
  return {
    values,
    errors,
    handleChange,
    setError,
    reset,
    getInputProps
  };
}

// Usage
function LoginForm() {
  const { values, getInputProps, setError } = useForm({ email: '', password: '' });
  
  return (
    <form>
      <input {...getInputProps('email')} placeholder="Email" />
      <input {...getInputProps('password')} type="password" placeholder="Password" />
    </form>
  );
}
```
</details>

- **Q2:** How do you compose multiple custom hooks together?
<details>
<summary>Answer</summary>

```jsx
function useApiWithAuth(url) {
  const { user, isAuthenticated } = useAuth(); // First custom hook
  const { data, loading, error } = useApi(url, { // Second custom hook
    enabled: isAuthenticated,
    headers: { Authorization: `Bearer ${user?.token}` }
  });
  
  return {
    data,
    loading,
    error,
    isAuthenticated,
    user
  };
}

// Combine with built-in hooks
function usePersistedCounter() {
  const [count, setCount] = useLocalStorage('count', 0); // Custom hook
  const increment = useCallback(() => setCount(c => c + 1), [setCount]); // Built-in hook
  
  return { count, increment };
}
```
</details>

---

## 🔹 **State Management**

---

### 🎯 **Context API**

#### **📋 Beginner:**
- **Q1:** How do you create and provide context values?
<details>
<summary>Answer</summary>

```jsx
// 1. Create context
const ThemeContext = React.createContext('light');

// 2. Provide context value
function App() {
  const [theme, setTheme] = useState('dark');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

// 3. Consume context
function Header() {
  const { theme, setTheme } = useContext(ThemeContext);
  return <div className={theme}>Header</div>;
}
```
</details>

- **Q2:** What causes Context consumers to re-render?
<details>
<summary>Answer</summary>
Context consumers re-render when:
- **Context value changes** (reference comparison)
- **Provider component re-renders** with a new value object
- **Any part of the context value changes**

```jsx
// This causes re-renders on every parent render
<Context.Provider value={{ user, theme }}>

// Better - memoize the value
const value = useMemo(() => ({ user, theme }), [user, theme]);
<Context.Provider value={value}>
```
</details>

- **Q3:** How do you consume context in functional components?
<details>
<summary>Answer</summary>
Use the `useContext` hook:

```jsx
import { useContext } from 'react';

function MyComponent() {
  const contextValue = useContext(MyContext);
  
  // Use contextValue here
  return <div>{contextValue.someProperty}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Context to prevent unnecessary re-renders?
<details>
<summary>Answer</summary>
**Optimization strategies:**
1. **Memoize context value**:

```jsx
const value = useMemo(() => ({ user, actions }), [user]);
```

2. **Split contexts** by update frequency:

```jsx
<UserContext.Provider value={user}>
  <ThemeContext.Provider value={theme}>
    <App />
  </ThemeContext.Provider>
</UserContext.Provider>
```

3. **Memoize consumers**:

```jsx
const ExpensiveComponent = React.memo(() => {
  const { theme } = useContext(ThemeContext);
  return <div className={theme}>Content</div>;
});
```
</details>

- **Q2:** When should you use multiple contexts vs one large context?
<details>
<summary>Answer</summary>
**Use multiple contexts when:**
- Different parts of state change at **different frequencies**
- Components need **different subsets** of data
- You want to **avoid unnecessary re-renders**

**Use single context when:**
- Data is **tightly coupled** and changes together
- **Simple applications** with minimal state
- **Performance isn't a concern**


```jsx
// Multiple contexts approach
<AuthContext.Provider value={auth}>
  <ThemeContext.Provider value={theme}>
    <SettingsContext.Provider value={settings}>
      <App />
    </SettingsContext.Provider>
  </ThemeContext.Provider>
</AuthContext.Provider>
```
</details>

---

### 🎯 **Redux (actions, reducers, store, middleware)**

#### **📋 Beginner:**
- **Q1:** What are the three core principles of Redux?
<details>
<summary>Answer</summary>
1. **Single Source of Truth**: The entire application state is stored in one store
2. **State is Read-Only**: State can only be changed by dispatching actions
3. **Changes are made with Pure Functions**: Reducers are pure functions that specify how state changes
</details>

- **Q2:** What is an action in Redux?
<details>
<summary>Answer</summary>
An action is a **plain JavaScript object** that describes what happened. It must have a `type` property:

```jsx
// Simple action
const incrementAction = { type: 'INCREMENT' };

// Action with payload
const addTodoAction = {
  type: 'ADD_TODO',
  payload: {
    id: 1,
    text: 'Learn Redux',
    completed: false
  }
};

// Action creator function
const addTodo = (text) => ({
  type: 'ADD_TODO',
  payload: { id: Date.now(), text, completed: false }
});
```
</details>

- **Q3:** What is a reducer and what rules must it follow?
<details>
<summary>Answer</summary>
A reducer is a **pure function** that takes current state and action, returns new state. Rules:
- **Pure function**: No side effects, same input = same output
- **Immutable updates**: Never mutate state directly
- **Handle unknown actions**: Return current state for unknown action types

```jsx
function todosReducer(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.payload]; // Immutable update
    case 'TOGGLE_TODO':
      return state.map(todo =>
        todo.id === action.payload.id
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state; // Handle unknown actions
  }
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How does middleware work in Redux?
<details>
<summary>Answer</summary>
Middleware provides a **third-party extension point** between dispatching an action and reaching the reducer:

```jsx
// Custom middleware
const loggerMiddleware = (store) => (next) => (action) => {
  console.log('Dispatching:', action);
  const result = next(action);
  console.log('New state:', store.getState());
  return result;
};

// Apply middleware
const store = createStore(
  rootReducer,
  applyMiddleware(loggerMiddleware, thunk)
);
```
**Common middleware**: Redux Thunk, Redux Saga, Redux Logger
</details>

- **Q2:** What is the purpose of normalizing state in Redux?
<details>
<summary>Answer</summary>
Normalizing state **flattens nested data** for better performance and easier updates:

```jsx
// Non-normalized (bad)
const state = {
  posts: [
    { id: 1, title: 'Post 1', author: { id: 1, name: 'John' } },
    { id: 2, title: 'Post 2', author: { id: 1, name: 'John' } }
  ]
};

// Normalized (good)
const state = {
  posts: {
    byId: {
      1: { id: 1, title: 'Post 1', authorId: 1 },
      2: { id: 2, title: 'Post 2', authorId: 1 }
    },
    allIds: [1, 2]
  },
  authors: {
    byId: {
      1: { id: 1, name: 'John' }
    },
    allIds: [1]
  }
};
```
**Benefits**: Avoid duplication, easier updates, better performance
</details>

---

### 🎯 **Redux Toolkit**

#### **📋 Beginner:**
- **Q1:** What problems does Redux Toolkit solve?
<details>
<summary>Answer</summary>
Redux Toolkit (RTK) solves common Redux problems:
- **Too much boilerplate**: Simplifies action creators and reducers
- **Complex store setup**: Provides `configureStore` with good defaults
- **Accidental mutations**: Uses Immer for immutable updates
- **DevTools setup**: Automatically includes Redux DevTools
- **Async logic**: Built-in `createAsyncThunk` for async actions
</details>

- **Q2:** What is createSlice and what does it generate?
<details>
<summary>Answer</summary>
`createSlice` is a function that generates Redux logic. It creates:
- **Action creators**: Automatically generated from reducer names
- **Reducer function**: Handles the actions
- **Action types**: Based on slice name and reducer names

```jsx
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1; // Immer allows "mutations"
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

// Auto-generated action creators
export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```
</details>

- **Q3:** How does Redux Toolkit handle immutability?
<details>
<summary>Answer</summary>
RTK uses **Immer** under the hood, which allows you to write "mutative" logic that actually produces immutable updates:

```jsx
// With RTK + Immer (looks like mutation, but isn't)
const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      state.push(action.payload); // Immer handles immutability
    },
    toggleTodo: (state, action) => {
      const todo = state.find(todo => todo.id === action.payload);
      if (todo) {
        todo.completed = !todo.completed; // Safe with Immer
      }
    }
  }
});
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What is createAsyncThunk and when would you use it?
<details>
<summary>Answer</summary>
`createAsyncThunk` creates thunks for async operations and automatically dispatches lifecycle actions:

```jsx
const fetchUserById = createAsyncThunk(
  'users/fetchById',
  async (userId, { rejectWithValue }) => {
    try {
      const response = await api.fetchUser(userId);
      return response.data;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const usersSlice = createSlice({
  name: 'users',
  initialState: { users: [], loading: false, error: null },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserById.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchUserById.fulfilled, (state, action) => {
        state.loading = false;
        state.users.push(action.payload);
      })
      .addCase(fetchUserById.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});
```
**Use for**: API calls, any async operations, consistent loading states
</details>

- **Q2:** How does RTK Query compare to other data fetching solutions?
<details>
<summary>Answer</summary>
**RTK Query vs other solutions:**

| Feature | RTK Query | React Query | Apollo |
|---------|-----------|-------------|---------|
| **Cache management** | ✅ Automatic | ✅ Automatic | ✅ Automatic |
| **Background refetch** | ✅ | ✅ | ✅ |
| **Optimistic updates** | ✅ | ✅ | ✅ |
| **Redux integration** | ✅ Built-in | ❌ | ❌ |
| **GraphQL support** | ❌ | ✅ Limited | ✅ Native |
| **Bundle size** | Medium | Small | Large |
| **Learning curve** | Medium | Low | High |

**RTK Query is best when**: Already using Redux, need tight integration, REST APIs
</details>

---

## 🔹 **Rendering & Performance**

---

### 🎯 **Virtual DOM**

#### **📋 Beginner:**
- **Q1:** What is the Virtual DOM and why does React use it?
<details>
<summary>Answer</summary>
The Virtual DOM is a **JavaScript representation** of the real DOM kept in memory. React uses it because:
- **Performance**: Faster to manipulate JavaScript objects than DOM
- **Predictability**: Declarative updates are easier to reason about
- **Batching**: Multiple changes can be batched into a single DOM update
- **Cross-browser**: Abstracts browser differences
</details>

- **Q2:** How does the Virtual DOM improve performance?
<details>
<summary>Answer</summary>
Virtual DOM improves performance through:
1. **Diffing**: Compares new virtual DOM with previous version
2. **Minimal updates**: Only changes necessary DOM nodes
3. **Batching**: Groups multiple updates together
4. **Avoiding layout thrashing**: Reduces browser reflows/repaints

```jsx
// React calculates minimal changes needed
// Old: <div><span>0</span></div>
// New: <div><span>1</span></div>
// Only updates the text content, not the entire structure
```
</details>

- **Q3:** Is the Virtual DOM always faster than direct DOM manipulation?
<details>
<summary>Answer</summary>
**No**, Virtual DOM isn't always faster. It's slower for:
- **Simple, targeted updates**: Direct DOM manipulation can be faster
- **Single element changes**: Virtual DOM adds overhead
- **Very small applications**: The abstraction cost outweighs benefits

Virtual DOM is faster for:
- **Complex UIs** with many elements
- **Frequent updates** across multiple components
- **Declarative programming** where manual optimization is hard
</details>

#### **🚀 Intermediate:**
- **Q1:** Describe the process from state change to DOM update in React.
<details>
<summary>Answer</summary>
1. **State change triggered** (setState, hooks)
2. **Component re-render** scheduled
3. **New Virtual DOM tree** created
4. **Diffing algorithm** compares old vs new Virtual DOM
5. **Reconciliation** determines minimal changes needed
6. **DOM mutations** applied to real DOM
7. **Lifecycle methods** called (useEffect, etc.)
8. **Browser re-paints** the updated UI
</details>

- **Q2:** What are the limitations of the Virtual DOM approach?
<details>
<summary>Answer</summary>
**Limitations:**
- **Memory overhead**: Keeps two DOM trees in memory
- **Not always optimal**: Can be slower for simple updates
- **Learning curve**: Abstraction layer to understand
- **Bundle size**: Additional JavaScript code
- **CPU overhead**: Diffing algorithm computation
- **Not magic**: Still requires good programming practices
</details>

---

### 🎯 **Reconciliation**

#### **📋 Beginner:**
- **Q1:** What is reconciliation in React?
<details>
<summary>Answer</summary>
Reconciliation is the **process of comparing** the new Virtual DOM tree with the previous one and determining what changes need to be made to the real DOM. It's React's "diffing" algorithm that makes updates efficient.
</details>

- **Q2:** What triggers the reconciliation process?
<details>
<summary>Answer</summary>
Reconciliation is triggered by:
- **State updates** (useState, setState)
- **Props changes** from parent components
- **Context value changes**
- **Parent component re-renders**
- **forceUpdate()** calls (not recommended)
</details>

- **Q3:** How does React handle different element types during reconciliation?
<details>
<summary>Answer</summary>
React handles different scenarios:
- **Same element type**: Compares attributes and recurses on children
- **Different element types**: Destroys old tree and builds new one
- **Component elements**: Calls component lifecycle methods

```jsx
// Same type - updates className only
<div className="old" /> → <div className="new" />

// Different type - destroys <div>, creates <span>
<div>Hello</div> → <span>Hello</span>
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How does React's reconciliation algorithm achieve O(n) complexity?
<details>
<summary>Answer</summary>
React's algorithm is O(n) through these assumptions:
1. **Different element types** produce different trees (no cross-comparison)
2. **Keys help identify** which children have changed, moved, or removed
3. **Level-by-level comparison** instead of full tree comparison
4. **Component boundaries** limit comparison scope

Without these assumptions, general tree diffing would be O(n³).
</details>

- **Q2:** What happens when component types change during reconciliation?
<details>
<summary>Answer</summary>
When component types change:
1. **Old component unmounts** (componentWillUnmount, cleanup effects)
2. **Old DOM nodes removed**
3. **New component mounts** (constructor, componentDidMount, effects)
4. **New DOM nodes inserted**
5. **State is lost** (components are completely different instances)

```jsx
// This change destroys UserProfile and creates AdminProfile
{isAdmin ? <AdminProfile /> : <UserProfile />}
```
</details>

---

### 🎯 **Diffing algorithm**

#### **📋 Beginner:**
- **Q1:** What assumptions does React's diffing algorithm make?
<details>
<summary>Answer</summary>
React's diffing algorithm makes two key assumptions:
1. **Elements of different types** will produce different trees
2. **Developers can hint** at which child elements may be stable across renders using keys

These assumptions allow React to use a fast O(n) algorithm instead of O(n³).
</details>

- **Q2:** How does React handle list reconciliation?
<details>
<summary>Answer</summary>
For lists, React:
1. **Compares children by position** if no keys provided
2. **Uses keys to match elements** across renders when available
3. **Preserves component state** for elements with stable keys
4. **Efficiently reorders** elements when keys indicate movement

```jsx
// Without keys - poor performance on reorder
{items.map(item => <Item data={item} />)}

// With keys - efficient reordering
{items.map(item => <Item key={item.id} data={item} />)}
```
</details>

- **Q3:** Why are keys important for the diffing algorithm?
<details>
<summary>Answer</summary>
Keys are important because they:
- **Help React identify** which items have changed, moved, or been removed
- **Preserve component state** during list reordering
- **Improve performance** by avoiding unnecessary DOM manipulations
- **Prevent bugs** where component state gets mixed up
</details>

#### **🚀 Intermediate:**
- **Q1:** What are the performance implications of changing all keys in a list?
<details>
<summary>Answer</summary>
Changing all keys forces React to:
- **Unmount all existing components** in the list
- **Mount completely new components** for each item
- **Lose all component state** (like input focus, scroll position)
- **Perform expensive DOM operations** (create/destroy elements)

```jsx
// Bad - generates new keys every render
{items.map((item, index) => 
  <Item key={Math.random()} data={item} />
)}

// Good - stable keys
{items.map(item => 
  <Item key={item.id} data={item} />
)}
```
</details>

- **Q2:** How can poor key choices lead to bugs?
<details>
<summary>Answer</summary>
Poor key choices can cause:
- **State preservation bugs**: Input values staying with wrong items
- **Animation glitches**: Transitions applying to wrong elements
- **Performance issues**: Unnecessary re-renders and DOM operations
- **Focus management problems**: Tab order and focus getting confused

```jsx
// Bug: using array index as key during reordering
const [items, setItems] = useState([{name: 'A'}, {name: 'B'}]);

// When reordered, state sticks to index position, not logical item
{items.map((item, index) => 
  <input key={index} defaultValue={item.name} />
)}
```
</details>

---

### 🎯 **Controlled vs uncontrolled components**

#### **📋 Beginner:**
- **Q1:** What's the difference between controlled and uncontrolled components?
<details>
<summary>Answer</summary>
- **Controlled components**: Form data is handled by React state. React controls the input value.
- **Uncontrolled components**: Form data is handled by the DOM itself. Uses refs to access values.


```jsx
// Controlled
function ControlledInput() {
  const [value, setValue] = useState('');
  return (
    <input 
      value={value} 
      onChange={(e) => setValue(e.target.value)} 
    />
  );
}

// Uncontrolled
function UncontrolledInput() {
  const inputRef = useRef();
  return <input ref={inputRef} defaultValue="initial" />;
}
```
</details>

- **Q2:** How do you create a controlled input component?
<details>
<summary>Answer</summary>

```jsx
function ControlledForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };
  
  return (
    <form>
      <input 
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input 
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <textarea 
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
    </form>
  );
}
```
</details>

- **Q3:** When might you use an uncontrolled component?
<details>
<summary>Answer</summary>
Use uncontrolled components when:
- **Simple forms** where you only need the value on submit
- **File inputs** (always uncontrolled in React)
- **Integration with non-React libraries**
- **Performance optimization** for large forms
- **Minimal validation** requirements

```jsx
function UncontrolledForm() {
  const formRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const formData = new FormData(formRef.current);
    const data = Object.fromEntries(formData);
    console.log(data);
  };
  
  return (
    <form ref={formRef} onSubmit={handleSubmit}>
      <input name="username" defaultValue="" />
      <input type="file" name="avatar" />
      <button type="submit">Submit</button>
    </form>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What are the trade-offs between controlled and uncontrolled components?
<details>
<summary>Answer</summary>
**Controlled Components:**
- ✅ **Pros**: Immediate validation, dynamic behavior, consistent with React patterns
- ❌ **Cons**: More code, potential performance issues with large forms

**Uncontrolled Components:**
- ✅ **Pros**: Less code, better performance, easier integration with non-React code
- ❌ **Cons**: Less control, harder validation, not reactive to state changes

| Feature | Controlled | Uncontrolled |
|---------|------------|--------------|
| **Data source** | React state | DOM |
| **Validation** | Real-time | On submit |
| **Performance** | Can be slower | Faster |
| **Code complexity** | Higher | Lower |
</details>

- **Q2:** How do you handle file inputs (which are always uncontrolled)?
<details>
<summary>Answer</summary>
File inputs are always uncontrolled because their value cannot be set programmatically for security reasons:

```jsx
function FileUpload() {
  const [selectedFile, setSelectedFile] = useState(null);
  const [preview, setPreview] = useState(null);
  
  const handleFileChange = (e) => {
    const file = e.target.files[0];
    setSelectedFile(file);
    
    if (file && file.type.startsWith('image/')) {
      const reader = new FileReader();
      reader.onloadend = () => setPreview(reader.result);
      reader.readAsDataURL(file);
    }
  };
  
  return (
    <div>
      <input 
        type="file" 
        onChange={handleFileChange}
        accept="image/*"
      />
      {preview && <img src={preview} alt="Preview" />}
      {selectedFile && <p>Selected: {selectedFile.name}</p>}
    </div>
  );
}
```
</details>

---

### 🎯 **Memoization (React.memo, useMemo, useCallback)**

#### **📋 Beginner:**
- **Q1:** What is React.memo and when should you use it?
<details>
<summary>Answer</summary>
`React.memo` is a **higher-order component** that memoizes the result of a component. It only re-renders if props change (shallow comparison):

```jsx
const ExpensiveComponent = React.memo(({ name, age }) => {
  console.log('Rendering ExpensiveComponent');
  return <div>{name} is {age} years old</div>;
});

// Only re-renders when name or age props change
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <ExpensiveComponent name="John" age={30} />
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
    </>
  );
}
```
**Use when**: Component renders often with same props, expensive rendering logic
</details>

- **Q2:** How do React.memo, useMemo, and useCallback work together?
<details>
<summary>Answer</summary>
They work together to optimize performance:
- **React.memo**: Prevents component re-renders
- **useMemo**: Memoizes expensive calculations
- **useCallback**: Memoizes function references


```jsx
const Child = React.memo(({ items, onItemClick }) => {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id} onClick={() => onItemClick(item.id)}>
          {item.name}
        </li>
      ))}
    </ul>
  );
});

function Parent({ data }) {
  const [count, setCount] = useState(0);
  
  // Memoize expensive calculation
  const expensiveItems = useMemo(() => {
    return data.filter(item => item.active).sort((a, b) => a.name.localeCompare(b.name));
  }, [data]);
  
  // Memoize callback to prevent Child re-renders
  const handleItemClick = useCallback((id) => {
    console.log('Clicked item:', id);
  }, []);
  
  return (
    <>
      <Child items={expensiveItems} onItemClick={handleItemClick} />
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
    </>
  );
}
```
</details>

- **Q3:** When does React.memo not prevent re-renders?
<details>
<summary>Answer</summary>
React.memo doesn't prevent re-renders when:
- **Props contain new object/array references** each render
- **Props contain functions** that are recreated each render
- **Context value changes** (for context consumers)
- **Parent uses children** as props

```jsx
// ❌ These cause re-renders despite React.memo
<MemoComponent 
  data={{ name: 'John' }}  // New object each render
  onClick={() => {}}       // New function each render
  style={{ color: 'red' }} // New object each render
/>

// ✅ These don't cause unnecessary re-renders
const data = useMemo(() => ({ name: 'John' }), []);
const onClick = useCallback(() => {}, []);
const style = useMemo(() => ({ color: 'red' }), []);
```
</details>

#### **🚀 Intermediate:**
- **Q1:** When can memoization actually hurt performance?
<details>
<summary>Answer</summary>
Memoization can hurt performance when:
- **Overused on cheap calculations**: Memoization overhead > calculation cost
- **Dependencies change frequently**: Defeats the purpose
- **Used everywhere**: Unnecessary memory usage and complexity
- **Complex comparison logic**: Custom comparison functions are expensive

```jsx
// ❌ Anti-patterns
const sum = useMemo(() => a + b, [a, b]); // Too simple
const obj = useMemo(() => ({ ...data }), [data]); // Dependencies change often

// ✅ Good use cases
const expensiveCalculation = useMemo(() => {
  return heavyProcessing(largeDataset);
}, [largeDataset]);
```
</details>

- **Q2:** How do you properly memoize a component with object props?
<details>
<summary>Answer</summary>

```jsx
// Option 1: Memoize the object prop
function Parent() {
  const [name, setName] = useState('John');
  const [age, setAge] = useState(30);
  
  const user = useMemo(() => ({ name, age }), [name, age]);
  
  return <UserProfile user={user} />;
}

// Option 2: Custom comparison function
const UserProfile = React.memo(({ user, settings }) => {
  return <div>{user.name} - {settings.theme}</div>;
}, (prevProps, nextProps) => {
  return (
    prevProps.user.name === nextProps.user.name &&
    prevProps.user.age === nextProps.user.age &&
    prevProps.settings.theme === nextProps.settings.theme
  );
});

// Option 3: Destructure props to avoid object comparison
const UserCard = React.memo(({ name, age, theme }) => {
  return <div className={theme}>{name} is {age}</div>;
});
```
</details>

---

### 🎯 **Code splitting & lazy loading**

#### **📋 Beginner:**
- **Q1:** What is code splitting and why is it useful?
<details>
<summary>Answer</summary>
Code splitting is **dividing your bundle** into smaller chunks that can be loaded on demand. Benefits:
- **Faster initial load**: Only load what's needed immediately
- **Better user experience**: App starts faster
- **Reduced bandwidth**: Users don't download unused code
- **Improved caching**: Changes to one part don't invalidate entire bundle

```jsx
// Instead of one large bundle
import HomePage from './HomePage';
import AboutPage from './AboutPage';
import ContactPage from './ContactPage';

// Split into separate chunks loaded when needed
const HomePage = lazy(() => import('./HomePage'));
const AboutPage = lazy(() => import('./AboutPage'));
const ContactPage = lazy(() => import('./ContactPage'));
```
</details>

- **Q2:** How do you implement lazy loading with React.lazy?
<details>
<summary>Answer</summary>

```jsx
import { Suspense, lazy } from 'react';

// Lazy load components
const Dashboard = lazy(() => import('./Dashboard'));
const Profile = lazy(() => import('./Profile'));
const Settings = lazy(() => import('./Settings'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/settings" element={<Settings />} />
        </Routes>
      </Suspense>
    </div>
  );
}
```
</details>

- **Q3:** What is Suspense and how does it work with lazy loading?
<details>
<summary>Answer</summary>
`Suspense` is a component that **handles loading states** for lazy components. It:
- **Catches loading promises** from lazy components
- **Shows fallback UI** while loading
- **Automatically renders** the component when loaded

```jsx
<Suspense fallback={<LoadingSpinner />}>
  <LazyComponent />
</Suspense>

// Can wrap multiple lazy components
<Suspense fallback={<div>Loading page...</div>}>
  <Header />
  <LazyMainContent />
  <LazySidebar />
</Suspense>
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What are the best strategies for code splitting in a React app?
<details>
<summary>Answer</summary>
**Effective code splitting strategies:**

1. **Route-based splitting**: Split by pages/routes

```jsx
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
```

2. **Feature-based splitting**: Split by functionality

```jsx
const AdminPanel = lazy(() => import('./features/admin/AdminPanel'));
const UserDashboard = lazy(() => import('./features/user/Dashboard'));
```

3. **Component-based splitting**: Split heavy components

```jsx
const DataVisualization = lazy(() => import('./components/DataVisualization'));
```

4. **Library splitting**: Split third-party libraries

```jsx
const ChartComponent = lazy(() => 
  import('./ChartComponent').then(module => ({
    default: module.ChartComponent
  }))
);
```

5. **Conditional splitting**: Load based on user permissions

```jsx
const AdminTools = lazy(() => 
  userRole === 'admin' 
    ? import('./AdminTools')
    : Promise.resolve({ default: () => null })
);
```
</details>

- **Q2:** How do you handle loading errors with lazy components?
<details>
<summary>Answer</summary>

```jsx
import { Suspense, lazy } from 'react';

// Error boundary for lazy loading errors
class LazyErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Lazy loading error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong loading this page.</h2>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }
    return this.props.children;
  }
}

// Usage with retry logic
const LazyComponent = lazy(() =>
  import('./MyComponent').catch(error => {
    console.error('Failed to load component:', error);
    // Return fallback component
    return { default: () => <div>Failed to load component</div> };
  })
);

function App() {
  return (
    <LazyErrorBoundary>
      <Suspense fallback={<LoadingSpinner />}>
        <LazyComponent />
      </Suspense>
    </LazyErrorBoundary>
  );
}
```
</details>

---

### 🎯 **Suspense**

#### **📋 Beginner:**
- **Q1:** What is Suspense and what can it be used for?
<details>
<summary>Answer</summary>
`Suspense` is a React component that lets you **handle loading states** declaratively. Currently used for:
- **Lazy loading components** with React.lazy
- **Code splitting** at the component level
- **Future**: Data fetching (experimental)

```jsx
<Suspense fallback={<LoadingSpinner />}>
  <LazyComponent />
</Suspense>
```
It catches "loading promises" thrown by components and shows fallback UI until resolved.
</details>

- **Q2:** How do you use Suspense with React.lazy?
<details>
<summary>Answer</summary>

```jsx
import { Suspense, lazy } from 'react';

// Create lazy component
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<div>Loading heavy component...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}

// Can also wrap multiple lazy components
<Suspense fallback={<PageLoader />}>
  <Header />
  <LazyMainContent />
  <LazySidebar />
  <Footer />
</Suspense>
```
</details>

- **Q3:** What is a fallback in Suspense?
<details>
<summary>Answer</summary>
A **fallback** is the UI shown while suspended components are loading. It can be:
- **Simple text**: `fallback="Loading..."`
- **Components**: `fallback={<LoadingSpinner />}`
- **Complex UI**: `fallback={<SkeletonLoader />}`

```jsx
// Different fallback examples
<Suspense fallback="Loading...">
<Suspense fallback={<div className="spinner" />}>
<Suspense fallback={
  <div className="loading-container">
    <h3>Loading awesome content...</h3>
    <ProgressBar />
  </div>
}>
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How will Suspense work with data fetching in the future?
<details>
<summary>Answer</summary>
**Future Suspense for data fetching** (experimental):
- Components can **"suspend"** while data loads
- **Declarative loading states** without useEffect
- **Automatic coordination** of multiple loading operations
- **Better user experience** with coordinated loading


```jsx
// Future experimental pattern
function UserProfile({ userId }) {
  // This would suspend the component until data loads
  const user = use(fetchUser(userId)); // Experimental 'use' hook
  
  return <div>{user.name}</div>;
}

function App() {
  return (
    <Suspense fallback={<ProfileSkeleton />}>
      <UserProfile userId={123} />
    </Suspense>
  );
}
```
**Note**: This is still experimental and APIs may change.
</details>

- **Q2:** How do you handle nested Suspense boundaries?
<details>
<summary>Answer</summary>
Nested Suspense boundaries allow **granular loading states**:

```jsx
function App() {
  return (
    <Suspense fallback={<PageLoader />}>
      <Header />
      
      <main>
        <Suspense fallback={<SidebarSkeleton />}>
          <LazySidebar />
        </Suspense>
        
        <section>
          <Suspense fallback={<ContentSkeleton />}>
            <LazyMainContent />
            
            <Suspense fallback={<WidgetLoader />}>
              <LazyWidget />
            </Suspense>
          </Suspense>
        </section>
      </main>
      
      <Suspense fallback={<FooterSkeleton />}>
        <LazyFooter />
      </Suspense>
    </Suspense>
  );
}
```

**Benefits of nesting:**
- **Independent loading**: Each boundary loads independently
- **Better UX**: Show content as it becomes available
- **Granular fallbacks**: Different loading UI for different sections
- **Parallel loading**: Multiple components can load simultaneously
</details>

---

## 🔹 **Routing**

---

### 🎯 **React Router basics (BrowserRouter, Route, Link)**

#### **📋 Beginner:**
- **Q1:** What is React Router and why is it needed?
<details>
<summary>Answer</summary>
React Router is a **client-side routing library** for React that enables navigation between different components/pages without full page refreshes. It's needed because:
- **SPAs need routing**: Single Page Applications need to handle multiple views
- **URL synchronization**: Keep URL in sync with UI state
- **Navigation**: Handle browser back/forward buttons
- **Deep linking**: Allow users to bookmark specific pages

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```
</details>

- **Q2:** What's the difference between BrowserRouter and HashRouter?
<details>
<summary>Answer</summary>
| BrowserRouter | HashRouter |
|---------------|------------|
| **URL format**: `/home/profile` | **URL format**: `/#/home/profile` |
| **Uses HTML5 History API** | **Uses URL hash** |
| **Requires server config** | **Works without server config** |
| **Cleaner URLs** | **URLs have # symbol** |
| **Better for production** | **Better for static hosting** |


```jsx
// BrowserRouter - clean URLs
<BrowserRouter>
  <Routes>...</Routes>
</BrowserRouter>
// URLs: example.com/home, example.com/about

// HashRouter - hash URLs  
<HashRouter>
  <Routes>...</Routes>
</HashRouter>
// URLs: example.com/#/home, example.com/#/about
```
</details>

- **Q3:** How do you create basic routes using Route components?
<details>
<summary>Answer</summary>

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/products">Products</Link>
        <Link to="/about">About</Link>
      </nav>
      
      <Routes>
        {/* Exact path matching */}
        <Route path="/" element={<HomePage />} />
        <Route path="/products" element={<ProductsPage />} />
        <Route path="/about" element={<AboutPage />} />
        
        {/* Dynamic route with parameter */}
        <Route path="/products/:id" element={<ProductDetail />} />
        
        {/* Catch-all route for 404 */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** What's the difference between Link and regular anchor tags?
<details>
<summary>Answer</summary>
| Link | Anchor Tag |
|------|------------|
| **Client-side navigation** | **Full page reload** |
| **Preserves app state** | **Loses app state** |
| **Faster navigation** | **Slower (page reload)** |
| **Uses History API** | **Browser navigation** |
| **SPA behavior** | **Traditional web behavior** |


```jsx
// ❌ Anchor - causes page reload
<a href="/about">About</a>

// ✅ Link - client-side navigation
<Link to="/about">About</Link>

// NavLink - active state support
<NavLink 
  to="/about" 
  className={({ isActive }) => isActive ? 'active' : ''}
>
  About
</NavLink>
```
</details>

- **Q2:** How do you implement nested routing layouts?
<details>
<summary>Answer</summary>

```jsx
import { Outlet } from 'react-router-dom';

// Layout component
function DashboardLayout() {
  return (
    <div className="dashboard">
      <nav>
        <Link to="/dashboard">Overview</Link>
        <Link to="/dashboard/users">Users</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <main>
        <Outlet /> {/* Child routes render here */}
      </main>
    </div>
  );
}

// App routing
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<HomePage />} />
        
        {/* Nested routes */}
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index element={<DashboardOverview />} />
          <Route path="users" element={<UsersPage />} />
          <Route path="users/:id" element={<UserDetail />} />
          <Route path="settings" element={<SettingsPage />} />
        </Route>
        
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```
</details>

---

### 🎯 **Route parameters**

#### **📋 Beginner:**
- **Q1:** How do you define route parameters in React Router?
<details>
<summary>Answer</summary>
Route parameters are defined using **colon (`:`) syntax** in the route path:

```jsx
<Routes>
  {/* Single parameter */}
  <Route path="/users/:id" element={<UserProfile />} />
  
  {/* Multiple parameters */}
  <Route path="/users/:userId/posts/:postId" element={<UserPost />} />
  
  {/* Optional parameter with ? */}
  <Route path="/products/:category/:id?" element={<ProductPage />} />
  
  {/* Wildcard parameter */}
  <Route path="/files/*" element={<FileExplorer />} />
</Routes>
```
</details>

- **Q2:** How do you access route parameters in your components?
<details>
<summary>Answer</summary>
Use the `useParams` hook to access route parameters:

```jsx
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { id } = useParams();
  
  useEffect(() => {
    fetchUser(id);
  }, [id]);
  
  return <div>User ID: {id}</div>;
}

function UserPost() {
  const { userId, postId } = useParams();
  
  return (
    <div>
      <h1>User {userId}</h1>
      <h2>Post {postId}</h2>
    </div>
  );
}

// With TypeScript
function TypedComponent() {
  const { id } = useParams<{ id: string }>();
  // id is typed as string | undefined
}
```
</details>

- **Q3:** What happens if a required parameter is missing?
<details>
<summary>Answer</summary>
If a required parameter is missing, the route **won't match** and React Router will:
- Continue to next route in the Routes list
- Eventually show 404/catch-all route if no other routes match
- `useParams` will return `undefined` for missing parameters


```jsx
<Routes>
  <Route path="/users/:id" element={<UserProfile />} />
  <Route path="*" element={<NotFound />} />
</Routes>

// URL: /users (missing :id parameter)
// Result: Shows NotFound component

function UserProfile() {
  const { id } = useParams();
  
  if (!id) {
    return <div>Invalid user ID</div>;
  }
  
  return <div>User: {id}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you handle optional route parameters?
<details>
<summary>Answer</summary>

```jsx
// Method 1: Optional parameter with ?
<Route path="/products/:category/:id?" element={<ProductPage />} />

function ProductPage() {
  const { category, id } = useParams();
  
  if (id) {
    return <ProductDetail category={category} id={id} />;
  } else {
    return <ProductList category={category} />;
  }
}

// Method 2: Multiple routes
<Routes>
  <Route path="/products/:category" element={<ProductList />} />
  <Route path="/products/:category/:id" element={<ProductDetail />} />
</Routes>

// Method 3: Query parameters for optional data
function ProductSearch() {
  const [searchParams] = useSearchParams();
  const category = searchParams.get('category');
  const minPrice = searchParams.get('minPrice');
  
  return <div>Search: {category}, Min: {minPrice}</div>;
}
// URL: /search?category=electronics&minPrice=100
```
</details>

- **Q2:** How do you validate route parameters?
<details>
<summary>Answer</summary>

```jsx
// Custom hook for parameter validation
function useValidatedParams(schema) {
  const params = useParams();
  const navigate = useNavigate();
  
  useEffect(() => {
    const isValid = schema.validate(params);
    if (!isValid) {
      navigate('/404', { replace: true });
    }
  }, [params, schema, navigate]);
  
  return params;
}

// Usage
function UserProfile() {
  const params = useValidatedParams({
    validate: ({ id }) => /^\d+$/.test(id) // Only numeric IDs
  });
  
  return <div>User: {params.id}</div>;
}

// Or validate in component
function ProductDetail() {
  const { id } = useParams();
  const navigate = useNavigate();
  
  useEffect(() => {
    if (!id || !/^[0-9]+$/.test(id)) {
      navigate('/products', { replace: true });
    }
  }, [id, navigate]);
  
  return <div>Product: {id}</div>;
}

// Loader-based validation (React Router v6.4+)
const productLoader = ({ params }) => {
  if (!/^\d+$/.test(params.id)) {
    throw new Response('Invalid product ID', { status: 400 });
  }
  return fetchProduct(params.id);
};

<Route 
  path="/products/:id" 
  element={<ProductDetail />}
  loader={productLoader}
/>
```
</details>

---

### 🎯 **Nested routes**

#### **📋 Beginner:**
- **Q1:** What are nested routes and how do you implement them?
<details>
<summary>Answer</summary>
Nested routes allow you to render components inside other components based on URL structure. They create **hierarchical routing**:

```jsx
import { Outlet } from 'react-router-dom';

// Parent layout component
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="/dashboard">Overview</Link>
        <Link to="/dashboard/profile">Profile</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <Outlet /> {/* Child routes render here */}
    </div>
  );
}

// Route configuration
<Routes>
  <Route path="/dashboard" element={<Dashboard />}>
    <Route index element={<DashboardHome />} />
    <Route path="profile" element={<Profile />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```
</details>

- **Q2:** How does the Outlet component work?
<details>
<summary>Answer</summary>
`Outlet` is a **placeholder component** that renders the child route's element. It:
- **Marks the spot** where child routes should render
- **Passes context** to child routes
- **Handles route matching** automatically

```jsx
function Layout() {
  return (
    <div>
      <header>Site Header</header>
      <main>
        <Outlet /> {/* Child route content appears here */}
      </main>
      <footer>Site Footer</footer>
    </div>
  );
}

// With context passing
function DashboardLayout() {
  const [user, setUser] = useState(null);
  
  return (
    <div>
      <nav>Dashboard Nav</nav>
      <Outlet context={{ user, setUser }} />
    </div>
  );
}

// Child component can access context
function ProfilePage() {
  const { user } = useOutletContext();
  return <div>Welcome, {user.name}</div>;
}
```
</details>

- **Q3:** How do you structure components for nested routes?
<details>
<summary>Answer</summary>

```jsx
// File structure
src/
  components/
    Layout.jsx          // Main app layout
    DashboardLayout.jsx // Dashboard-specific layout
  pages/
    Home.jsx
    Dashboard/
      index.jsx         // Dashboard overview
      Profile.jsx
      Settings.jsx
      Users/
        index.jsx       // Users list
        UserDetail.jsx

// Route structure
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Main layout */}
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          
          {/* Dashboard with nested routes */}
          <Route path="dashboard" element={<DashboardLayout />}>
            <Route index element={<DashboardOverview />} />
            <Route path="profile" element={<Profile />} />
            <Route path="settings" element={<Settings />} />
            
            {/* Further nested routes */}
            <Route path="users" element={<UsersLayout />}>
              <Route index element={<UsersList />} />
              <Route path=":id" element={<UserDetail />} />
            </Route>
          </Route>
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement breadcrumbs with nested routes?
<details>
<summary>Answer</summary>

```jsx
import { useMatches } from 'react-router-dom';

// Route configuration with handle for breadcrumbs
const routes = [
  {
    path: "/",
    element: <Layout />,
    handle: { crumb: () => "Home" },
    children: [
      {
        path: "dashboard",
        element: <Dashboard />,
        handle: { crumb: () => "Dashboard" },
        children: [
          {
            path: "users",
            element: <Users />,
            handle: { crumb: () => "Users" },
          },
          {
            path: "users/:id",
            element: <UserDetail />,
            handle: { 
              crumb: (match) => `User ${match.params.id}` 
            },
          }
        ]
      }
    ]
  }
];

// Breadcrumb component
function Breadcrumbs() {
  const matches = useMatches();
  
  const crumbs = matches
    .filter(match => match.handle?.crumb)
    .map(match => ({
      pathname: match.pathname,
      label: match.handle.crumb(match)
    }));
  
  return (
    <nav>
      {crumbs.map((crumb, index) => (
        <span key={crumb.pathname}>
          {index > 0 && " > "}
          {index === crumbs.length - 1 ? (
            crumb.label
          ) : (
            <Link to={crumb.pathname}>{crumb.label}</Link>
          )}
        </span>
      ))}
    </nav>
  );
}
```
</details>

- **Q2:** How do you handle data loading for nested routes?
<details>
<summary>Answer</summary>

```jsx
// Using loaders (React Router v6.4+)
const dashboardLoader = async () => {
  const user = await fetchCurrentUser();
  return { user };
};

const usersLoader = async () => {
  const users = await fetchUsers();
  return { users };
};

const userDetailLoader = async ({ params }) => {
  const user = await fetchUser(params.id);
  return { user };
};

// Route configuration
const router = createBrowserRouter([
  {
    path: "/dashboard",
    element: <Dashboard />,
    loader: dashboardLoader,
    children: [
      {
        path: "users",
        element: <Users />,
        loader: usersLoader,
        children: [
          {
            path: ":id",
            element: <UserDetail />,
            loader: userDetailLoader,
          }
        ]
      }
    ]
  }
]);

// Access loaded data in components
function Dashboard() {
  const { user } = useLoaderData();
  
  return (
    <div>
      <h1>Welcome, {user.name}</h1>
      <Outlet />
    </div>
  );
}

function UserDetail() {
  const { user } = useLoaderData();
  const parentData = useOutletContext(); // Access parent data
  
  return <div>User: {user.name}</div>;
}

// Alternative: Custom data fetching hook
function useNestedData() {
  const location = useLocation();
  const [data, setData] = useState({});
  
  useEffect(() => {
    const loadData = async () => {
      // Load data based on current route
      if (location.pathname.includes('/dashboard')) {
        const userData = await fetchUserData();
        setData(prev => ({ ...prev, user: userData }));
      }
    };
    
    loadData();
  }, [location.pathname]);
  
  return data;
}
```
</details>

---

### 🎯 **Redirects**

#### **📋 Beginner:**
- **Q1:** How do you redirect users to different routes?
<details>
<summary>Answer</summary>
You can redirect users using:

**1. Navigate component (declarative)**:

```jsx
import { Navigate } from 'react-router-dom';

function ProtectedPage() {
  const { isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <div>Protected content</div>;
}
```

**2. useNavigate hook (programmatic)**:

```jsx
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();
  
  const handleSubmit = async (credentials) => {
    const success = await login(credentials);
    if (success) {
      navigate('/dashboard'); // Redirect after login
    }
  };
  
  return <form onSubmit={handleSubmit}>...</form>;
}
```
</details>

- **Q2:** What's the difference between Navigate and useNavigate?
<details>
<summary>Answer</summary>
| Navigate Component | useNavigate Hook |
|-------------------|------------------|
| **Declarative** | **Imperative** |
| **Renders in JSX** | **Call in functions** |
| **Conditional rendering** | **Event handlers/effects** |
| **Immediate redirect** | **Programmatic control** |


```jsx
// Navigate - declarative, in render
function App() {
  return isLoggedIn ? <Dashboard /> : <Navigate to="/login" />;
}

// useNavigate - imperative, in handlers
function LoginButton() {
  const navigate = useNavigate();
  
  const handleLogin = () => {
    // Do login logic
    navigate('/dashboard');
  };
  
  return <button onClick={handleLogin}>Login</button>;
}
```
</details>

- **Q3:** How do you implement protected routes?
<details>
<summary>Answer</summary>

```jsx
// Protected Route component
function ProtectedRoute({ children }) {
  const { isAuthenticated, isLoading } = useAuth();
  
  if (isLoading) {
    return <LoadingSpinner />;
  }
  
  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}

// Usage in routes
<Routes>
  <Route path="/login" element={<LoginPage />} />
  <Route path="/dashboard" element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  } />
  <Route path="/profile" element={
    <ProtectedRoute>
      <Profile />
    </ProtectedRoute>
  } />
</Routes>

// Role-based protection
function RoleProtectedRoute({ children, allowedRoles }) {
  const { user } = useAuth();
  
  if (!allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you preserve the intended destination after login redirects?
<details>
<summary>Answer</summary>

```jsx
// Protected route saves intended destination
function ProtectedRoute({ children }) {
  const { isAuthenticated } = useAuth();
  const location = useLocation();
  
  if (!isAuthenticated) {
    return (
      <Navigate 
        to="/login" 
        state={{ from: location }} 
        replace 
      />
    );
  }
  
  return children;
}

// Login component redirects to intended destination
function LoginPage() {
  const navigate = useNavigate();
  const location = useLocation();
  
  const from = location.state?.from?.pathname || '/dashboard';
  
  const handleLogin = async (credentials) => {
    const success = await login(credentials);
    if (success) {
      navigate(from, { replace: true });
    }
  };
  
  return (
    <form onSubmit={handleLogin}>
      <p>Please log in to access {from}</p>
      {/* Login form fields */}
    </form>
  );
}

// Custom hook for redirect logic
function useRedirectAfterLogin() {
  const navigate = useNavigate();
  const location = useLocation();
  
  return useCallback((defaultPath = '/dashboard') => {
    const intendedPath = location.state?.from?.pathname || defaultPath;
    navigate(intendedPath, { replace: true });
  }, [navigate, location.state]);
}
```
</details>

- **Q2:** How do you handle programmatic navigation with state?
<details>
<summary>Answer</summary>

```jsx
function ProductList() {
  const navigate = useNavigate();
  
  const handleProductClick = (product) => {
    navigate(`/products/${product.id}`, {
      state: { 
        product,
        returnTo: '/products',
        searchFilters: currentFilters 
      }
    });
  };
  
  return (
    <div>
      {products.map(product => (
        <div key={product.id} onClick={() => handleProductClick(product)}>
          {product.name}
        </div>
      ))}
    </div>
  );
}

function ProductDetail() {
  const location = useLocation();
  const navigate = useNavigate();
  
  // Access passed state
  const { product, returnTo, searchFilters } = location.state || {};
  
  const handleBack = () => {
    if (returnTo) {
      navigate(returnTo, { state: { filters: searchFilters } });
    } else {
      navigate(-1); // Go back in history
    }
  };
  
  return (
    <div>
      <button onClick={handleBack}>← Back</button>
      <h1>{product?.name}</h1>
      {/* Product details */}
    </div>
  );
}

// Navigation with replace
function FormPage() {
  const navigate = useNavigate();
  
  const handleSubmit = async (data) => {
    await saveData(data);
    
    // Replace current entry in history (prevent back to form)
    navigate('/success', { 
      replace: true,
      state: { message: 'Data saved successfully!' }
    });
  };
  
  // Navigation with relative paths
  const handleCancel = () => {
    navigate('..', { relative: 'path' }); // Go up one level
  };
}
```
</details>

---

### 🎯 **Hooks in React Router (useParams, useNavigate)**

#### **📋 Beginner:**
- **Q1:** What does useParams return and how do you use it?
<details>
<summary>Answer</summary>
`useParams` returns an **object containing route parameters** as key-value pairs:

```jsx
import { useParams } from 'react-router-dom';

// Route: /users/:userId/posts/:postId
function BlogPost() {
  const params = useParams();
  // params = { userId: "123", postId: "456" }
  
  const { userId, postId } = useParams();
  
  useEffect(() => {
    fetchPost(userId, postId);
  }, [userId, postId]);
  
  return (
    <div>
      <h1>User {userId} - Post {postId}</h1>
    </div>
  );
}

// With TypeScript
function TypedComponent() {
  const { id } = useParams<{ id: string }>();
  // id is typed as string | undefined
}
```
</details>

- **Q2:** How do you navigate programmatically using useNavigate?
<details>
<summary>Answer</summary>

```jsx
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();
  
  // Basic navigation
  const goToHome = () => navigate('/');
  const goToProfile = () => navigate('/profile');
  
  // Navigation with parameters
  const goToUser = (userId) => navigate(`/users/${userId}`);
  
  // Navigation with state
  const goToProductWithData = (product) => {
    navigate('/products/' + product.id, {
      state: { product, fromPage: 'search' }
    });
  };
  
  // Replace current entry (no back button)
  const replaceWithLogin = () => {
    navigate('/login', { replace: true });
  };
  
  // Relative navigation
  const goBack = () => navigate(-1);
  const goForward = () => navigate(1);
  const goUp = () => navigate('..', { relative: 'path' });
  
  return (
    <div>
      <button onClick={goToHome}>Home</button>
      <button onClick={goBack}>Back</button>
    </div>
  );
}
```
</details>

- **Q3:** What other React Router hooks are commonly used?
<details>
<summary>Answer</summary>
**Common React Router hooks:**


```jsx
import { 
  useLocation,
  useSearchParams, 
  useNavigationType,
  useMatches,
  useOutletContext 
} from 'react-router-dom';

function ComponentUsingHooks() {
  // Current location object
  const location = useLocation();
  console.log(location.pathname, location.search, location.state);
  
  // Query parameters
  const [searchParams, setSearchParams] = useSearchParams();
  const category = searchParams.get('category');
  const page = parseInt(searchParams.get('page')) || 1;
  
  // Navigation type (POP, PUSH, REPLACE)
  const navigationType = useNavigationType();
  
  // All current route matches
  const matches = useMatches();
  
  // Context from parent Outlet
  const outletContext = useOutletContext();
  
  const updateSearch = () => {
    setSearchParams({ category: 'electronics', page: '2' });
  };
  
  return <div>Current page: {page}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you access query parameters and location state?
<details>
<summary>Answer</summary>

```jsx
import { useSearchParams, useLocation } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const location = useLocation();
  
  // Query parameters
  const query = searchParams.get('q') || '';
  const category = searchParams.get('category') || 'all';
  const page = parseInt(searchParams.get('page')) || 1;
  
  // Location state (passed via navigate)
  const { filters, sortBy } = location.state || {};
  
  // Update query parameters
  const handleSearch = (newQuery) => {
    setSearchParams(prev => {
      const newParams = new URLSearchParams(prev);
      newParams.set('q', newQuery);
      newParams.set('page', '1'); // Reset page
      return newParams;
    });
  };
  
  // Preserve existing params while updating
  const handleCategoryChange = (newCategory) => {
    setSearchParams(prev => ({
      ...Object.fromEntries(prev),
      category: newCategory,
      page: '1'
    }));
  };
  
  // Clear specific parameter
  const clearCategory = () => {
    setSearchParams(prev => {
      const newParams = new URLSearchParams(prev);
      newParams.delete('category');
      return newParams;
    });
  };
  
  return (
    <div>
      <input 
        value={query} 
        onChange={(e) => handleSearch(e.target.value)} 
      />
      <p>Page {page} of {category} results</p>
      {filters && <p>Applied filters: {JSON.stringify(filters)}</p>}
    </div>
  );
}

// Custom hook for search params
function useSearchParam(key, defaultValue) {
  const [searchParams, setSearchParams] = useSearchParams();
  
  const value = searchParams.get(key) || defaultValue;
  
  const setValue = useCallback((newValue) => {
    setSearchParams(prev => {
      const newParams = new URLSearchParams(prev);
      if (newValue === null || newValue === undefined) {
        newParams.delete(key);
      } else {
        newParams.set(key, String(newValue));
      }
      return newParams;
    });
  }, [key, setSearchParams]);
  
  return [value, setValue];
}
```
</details>

- **Q2:** How do you implement navigation guards with React Router hooks?
<details>
<summary>Answer</summary>

```jsx
import { useEffect, useCallback } from 'react';
import { useNavigate, useLocation, useBlocker } from 'react-router-dom';

// Custom hook for navigation guards
function useNavigationGuard(shouldBlock, message = 'Are you sure you want to leave?') {
  const navigate = useNavigate();
  
  // Block navigation when form is dirty
  const blocker = useBlocker(shouldBlock);
  
  useEffect(() => {
    if (blocker.state === 'blocked') {
      const proceed = window.confirm(message);
      if (proceed) {
        blocker.proceed();
      } else {
        blocker.reset();
      }
    }
  }, [blocker, message]);
  
  // Block browser back/forward/refresh
  useEffect(() => {
    if (shouldBlock) {
      const handleBeforeUnload = (e) => {
        e.preventDefault();
        e.returnValue = message;
        return message;
      };
      
      window.addEventListener('beforeunload', handleBeforeUnload);
      return () => window.removeEventListener('beforeunload', handleBeforeUnload);
    }
  }, [shouldBlock, message]);
}

// Usage in form component
function FormWithGuard() {
  const [formData, setFormData] = useState({});
  const [isDirty, setIsDirty] = useState(false);
  
  useNavigationGuard(isDirty, 'You have unsaved changes. Are you sure you want to leave?');
  
  const handleInputChange = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
    setIsDirty(true);
  };
  
  const handleSubmit = async () => {
    await saveForm(formData);
    setIsDirty(false); // Allow navigation after save
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
    </form>
  );
}

// Route-level guards
function ProtectedRouteGuard({ children, canAccess }) {
  const navigate = useNavigate();
  const location = useLocation();
  
  useEffect(() => {
    if (!canAccess) {
      navigate('/unauthorized', { 
        state: { from: location },
        replace: true 
      });
    }
  }, [canAccess, navigate, location]);
  
  if (!canAccess) {
    return <div>Checking permissions...</div>;
  }
  
  return children;
}
```
</details>

---

## 🔹 **Error Handling**

---

### 🎯 **Error boundaries**

#### **📋 Beginner:**
- **Q1:** What are Error Boundaries and what errors do they catch?
<details>
<summary>Answer</summary>
Error Boundaries are **React components that catch JavaScript errors** anywhere in their child component tree. They catch errors during:
- **Rendering**
- **Lifecycle methods**
- **Constructors of the whole tree below them**


```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```
</details>

- **Q2:** How do you create an Error Boundary?
<details>
<summary>Answer</summary>

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { 
      hasError: false, 
      error: null, 
      errorInfo: null 
    };
  }
  
  static getDerivedStateFromError(error) {
    // Update state to show fallback UI
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log error details
    this.setState({
      error: error,
      errorInfo: errorInfo
    });
    
    // Log to error reporting service
    console.error('Error Boundary caught an error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong.</h2>
          {process.env.NODE_ENV === 'development' && (
            <details style={{ whiteSpace: 'pre-wrap' }}>
              <summary>Error details (dev only)</summary>
              {this.state.error && this.state.error.toString()}
              <br />
              {this.state.errorInfo.componentStack}
            </details>
          )}
        </div>
      );
    }
    
    return this.props.children;
  }
}
```
</details>

- **Q3:** What errors do Error Boundaries NOT catch?
<details>
<summary>Answer</summary>
Error Boundaries **do NOT catch** errors in:
- **Event handlers** (use try-catch instead)
- **Asynchronous code** (setTimeout, promises, async/await)
- **Server-side rendering**
- **Errors in the error boundary itself**


```jsx
// ❌ These errors are NOT caught by Error Boundaries
function MyComponent() {
  const handleClick = () => {
    throw new Error('Event handler error'); // Not caught
  };
  
  useEffect(() => {
    setTimeout(() => {
      throw new Error('Async error'); // Not caught
    }, 1000);
  }, []);
  
  const handleAsync = async () => {
    await Promise.reject('Promise error'); // Not caught
  };
  
  return <button onClick={handleClick}>Click me</button>;
}

// ✅ Handle these errors manually
function MyComponent() {
  const handleClick = () => {
    try {
      riskyOperation();
    } catch (error) {
      console.error('Event error:', error);
    }
  };
  
  useEffect(() => {
    const timer = setTimeout(() => {
      try {
        riskyAsyncOperation();
      } catch (error) {
        console.error('Async error:', error);
      }
    }, 1000);
    
    return () => clearTimeout(timer);
  }, []);
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement error logging in Error Boundaries?
<details>
<summary>Answer</summary>

```jsx
// Error reporting service integration
import * as Sentry from '@sentry/react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, eventId: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log to console in development
    if (process.env.NODE_ENV === 'development') {
      console.error('Error Boundary caught error:', error);
      console.error('Component stack:', errorInfo.componentStack);
    }
    
    // Send to error tracking service
    const eventId = Sentry.captureException(error, {
      contexts: {
        react: {
          componentStack: errorInfo.componentStack
        }
      },
      tags: {
        section: this.props.section || 'unknown'
      }
    });
    
    this.setState({ eventId });
    
    // Custom error reporting
    this.reportError(error, errorInfo);
  }
  
  reportError = (error, errorInfo) => {
    const errorReport = {
      message: error.message,
      stack: error.stack,
      componentStack: errorInfo.componentStack,
      timestamp: new Date().toISOString(),
      userAgent: navigator.userAgent,
      url: window.location.href,
      userId: this.props.userId
    };
    
    // Send to your error tracking service
    fetch('/api/errors', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(errorReport)
    }).catch(err => console.error('Failed to report error:', err));
  };
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Oops! Something went wrong</h2>
          <p>We've been notified about this error.</p>
          {this.state.eventId && (
            <p>Error ID: {this.state.eventId}</p>
          )}
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```
</details>

- **Q2:** How do you reset Error Boundaries after an error?
<details>
<summary>Answer</summary>

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
  }
  
  // Reset error state
  resetError = () => {
    this.setState({ hasError: false, error: null });
  };
  
  // Reset when key prop changes
  componentDidUpdate(prevProps) {
    if (prevProps.resetKey !== this.props.resetKey) {
      this.resetError();
    }
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-fallback">
          <h2>Something went wrong</h2>
          <button onClick={this.resetError}>Try again</button>
          {this.props.fallback && this.props.fallback(this.state.error, this.resetError)}
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Usage with reset functionality
function App() {
  const [resetKey, setResetKey] = useState(0);
  
  const handleReset = () => {
    setResetKey(prev => prev + 1);
  };
  
  return (
    <ErrorBoundary 
      resetKey={resetKey}
      fallback={(error, reset) => (
        <div>
          <p>Error: {error.message}</p>
          <button onClick={reset}>Reset</button>
          <button onClick={handleReset}>Force Reset</button>
        </div>
      )}
    >
      <MyApp />
    </ErrorBoundary>
  );
}

// Using react-error-boundary library (recommended)
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <h2>Something went wrong:</h2>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function MyApp() {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onReset={() => window.location.reload()}
      resetKeys={[user.id]} // Reset when user changes
    >
      <App />
    </ErrorBoundary>
  );
}
```
</details>

---

### 🎯 **componentDidCatch**

#### **📋 Beginner:**
- **Q1:** What is componentDidCatch and when is it called?
<details>
<summary>Answer</summary>
`componentDidCatch` is a **lifecycle method** called when an error is thrown by any component in the component tree below. It's called during the **"commit" phase** and allows you to:
- **Log error information**
- **Send error reports to services**
- **Perform side effects** (unlike getDerivedStateFromError)


```jsx
class ErrorBoundary extends React.Component {
  componentDidCatch(error, errorInfo) {
    // Called when a child component throws an error
    console.log('Error:', error);
    console.log('Error Info:', errorInfo);
    
    // errorInfo.componentStack shows where error occurred
  }
  
  render() {
    return this.props.children;
  }
}
```
</details>

- **Q2:** What's the difference between componentDidCatch and getDerivedStateFromError?
<details>
<summary>Answer</summary>
| componentDidCatch | getDerivedStateFromError |
|-------------------|--------------------------|
| **Side effects allowed** | **Pure function only** |
| **Called during commit phase** | **Called during render phase** |
| **For logging/reporting** | **For updating state** |
| **Can access this** | **Static method** |
| **Async operations OK** | **Synchronous only** |


```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  // Pure function - update state only
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  // Side effects - logging, reporting
  componentDidCatch(error, errorInfo) {
    // ✅ Can do side effects here
    this.logErrorToService(error, errorInfo);
    this.setState({ errorDetails: error.message });
  }
  
  logErrorToService = (error, errorInfo) => {
    // Send to error monitoring service
    errorReportingService.captureException(error, {
      extra: errorInfo
    });
  };
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```
</details>

- **Q3:** Can functional components implement error boundaries?
<details>
<summary>Answer</summary>
**No**, functional components **cannot be error boundaries**. Error boundaries require:
- `getDerivedStateFromError` (static method)
- `componentDidCatch` (lifecycle method)

These are only available in **class components**.


```jsx
// ❌ Cannot create error boundary with hooks
function ErrorBoundary({ children }) {
  const [hasError, setHasError] = useState(false);
  
  // No equivalent hooks for error boundary methods
  // useErrorBoundary() doesn't exist
  
  return hasError ? <div>Error occurred</div> : children;
}

// ✅ Must use class component
class ErrorBoundary extends React.Component {
  // ... error boundary implementation
}

// ✅ Alternative: Use react-error-boundary library
import { ErrorBoundary } from 'react-error-boundary';

function App() {
  return (
    <ErrorBoundary fallback={<ErrorFallback />}>
      <MyComponent />
    </ErrorBoundary>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you integrate error boundaries with error monitoring services?
<details>
<summary>Answer</summary>

```jsx
import * as Sentry from '@sentry/react';
import * as LogRocket from 'logrocket';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, eventId: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Multiple error tracking services
    this.reportToMultipleServices(error, errorInfo);
  }
  
  reportToMultipleServices = (error, errorInfo) => {
    // Sentry integration
    const sentryEventId = Sentry.withScope(scope => {
      scope.setTag('section', this.props.section);
      scope.setLevel('error');
      scope.setContext('errorInfo', errorInfo);
      scope.setUser({
        id: this.props.userId,
        email: this.props.userEmail
      });
      
      return Sentry.captureException(error);
    });
    
    // LogRocket integration
    LogRocket.captureException(error);
    
    // Bugsnag integration
    if (window.Bugsnag) {
      window.Bugsnag.notify(error, {
        metaData: {
          react: errorInfo,
          user: {
            id: this.props.userId,
            section: this.props.section
          }
        }
      });
    }
    
    // Custom analytics
    if (window.gtag) {
      window.gtag('event', 'exception', {
        description: error.message,
        fatal: true
      });
    }
    
    this.setState({ eventId: sentryEventId });
  };
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Application Error</h2>
          <p>We've been notified and are working on a fix.</p>
          
          {/* Show error ID for support */}
          {this.state.eventId && (
            <details>
              <summary>Error Reference</summary>
              <p>Error ID: {this.state.eventId}</p>
            </details>
          )}
          
          {/* User feedback for Sentry */}
          <button
            onClick={() => 
              Sentry.showReportDialog({ eventId: this.state.eventId })
            }
          >
            Report Feedback
          </button>
          
          <button onClick={() => window.location.reload()}>
            Reload Application
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// HOC for automatic error boundary wrapping
export const withErrorBoundary = (Component, errorBoundaryConfig = {}) => {
  return function WrappedComponent(props) {
    return (
      <ErrorBoundary {...errorBoundaryConfig}>
        <Component {...props} />
      </ErrorBoundary>
    );
  };
};

// Usage
const SafeComponent = withErrorBoundary(MyComponent, {
  section: 'user-dashboard',
  userId: user.id
});
```
</details>

- **Q2:** How do you test error boundaries?
<details>
<summary>Answer</summary>

```jsx
import { render, screen } from '@testing-library/react';
import ErrorBoundary from './ErrorBoundary';

// Component that throws an error
const ThrowError = ({ shouldThrow }) => {
  if (shouldThrow) {
    throw new Error('Test error');
  }
  return <div>No error</div>;
};

describe('ErrorBoundary', () => {
  // Suppress console.error for cleaner test output
  beforeEach(() => {
    jest.spyOn(console, 'error').mockImplementation(() => {});
  });
  
  afterEach(() => {
    console.error.mockRestore();
  });
  
  test('renders children when there is no error', () => {
    render(
      <ErrorBoundary>
        <ThrowError shouldThrow={false} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText('No error')).toBeInTheDocument();
  });
  
  test('renders error UI when child throws', () => {
    render(
      <ErrorBoundary>
        <ThrowError shouldThrow={true} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText(/something went wrong/i)).toBeInTheDocument();
  });
  
  test('calls error reporting service', () => {
    const mockReportError = jest.fn();
    
    class TestErrorBoundary extends React.Component {
      constructor(props) {
        super(props);
        this.state = { hasError: false };
      }
      
      static getDerivedStateFromError() {
        return { hasError: true };
      }
      
      componentDidCatch(error, errorInfo) {
        mockReportError(error, errorInfo);
      }
      
      render() {
        if (this.state.hasError) {
          return <div>Error occurred</div>;
        }
        return this.props.children;
      }
    }
    
    render(
      <TestErrorBoundary>
        <ThrowError shouldThrow={true} />
      </TestErrorBoundary>
    );
    
    expect(mockReportError).toHaveBeenCalledWith(
      expect.any(Error),
      expect.objectContaining({
        componentStack: expect.any(String)
      })
    );
  });
  
  test('resets error state when reset button clicked', () => {
    const { rerender } = render(
      <ErrorBoundary>
        <ThrowError shouldThrow={true} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText(/something went wrong/i)).toBeInTheDocument();
    
    // Simulate reset
    rerender(
      <ErrorBoundary>
        <ThrowError shouldThrow={false} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText('No error')).toBeInTheDocument();
  });
});

// Integration test with error monitoring
test('integrates with Sentry', async () => {
  const mockSentryCapture = jest.fn().mockReturnValue('test-event-id');
  jest.mock('@sentry/react', () => ({
    captureException: mockSentryCapture,
    withScope: (callback) => callback({
      setTag: jest.fn(),
      setLevel: jest.fn(),
      setContext: jest.fn(),
      setUser: jest.fn()
    })
  }));
  
  render(
    <ErrorBoundary userId="123">
      <ThrowError shouldThrow={true} />
    </ErrorBoundary>
  );
  
  expect(mockSentryCapture).toHaveBeenCalled();
});
```
</details>

---

### 🎯 **Handling async errors**

#### **📋 Beginner:**
- **Q1:** Why don't Error Boundaries catch async errors?
<details>
<summary>Answer</summary>
Error Boundaries **don't catch async errors** because:
- They only catch errors during **React's render phase**
- Async errors happen **outside the React call stack**
- Promises and setTimeout execute **after the render cycle**
- React can't associate async errors with specific components


```jsx
// ❌ These errors are NOT caught by Error Boundaries
function MyComponent() {
  useEffect(() => {
    // Async errors not caught
    setTimeout(() => {
      throw new Error('Timer error');
    }, 1000);
    
    fetch('/api/data')
      .then(response => {
        throw new Error('Fetch error');
      });
    
    async function asyncFunction() {
      throw new Error('Async function error');
    }
    asyncFunction();
  }, []);
  
  return <div>Component</div>;
}

// ✅ Handle async errors manually
function MyComponent() {
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const timer = setTimeout(() => {
      try {
        riskyOperation();
      } catch (error) {
        setError(error);
      }
    }, 1000);
    
    fetch('/api/data')
      .catch(error => setError(error));
    
    return () => clearTimeout(timer);
  }, []);
  
  if (error) {
    throw error; // Now Error Boundary can catch it
  }
  
  return <div>Component</div>;
}
```
</details>

- **Q2:** How do you handle errors in async operations?
<details>
<summary>Answer</summary>


```jsx
// Method 1: Try-catch with async/await
function DataComponent() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch('/api/data');
        
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (error) {
        setError(error);
        console.error('Fetch error:', error);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  return <div>Data: {JSON.stringify(data)}</div>;
}

// Method 2: Promise .catch()
function PromiseComponent() {
  const [result, setResult] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('/api/data')
      .then(response => response.json())
      .then(setResult)
      .catch(error => {
        setError(error);
        console.error('Promise error:', error);
      });
  }, []);
  
  if (error) throw error; // Let Error Boundary handle it
  return <div>{result}</div>;
}

// Method 3: Custom hook for error handling
function useAsyncError() {
  const [, setError] = useState();
  
  return useCallback((error) => {
    setError(() => {
      throw error;
    });
  }, []);
}

function ComponentWithAsyncError() {
  const throwError = useAsyncError();
  
  useEffect(() => {
    someAsyncOperation()
      .catch(throwError); // This will be caught by Error Boundary
  }, [throwError]);
  
  return <div>Component</div>;
}
```
</details>

- **Q3:** How do you handle promise rejections in useEffect?
<details>
<summary>Answer</summary>

```jsx
function PromiseHandler() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    const fetchData = async () => {
      try {
        const result = await apiCall();
        
        // Check if component is still mounted
        if (!cancelled) {
          setData(result);
        }
      } catch (error) {
        if (!cancelled) {
          setError(error);
        }
      }
    };
    
    fetchData();
    
    // Cleanup function
    return () => {
      cancelled = true;
    };
  }, []);
  
  // Another approach with AbortController
  useEffect(() => {
    const abortController = new AbortController();
    
    fetch('/api/data', { signal: abortController.signal })
      .then(response => response.json())
      .then(setData)
      .catch(error => {
        if (error.name !== 'AbortError') {
          setError(error);
        }
      });
    
    return () => {
      abortController.abort();
    };
  }, []);
  
  // Handle unhandled promise rejections globally
  useEffect(() => {
    const handleUnhandledRejection = (event) => {
      console.error('Unhandled promise rejection:', event.reason);
      setError(new Error('An unexpected error occurred'));
    };
    
    window.addEventListener('unhandledrejection', handleUnhandledRejection);
    
    return () => {
      window.removeEventListener('unhandledrejection', handleUnhandledRejection);
    };
  }, []);
  
  if (error) throw error;
  return <div>Data: {data}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you create a global error handler for async operations?
<details>
<summary>Answer</summary>

```jsx
// Global error context
const ErrorContext = createContext();

export function ErrorProvider({ children }) {
  const [errors, setErrors] = useState([]);
  
  const addError = useCallback((error) => {
    const errorId = Date.now() + Math.random();
    setErrors(prev => [...prev, { id: errorId, error, timestamp: new Date() }]);
    
    // Auto-remove error after 5 seconds
    setTimeout(() => {
      setErrors(prev => prev.filter(e => e.id !== errorId));
    }, 5000);
  }, []);
  
  const removeError = useCallback((errorId) => {
    setErrors(prev => prev.filter(e => e.id !== errorId));
  }, []);
  
  // Global unhandled rejection handler
  useEffect(() => {
    const handleUnhandledRejection = (event) => {
      event.preventDefault();
      addError(new Error(event.reason?.message || 'Unhandled async error'));
    };
    
    window.addEventListener('unhandledrejection', handleUnhandledRejection);
    return () => window.removeEventListener('unhandledrejection', handleUnhandledRejection);
  }, [addError]);
  
  return (
    <ErrorContext.Provider value={{ errors, addError, removeError }}>
      {children}
      <ErrorToast errors={errors} onDismiss={removeError} />
    </ErrorContext.Provider>
  );
}

// Custom hook for global error handling
export function useErrorHandler() {
  const context = useContext(ErrorContext);
  if (!context) {
    throw new Error('useErrorHandler must be used within ErrorProvider');
  }
  
  return context.addError;
}

// Error Toast component
function ErrorToast({ errors, onDismiss }) {
  return (
    <div className="error-toast-container">
      {errors.map(({ id, error }) => (
        <div key={id} className="error-toast">
          <span>{error.message}</span>
          <button onClick={() => onDismiss(id)}>×</button>
        </div>
      ))}
    </div>
  );
}

// Usage in components
function DataComponent() {
  const handleError = useErrorHandler();
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetch('/api/data')
      .then(response => {
        if (!response.ok) {
          throw new Error(`Failed to fetch: ${response.status}`);
        }
        return response.json();
      })
      .then(setData)
      .catch(handleError); // Global error handling
  }, [handleError]);
  
  return <div>Data: {JSON.stringify(data)}</div>;
}

// Enhanced hook with retry functionality
export function useAsyncOperation() {
  const handleError = useErrorHandler();
  
  return useCallback(async (operation, retries = 3) => {
    for (let attempt = 1; attempt <= retries; attempt++) {
      try {
        return await operation();
      } catch (error) {
        if (attempt === retries) {
          handleError(error);
          throw error;
        }
        
        // Exponential backoff
        await new Promise(resolve => 
          setTimeout(resolve, Math.pow(2, attempt) * 1000)
        );
      }
    }
  }, [handleError]);
}
```
</details>

- **Q2:** How do you implement retry logic for failed async operations?
<details>
<summary>Answer</summary>

```jsx
// Custom hook with retry logic
function useRetryableAsync(asyncFn, dependencies = [], options = {}) {
  const {
    retries = 3,
    retryDelay = 1000,
    exponentialBackoff = true,
    retryCondition = () => true
  } = options;
  
  const [state, setState] = useState({
    data: null,
    loading: false,
    error: null,
    attempt: 0
  });
  
  const execute = useCallback(async (...args) => {
    setState(prev => ({ ...prev, loading: true, error: null }));
    
    for (let attempt = 1; attempt <= retries + 1; attempt++) {
      try {
        setState(prev => ({ ...prev, attempt }));
        const result = await asyncFn(...args);
        
        setState({
          data: result,
          loading: false,
          error: null,
          attempt
        });
        
        return result;
      } catch (error) {
        const isLastAttempt = attempt > retries;
        const shouldRetry = retryCondition(error, attempt);
        
        if (isLastAttempt || !shouldRetry) {
          setState(prev => ({
            ...prev,
            loading: false,
            error,
            attempt
          }));
          throw error;
        }
        
        // Calculate delay
        const delay = exponentialBackoff 
          ? retryDelay * Math.pow(2, attempt - 1)
          : retryDelay;
        
        console.warn(`Attempt ${attempt} failed, retrying in ${delay}ms...`);
        await new Promise(resolve => setTimeout(resolve, delay));
      }
    }
  }, [asyncFn, retries, retryDelay, exponentialBackoff, retryCondition]);
  
  useEffect(() => {
    execute();
  }, dependencies);
  
  return { ...state, execute, retry: execute };
}

// Usage
function DataComponent() {
  const fetchData = useCallback(async () => {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    return response.json();
  }, []);
  
  const { data, loading, error, attempt, retry } = useRetryableAsync(
    fetchData,
    [], // dependencies
    {
      retries: 3,
      retryDelay: 1000,
      exponentialBackoff: true,
      retryCondition: (error, attempt) => {
        // Only retry on network errors, not 400/401/403
        return !error.message.includes('400') && 
               !error.message.includes('401') && 
               !error.message.includes('403');
      }
    }
  );
  
  if (loading) {
    return <div>Loading... (Attempt {attempt})</div>;
  }
  
  if (error) {
    return (
      <div>
        <p>Error: {error.message}</p>
        <p>Failed after {attempt} attempts</p>
        <button onClick={retry}>Retry</button>
      </div>
    );
  }
  
  return <div>Data: {JSON.stringify(data)}</div>;
}

// Circuit breaker pattern
class CircuitBreaker {
  constructor(options = {}) {
    this.failureThreshold = options.failureThreshold || 5;
    this.resetTimeout = options.resetTimeout || 60000;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.failureCount = 0;
    this.nextAttempt = Date.now();
  }
  
  async execute(operation) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      } else {
        this.state = 'HALF_OPEN';
      }
    }
    
    try {
      const result = await operation();
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failureCount = 0;
    this.state = 'CLOSED';
  }
  
  onFailure() {
    this.failureCount++;
    if (this.failureCount >= this.failureThreshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.resetTimeout;
    }
  }
}

// Usage with circuit breaker
const apiCircuitBreaker = new CircuitBreaker({
  failureThreshold: 3,
  resetTimeout: 30000
});

function ComponentWithCircuitBreaker() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  
  const fetchData = async () => {
    try {
      const result = await apiCircuitBreaker.execute(async () => {
        const response = await fetch('/api/data');
        if (!response.ok) throw new Error('API Error');
        return response.json();
      });
      setData(result);
      setError(null);
    } catch (error) {
      setError(error);
    }
  };
  
  return (
    <div>
      <button onClick={fetchData}>Fetch Data</button>
      {error && <p>Error: {error.message}</p>}
      {data && <p>Data: {JSON.stringify(data)}</p>}
    </div>
  );
}
```
</details>

---

## 🔹 **Context & Prop Drilling**

---

### 🎯 **Context API usage**

#### **📋 Beginner:**
- **Q1:** What problem does Context API solve?
<details>
<summary>Answer</summary>
The **Context API** solves the **prop drilling problem** - passing data through multiple component layers to reach a deeply nested component.

**Problems Context solves:**
- **Prop drilling** - Avoiding unnecessary prop passing
- **Global state sharing** - Components at any level can access shared data
- **Component coupling** - Reduces dependencies between parent/child components
- **Code maintainability** - Cleaner component hierarchy


```jsx
// ❌ Prop drilling problem
function App() {
  const user = { name: 'John', role: 'admin' };
  const theme = 'dark';
  
  return <Dashboard user={user} theme={theme} />;
}

function Dashboard({ user, theme }) {
  return (
    <div>
      <Sidebar user={user} theme={theme} />
      <MainContent user={user} theme={theme} />
    </div>
  );
}

function Sidebar({ user, theme }) {
  return <Navigation user={user} theme={theme} />;
}

function Navigation({ user, theme }) {
  return (
    <nav className={theme}>
      <UserInfo user={user} />
    </nav>
  );
}

function UserInfo({ user }) {
  return <span>Welcome, {user.name}</span>;
}

// ✅ Context API solution
const UserContext = createContext();
const ThemeContext = createContext();

function App() {
  const user = { name: 'John', role: 'admin' };
  const theme = 'dark';
  
  return (
    <UserContext.Provider value={user}>
      <ThemeContext.Provider value={theme}>
        <Dashboard />
      </ThemeContext.Provider>
    </UserContext.Provider>
  );
}

function Dashboard() {
  return (
    <div>
      <Sidebar />
      <MainContent />
    </div>
  );
}

function Navigation() {
  const theme = useContext(ThemeContext);
  return (
    <nav className={theme}>
      <UserInfo />
    </nav>
  );
}

function UserInfo() {
  const user = useContext(UserContext);
  return <span>Welcome, {user.name}</span>;
}
```

**When to use Context:**
- **Authentication state** (user login status)
- **Theme/UI preferences** (dark/light mode)
- **Internationalization** (language settings)
- **Shopping cart data**
- **Application configuration**
</details>

- **Q2:** How do you avoid prop drilling using Context?
<details>
<summary>Answer</summary>

```jsx
// Step 1: Create Context
const AppContext = createContext();

// Step 2: Create Provider Component
function AppProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  const [language, setLanguage] = useState('en');
  
  const contextValue = {
    user,
    setUser,
    theme,
    setTheme,
    language,
    setLanguage
  };
  
  return (
    <AppContext.Provider value={contextValue}>
      {children}
    </AppContext.Provider>
  );
}

// Step 3: Custom hook for easy access
function useApp() {
  const context = useContext(AppContext);
  if (!context) {
    throw new Error('useApp must be used within AppProvider');
  }
  return context;
}

// Step 4: Wrap your app
function App() {
  return (
    <AppProvider>
      <Header />
      <Main />
      <Footer />
    </AppProvider>
  );
}

// Step 5: Use context anywhere in the tree
function Header() {
  const { user, theme, setTheme } = useApp();
  
  return (
    <header className={`header-${theme}`}>
      <h1>Welcome {user?.name || 'Guest'}</h1>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </header>
  );
}

function DeepNestedComponent() {
  // Direct access without prop drilling
  const { user, language } = useApp();
  
  return (
    <div>
      <p>User: {user?.name}</p>
      <p>Language: {language}</p>
    </div>
  );
}

// Alternative: Multiple specific contexts
const UserContext = createContext();
const ThemeContext = createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const login = async (credentials) => {
    const userData = await authenticate(credentials);
    setUser(userData);
  };
  
  const logout = () => setUser(null);
  
  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {children}
    </UserContext.Provider>
  );
}

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Compose providers
function App() {
  return (
    <UserProvider>
      <ThemeProvider>
        <MainApp />
      </ThemeProvider>
    </UserProvider>
  );
}
```
</details>

- **Q3:** When should you use Context vs props?
<details>
<summary>Answer</summary>
**Use Context when:**
- Data is needed by **many components** at different nesting levels
- **Prop drilling** becomes cumbersome (3+ levels deep)
- Data is **truly global** (theme, auth, language)
- Components are **far apart** in the tree
- You want to **decouple** data from component hierarchy

**Use Props when:**
- **Direct parent-child** communication
- Data is **local** to a component subtree
- **Simple data flow** (1-2 levels deep)
- **Performance critical** updates
- **Testing** is easier with explicit dependencies


```jsx
// ✅ Good use of Props
function UserCard({ user, onEdit, onDelete }) {
  return (
    <div className="user-card">
      <Avatar src={user.avatar} />
      <UserInfo user={user} />
      <ActionButtons onEdit={onEdit} onDelete={onDelete} />
    </div>
  );
}

function UserInfo({ user }) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
}

// ✅ Good use of Context
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  
  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}

// Any component can access auth state
function Navbar() {
  const { user } = useContext(AuthContext);
  return user ? <LoggedInNav /> : <GuestNav />;
}

function ProfilePage() {
  const { user } = useContext(AuthContext);
  return <Profile user={user} />;
}

function Sidebar() {
  const { user } = useContext(AuthContext);
  return <UserMenu user={user} />;
}

// ❌ Avoid Context for
function Counter() {
  const [count, setCount] = useState(0); // Local state is better
  return (
    <div>
      <span>{count}</span>
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}

// ❌ Don't use Context for simple prop passing
function App() {
  const title = "My App";
  return <Header title={title} />; // Direct prop is simpler
}

// Comparison table
const comparison = {
  criteria: {
    "Scope": {
      "Props": "Local, parent-child",
      "Context": "Global, cross-component"
    },
    "Performance": {
      "Props": "Better, no unnecessary re-renders",
      "Context": "Can cause re-renders if not optimized"
    },
    "Testing": {
      "Props": "Easier, explicit dependencies",
      "Context": "Requires provider setup"
    },
    "Type Safety": {
      "Props": "Full TypeScript support",
      "Context": "Requires additional typing"
    },
    "Debugging": {
      "Props": "Clear data flow",
      "Context": "Less obvious data source"
    }
  }
};
```

**Decision Framework:**
1. **Start with props** - Default choice for component communication
2. **Consider Context** when prop drilling becomes annoying (3+ levels)
3. **Use Context** for truly global state (auth, theme, i18n)
4. **Optimize Context** with useMemo and multiple contexts
5. **Consider alternatives** like component composition or state management libraries
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Context usage for large applications?
<details>
<summary>Answer</summary>

```jsx
// 1. Split Context by Concern and Update Frequency
const UserContext = createContext(); // Rarely changes
const UIContext = createContext();   // Changes frequently

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  // Memoize to prevent unnecessary re-renders
  const value = useMemo(() => ({
    user,
    setUser
  }), [user]);
  
  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}

function UIProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const [sidebar, setSidebar] = useState(false);
  
  const value = useMemo(() => ({
    theme,
    setTheme,
    sidebar,
    setSidebar
  }), [theme, sidebar]);
  
  return (
    <UIContext.Provider value={value}>
      {children}
    </UIContext.Provider>
  );
}

// 2. Selective Context Consumption
function createSelectorContext(initialState) {
  const StateContext = createContext();
  const SelectorContext = createContext();
  
  function Provider({ children }) {
    const [state, setState] = useState(initialState);
    
    const selectors = useMemo(() => ({
      getUser: () => state.user,
      getTheme: () => state.theme,
      getCounter: () => state.counter
    }), [state]);
    
    return (
      <StateContext.Provider value={{ state, setState }}>
        <SelectorContext.Provider value={selectors}>
          {children}
        </SelectorContext.Provider>
      </StateContext.Provider>
    );
  }
  
  function useSelector(selector) {
    const selectors = useContext(SelectorContext);
    return selector(selectors);
  }
  
  return { Provider, useSelector };
}

// Usage
const { Provider: AppProvider, useSelector } = createSelectorContext({
  user: null,
  theme: 'light',
  counter: 0
});

function UserComponent() {
  const user = useSelector(s => s.getUser()); // Only re-renders when user changes
  return <div>{user?.name}</div>;
}

// 3. Context with Reducers for Complex State
const AppStateContext = createContext();
const AppDispatchContext = createContext();

function appReducer(state, action) {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'SET_THEME':
      return { ...state, theme: action.payload };
    case 'TOGGLE_SIDEBAR':
      return { ...state, sidebarOpen: !state.sidebarOpen };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, {
    user: null,
    theme: 'light',
    sidebarOpen: false
  });
  
  return (
    <AppStateContext.Provider value={state}>
      <AppDispatchContext.Provider value={dispatch}>
        {children}
      </AppDispatchContext.Provider>
    </AppStateContext.Provider>
  );
}

// Custom hooks
function useAppState() {
  const context = useContext(AppStateContext);
  if (!context) {
    throw new Error('useAppState must be used within AppProvider');
  }
  return context;
}

function useAppDispatch() {
  const context = useContext(AppDispatchContext);
  if (!context) {
    throw new Error('useAppDispatch must be used within AppProvider');
  }
  return context;
}

// 4. Lazy Context Loading
function createLazyContext(loader) {
  const LazyContext = createContext();
  
  function Provider({ children }) {
    const [state, setState] = useState(null);
    const [loading, setLoading] = useState(false);
    
    const loadData = useCallback(async () => {
      if (!state && !loading) {
        setLoading(true);
        try {
          const data = await loader();
          setState(data);
        } finally {
          setLoading(false);
        }
      }
    }, [state, loading]);
    
    const value = useMemo(() => ({
      state,
      loading,
      loadData
    }), [state, loading, loadData]);
    
    return (
      <LazyContext.Provider value={value}>
        {children}
      </LazyContext.Provider>
    );
  }
  
  function useLazyContext() {
    return useContext(LazyContext);
  }
  
  return { Provider, useLazyContext };
}

// Usage
const { Provider: ConfigProvider, useLazyContext: useConfig } = createLazyContext(
  () => fetch('/api/config').then(res => res.json())
);

function ConfigConsumer() {
  const { state: config, loading, loadData } = useConfig();
  
  useEffect(() => {
    loadData();
  }, [loadData]);
  
  if (loading) return <div>Loading config...</div>;
  return <div>{config?.appName}</div>;
}

// 5. Context Debugging and DevTools
function createDebuggableContext(name, initialState) {
  const Context = createContext();
  
  function Provider({ children }) {
    const [state, setState] = useState(initialState);
    
    // Debug logging
    useEffect(() => {
      if (process.env.NODE_ENV === 'development') {
        console.log(`${name} Context updated:`, state);
      }
    }, [state]);
    
    const value = useMemo(() => ({
      state,
      setState,
      _debug: {
        name,
        timestamp: Date.now()
      }
    }), [state]);
    
    return (
      <Context.Provider value={value}>
        {children}
      </Context.Provider>
    );
  }
  
  return { Provider, Context };
}

// 6. Performance Monitoring
function useContextPerformance(contextName, dependencies) {
  const renderCount = useRef(0);
  const lastRender = useRef(Date.now());
  
  useEffect(() => {
    renderCount.current++;
    const now = Date.now();
    const timeSinceLastRender = now - lastRender.current;
    
    if (process.env.NODE_ENV === 'development') {
      console.log(`${contextName} render #${renderCount.current}, time since last: ${timeSinceLastRender}ms`);
    }
    
    lastRender.current = now;
  }, dependencies);
}

// Usage in components
function ExpensiveComponent() {
  const { user } = useAppState();
  
  useContextPerformance('ExpensiveComponent', [user]);
  
  return <div>{user?.name}</div>;
}
```
</details>

- **Q2:** How do you implement multiple contexts efficiently?
<details>
<summary>Answer</summary>

```jsx
// 1. Context Composition Pattern
function createContextComposer() {
  const providers = [];
  
  const addProvider = (Provider) => {
    providers.push(Provider);
    return composer;
  };
  
  const Compose = ({ children }) => {
    return providers.reduceRight(
      (acc, Provider) => <Provider>{acc}</Provider>,
      children
    );
  };
  
  const composer = { addProvider, Compose };
  return composer;
}

// Usage
const AppProviders = createContextComposer()
  .addProvider(AuthProvider)
  .addProvider(ThemeProvider)
  .addProvider(I18nProvider)
  .addProvider(NotificationProvider);

function App() {
  return (
    <AppProviders.Compose>
      <MainApp />
    </AppProviders.Compose>
  );
}

// 2. Factory Pattern for Context Creation
function createContextProvider(name, initialState, reducer) {
  const StateContext = createContext();
  const DispatchContext = createContext();
  
  function Provider({ children }) {
    const [state, dispatch] = useReducer(reducer, initialState);
    
    return (
      <StateContext.Provider value={state}>
        <DispatchContext.Provider value={dispatch}>
          {children}
        </DispatchContext.Provider>
      </StateContext.Provider>
    );
  }
  
  function useContextState() {
    const context = useContext(StateContext);
    if (!context) {
      throw new Error(`use${name}State must be used within ${name}Provider`);
    }
    return context;
  }
  
  function useContextDispatch() {
    const context = useContext(DispatchContext);
    if (!context) {
      throw new Error(`use${name}Dispatch must be used within ${name}Provider`);
    }
    return context;
  }
  
  return {
    Provider,
    useContextState,
    useContextDispatch
  };
}

// Create multiple contexts
const {
  Provider: TodoProvider,
  useContextState: useTodos,
  useContextDispatch: useTodoDispatch
} = createContextProvider('Todo', [], todosReducer);

const {
  Provider: UserProvider,
  useContextState: useUser,
  useContextDispatch: useUserDispatch
} = createContextProvider('User', null, userReducer);

// 3. Hierarchical Context Structure
function createHierarchicalContext() {
  const contexts = new Map();
  
  const registerContext = (name, Provider) => {
    contexts.set(name, Provider);
  };
  
  const ContextTree = ({ structure, children }) => {
    const buildTree = (node, child) => {
      if (typeof node === 'string') {
        const Provider = contexts.get(node);
        return Provider ? <Provider>{child}</Provider> : child;
      }
      
      if (Array.isArray(node)) {
        return node.reduceRight((acc, item) => buildTree(item, acc), child);
      }
      
      return child;
    };
    
    return buildTree(structure, children);
  };
  
  return { registerContext, ContextTree };
}

// Usage
const { registerContext, ContextTree } = createHierarchicalContext();

registerContext('auth', AuthProvider);
registerContext('theme', ThemeProvider);
registerContext('i18n', I18nProvider);

function App() {
  return (
    <ContextTree structure={['auth', ['theme', 'i18n']]}>
      <MainApp />
    </ContextTree>
  );
}

// 4. Conditional Context Providers
function ConditionalProviders({ children, features = {} }) {
  let app = children;
  
  if (features.auth) {
    app = <AuthProvider>{app}</AuthProvider>;
  }
  
  if (features.theme) {
    app = <ThemeProvider>{app}</ThemeProvider>;
  }
  
  if (features.analytics) {
    app = <AnalyticsProvider>{app}</AnalyticsProvider>;
  }
  
  return app;
}

// Usage
function App() {
  const features = {
    auth: true,
    theme: true,
    analytics: process.env.NODE_ENV === 'production'
  };
  
  return (
    <ConditionalProviders features={features}>
      <MainApp />
    </ConditionalProviders>
  );
}

// 5. Context Registry with Dependency Injection
class ContextRegistry {
  constructor() {
    this.contexts = new Map();
    this.dependencies = new Map();
  }
  
  register(name, Provider, deps = []) {
    this.contexts.set(name, Provider);
    this.dependencies.set(name, deps);
  }
  
  resolve(names) {
    const sorted = this.topologicalSort(names);
    
    return ({ children }) => {
      return sorted.reduce((acc, name) => {
        const Provider = this.contexts.get(name);
        return Provider ? <Provider>{acc}</Provider> : acc;
      }, children);
    };
  }
  
  topologicalSort(names) {
    const visited = new Set();
    const result = [];
    
    const visit = (name) => {
      if (visited.has(name)) return;
      visited.add(name);
      
      const deps = this.dependencies.get(name) || [];
      deps.forEach(visit);
      
      result.push(name);
    };
    
    names.forEach(visit);
    return result;
  }
}

// Usage
const registry = new ContextRegistry();

registry.register('auth', AuthProvider);
registry.register('theme', ThemeProvider, ['auth']); // theme depends on auth
registry.register('notifications', NotificationProvider, ['auth']);

const AppProviders = registry.resolve(['auth', 'theme', 'notifications']);

function App() {
  return (
    <AppProviders>
      <MainApp />
    </AppProviders>
  );
}

// 6. Performance-Optimized Multi-Context
function createOptimizedContexts(configs) {
  const contexts = {};
  
  configs.forEach(({ name, initialState, actions }) => {
    const StateContext = createContext();
    const ActionsContext = createContext();
    
    function Provider({ children }) {
      const [state, setState] = useState(initialState);
      
      const memoizedActions = useMemo(() => {
        const actionHandlers = {};
        
        Object.entries(actions).forEach(([actionName, actionFn]) => {
          actionHandlers[actionName] = (...args) => {
            setState(prevState => actionFn(prevState, ...args));
          };
        });
        
        return actionHandlers;
      }, []);
      
      return (
        <StateContext.Provider value={state}>
          <ActionsContext.Provider value={memoizedActions}>
            {children}
          </ActionsContext.Provider>
        </StateContext.Provider>
      );
    }
    
    contexts[name] = {
      Provider,
      useState: () => useContext(StateContext),
      useActions: () => useContext(ActionsContext)
    };
  });
  
  return contexts;
}

// Usage
const { user, todo, theme } = createOptimizedContexts([
  {
    name: 'user',
    initialState: null,
    actions: {
      setUser: (state, user) => user,
      updateUser: (state, updates) => ({ ...state, ...updates }),
      clearUser: () => null
    }
  },
  {
    name: 'todo',
    initialState: [],
    actions: {
      addTodo: (state, todo) => [...state, todo],
      removeTodo: (state, id) => state.filter(t => t.id !== id),
      toggleTodo: (state, id) => state.map(t => 
        t.id === id ? { ...t, completed: !t.completed } : t
      )
    }
  }
]);

function UserComponent() {
  const currentUser = user.useState();
  const { setUser, updateUser } = user.useActions();
  
  return <div>{currentUser?.name}</div>;
}
```
</details>

---

### 🎯 **Comparison: Context vs Redux**

#### **📋 Beginner:**
- **Q1:** What are the main differences between Context and Redux?
<details>
<summary>Answer</summary>
| Feature | Context API | Redux |
|---------|-------------|--------|
| **Purpose** | Built-in React feature for prop drilling | External state management library |
| **Boilerplate** | Minimal setup | More setup (actions, reducers, store) |
| **Learning Curve** | Easy - React concepts | Steeper - additional concepts |
| **Bundle Size** | 0KB (built-in) | ~2.5KB (Redux Toolkit) |
| **DevTools** | Limited | Excellent Redux DevTools |
| **Time Travel** | Not built-in | Yes, with DevTools |
| **Middleware** | No built-in support | Rich middleware ecosystem |
| **Performance** | Can cause unnecessary re-renders | Optimized with selectors |
| **Async Handling** | Manual (useEffect + useState) | Built-in with middleware (Thunk, Saga) |
| **State Structure** | Flexible, any shape | Encourages normalized state |


```jsx
// Context API Example
const UserContext = createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);
  
  const login = async (credentials) => {
    setLoading(true);
    try {
      const userData = await api.login(credentials);
      setUser(userData);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <UserContext.Provider value={{ user, loading, login }}>
      {children}
    </UserContext.Provider>
  );
}

// Redux Example
// Actions
const loginStart = () => ({ type: 'LOGIN_START' });
const loginSuccess = (user) => ({ type: 'LOGIN_SUCCESS', payload: user });
const loginFailure = (error) => ({ type: 'LOGIN_FAILURE', payload: error });

// Reducer
function userReducer(state = { user: null, loading: false }, action) {
  switch (action.type) {
    case 'LOGIN_START':
      return { ...state, loading: true };
    case 'LOGIN_SUCCESS':
      return { user: action.payload, loading: false, error: null };
    case 'LOGIN_FAILURE':
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}

// Thunk action creator
const login = (credentials) => async (dispatch) => {
  dispatch(loginStart());
  try {
    const user = await api.login(credentials);
    dispatch(loginSuccess(user));
  } catch (error) {
    dispatch(loginFailure(error.message));
  }
};
```
</details>

- **Q2:** When would you choose Context over Redux?
<details>
<summary>Answer</summary>
**Choose Context when:**

1. **Simple state management** - Few state values, minimal complexity
2. **Small to medium apps** - Limited number of components
3. **Prop drilling solution** - Main goal is avoiding prop passing
4. **React-only project** - No external dependencies preferred
5. **Quick prototyping** - Fast development, minimal setup
6. **Theme/UI state** - Simple global UI preferences


```jsx
// Good Context use cases

// 1. Theme Management
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 2. Authentication State
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  
  const login = (userData) => {
    setUser(userData);
    setIsAuthenticated(true);
  };
  
  const logout = () => {
    setUser(null);
    setIsAuthenticated(false);
  };
  
  return (
    <AuthContext.Provider value={{ user, isAuthenticated, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

// 3. Language/Internationalization
const I18nContext = createContext();

function I18nProvider({ children }) {
  const [language, setLanguage] = useState('en');
  const [translations, setTranslations] = useState({});
  
  const t = (key) => translations[key] || key;
  
  return (
    <I18nContext.Provider value={{ language, setLanguage, t }}>
      {children}
    </I18nContext.Provider>
  );
}

// 4. Simple Shopping Cart
const CartContext = createContext();

function CartProvider({ children }) {
  const [items, setItems] = useState([]);
  
  const addItem = (item) => setItems(prev => [...prev, item]);
  const removeItem = (id) => setItems(prev => prev.filter(item => item.id !== id));
  const clearCart = () => setItems([]);
  
  const total = items.reduce((sum, item) => sum + item.price, 0);
  
  return (
    <CartContext.Provider value={{ items, addItem, removeItem, clearCart, total }}>
      {children}
    </CartContext.Provider>
  );
}
```

**Context advantages:**
- **Zero configuration** - Works out of the box
- **Type safety** - Easy TypeScript integration
- **React integration** - Follows React patterns
- **Bundle size** - No additional dependencies
- **Simple mental model** - Just props at distance
</details>

- **Q3:** When would you choose Redux over Context?
<details>
<summary>Answer</summary>
**Choose Redux when:**

1. **Complex state logic** - Multiple reducers, complex updates
2. **Large applications** - Many components, deep nesting
3. **Time travel debugging** - Need to replay actions
4. **Middleware needs** - Logging, analytics, side effects
5. **Performance critical** - Need fine-grained optimizations
6. **Team collaboration** - Standardized patterns, predictable structure


```jsx
// Good Redux use cases

// 1. Complex E-commerce State
const store = configureStore({
  reducer: {
    user: userReducer,
    products: productsReducer,
    cart: cartReducer,
    orders: ordersReducer,
    ui: uiReducer,
    notifications: notificationsReducer
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(
      loggerMiddleware,
      analyticsMiddleware
    )
});

// 2. Real-time Data Management
const socketMiddleware = (store) => (next) => (action) => {
  if (action.type === 'SOCKET_CONNECT') {
    const socket = io();
    socket.on('message', (data) => {
      store.dispatch({ type: 'MESSAGE_RECEIVED', payload: data });
    });
  }
  return next(action);
};

// 3. Complex Forms with Validation
const formReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'FIELD_CHANGE':
      return {
        ...state,
        values: { ...state.values, [action.field]: action.value },
        errors: validateField(action.field, action.value, state.values)
      };
    case 'SUBMIT_START':
      return { ...state, submitting: true };
    case 'SUBMIT_SUCCESS':
      return { ...state, submitting: false, submitted: true };
    default:
      return state;
  }
};

// 4. Data Normalization
const postsAdapter = createEntityAdapter();

const postsSlice = createSlice({
  name: 'posts',
  initialState: postsAdapter.getInitialState(),
  reducers: {
    postsReceived: postsAdapter.setAll,
    postAdded: postsAdapter.addOne,
    postUpdated: postsAdapter.updateOne
  }
});

// Selectors for optimized access
const selectAllPosts = postsAdapter.getSelectors().selectAll;
const selectPostById = (state, postId) => 
  postsAdapter.getSelectors().selectById(state, postId);
```

**Redux advantages:**
- **Predictable state** - Pure functions, immutable updates
- **DevTools** - Time travel, action replay, state inspection
- **Ecosystem** - Rich middleware, tools, patterns
- **Performance** - Memoized selectors, optimized updates
- **Testing** - Pure functions easy to test
- **Scalability** - Handles complex state relationships

**Decision Matrix:**

```jsx
const decisionGuide = {
  projectSize: {
    small: "Context",
    medium: "Context or Redux",
    large: "Redux"
  },
  stateComplexity: {
    simple: "Context",
    moderate: "Context with useReducer",
    complex: "Redux"
  },
  teamSize: {
    solo: "Context",
    small: "Either",
    large: "Redux (consistency)"
  },
  debuggingNeeds: {
    basic: "Context",
    advanced: "Redux"
  },
  performanceRequirements: {
    basic: "Context",
    critical: "Redux with selectors"
  }
};
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do the performance characteristics of Context and Redux compare?
<details>
<summary>Answer</summary>
**Performance Comparison:**

| Aspect | Context API | Redux |
|--------|-------------|--------|
| **Re-render Scope** | All consumers re-render | Only connected components |
| **Optimization** | Manual (useMemo, split contexts) | Built-in (selectors, React-Redux) |
| **Bundle Impact** | 0KB | ~2.5KB (RTK) |
| **Update Efficiency** | Object equality checks | Shallow equality with selectors |
| **Memory Usage** | Lower overhead | Higher (normalized state) |


```jsx
// Context Performance Issues
function UserContext() {
  const [user, setUser] = useState({ name: 'John', theme: 'light' });
  
  // ❌ All consumers re-render when any part changes
  const value = {
    user,
    setUser,
    updateName: (name) => setUser(prev => ({ ...prev, name })),
    updateTheme: (theme) => setUser(prev => ({ ...prev, theme }))
  };
  
  return (
    <UserContext.Provider value={value}>
      <App />
    </UserContext.Provider>
  );
}

// All these components re-render when user.name OR user.theme changes
function UserName() {
  const { user } = useContext(UserContext);
  return <span>{user.name}</span>; // Only needs name
}

function ThemeButton() {
  const { user, updateTheme } = useContext(UserContext);
  return <button onClick={() => updateTheme('dark')}>{user.theme}</button>;
}

// ✅ Context Performance Solutions
// 1. Split contexts by concern
const UserNameContext = createContext();
const UserThemeContext = createContext();

function OptimizedProvider({ children }) {
  const [name, setName] = useState('John');
  const [theme, setTheme] = useState('light');
  
  return (
    <UserNameContext.Provider value={{ name, setName }}>
      <UserThemeContext.Provider value={{ theme, setTheme }}>
        {children}
      </UserThemeContext.Provider>
    </UserNameContext.Provider>
  );
}

// 2. Memoize context values
function MemoizedProvider({ children }) {
  const [user, setUser] = useState({ name: 'John', theme: 'light' });
  
  const nameContext = useMemo(() => ({
    name: user.name,
    setName: (name) => setUser(prev => ({ ...prev, name }))
  }), [user.name]);
  
  const themeContext = useMemo(() => ({
    theme: user.theme,
    setTheme: (theme) => setUser(prev => ({ ...prev, theme }))
  }), [user.theme]);
  
  return (
    <UserNameContext.Provider value={nameContext}>
      <UserThemeContext.Provider value={themeContext}>
        {children}
      </UserThemeContext.Provider>
    </UserNameContext.Provider>
  );
}

// Redux Performance Advantages
function UserComponent() {
  // Only re-renders when user.name changes
  const userName = useSelector(state => state.user.name);
  
  return <span>{userName}</span>;
}

function ThemeComponent() {
  // Only re-renders when user.theme changes
  const theme = useSelector(state => state.user.theme);
  const dispatch = useDispatch();
  
  return (
    <button onClick={() => dispatch(setTheme('dark'))}>
      {theme}
    </button>
  );
}

// Advanced Redux Performance
// Memoized selectors with Reselect
const selectUser = (state) => state.user;
const selectUserPosts = (state) => state.posts;

const selectUserWithPostCount = createSelector(
  [selectUser, selectUserPosts],
  (user, posts) => ({
    ...user,
    postCount: posts.filter(post => post.authorId === user.id).length
  })
);

// Component only re-renders when user data or relevant posts change
function UserProfile() {
  const userWithPosts = useSelector(selectUserWithPostCount);
  return <div>{userWithPosts.name} ({userWithPosts.postCount} posts)</div>;
}

// Performance Measurement
function PerformanceTest() {
  const [contextRenders, setContextRenders] = useState(0);
  const [reduxRenders, setReduxRenders] = useState(0);
  
  useEffect(() => {
    console.log(`Context renders: ${contextRenders}, Redux renders: ${reduxRenders}`);
  });
  
  return (
    <div>
      <ContextComponent onRender={() => setContextRenders(c => c + 1)} />
      <ReduxComponent onRender={() => setReduxRenders(c => c + 1)} />
    </div>
  );
}

// Context with performance monitoring
function useRenderCount(name) {
  const renderCount = useRef(0);
  useEffect(() => {
    renderCount.current++;
    console.log(`${name} rendered ${renderCount.current} times`);
  });
}

function ContextComponent() {
  useRenderCount('ContextComponent');
  const { user } = useContext(UserContext);
  return <div>{user.name}</div>;
}
```

**Performance Best Practices:**

**Context:**
- Split contexts by update frequency
- Memoize context values
- Use React.memo for consumer components
- Consider useCallback for functions

**Redux:**
- Use specific selectors
- Memoize expensive selectors with Reselect
- Normalize state shape
- Use RTK Query for API state
</details>

- **Q2:** How do you decide between Context, Redux, and other state management solutions?
<details>
<summary>Answer</summary>
**Decision Framework:**


```jsx
// Decision Tree Function
function chooseStateManagement(requirements) {
  const {
    appSize,
    stateComplexity,
    teamSize,
    performanceNeeds,
    debuggingNeeds,
    asyncOperations,
    serverState,
    realTimeUpdates
  } = requirements;
  
  // Local State - useState/useReducer
  if (stateComplexity === 'simple' && appSize === 'small') {
    return 'useState';
  }
  
  if (stateComplexity === 'moderate' && appSize === 'small') {
    return 'useReducer';
  }
  
  // Context API
  if (
    appSize === 'small-medium' &&
    stateComplexity === 'simple' &&
    !performanceNeeds.critical &&
    !debuggingNeeds.advanced
  ) {
    return 'Context API';
  }
  
  // Redux/Redux Toolkit
  if (
    appSize === 'large' ||
    stateComplexity === 'complex' ||
    debuggingNeeds.advanced ||
    teamSize === 'large'
  ) {
    return 'Redux Toolkit';
  }
  
  // Zustand
  if (
    appSize === 'medium' &&
    stateComplexity === 'moderate' &&
    performanceNeeds.good
  ) {
    return 'Zustand';
  }
  
  // React Query/SWR for server state
  if (serverState.heavy) {
    return 'React Query + ' + chooseStateManagement({
      ...requirements,
      serverState: { heavy: false }
    });
  }
  
  return 'Context API'; // Default fallback
}

// Usage examples
const smallApp = chooseStateManagement({
  appSize: 'small',
  stateComplexity: 'simple',
  teamSize: 'solo',
  performanceNeeds: { critical: false },
  debuggingNeeds: { advanced: false },
  serverState: { heavy: false }
});
// Result: "useState"

const enterpriseApp = chooseStateManagement({
  appSize: 'large',
  stateComplexity: 'complex',
  teamSize: 'large',
  performanceNeeds: { critical: true },
  debuggingNeeds: { advanced: true },
  serverState: { heavy: true }
});
// Result: "React Query + Redux Toolkit"
```

**State Management Solutions Comparison:**

| Solution | Best For | Pros | Cons |
|----------|----------|------|------|
| **useState** | Component-local state | Simple, built-in | Limited scope |
| **useReducer** | Complex local state | Predictable updates | Still local |
| **Context** | Simple global state | No dependencies, React-native | Performance issues |
| **Redux Toolkit** | Complex apps, large teams | DevTools, ecosystem, predictable | Learning curve, boilerplate |
| **Zustand** | Medium complexity | Small bundle, simple API | Smaller ecosystem |
| **Jotai** | Atomic state | Fine-grained reactivity | New paradigm |
| **Valtio** | Mutable state | Proxy-based, intuitive | Less predictable |
| **React Query** | Server state | Caching, synchronization | Only for server state |


```jsx
// Practical Examples

// 1. Simple Counter - useState
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <span>{count}</span>
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}

// 2. Form with validation - useReducer
function useForm(initialState, validations) {
  const [state, dispatch] = useReducer(formReducer, {
    values: initialState,
    errors: {},
    touched: {},
    isValid: false
  });
  
  const updateField = (name, value) => {
    dispatch({ type: 'UPDATE_FIELD', payload: { name, value } });
    
    if (validations[name]) {
      const error = validations[name](value);
      dispatch({ type: 'SET_ERROR', payload: { name, error } });
    }
  };
  
  return { state, updateField };
}

// 3. Theme system - Context
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 4. E-commerce app - Redux Toolkit
const store = configureStore({
  reducer: {
    auth: authSlice.reducer,
    products: productsSlice.reducer,
    cart: cartSlice.reducer,
    orders: ordersSlice.reducer
  }
});

// 5. Quick prototype - Zustand
const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 }))
}));

// 6. Atomic state - Jotai
const countAtom = atom(0);
const doubleCountAtom = atom((get) => get(countAtom) * 2);

function Counter() {
  const [count, setCount] = useAtom(countAtom);
  const doubleCount = useAtomValue(doubleCountAtom);
  
  return (
    <div>
      <div>Count: {count}</div>
      <div>Double: {doubleCount}</div>
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}

// 7. Server state - React Query
function UserProfile({ userId }) {
  const { data: user, isLoading, error } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    staleTime: 5 * 60 * 1000 // 5 minutes
  });
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return <div>Welcome, {user.name}!</div>;
}

// Hybrid approach for complex apps
function App() {
  return (
    <QueryClient client={queryClient}>
      <Provider store={reduxStore}>
        <ThemeProvider>
          <Router>
            <Routes>
              <Route path="/" element={<Home />} />
              <Route path="/profile" element={<Profile />} />
            </Routes>
          </Router>
        </ThemeProvider>
      </Provider>
    </QueryClient>
  );
}
```

**Decision Guidelines:**
1. **Start simple** - Begin with local state (useState/useReducer)
2. **Identify pain points** - Prop drilling, complex updates, performance
3. **Choose incrementally** - Add state management as needed
4. **Consider team** - Experience level, preferences, standards
5. **Evaluate ecosystem** - DevTools, middleware, community support
6. **Plan for growth** - How will requirements change over time
</details>

---

## 🔹 **Asynchronous React**

---

### 🎯 **Data fetching (fetch, axios)**

#### **📋 Beginner:**
- **Q1:** How do you fetch data in React components?
<details>
<summary>Answer</summary>
Use **useEffect** for data fetching on mount:


```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUser = async () => {
      try {
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        setUser(userData);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <div>Welcome, {user?.name}</div>;
}
```

**Key patterns:**
- Fetch in `useEffect`, not render
- Handle loading/error states
- Include dependencies in dependency array
</details>

- **Q2:** Why shouldn't you fetch data directly in the render method?
<details>
<summary>Answer</summary>
**Problems with fetching in render:**
- **Infinite requests** - Component re-renders trigger new fetches
- **Performance** - Blocks rendering
- **Side effects** - Render should be pure


```jsx
// ❌ Wrong - fetches on every render
function BadComponent() {
  const [data, setData] = useState(null);
  
  // This runs on every render!
  fetch('/api/data').then(res => res.json()).then(setData);
  
  return <div>{data?.message}</div>;
}

// ✅ Correct - fetch in useEffect
function GoodComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetch('/api/data').then(res => res.json()).then(setData);
  }, []); // Runs once on mount
  
  return <div>{data?.message}</div>;
}
```
</details>

- **Q3:** What's the difference between fetch and axios for React applications?
<details>
<summary>Answer</summary>
| Feature | fetch | axios |
|---------|-------|-------|
| **Bundle size** | Built-in (0KB) | ~13KB |
| **JSON parsing** | Manual | Automatic |
| **Error handling** | Only network errors | HTTP errors too |
| **Request/Response interceptors** | No | Yes |
| **Request cancellation** | AbortController | CancelToken |
| **Timeout** | Manual | Built-in |


```jsx
// fetch
const fetchData = async () => {
  const response = await fetch('/api/data');
  if (!response.ok) {
    throw new Error('Request failed');
  }
  return response.json();
};

// axios
const fetchData = async () => {
  const response = await axios.get('/api/data');
  return response.data; // Already parsed
};
```

**Choose fetch for:** Simple requests, smaller bundle size
**Choose axios for:** Complex apps, interceptors, better DX
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you handle race conditions when fetching data?
<details>
<summary>Answer</summary>
**Race condition:** Multiple requests in flight, latest request might not be last response.


```jsx
// ❌ Race condition problem
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    if (query) {
      searchAPI(query).then(setResults); // Race condition!
    }
  }, [query]);
  
  return <div>{results.map(r => <div key={r.id}>{r.title}</div>)}</div>;
}

// ✅ Solution 1: Ignore stale requests
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    let ignore = false;
    
    if (query) {
      searchAPI(query).then(data => {
        if (!ignore) setResults(data);
      });
    }
    
    return () => { ignore = true; };
  }, [query]);
  
  return <div>{results.map(r => <div key={r.id}>{r.title}</div>)}</div>;
}

// ✅ Solution 2: AbortController
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    const controller = new AbortController();
    
    if (query) {
      fetch(`/api/search?q=${query}`, { signal: controller.signal })
        .then(res => res.json())
        .then(setResults)
        .catch(err => {
          if (err.name !== 'AbortError') console.error(err);
        });
    }
    
    return () => controller.abort();
  }, [query]);
  
  return <div>{results.map(r => <div key={r.id}>{r.title}</div>)}</div>;
}
```
</details>

- **Q2:** How do you implement request cancellation?
<details>
<summary>Answer</summary>

```jsx
// Method 1: AbortController (fetch)
function DataComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    const controller = new AbortController();
    
    fetch('/api/data', { signal: controller.signal })
      .then(res => res.json())
      .then(setData)
      .catch(err => {
        if (err.name !== 'AbortError') {
          console.error('Fetch failed:', err);
        }
      });
    
    return () => controller.abort();
  }, []);
  
  return <div>{data?.message}</div>;
}

// Method 2: Axios cancellation
import axios from 'axios';

function DataComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    const source = axios.CancelToken.source();
    
    axios.get('/api/data', { cancelToken: source.token })
      .then(response => setData(response.data))
      .catch(err => {
        if (!axios.isCancel(err)) {
          console.error('Request failed:', err);
        }
      });
    
    return () => source.cancel('Request cancelled');
  }, []);
  
  return <div>{data?.message}</div>;
}

// Custom hook for cancellable requests
function useCancellableRequest() {
  const controller = useRef();
  
  useEffect(() => {
    controller.current = new AbortController();
    return () => controller.current?.abort();
  }, []);
  
  const request = useCallback(async (url) => {
    return fetch(url, { signal: controller.current.signal });
  }, []);
  
  return request;
}
```
</details>

---

### 🎯 **Async in useEffect**

#### **📋 Beginner:**
- **Q1:** Can you make the useEffect callback function async?
<details>
<summary>Answer</summary>
**No, you cannot make the useEffect callback async directly.**


```jsx
// ❌ Wrong - useEffect callback cannot be async
useEffect(async () => {
  const data = await fetch('/api/data');
  setData(data);
}, []); // Error: Effect callbacks are synchronous

// ✅ Correct - define async function inside
useEffect(() => {
  const fetchData = async () => {
    const response = await fetch('/api/data');
    const data = await response.json();
    setData(data);
  };
  
  fetchData();
}, []);

// ✅ Alternative - IIFE (Immediately Invoked Function Expression)
useEffect(() => {
  (async () => {
    const response = await fetch('/api/data');
    const data = await response.json();
    setData(data);
  })();
}, []);
```

**Why?** useEffect expects either nothing or a cleanup function to be returned, not a Promise.
</details>

- **Q2:** How do you properly handle async operations in useEffect?
<details>
<summary>Answer</summary>

```jsx
function DataComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let isCancelled = false;
    
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      
      try {
        const response = await fetch('/api/data');
        if (!response.ok) throw new Error('Failed to fetch');
        
        const result = await response.json();
        
        // Check if component is still mounted
        if (!isCancelled) {
          setData(result);
        }
      } catch (err) {
        if (!isCancelled) {
          setError(err.message);
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    };
    
    fetchData();
    
    // Cleanup function
    return () => {
      isCancelled = true;
    };
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <div>{JSON.stringify(data)}</div>;
}
```

**Best practices:**
- Use cancellation flags
- Handle loading/error states
- Cleanup on unmount
</details>

- **Q3:** How do you clean up async operations in useEffect?
<details>
<summary>Answer</summary>

```jsx
// Method 1: Cancellation flag
useEffect(() => {
  let cancelled = false;
  
  const fetchData = async () => {
    const data = await api.getData();
    if (!cancelled) setData(data);
  };
  
  fetchData();
  
  return () => { cancelled = true; };
}, []);

// Method 2: AbortController
useEffect(() => {
  const controller = new AbortController();
  
  fetch('/api/data', { signal: controller.signal })
    .then(res => res.json())
    .then(setData)
    .catch(err => {
      if (err.name !== 'AbortError') console.error(err);
    });
  
  return () => controller.abort();
}, []);

// Method 3: Custom hook for cleanup
function useAsyncEffect(asyncFn, deps) {
  useEffect(() => {
    let cancelled = false;
    
    asyncFn().then(result => {
      if (!cancelled) return result;
    });
    
    return () => { cancelled = true; };
  }, deps);
}

// Usage
function Component() {
  const [data, setData] = useState(null);
  
  useAsyncEffect(async () => {
    const result = await fetch('/api/data').then(r => r.json());
    setData(result);
  }, []);
  
  return <div>{data?.message}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you handle component unmounting during async operations?
<details>
<summary>Answer</summary>

```jsx
// Custom hook for safe async operations
function useSafeAsync() {
  const mountedRef = useRef(true);
  
  useEffect(() => {
    return () => { mountedRef.current = false; };
  }, []);
  
  const safeSetState = useCallback((setter) => {
    if (mountedRef.current) setter();
  }, []);
  
  return safeSetState;
}

// Usage
function DataComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const safeSetState = useSafeAsync();
  
  useEffect(() => {
    const fetchData = async () => {
      safeSetState(() => setLoading(true));
      
      try {
        const result = await fetch('/api/data').then(r => r.json());
        safeSetState(() => {
          setData(result);
          setLoading(false);
        });
      } catch (error) {
        safeSetState(() => setLoading(false));
      }
    };
    
    fetchData();
  }, [safeSetState]);
  
  return loading ? <div>Loading...</div> : <div>{data?.message}</div>;
}

// Alternative: useIsMounted hook
function useIsMounted() {
  const isMounted = useRef(true);
  
  useEffect(() => {
    return () => { isMounted.current = false; };
  }, []);
  
  return useCallback(() => isMounted.current, []);
}
```
</details>

- **Q2:** How do you implement proper error handling for async useEffect operations?
<details>
<summary>Answer</summary>

```jsx
function useAsyncError() {
  const [, setError] = useState();
  
  return useCallback((error) => {
    setError(() => { throw error; });
  }, []);
}

function DataComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const throwError = useAsyncError();
  
  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      
      try {
        const response = await fetch('/api/data');
        
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (error) {
        // This will be caught by Error Boundary
        throwError(error);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [throwError]);
  
  return loading ? <div>Loading...</div> : <div>{data?.message}</div>;
}

// Enhanced error handling with retry
function useAsyncOperation() {
  const [state, setState] = useState({
    data: null,
    loading: false,
    error: null
  });
  
  const execute = useCallback(async (asyncFn, retries = 3) => {
    setState(prev => ({ ...prev, loading: true, error: null }));
    
    for (let attempt = 1; attempt <= retries; attempt++) {
      try {
        const result = await asyncFn();
        setState({ data: result, loading: false, error: null });
        return result;
      } catch (error) {
        if (attempt === retries) {
          setState(prev => ({ ...prev, loading: false, error }));
          throw error;
        }
        
        // Wait before retry
        await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
      }
    }
  }, []);
  
  return { ...state, execute };
}
```
</details>

---

### 🎯 **Suspense for data fetching**

#### **📋 Beginner:**
- **Q1:** How will Suspense work with data fetching?
<details>
<summary>Answer</summary>
**Suspense** allows components to "wait" for data and show fallback UI while loading.


```jsx
// Component that uses Suspense-compatible data fetching
function UserProfile({ userId }) {
  const user = useSuspenseQuery(['user', userId], () => 
    fetch(`/api/users/${userId}`).then(r => r.json())
  );
  
  return <div>Welcome, {user.name}</div>;
}

// Wrap with Suspense boundary
function App() {
  return (
    <Suspense fallback={<div>Loading user...</div>}>
      <UserProfile userId={1} />
    </Suspense>
  );
}

// Multiple components can share loading state
function Dashboard() {
  return (
    <Suspense fallback={<div>Loading dashboard...</div>}>
      <UserProfile userId={1} />
      <UserPosts userId={1} />
      <UserSettings userId={1} />
    </Suspense>
  );
}
```

**Benefits:**
- Declarative loading states
- Coordinated loading (multiple components)
- Better UX with concurrent features
</details>

- **Q2:** What is concurrent rendering and how does it relate to Suspense?
<details>
<summary>Answer</summary>
**Concurrent rendering** allows React to pause and resume work, making apps more responsive.


```jsx
// Before: Blocking rendering
function App() {
  const [count, setCount] = useState(0);
  const [data, setData] = useState(null);
  
  // This blocks everything until data loads
  useEffect(() => {
    fetch('/api/data').then(setData);
  }, []);
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>
        Count: {count}
      </button>
      {data ? <DataView data={data} /> : <div>Loading...</div>}
    </div>
  );
}

// After: Non-blocking with Suspense
function App() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>
        Count: {count} {/* This stays responsive */}
      </button>
      <Suspense fallback={<div>Loading...</div>}>
        <DataView /> {/* This can suspend without blocking */}
      </Suspense>
    </div>
  );
}

// Concurrent features
function DataView() {
  const data = useSuspenseQuery(['data'], fetchData);
  
  return (
    <div>
      {/* React can interrupt this render if higher priority work comes in */}
      {data.items.map(item => <ExpensiveItem key={item.id} item={item} />)}
    </div>
  );
}
```

**Key concepts:**
- **Interruptible rendering** - React can pause work
- **Priority-based updates** - Urgent updates interrupt less urgent ones
- **Time slicing** - Break work into chunks
</details>

- **Q3:** What are the current limitations of Suspense for data fetching?
<details>
<summary>Answer</summary>
**Current limitations:**

1. **Limited library support** - Only React Query, SWR, Relay support it
2. **No built-in data fetching** - Need third-party libraries
3. **SSR complexity** - Server-side rendering needs special handling
4. **Error boundaries required** - Need error boundaries for error handling


```jsx
// ❌ Won't work - regular fetch doesn't support Suspense
function UserProfile() {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetch('/api/user').then(setUser);
  }, []);
  
  return <div>{user?.name}</div>;
}

// ✅ Works - using library with Suspense support
function UserProfile() {
  const { data: user } = useSuspenseQuery(['user'], fetchUser);
  return <div>{user.name}</div>;
}

// Need Error Boundary for error handling
function App() {
  return (
    <ErrorBoundary fallback={<div>Something went wrong</div>}>
      <Suspense fallback={<div>Loading...</div>}>
        <UserProfile />
      </Suspense>
    </ErrorBoundary>
  );
}
```

**Workarounds:**
- Use React Query/SWR with Suspense mode
- Implement custom Suspense-compatible wrappers
- Wait for more ecosystem support
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement data fetching libraries that work with Suspense?
<details>
<summary>Answer</summary>

```jsx
// Basic Suspense-compatible cache
class SuspenseCache {
  constructor() {
    this.cache = new Map();
  }
  
  read(key, fetcher) {
    if (this.cache.has(key)) {
      const entry = this.cache.get(key);
      
      if (entry.status === 'resolved') {
        return entry.data;
      } else if (entry.status === 'rejected') {
        throw entry.error;
      } else {
        throw entry.promise; // Suspense expects a Promise
      }
    }
    
    // Start the request
    const promise = fetcher()
      .then(data => {
        this.cache.set(key, { status: 'resolved', data });
      })
      .catch(error => {
        this.cache.set(key, { status: 'rejected', error });
      });
    
    this.cache.set(key, { status: 'pending', promise });
    throw promise; // Suspend until resolved
  }
}

const cache = new SuspenseCache();

// Hook that works with Suspense
function useSuspenseData(key, fetcher) {
  return cache.read(key, fetcher);
}

// Usage
function UserProfile({ userId }) {
  const user = useSuspenseData(
    `user-${userId}`,
    () => fetch(`/api/users/${userId}`).then(r => r.json())
  );
  
  return <div>Welcome, {user.name}</div>;
}

// Advanced: Resource pattern
function createResource(fetcher) {
  let status = 'pending';
  let result;
  
  const promise = fetcher()
    .then(data => {
      status = 'resolved';
      result = data;
    })
    .catch(error => {
      status = 'rejected';
      result = error;
    });
  
  return {
    read() {
      if (status === 'pending') throw promise;
      if (status === 'rejected') throw result;
      return result;
    }
  };
}

const userResource = createResource(() => 
  fetch('/api/user').then(r => r.json())
);

function UserProfile() {
  const user = userResource.read();
  return <div>{user.name}</div>;
}
```
</details>

- **Q2:** How do you handle error boundaries with Suspense data fetching?
<details>
<summary>Answer</summary>

```jsx
// Suspense-aware Error Boundary
class SuspenseErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Suspense error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Try again
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Usage with nested boundaries
function App() {
  return (
    <SuspenseErrorBoundary>
      <Suspense fallback={<div>Loading app...</div>}>
        <Header />
        
        <SuspenseErrorBoundary>
          <Suspense fallback={<div>Loading user data...</div>}>
            <UserProfile />
          </Suspense>
        </SuspenseErrorBoundary>
        
        <SuspenseErrorBoundary>
          <Suspense fallback={<div>Loading posts...</div>}>
            <PostsList />
          </Suspense>
        </SuspenseErrorBoundary>
      </Suspense>
    </SuspenseErrorBoundary>
  );
}

// Hook for resetting error boundaries
function useErrorReset() {
  const [resetKey, setResetKey] = useState(0);
  
  const reset = useCallback(() => {
    setResetKey(prev => prev + 1);
  }, []);
  
  return [resetKey, reset];
}

function AppWithReset() {
  const [resetKey, resetError] = useErrorReset();
  
  return (
    <SuspenseErrorBoundary key={resetKey}>
      <Suspense fallback={<div>Loading...</div>}>
        <DataComponent />
        <button onClick={resetError}>Reset</button>
      </Suspense>
    </SuspenseErrorBoundary>
  );
}
```
</details>

---

## 🔹 **React 18 Features**

---

### 🎯 **Automatic batching**

#### **📋 Beginner:**
- **Q1:** What is automatic batching in React 18?
<details>
<summary>Answer</summary>
**Automatic batching** groups multiple state updates into a single re-render for better performance.


```jsx
// React 18 - All these updates are batched automatically
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);
  
  const handleClick = () => {
    setCount(c => c + 1); // Doesn't re-render yet
    setFlag(f => !f);     // Doesn't re-render yet
    // React batches both updates - only 1 re-render
  };
  
  // Even in setTimeout, Promise.then, etc.
  const handleAsync = () => {
    setTimeout(() => {
      setCount(c => c + 1); // Batched
      setFlag(f => !f);     // Batched together
    }, 1000);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <p>Flag: {flag.toString()}</p>
      <button onClick={handleClick}>Sync Update</button>
      <button onClick={handleAsync}>Async Update</button>
    </div>
  );
}
```

**Benefits:**
- Fewer re-renders
- Better performance
- More consistent behavior
</details>

- **Q2:** How does automatic batching improve performance?
<details>
<summary>Answer</summary>
**Performance improvements:**


```jsx
// Without batching (React 17 behavior)
function Component() {
  const [name, setName] = useState('');
  const [age, setAge] = useState(0);
  const [email, setEmail] = useState('');
  
  const handleSubmit = async () => {
    const data = await fetchUserData();
    
    // In React 17: 3 separate re-renders
    setName(data.name);    // Re-render 1
    setAge(data.age);      // Re-render 2  
    setEmail(data.email);  // Re-render 3
  };
  
  // Expensive computation runs 3 times
  const expensiveValue = useMemo(() => {
    console.log('Computing expensive value...');
    return name + age + email;
  }, [name, age, email]);
  
  return <div>{expensiveValue}</div>;
}

// With automatic batching (React 18)
// Same code results in only 1 re-render and 1 computation
```

**Measurements:**
- **3x fewer renders** in many async scenarios
- **Reduced layout thrashing** in DOM
- **Better frame rates** for animations
- **Fewer effect runs** and computations
</details>

- **Q3:** What changed from React 17 to React 18 regarding batching?
<details>
<summary>Answer</summary>
| Scenario | React 17 | React 18 |
|----------|----------|----------|
| **Event handlers** | ✅ Batched | ✅ Batched |
| **setTimeout** | ❌ Not batched | ✅ Batched |
| **Promise.then** | ❌ Not batched | ✅ Batched |
| **Native events** | ❌ Not batched | ✅ Batched |
| **fetch callbacks** | ❌ Not batched | ✅ Batched |


```jsx
// React 17 vs 18 comparison
function Example() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  
  // React 17: Batched ✅
  // React 18: Batched ✅
  const handleClick = () => {
    setCount(1);
    setName('John');
  };
  
  // React 17: NOT batched ❌ (2 renders)
  // React 18: Batched ✅ (1 render)
  const handleTimeout = () => {
    setTimeout(() => {
      setCount(2);
      setName('Jane');
    }, 100);
  };
  
  // React 17: NOT batched ❌ (2 renders)  
  // React 18: Batched ✅ (1 render)
  const handleFetch = () => {
    fetch('/api/data').then(() => {
      setCount(3);
      setName('Bob');
    });
  };
  
  console.log('Render count:', ++renderCount);
  return <div>{count} - {name}</div>;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you opt out of automatic batching when needed?
<details>
<summary>Answer</summary>
Use **flushSync** to force synchronous updates:


```jsx
import { flushSync } from 'react-dom';

function SearchComponent() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  const handleSearch = (newQuery) => {
    // Force immediate update for query (for focus management)
    flushSync(() => {
      setQuery(newQuery);
    });
    
    // This will be a separate render
    setResults(searchResults(newQuery));
  };
  
  // Scroll to top immediately after query updates
  useLayoutEffect(() => {
    if (query) {
      document.getElementById('results').scrollTop = 0;
    }
  }, [query]);
  
  return (
    <div>
      <input value={query} onChange={e => handleSearch(e.target.value)} />
      <div id="results">{results.map(r => <div key={r.id}>{r.title}</div>)}</div>
    </div>
  );
}

// When to use flushSync
function FormWithValidation() {
  const [value, setValue] = useState('');
  const [error, setError] = useState('');
  
  const handleChange = (newValue) => {
    // Update value immediately for input responsiveness
    flushSync(() => {
      setValue(newValue);
    });
    
    // Validate in next render to avoid blocking input
    setError(validate(newValue));
  };
  
  return (
    <div>
      <input value={value} onChange={e => handleChange(e.target.value)} />
      {error && <div className="error">{error}</div>}
    </div>
  );
}
```

**Use flushSync sparingly** - it can hurt performance!
</details>

- **Q2:** What are the implications of automatic batching for testing?
<details>
<summary>Answer</summary>
**Testing considerations:**


```jsx
// Test might need act() for async updates
import { act, render, fireEvent } from '@testing-library/react';

function AsyncComponent() {
  const [count, setCount] = useState(0);
  const [status, setStatus] = useState('idle');
  
  const handleClick = () => {
    setTimeout(() => {
      setCount(1);
      setStatus('done');
    }, 0);
  };
  
  return (
    <div>
      <span data-testid="count">{count}</span>
      <span data-testid="status">{status}</span>
      <button onClick={handleClick}>Update</button>
    </div>
  );
}

// ✅ Correct test with act()
test('updates are batched in async operations', async () => {
  const { getByTestId, getByRole } = render(<AsyncComponent />);
  
  await act(async () => {
    fireEvent.click(getByRole('button'));
    
    // Wait for timeout
    await new Promise(resolve => setTimeout(resolve, 0));
  });
  
  // Both updates happened in single batch
  expect(getByTestId('count')).toHaveTextContent('1');
  expect(getByTestId('status')).toHaveTextContent('done');
});

// Testing render counts
test('automatic batching reduces renders', () => {
  let renderCount = 0;
  
  function TestComponent() {
    renderCount++;
    const [a, setA] = useState(0);
    const [b, setB] = useState(0);
    
    useEffect(() => {
      // This would cause 2 renders in React 17, 1 in React 18
      setTimeout(() => {
        setA(1);
        setB(2);
      }, 0);
    }, []);
    
    return <div>{a + b}</div>;
  }
  
  render(<TestComponent />);
  
  act(() => {
    jest.advanceTimersByTime(0);
  });
  
  // React 18: renderCount === 2 (initial + 1 batched update)
  // React 17: renderCount === 3 (initial + 2 separate updates)
  expect(renderCount).toBe(2);
});
```
</details>

---

### 🎯 **Transitions (useTransition)**

#### **📋 Beginner:**
- **Q1:** What is useTransition and what problem does it solve?
<details>
<summary>Answer</summary>
**useTransition** marks updates as non-urgent, letting React prioritize more important updates.


```jsx
import { useTransition, useState } from 'react';

function SearchApp() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleSearch = (newQuery) => {
    // Urgent: Update input immediately
    setQuery(newQuery);
    
    // Non-urgent: Search results can wait
    startTransition(() => {
      setResults(performExpensiveSearch(newQuery));
    });
  };
  
  return (
    <div>
      <input 
        value={query} 
        onChange={e => handleSearch(e.target.value)}
        placeholder="Search..."
      />
      
      {isPending && <div>Searching...</div>}
      
      <div>
        {results.map(result => (
          <div key={result.id}>{result.title}</div>
        ))}
      </div>
    </div>
  );
}
```

**Problems it solves:**
- **Input lag** - Keeps UI responsive during heavy updates
- **Blocking updates** - Prevents expensive renders from freezing app
- **Better UX** - Users can keep typing while search happens
</details>

- **Q2:** How do you mark updates as transitions?
<details>
<summary>Answer</summary>

```jsx
function App() {
  const [tab, setTab] = useState('home');
  const [content, setContent] = useState('');
  const [isPending, startTransition] = useTransition();
  
  const handleTabChange = (newTab) => {
    // Urgent: Tab selection should be immediate
    setTab(newTab);
    
    // Non-urgent: Content loading can be delayed
    startTransition(() => {
      setContent(loadHeavyContent(newTab));
    });
  };
  
  return (
    <div>
      <div className="tabs">
        {['home', 'profile', 'settings'].map(tabName => (
          <button
            key={tabName}
            className={tab === tabName ? 'active' : ''}
            onClick={() => handleTabChange(tabName)}
          >
            {tabName}
          </button>
        ))}
      </div>
      
      <div className={`content ${isPending ? 'loading' : ''}`}>
        {content}
      </div>
    </div>
  );
}

// Alternative: useDeferredValue
function SearchWithDeferred() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  const results = useMemo(() => 
    search(deferredQuery), [deferredQuery]
  );
  
  return (
    <div>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <SearchResults query={deferredQuery} results={results} />
    </div>
  );
}
```
</details>

- **Q3:** What is the isPending value in useTransition?
<details>
<summary>Answer</summary>
**isPending** indicates if any transition updates are currently pending.


```jsx
function DataTable() {
  const [filter, setFilter] = useState('');
  const [data, setData] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleFilterChange = (newFilter) => {
    setFilter(newFilter); // Immediate
    
    startTransition(() => {
      // Heavy filtering operation
      setData(filterLargeDataset(allData, newFilter));
    });
  };
  
  return (
    <div>
      <input 
        value={filter} 
        onChange={e => handleFilterChange(e.target.value)}
        placeholder="Filter data..."
      />
      
      {/* Show loading state during transition */}
      {isPending && (
        <div className="loading-overlay">
          Processing...
        </div>
      )}
      
      <table className={isPending ? 'dimmed' : ''}>
        <tbody>
          {data.map(row => (
            <tr key={row.id}>
              <td>{row.name}</td>
              <td>{row.value}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

// isPending usage patterns
function useTransitionState(initialState) {
  const [state, setState] = useState(initialState);
  const [isPending, startTransition] = useTransition();
  
  const setTransitionState = (newState) => {
    startTransition(() => {
      setState(newState);
    });
  };
  
  return [state, setTransitionState, isPending];
}
```

**isPending is true when:**
- Transition updates are being processed
- React is working on non-urgent updates
- Useful for showing loading states
</details>

#### **🚀 Intermediate:**
- **Q1:** How do transitions improve user experience in large lists?
<details>
<summary>Answer</summary>

```jsx
function ContactList() {
  const [searchTerm, setSearchTerm] = useState('');
  const [filteredContacts, setFilteredContacts] = useState(contacts);
  const [isPending, startTransition] = useTransition();
  
  const handleSearch = (term) => {
    // Keep input responsive
    setSearchTerm(term);
    
    // Defer expensive filtering
    startTransition(() => {
      const filtered = contacts.filter(contact =>
        contact.name.toLowerCase().includes(term.toLowerCase()) ||
        contact.email.toLowerCase().includes(term.toLowerCase())
      );
      setFilteredContacts(filtered);
    });
  };
  
  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={e => handleSearch(e.target.value)}
        placeholder="Search 10,000 contacts..."
        className={isPending ? 'searching' : ''}
      />
      
      <div className="contact-count">
        {filteredContacts.length} contacts
        {isPending && ' (updating...)'}
      </div>
      
      <VirtualizedList items={filteredContacts} />
    </div>
  );
}

// Optimized list rendering with transitions
function OptimizedList({ items }) {
  const [displayItems, setDisplayItems] = useState(items.slice(0, 50));
  const [isPending, startTransition] = useTransition();
  
  useEffect(() => {
    startTransition(() => {
      // Gradually load more items
      setDisplayItems(items);
    });
  }, [items]);
  
  return (
    <div>
      {displayItems.map(item => (
        <ExpensiveListItem key={item.id} item={item} />
      ))}
      {isPending && <div>Loading more items...</div>}
    </div>
  );
}
```

**Benefits:**
- **Responsive input** - Typing never feels laggy
- **Progressive loading** - Show partial results while processing
- **Interruptible updates** - New searches cancel old ones
</details>

- **Q2:** When should you avoid using transitions?
<details>
<summary>Answer</summary>
**Avoid transitions for:**


```jsx
// ❌ Don't use for urgent updates
function LoginForm() {
  const [error, setError] = useState('');
  const [isPending, startTransition] = useTransition();
  
  const handleSubmit = async () => {
    try {
      await login();
    } catch (err) {
      // Don't defer error messages!
      startTransition(() => {
        setError(err.message); // Wrong - errors should be immediate
      });
    }
  };
  
  return <form onSubmit={handleSubmit}>...</form>;
}

// ❌ Don't use for focus management
function Modal({ isOpen, onClose }) {
  const [isPending, startTransition] = useTransition();
  
  useEffect(() => {
    if (isOpen) {
      startTransition(() => {
        // Wrong - focus should be immediate for accessibility
        document.getElementById('modal-input')?.focus();
      });
    }
  }, [isOpen]);
  
  return isOpen ? <div>...</div> : null;
}

// ❌ Don't use for animations
function AnimatedButton({ onClick }) {
  const [isPressed, setIsPressed] = useState(false);
  const [isPending, startTransition] = useTransition();
  
  const handleClick = () => {
    // Wrong - button press feedback should be immediate
    startTransition(() => {
      setIsPressed(true);
    });
    
    setTimeout(() => setIsPressed(false), 150);
    onClick();
  };
  
  return <button onClick={handleClick}>...</button>;
}
```

**Use transitions for:**
- Heavy computations (filtering, sorting)
- Non-critical UI updates
- Background data processing

**Don't use for:**
- User feedback (errors, success messages)
- Accessibility actions (focus, announcements)
- Animation triggers
- Form validation
- Navigation responses
</details>

---

### 🎯 **useDeferredValue**

#### **📋 Beginner:**
- **Q1:** What is useDeferredValue and how does it work?
<details>
<summary>Answer</summary>
**useDeferredValue** lets you defer updates to a value during urgent updates.


```jsx
import { useDeferredValue, useState } from 'react';

function SearchApp() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      
      {/* This component uses the deferred value */}
      <SearchResults query={deferredQuery} />
    </div>
  );
}

function SearchResults({ query }) {
  const results = useMemo(() => {
    // Expensive search only runs when deferredQuery changes
    return performExpensiveSearch(query);
  }, [query]);
  
  return (
    <div>
      {results.map(result => (
        <div key={result.id}>{result.title}</div>
      ))}
    </div>
  );
}
```

**How it works:**
- During typing: `query` updates immediately, `deferredQuery` stays old
- When typing stops: `deferredQuery` updates to match `query`
- Keeps UI responsive during rapid changes
</details>

- **Q2:** How is useDeferredValue different from useTransition?
<details>
<summary>Answer</summary>
| Feature | useTransition | useDeferredValue |
|---------|---------------|------------------|
| **Control** | You control when to defer | React controls timing |
| **Usage** | Wrap state updates | Defer a value |
| **isPending** | Provides pending state | No pending indicator |
| **Best for** | Manual optimization | Component props |


```jsx
// useTransition - you control the deferring
function TransitionExample() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleSearch = (newQuery) => {
    setQuery(newQuery); // Immediate
    
    startTransition(() => {
      setResults(search(newQuery)); // Deferred
    });
  };
  
  return (
    <div>
      <input onChange={e => handleSearch(e.target.value)} />
      {isPending && <div>Loading...</div>}
      <Results data={results} />
    </div>
  );
}

// useDeferredValue - React controls the deferring
function DeferredExample() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)} 
      />
      
      {/* React defers this component's updates */}
      <ExpensiveResults query={deferredQuery} />
    </div>
  );
}
```

**Choose useTransition when:** You want control and loading states
**Choose useDeferredValue when:** Optimizing child component props
</details>

- **Q3:** When would you use useDeferredValue?
<details>
<summary>Answer</summary>
**Use useDeferredValue for:**


```jsx
// 1. Search/Filter inputs
function ProductSearch() {
  const [searchTerm, setSearchTerm] = useState('');
  const deferredSearchTerm = useDeferredValue(searchTerm);
  
  return (
    <div>
      <input 
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search products..."
      />
      <ProductList searchTerm={deferredSearchTerm} />
    </div>
  );
}

// 2. Expensive visualizations
function ChartComponent() {
  const [data, setData] = useState([]);
  const deferredData = useDeferredValue(data);
  
  return (
    <div>
      <DataControls onDataChange={setData} />
      <ExpensiveChart data={deferredData} />
    </div>
  );
}

// 3. Large lists with filtering
function ContactList() {
  const [filter, setFilter] = useState('');
  const deferredFilter = useDeferredValue(filter);
  
  const filteredContacts = useMemo(() => 
    contacts.filter(c => c.name.includes(deferredFilter)),
    [deferredFilter]
  );
  
  return (
    <div>
      <input 
        value={filter}
        onChange={e => setFilter(e.target.value)}
      />
      <VirtualizedList items={filteredContacts} />
    </div>
  );
}

// 4. Third-party components you can't modify
function MapWithSearch() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  return (
    <div>
      <input onChange={e => setQuery(e.target.value)} />
      {/* Can't modify this expensive component */}
      <ThirdPartyMapComponent searchQuery={deferredQuery} />
    </div>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement debounced search with useDeferredValue?
<details>
<summary>Answer</summary>

```jsx
// Method 1: Pure useDeferredValue (React handles timing)
function SearchWithDeferred() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  const searchResults = useMemo(() => {
    if (!deferredQuery) return [];
    return searchAPI(deferredQuery);
  }, [deferredQuery]);
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Type to search..."
      />
      
      <div>
        {query !== deferredQuery && <div>Searching...</div>}
        {searchResults.map(result => (
          <div key={result.id}>{result.title}</div>
        ))}
      </div>
    </div>
  );
}

// Method 2: Combined with custom debounce
function useDebounced(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => clearTimeout(timer);
  }, [value, delay]);
  
  return useDeferredValue(debouncedValue);
}

function AdvancedSearch() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDebounced(query, 300);
  
  const { data, isLoading } = useQuery({
    queryKey: ['search', deferredQuery],
    queryFn: () => searchAPI(deferredQuery),
    enabled: !!deferredQuery
  });
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)}
      />
      
      {isLoading && <div>Loading...</div>}
      <SearchResults results={data} />
    </div>
  );
}

// Method 3: Smart search with different strategies
function SmartSearch() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  // Fast local search
  const localResults = useMemo(() => 
    localData.filter(item => 
      item.title.toLowerCase().includes(query.toLowerCase())
    ).slice(0, 5),
    [query]
  );
  
  // Slower remote search (deferred)
  const { data: remoteResults } = useQuery({
    queryKey: ['remoteSearch', deferredQuery],
    queryFn: () => remoteSearch(deferredQuery),
    enabled: deferredQuery.length > 2
  });
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)}
      />
      
      {localResults.length > 0 && (
        <div>
          <h3>Quick Results</h3>
          {localResults.map(r => <div key={r.id}>{r.title}</div>)}
        </div>
      )}
      
      {query !== deferredQuery && (
        <div>Searching online...</div>
      )}
      
      {remoteResults && (
        <div>
          <h3>Full Results</h3>
          {remoteResults.map(r => <div key={r.id}>{r.title}</div>)}
        </div>
      )}
    </div>
  );
}
```
</details>

- **Q2:** What are the performance implications of useDeferredValue?
<details>
<summary>Answer</summary>

```jsx
// Performance monitoring with useDeferredValue
function PerformanceExample() {
  const [input, setInput] = useState('');
  const deferredInput = useDeferredValue(input);
  
  // Track render performance
  const renderStart = performance.now();
  
  const expensiveComputation = useMemo(() => {
    console.log('Computing with:', deferredInput);
    // Simulate heavy computation
    return heavyCalculation(deferredInput);
  }, [deferredInput]);
  
  useEffect(() => {
    const renderTime = performance.now() - renderStart;
    console.log(`Render took: ${renderTime}ms`);
  });
  
  return (
    <div>
      <input 
        value={input}
        onChange={e => setInput(e.target.value)}
      />
      
      {/* Show if values are in sync */}
      {input !== deferredInput && (
        <div style={{ color: 'orange' }}>
          Processing... (input ahead of computation)
        </div>
      )}
      
      <ExpensiveVisualization data={expensiveComputation} />
    </div>
  );
}

// Measuring impact
function useDeferredValueMetrics(value) {
  const deferredValue = useDeferredValue(value);
  const [metrics, setMetrics] = useState({
    syncRenders: 0,
    deferredRenders: 0,
    totalDelay: 0
  });
  
  const lastValueRef = useRef(value);
  const lastDeferredRef = useRef(deferredValue);
  const timestampRef = useRef(Date.now());
  
  useEffect(() => {
    if (lastValueRef.current !== value) {
      setMetrics(prev => ({
        ...prev,
        syncRenders: prev.syncRenders + 1
      }));
      timestampRef.current = Date.now();
    }
    lastValueRef.current = value;
  }, [value]);
  
  useEffect(() => {
    if (lastDeferredRef.current !== deferredValue) {
      const delay = Date.now() - timestampRef.current;
      setMetrics(prev => ({
        ...prev,
        deferredRenders: prev.deferredRenders + 1,
        totalDelay: prev.totalDelay + delay
      }));
    }
    lastDeferredRef.current = deferredValue;
  }, [deferredValue]);
  
  return { deferredValue, metrics };
}

// Best practices for performance
function OptimizedComponent() {
  const [filter, setFilter] = useState('');
  const deferredFilter = useDeferredValue(filter);
  
  // ✅ Memoize expensive operations
  const filteredData = useMemo(() => 
    largeDataset.filter(item => 
      item.name.includes(deferredFilter)
    ), 
    [deferredFilter]
  );
  
  // ✅ Use React.memo for expensive children
  const MemoizedList = useMemo(() => 
    React.memo(function List({ items }) {
      return (
        <div>
          {items.map(item => (
            <ExpensiveItem key={item.id} item={item} />
          ))}
        </div>
      );
    }),
    []
  );
  
  return (
    <div>
      <input 
        value={filter}
        onChange={e => setFilter(e.target.value)}
      />
      
      <MemoizedList items={filteredData} />
    </div>
  );
}
```

**Performance benefits:**
- **Reduced frame drops** during rapid input
- **Better responsiveness** for user interactions
- **Smoother animations** by deferring heavy work
- **Prevents UI blocking** during expensive computations
</details>

---

### 🎯 **Concurrent rendering basics**

#### **📋 Beginner:**
- **Q1:** What is concurrent rendering in React 18?
<details>
<summary>Answer</summary>
**Concurrent rendering** allows React to pause, resume, and prioritize work to keep the app responsive.


```jsx
// Before React 18: Blocking rendering
function App() {
  const [count, setCount] = useState(0);
  const [list, setList] = useState([]);
  
  const handleClick = () => {
    setCount(c => c + 1);
    
    // This blocks everything until complete
    setList(generateHugeList(10000));
  };
  
  return (
    <div>
      <button onClick={handleClick}>
        Count: {count} {/* Becomes unresponsive during list generation */}
      </button>
      <HugeList items={list} />
    </div>
  );
}

// React 18: Concurrent rendering
function App() {
  const [count, setCount] = useState(0);
  const [list, setList] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleClick = () => {
    setCount(c => c + 1); // High priority - immediate
    
    startTransition(() => {
      setList(generateHugeList(10000)); // Low priority - can be interrupted
    });
  };
  
  return (
    <div>
      <button onClick={handleClick}>
        Count: {count} {/* Stays responsive! */}
      </button>
      {isPending && <div>Updating list...</div>}
      <HugeList items={list} />
    </div>
  );
}
```

**Key features:**
- **Interruptible rendering** - React can pause work for urgent updates
- **Priority-based updates** - User interactions get higher priority
- **Time slicing** - Break work into small chunks
</details>

- **Q2:** How does concurrent rendering improve user experience?
<details>
<summary>Answer</summary>
**UX improvements:**


```jsx
// Example: Responsive search
function SearchApp() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleSearch = (newQuery) => {
    // Input stays responsive
    setQuery(newQuery);
    
    // Search can be interrupted by new input
    startTransition(() => {
      setResults(expensiveSearch(newQuery));
    });
  };
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => handleSearch(e.target.value)}
        placeholder="Type to search..." // Never lags!
      />
      
      {isPending && <div>Searching...</div>}
      <SearchResults results={results} />
    </div>
  );
}

// Example: Navigation that doesn't block
function App() {
  const [page, setPage] = useState('home');
  const [isPending, startTransition] = useTransition();
  
  const navigate = (newPage) => {
    startTransition(() => {
      setPage(newPage); // Heavy page renders don't block navigation
    });
  };
  
  return (
    <div>
      <nav>
        {['home', 'products', 'about'].map(pageName => (
          <button
            key={pageName}
            onClick={() => navigate(pageName)}
            className={page === pageName ? 'active' : ''}
          >
            {pageName}
          </button>
        ))}
      </nav>
      
      {isPending && <div>Loading page...</div>}
      <PageComponent page={page} />
    </div>
  );
}
```

**Benefits:**
- **No input lag** - Typing, clicking always feels instant
- **Smooth animations** - Concurrent work doesn't block frame updates  
- **Progressive loading** - Show partial results while processing
- **Better perceived performance** - App feels faster even if total time is same
</details>

- **Q3:** Do you need to change your code to benefit from concurrent rendering?
<details>
<summary>Answer</summary>
**Minimal changes needed** - Most apps benefit automatically:


```jsx
// 1. Update to React 18 and createRoot
// Before
import ReactDOM from 'react-dom';
ReactDOM.render(<App />, document.getElementById('root'));

// After
import { createRoot } from 'react-dom/client';
const root = createRoot(document.getElementById('root'));
root.render(<App />);

// 2. Existing code works but can be optimized
function ExistingComponent() {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    // This still works, but isn't optimized
    fetchData().then(setData);
  }, []);
  
  return <div>{data.map(item => <Item key={item.id} item={item} />)}</div>;
}

// 3. Add concurrent features for better UX
function OptimizedComponent() {
  const [query, setQuery] = useState('');
  const [data, setData] = useState([]);
  const [isPending, startTransition] = useTransition();
  
  const handleSearch = (newQuery) => {
    setQuery(newQuery);
    
    startTransition(() => {
      setData(searchData(newQuery));
    });
  };
  
  return (
    <div>
      <input value={query} onChange={e => handleSearch(e.target.value)} />
      {isPending && <div>Searching...</div>}
      <div>{data.map(item => <Item key={item.id} item={item} />)}</div>
    </div>
  );
}
```

**What's automatic:**
- **Automatic batching** - Groups state updates
- **Better scheduling** - React optimizes render timing
- **Improved hydration** - SSR apps load progressively

**What requires opt-in:**
- **useTransition** - For non-urgent updates
- **useDeferredValue** - For deferred values
- **Suspense** - For data fetching (with compatible libraries)
</details>

#### **🚀 Intermediate:**
- **Q1:** How does concurrent rendering affect component lifecycle?
<details>
<summary>Answer</summary>

```jsx
// Lifecycle methods may be called multiple times
class MyComponent extends Component {
  componentDidMount() {
    console.log('Mounted'); // Called once
  }
  
  componentDidUpdate() {
    console.log('Updated'); // May be called multiple times during concurrent work
  }
  
  render() {
    console.log('Rendering'); // Can be called multiple times and interrupted
    
    // ❌ Side effects in render can cause issues
    if (this.props.data) {
      analytics.track('data-loaded'); // May fire multiple times!
    }
    
    return <div>{this.props.data}</div>;
  }
}

// Functional components with hooks
function MyComponent({ data }) {
  // ✅ Effects run after commit, not during render
  useEffect(() => {
    if (data) {
      analytics.track('data-loaded'); // Fires once per commit
    }
  }, [data]);
  
  // ✅ Render function should be pure
  return <div>{data}</div>;
}

// Safe patterns for concurrent rendering
function SafeComponent() {
  const [count, setCount] = useState(0);
  const renderCount = useRef(0);
  
  // ✅ Safe - tracks commits, not renders
  useLayoutEffect(() => {
    renderCount.current += 1;
  });
  
  // ❌ Unsafe - tracks renders, which can be interrupted
  renderCount.current += 1;
  
  return (
    <div>
      Count: {count}
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}
```

**Key principles:**
- **Render functions must be pure** - No side effects
- **Use effects for side effects** - Not render functions
- **Multiple renders possible** - Don't rely on render count
</details>

- **Q2:** What are the implications of concurrent rendering for third-party libraries?
<details>
<summary>Answer</summary>

```jsx
// Libraries need to be concurrent-safe
// ❌ Problem: Library that mutates during render
class ProblematicLibrary {
  constructor() {
    this.listeners = [];
  }
  
  render() {
    // Bad: Side effect during render
    this.listeners.push(this.props.onUpdate);
    return <div>Content</div>;
  }
}

// ✅ Solution: Use effects for mutations
function SafeLibraryComponent({ onUpdate }) {
  const [listeners] = useState(() => new Set());
  
  useEffect(() => {
    listeners.add(onUpdate);
    return () => listeners.delete(onUpdate);
  }, [onUpdate, listeners]);
  
  return <div>Content</div>;
}

// Example: Fixing a chart library
function ChartWrapper({ data }) {
  const chartRef = useRef();
  const [chart, setChart] = useState(null);
  
  // ✅ Initialize chart in effect, not render
  useEffect(() => {
    if (chartRef.current && !chart) {
      const newChart = new ThirdPartyChart(chartRef.current);
      setChart(newChart);
    }
  }, [chart]);
  
  // ✅ Update chart in effect
  useEffect(() => {
    if (chart && data) {
      chart.updateData(data);
    }
  }, [chart, data]);
  
  // ❌ Don't do this in render
  // if (chartRef.current && data) {
  //   chart.updateData(data); // Can cause issues with concurrent rendering
  // }
  
  return <div ref={chartRef} />;
}

// Library compatibility checklist
const concurrentSafeChecklist = {
  pureRenders: "✅ Render functions have no side effects",
  effectsForSideEffects: "✅ Use useEffect/useLayoutEffect for mutations",
  noGlobalMutations: "✅ Don't mutate global state during render",
  properCleanup: "✅ Clean up subscriptions and timers",
  stableRefs: "✅ Don't create new objects/functions in render"
};

// Testing for concurrent safety
function TestConcurrentSafety() {
  const [, forceUpdate] = useReducer(x => x + 1, 0);
  
  // This should be safe to call multiple times
  const handleMultipleRenders = () => {
    // Simulate concurrent rendering by forcing multiple renders
    for (let i = 0; i < 5; i++) {
      setTimeout(forceUpdate, i * 10);
    }
  };
  
  return (
    <div>
      <button onClick={handleMultipleRenders}>
        Test Multiple Renders
      </button>
      <ThirdPartyComponent />
    </div>
  );
}
```

**Migration guidelines:**
- **Audit render functions** - Remove side effects
- **Use proper hooks** - Effects for subscriptions
- **Test with concurrent features** - Enable Strict Mode
- **Update to React 18** - Follow migration guide
</details>

---

### 🎯 **Streaming server rendering**

#### **📋 Beginner:**
- **Q1:** What is streaming server rendering?
<details>
<summary>Answer</summary>
**Streaming SSR** sends HTML to the browser in chunks as it's generated, rather than waiting for the entire page:

**Traditional SSR:**
1. Server generates complete HTML
2. Sends entire response at once
3. Browser waits for full HTML before showing anything

**Streaming SSR:**
1. Server starts sending HTML immediately
2. Sends chunks as components render
3. Browser displays content progressively


```jsx
// React 18 streaming with renderToPipeableStream
import { renderToPipeableStream } from 'react-dom/server';

function handleRequest(req, res) {
  const { pipe } = renderToPipeableStream(<App />, {
    bootstrapScripts: ['/client.js'],
    onShellReady() {
      // Send initial shell (navigation, loading states)
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/html');
      pipe(res);
    },
    onAllReady() {
      // All content including suspense boundaries resolved
      console.log('All content ready');
    },
    onError(error) {
      console.error('Streaming error:', error);
    }
  });
}

// App with streaming boundaries
function App() {
  return (
    <html>
      <head>
        <title>Streaming App</title>
      </head>
      <body>
        <Header /> {/* Renders immediately */}
        <Suspense fallback={<div>Loading posts...</div>}>
          <BlogPosts /> {/* Streams when data ready */}
        </Suspense>
        <Footer /> {/* Renders immediately */}
      </body>
    </html>
  );
}

async function BlogPosts() {
  const posts = await fetchPosts(); // Async data fetching
  return (
    <div>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.excerpt}</p>
        </article>
      ))}
    </div>
  );
}
```
</details>

- **Q2:** How does streaming SSR improve performance?
<details>
<summary>Answer</summary>
**Performance improvements:**

1. **Faster Time to First Byte (TTFB)**:

```jsx
// Traditional SSR timeline:
// 0ms: Request received
// 500ms: All data fetched, HTML generated
// 500ms: First byte sent

// Streaming SSR timeline:
// 0ms: Request received
// 50ms: Shell HTML sent (TTFB)
// 200ms: Header content streamed
// 500ms: Async content streamed
```

2. **Progressive rendering**:

```jsx
function StreamingApp() {
  return (
    <div>
      {/* Immediate: Shell renders first */}
      <Navigation />
      <main>
        {/* Fast: Synchronous content */}
        <Hero />
        
        {/* Streams when ready */}
        <Suspense fallback={<ProductsSkeleton />}>
          <ProductList /> {/* Async data */}
        </Suspense>
        
        {/* Streams independently */}
        <Suspense fallback={<ReviewsSkeleton />}>
          <ReviewSection /> {/* Different async data */}
        </Suspense>
      </main>
      <Footer />
    </div>
  );
}
```

3. **Better user experience**:
- **Immediate visual feedback** - Users see content sooner
- **Progressive enhancement** - Page builds up incrementally
- **Perceived performance** - Feels faster even if total time is same
- **Reduced layout shift** - Skeletons prevent content jumping

4. **Parallel data fetching**:

```jsx
// Multiple Suspense boundaries fetch in parallel
function Dashboard() {
  return (
    <div>
      <Suspense fallback={<UserSkeleton />}>
        <UserProfile /> {/* Fetches user data */}
      </Suspense>
      
      <Suspense fallback={<StatsSkeleton />}>
        <UserStats /> {/* Fetches stats data in parallel */}
      </Suspense>
      
      <Suspense fallback={<ActivitySkeleton />}>
        <RecentActivity /> {/* Fetches activity data in parallel */}
      </Suspense>
    </div>
  );
}
```

**Performance metrics impact:**
- **TTFB**: Significantly improved (shell sent immediately)
- **FCP**: Faster First Contentful Paint
- **LCP**: May improve if main content streams early
- **CLS**: Better with proper skeleton layouts
</details>

- **Q3:** What role does Suspense play in streaming SSR?
<details>
<summary>Answer</summary>
**Suspense enables streaming** by defining boundaries where rendering can be paused and resumed:


```jsx
// Suspense boundaries create streaming chunks
function App() {
  return (
    <div>
      {/* Chunk 1: Shell (renders immediately) */}
      <nav>Navigation</nav>
      <main>
        {/* Chunk 2: Streams when UserProfile resolves */}
        <Suspense fallback={<div>Loading profile...</div>}>
          <UserProfile />
        </Suspense>
        
        {/* Chunk 3: Streams when Posts resolves */}
        <Suspense fallback={<div>Loading posts...</div>}>
          <PostsList />
        </Suspense>
      </main>
    </div>
  );
}

// Async components that trigger Suspense
async function UserProfile() {
  const user = await fetchUser(); // Suspends here
  return <div>Welcome, {user.name}!</div>;
}

async function PostsList() {
  const posts = await fetchPosts(); // Suspends here
  return (
    <div>
      {posts.map(post => (
        <article key={post.id}>{post.title}</article>
      ))}
    </div>
  );
}
```

**Streaming process with Suspense:**

1. **Initial render**: Non-suspended components render to shell
2. **Suspense hit**: Rendering pauses, fallback shown
3. **Shell sent**: Browser receives and displays initial content
4. **Async resolves**: Component renders, content streams to browser
5. **Client hydrates**: JavaScript makes streamed content interactive


```jsx
// Advanced Suspense patterns for streaming
function BlogPage() {
  return (
    <article>
      <header>
        <h1>Blog Post Title</h1> {/* Shell content */}
        <Suspense fallback={<AuthorSkeleton />}>
          <AuthorInfo /> {/* Streams when author data ready */}
        </Suspense>
      </header>
      
      <Suspense fallback={<ContentSkeleton />}>
        <BlogContent /> {/* Streams when content ready */}
      </Suspense>
      
      <aside>
        <Suspense fallback={<SidebarSkeleton />}>
          <RelatedPosts /> {/* Streams independently */}
        </Suspense>
      </aside>
      
      <section>
        <h3>Comments</h3>
        <Suspense fallback={<CommentsSkeleton />}>
          <CommentsList /> {/* Streams when comments ready */}
        </Suspense>
      </section>
    </article>
  );
}
```

**Key Suspense patterns for streaming:**
- **Granular boundaries** - Small, focused Suspense boundaries
- **Skeleton fallbacks** - Prevent layout shift during streaming
- **Independent data** - Each boundary can resolve at different times
- **Nested Suspense** - Fine-grained control over streaming chunks
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you implement selective hydration with streaming SSR?
<details>
<summary>Answer</summary>
**Selective hydration** allows parts of the page to become interactive before the entire page has hydrated:


```jsx
// React 18 automatically enables selective hydration
import { hydrateRoot } from 'react-dom/client';

// Client-side hydration
hydrateRoot(document.getElementById('root'), <App />);

// App structure with selective hydration
function App() {
  return (
    <div>
      <Header /> {/* Hydrates first */}
      
      {/* This will hydrate when user interactions are detected */}
      <Suspense fallback={<SearchSkeleton />}>
        <SearchComponent />
      </Suspense>
      
      {/* This can hydrate independently */}
      <Suspense fallback={<ProductsSkeleton />}>
        <ProductList />
      </Suspense>
      
      {/* Lower priority - hydrates last */}
      <Suspense fallback={<FooterSkeleton />}>
        <Footer />
      </Suspense>
    </div>
  );
}

// Priority-based hydration
function PriorityApp() {
  return (
    <div>
      {/* High priority - hydrates immediately */}
      <CriticalNavigation />
      
      {/* Medium priority - hydrates on interaction */}
      <Suspense fallback={<div>Loading...</div>}>
        <InteractiveWidget />
      </Suspense>
      
      {/* Low priority - hydrates when idle */}
      <Suspense fallback={<div>Loading...</div>}>
        <AnalyticsWidget />
      </Suspense>
    </div>
  );
}

// Custom hook for controlling hydration priority
function useHydrationPriority(priority = 'normal') {
  const [shouldHydrate, setShouldHydrate] = useState(false);
  
  useEffect(() => {
    const timeouts = {
      immediate: 0,
      high: 100,
      normal: 500,
      low: 2000
    };
    
    const timer = setTimeout(() => {
      setShouldHydrate(true);
    }, timeouts[priority]);
    
    return () => clearTimeout(timer);
  }, [priority]);
  
  return shouldHydrate;
}

// Component with controlled hydration
function LazyHydratedComponent({ children, priority = 'normal' }) {
  const shouldHydrate = useHydrationPriority(priority);
  const [mounted, setMounted] = useState(false);
  
  useEffect(() => {
    if (shouldHydrate) {
      setMounted(true);
    }
  }, [shouldHydrate]);
  
  if (!mounted) {
    return <div suppressHydrationWarning={true}>Loading...</div>;
  }
  
  return children;
}

// Usage with priority control
function Dashboard() {
  return (
    <div>
      <LazyHydratedComponent priority="immediate">
        <CriticalUserInfo />
      </LazyHydratedComponent>
      
      <LazyHydratedComponent priority="normal">
        <DashboardCharts />
      </LazyHydratedComponent>
      
      <LazyHydratedComponent priority="low">
        <RecommendationsWidget />
      </LazyHydratedComponent>
    </div>
  );
}

// Interaction-based hydration
function InteractionBasedHydration({ children, triggerEvents = ['click', 'focus'] }) {
  const [shouldHydrate, setShouldHydrate] = useState(false);
  const ref = useRef();
  
  useEffect(() => {
    const element = ref.current;
    if (!element) return;
    
    const handleInteraction = () => {
      setShouldHydrate(true);
    };
    
    triggerEvents.forEach(event => {
      element.addEventListener(event, handleInteraction, { once: true });
    });
    
    return () => {
      triggerEvents.forEach(event => {
        element.removeEventListener(event, handleInteraction);
      });
    };
  }, [triggerEvents]);
  
  return (
    <div ref={ref}>
      {shouldHydrate ? children : <div>Click to activate</div>}
    </div>
  );
}
```

**Benefits of selective hydration:**
- **Faster initial interactivity** - Critical parts hydrate first
- **Better user experience** - Page responds to user interactions sooner
- **Reduced main thread blocking** - Hydration work distributed over time
- **Smart prioritization** - React prioritizes based on user behavior
</details>

- **Q2:** What are the challenges of streaming SSR implementation?
<details>
<summary>Answer</summary>
**Implementation challenges:**

1. **Error handling in streams**:

```jsx
// Robust error handling for streaming
function handleRequest(req, res) {
  let didError = false;
  
  const { pipe } = renderToPipeableStream(<App />, {
    onShellError(error) {
      // Shell failed - send error page
      res.statusCode = 500;
      res.setHeader('Content-Type', 'text/html');
      res.send('<h1>Something went wrong</h1>');
    },
    onError(error) {
      didError = true;
      console.error('Streaming error:', error);
      
      // Log error but continue streaming
      trackError(error);
    },
    onAllReady() {
      // Set status based on whether errors occurred
      res.statusCode = didError ? 500 : 200;
    }
  });
  
  pipe(res);
}

// Component-level error boundaries for streaming
class StreamingErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log error but don't break the stream
    console.error('Boundary caught error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <div>Failed to load this section</div>;
    }
    
    return this.props.children;
  }
}
```

2. **SEO and crawlers**:

```jsx
// Detect crawlers and use traditional SSR
function handleRequest(req, res) {
  const userAgent = req.headers['user-agent'] || '';
  const isCrawler = /googlebot|bingbot|slurp|duckduckbot/i.test(userAgent);
  
  if (isCrawler) {
    // Use traditional SSR for crawlers
    const html = renderToString(<App />);
    res.send(html);
  } else {
    // Use streaming for users
    const { pipe } = renderToPipeableStream(<App />, {
      onShellReady() {
        pipe(res);
      }
    });
  }
}

// Ensure critical content is in shell for SEO
function App() {
  return (
    <html>
      <head>
        <title>Page Title</title> {/* Critical for SEO */}
        <meta name="description" content="Page description" />
      </head>
      <body>
        <h1>Main Heading</h1> {/* In shell - crawlers see it */}
        
        <Suspense fallback={<div>Loading...</div>}>
          <DynamicContent /> {/* May not be seen by crawlers */}
        </Suspense>
      </body>
    </html>
  );
}
```

3. **Data fetching complexity**:

```jsx
// Coordinating multiple data sources
function ComplexPage() {
  return (
    <div>
      {/* Fast data - in shell */}
      <UserGreeting />
      
      {/* Slow data - streamed */}
      <Suspense fallback={<ReportsSkeleton />}>
        <AsyncReports />
      </Suspense>
      
      {/* Very slow data - streamed separately */}
      <Suspense fallback={<AnalyticsSkeleton />}>
        <AsyncAnalytics />
      </Suspense>
    </div>
  );
}

// Data dependency management
async function AsyncReports() {
  // Multiple dependent API calls
  const user = await fetchUser();
  const permissions = await fetchPermissions(user.id);
  const reports = await fetchReports(permissions.reportIds);
  
  return <ReportsDisplay reports={reports} />;
}

// Timeout handling for streaming components
function withTimeout(Component, timeoutMs = 5000) {
  return function TimeoutWrapper(props) {
    const [timedOut, setTimedOut] = useState(false);
    
    useEffect(() => {
      const timer = setTimeout(() => {
        setTimedOut(true);
      }, timeoutMs);
      
      return () => clearTimeout(timer);
    }, [timeoutMs]);
    
    if (timedOut) {
      return <div>Content taking too long to load</div>;
    }
    
    return <Component {...props} />;
  };
}

const TimeoutAwareComponent = withTimeout(SlowComponent, 3000);
```

4. **State management challenges**:

```jsx
// Hydration mismatches with streaming
function StreamingCounter() {
  const [count, setCount] = useState(0);
  const [isClient, setIsClient] = useState(false);
  
  useEffect(() => {
    setIsClient(true);
  }, []);
  
  // Prevent hydration mismatches
  if (!isClient) {
    return <div suppressHydrationWarning>Loading counter...</div>;
  }
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}

// Server state vs client state coordination
function useServerState(initialState) {
  const [state, setState] = useState(initialState);
  const [isHydrated, setIsHydrated] = useState(false);
  
  useEffect(() => {
    setIsHydrated(true);
  }, []);
  
  return [isHydrated ? state : initialState, setState];
}
```

**Common pitfalls:**
- **Not handling streaming errors** - Can break the entire response
- **SEO content in suspended boundaries** - Crawlers may not see it
- **Complex data dependencies** - Can cause waterfalls even with streaming
- **Hydration mismatches** - Server and client state differences
- **Timeout management** - Slow components can hang indefinitely
</details>

---

## 🔹 **Build & Deployment**

---

### 🎯 **Webpack basics**

#### **📋 Beginner:**
- **Q1:** What is Webpack and why is it used in React projects?
<details>
<summary>Answer</summary>
**Webpack** is a module bundler that packages JavaScript files and assets for browsers.


```javascript
// webpack.config.js
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        use: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
};
```

**Why use with React:**
- **JSX compilation** - Transform JSX to JavaScript
- **Module bundling** - Combine multiple files
- **Asset handling** - Process CSS, images, fonts
- **Development server** - Hot reloading
- **Production optimization** - Minification, code splitting
</details>

- **Q2:** What are the main concepts in Webpack (entry, output, loaders, plugins)?
<details>
<summary>Answer</summary>

```javascript
module.exports = {
  // Entry: Starting point
  entry: './src/index.js',
  
  // Output: Where to put bundled files
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js'
  },
  
  // Loaders: Transform files
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        use: 'babel-loader' // Transform JSX/ES6
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'] // Process CSS
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: 'file-loader' // Handle images
      }
    ]
  },
  
  // Plugins: Additional functionality
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html'
    }),
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css'
    })
  ]
};
```

**Core concepts:**
- **Entry** - Input files to start bundling
- **Output** - Where and how to output bundles
- **Loaders** - Transform different file types
- **Plugins** - Extend webpack functionality
</details>

- **Q3:** How does Webpack handle different file types?
<details>
<summary>Answer</summary>

```javascript
module.exports = {
  module: {
    rules: [
      // JavaScript/JSX
      {
        test: /\.jsx?$/,
        use: 'babel-loader',
        exclude: /node_modules/
      },
      
      // CSS
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      
      // SCSS
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      },
      
      // Images
      {
        test: /\.(png|jpg|jpeg|gif|svg)$/,
        type: 'asset/resource'
      },
      
      // Fonts
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/,
        type: 'asset/resource'
      },
      
      // JSON
      {
        test: /\.json$/,
        type: 'json'
      }
    ]
  }
};

// Usage in React
import React from 'react';
import './App.css';           // CSS loader
import logo from './logo.png'; // File loader
import data from './data.json'; // JSON loader

function App() {
  return (
    <div>
      <img src={logo} alt="Logo" />
      <p>{data.message}</p>
    </div>
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Webpack builds for production?
<details>
<summary>Answer</summary>

```javascript
const path = require('path');
const TerserPlugin = require('terser-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');

module.exports = {
  mode: 'production',
  
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true, // Remove console.log
          }
        }
      })
    ],
    
    // Code splitting
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  },
  
  plugins: [
    // Extract CSS
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css'
    }),
    
    // Analyze bundle size
    new BundleAnalyzerPlugin({
      analyzerMode: 'static',
      openAnalyzer: false
    })
  ],
  
  // Performance budgets
  performance: {
    maxAssetSize: 250000,
    maxEntrypointSize: 250000
  }
};
```

**Key optimizations:**
- **Minification** - Compress JavaScript and CSS
- **Tree shaking** - Remove unused code
- **Code splitting** - Split bundles
- **Asset optimization** - Compress images
- **Caching** - Use content hashes
</details>

- **Q2:** How do you implement code splitting with Webpack?
<details>
<summary>Answer</summary>

```javascript
// 1. Dynamic imports in React
import { lazy, Suspense } from 'react';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}

// 2. Route-based splitting
import { lazy } from 'react';
import { Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Contact = lazy(() => import('./pages/Contact'));

function App() {
  return (
    <Suspense fallback={<div>Loading page...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </Suspense>
  );
}

// 3. Webpack configuration
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        // Vendor code
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        },
        
        // Common code
        common: {
          name: 'common',
          minChunks: 2,
          chunks: 'all',
          enforce: true
        }
      }
    }
  }
};

// 4. Manual chunk splitting
import(/* webpackChunkName: "lodash" */ 'lodash').then((_) => {
  // Use lodash
});
```
</details>

---

### 🎯 **Babel & JSX compilation**

#### **📋 Beginner:**
- **Q1:** What is Babel and why is it needed for React?
<details>
<summary>Answer</summary>
**Babel** is a JavaScript compiler that transforms modern JS/JSX into browser-compatible code.


```javascript
// Input (JSX + ES6)
const App = () => {
  const [count, setCount] = useState(0);
  return <div onClick={() => setCount(count + 1)}>{count}</div>;
};

// Output (after Babel)
const App = () => {
  const [count, setCount] = useState(0);
  return React.createElement(
    "div",
    { onClick: () => setCount(count + 1) },
    count
  );
};
```

**Why needed:**
- **JSX transformation** - Browsers don't understand JSX
- **ES6+ features** - Arrow functions, classes, destructuring
- **Browser compatibility** - Support older browsers
- **Modern syntax** - async/await, optional chaining
</details>

- **Q2:** How does JSX get transformed into JavaScript?
<details>
<summary>Answer</summary>

```javascript
// JSX input
const element = (
  <div className="container">
    <h1>Hello, {name}!</h1>
    <Button onClick={handleClick}>Click me</Button>
  </div>
);

// Classic transform (React 16 and earlier)
const element = React.createElement(
  "div",
  { className: "container" },
  React.createElement("h1", null, "Hello, ", name, "!"),
  React.createElement(Button, { onClick: handleClick }, "Click me")
);

// New JSX transform (React 17+)
import { jsx as _jsx, jsxs as _jsxs } from "react/jsx-runtime";

const element = _jsxs("div", {
  className: "container",
  children: [
    _jsxs("h1", { children: ["Hello, ", name, "!"] }),
    _jsx(Button, { onClick: handleClick, children: "Click me" })
  ]
});
```

**Transformation steps:**
1. Parse JSX syntax
2. Transform to function calls
3. Generate JavaScript code
4. Optimize output
</details>

- **Q3:** What are Babel presets and plugins?
<details>
<summary>Answer</summary>

```javascript
// .babelrc or babel.config.js
module.exports = {
  // Presets: Collection of plugins
  presets: [
    '@babel/preset-env',    // ES6+ features
    '@babel/preset-react'   // JSX transformation
  ],
  
  // Individual plugins
  plugins: [
    '@babel/plugin-proposal-class-properties',
    '@babel/plugin-transform-runtime'
  ]
};

// Common React preset configuration
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          browsers: ['> 1%', 'last 2 versions']
        },
        useBuiltIns: 'usage',
        corejs: 3
      }
    ],
    [
      '@babel/preset-react',
      {
        runtime: 'automatic' // New JSX transform
      }
    ]
  ]
};
```

**Key concepts:**
- **Presets** - Bundles of plugins for common use cases
- **Plugins** - Individual transformations
- **@babel/preset-env** - Smart ES6+ compilation
- **@babel/preset-react** - JSX and React features
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you configure Babel for optimal React builds?
<details>
<summary>Answer</summary>

```javascript
// babel.config.js
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          browsers: ['> 0.5%', 'last 2 versions', 'not dead']
        },
        useBuiltIns: 'usage',
        corejs: 3,
        modules: false // Let webpack handle modules
      }
    ],
    [
      '@babel/preset-react',
      {
        runtime: 'automatic', // New JSX transform
        development: process.env.NODE_ENV === 'development'
      }
    ]
  ],
  
  plugins: [
    // Development only
    ...(process.env.NODE_ENV === 'development' ? [
      'react-refresh/babel'
    ] : []),
    
    // Production only
    ...(process.env.NODE_ENV === 'production' ? [
      'babel-plugin-transform-react-remove-prop-types'
    ] : [])
  ],
  
  env: {
    test: {
      presets: [
        ['@babel/preset-env', { targets: { node: 'current' } }]
      ]
    }
  }
};

// Package.json
{
  "browserslist": [
    "> 0.5%",
    "last 2 versions",
    "not dead",
    "not ie 11"
  ]
}
```
</details>

- **Q2:** What are the differences between different JSX transforms?
<details>
<summary>Answer</summary>

```javascript
// Classic JSX Transform (React 16 and earlier)
// Requires React import
import React from 'react';

function App() {
  return <div>Hello World</div>;
}

// Compiled to:
function App() {
  return React.createElement("div", null, "Hello World");
}

// New JSX Transform (React 17+)
// No React import needed
function App() {
  return <div>Hello World</div>;
}

// Compiled to:
import { jsx as _jsx } from "react/jsx-runtime";

function App() {
  return _jsx("div", { children: "Hello World" });
}

// Configuration comparison
// Classic transform
{
  "presets": [
    ["@babel/preset-react", { "runtime": "classic" }]
  ]
}

// New transform
{
  "presets": [
    ["@babel/preset-react", { "runtime": "automatic" }]
  ]
}
```

**Benefits of new transform:**
- **No React import** - Smaller bundle size
- **Better tree shaking** - Dead code elimination
- **Future-proof** - Ready for React Server Components
- **Smaller output** - More efficient runtime functions
</details>

---

### 🎯 **Production build optimizations**

#### **📋 Beginner:**
- **Q1:** What optimizations should you apply for React production builds?
<details>
<summary>Answer</summary>

```javascript
// Essential production optimizations
const optimizations = {
  // 1. Environment variable
  NODE_ENV: 'production',
  
  // 2. Code minification
  minify: true,
  
  // 3. Remove development tools
  removeConsole: true,
  removePropTypes: true,
  
  // 4. Enable production mode
  productionMode: true,
  
  // 5. Bundle splitting
  codeSplitting: true,
  
  // 6. Asset optimization
  compressImages: true,
  optimizeCSS: true
};

// Create React App production build
npm run build

// Manual webpack configuration
module.exports = {
  mode: 'production',
  
  optimization: {
    minimize: true,
    sideEffects: false, // Enable tree shaking
    splitChunks: {
      chunks: 'all'
    }
  },
  
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
};
```

**Key optimizations:**
- **Set NODE_ENV=production** - Removes dev code
- **Minify JavaScript/CSS** - Reduce file sizes
- **Code splitting** - Split large bundles
- **Tree shaking** - Remove unused code
- **Image optimization** - Compress assets
</details>

- **Q2:** How do you minify and compress React applications?
<details>
<summary>Answer</summary>

```javascript
// Webpack configuration for minification
const TerserPlugin = require('terser-webpack-plugin');
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const CompressionPlugin = require('compression-webpack-plugin');

module.exports = {
  optimization: {
    minimizer: [
      // JavaScript minification
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true,
            drop_debugger: true
          },
          mangle: true
        }
      }),
      
      // CSS minification
      new CssMinimizerPlugin()
    ]
  },
  
  plugins: [
    // Gzip compression
    new CompressionPlugin({
      algorithm: 'gzip',
      test: /\.(js|css|html|svg)$/,
      threshold: 8192,
      minRatio: 0.8
    }),
    
    // Brotli compression
    new CompressionPlugin({
      algorithm: 'brotliCompress',
      test: /\.(js|css|html|svg)$/,
      compressionOptions: { level: 11 },
      threshold: 10240,
      minRatio: 0.8,
      deleteOriginalAssets: false
    })
  ]
};

// Server configuration for compression
// Express.js example
app.use(compression({
  level: 6,
  threshold: 1000,
  filter: (req, res) => {
    return compression.filter(req, res);
  }
}));
```
</details>

- **Q3:** What is tree shaking and how does it help?
<details>
<summary>Answer</summary>
**Tree shaking** removes unused code from bundles.


```javascript
// utils.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b; // Not used

// main.js
import { add, subtract } from './utils';

console.log(add(1, 2));

// After tree shaking, multiply() is removed from bundle

// Enable tree shaking in webpack
module.exports = {
  mode: 'production',
  optimization: {
    usedExports: true,
    sideEffects: false // Mark as side-effect free
  }
};

// Package.json
{
  "sideEffects": false // Entire package is side-effect free
}

// Or specify which files have side effects
{
  "sideEffects": [
    "*.css",
    "./src/polyfills.js"
  ]
}

// Import only what you need
// ❌ Imports entire library
import _ from 'lodash';

// ✅ Import specific functions
import { debounce } from 'lodash';
// or
import debounce from 'lodash/debounce';
```

**Benefits:**
- **Smaller bundles** - Only ship used code
- **Faster loading** - Less JavaScript to parse
- **Better performance** - Reduced memory usage
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you analyze and optimize bundle sizes?
<details>
<summary>Answer</summary>

```javascript
// 1. Bundle analyzer
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin({
      analyzerMode: 'static',
      openAnalyzer: false,
      reportFilename: 'bundle-report.html'
    })
  ]
};

// 2. Source map explorer (for CRA)
npm install -g source-map-explorer
npm run build
npx source-map-explorer 'build/static/js/*.js'

// 3. Performance budgets
module.exports = {
  performance: {
    maxAssetSize: 250000,
    maxEntrypointSize: 250000,
    hints: 'warning'
  }
};

// 4. Dynamic imports for code splitting
const LazyComponent = lazy(() => 
  import(/* webpackChunkName: "lazy-component" */ './LazyComponent')
);

// 5. Vendor splitting
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        },
        common: {
          name: 'common',
          minChunks: 2,
          chunks: 'all'
        }
      }
    }
  }
};

// 6. Monitor with tools
// webpack-bundle-analyzer
// bundlephobia.com
// Package Phobia
```
</details>

- **Q2:** What are the best practices for caching strategies in React apps?
<details>
<summary>Answer</summary>

```javascript
// 1. Content-based hashing
module.exports = {
  output: {
    filename: '[name].[contenthash].js',
    chunkFilename: '[name].[contenthash].chunk.js'
  },
  
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css'
    })
  ]
};

// 2. Long-term caching headers
// nginx configuration
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
  expires 1y;
  add_header Cache-Control "public, immutable";
}

location /index.html {
  expires -1;
  add_header Cache-Control "no-cache, no-store, must-revalidate";
}

// 3. Service Worker caching
// workbox-webpack-plugin
const { GenerateSW } = require('workbox-webpack-plugin');

module.exports = {
  plugins: [
    new GenerateSW({
      runtimeCaching: [{
        urlPattern: /^https:\/\/api\./,
        handler: 'StaleWhileRevalidate',
        options: {
          cacheName: 'api-cache'
        }
      }]
    })
  ]
};

// 4. CDN optimization
const cdn = {
  js: 'https://cdn.example.com/js/',
  css: 'https://cdn.example.com/css/',
  images: 'https://cdn.example.com/images/'
};

module.exports = {
  output: {
    publicPath: process.env.NODE_ENV === 'production' ? cdn.js : '/'
  }
};

// 5. Preloading critical resources
function App() {
  useEffect(() => {
    // Preload critical routes
    import('./pages/ImportantPage');
  }, []);
  
  return <Router>...</Router>;
}
```

**Caching strategies:**
- **Immutable assets** - Long cache with content hash
- **HTML files** - No cache or short cache
- **API responses** - Stale-while-revalidate
- **Static assets** - Long-term caching
- **Critical resources** - Preload or prefetch
</details>

---

## 🔹 **Miscellaneous**

---

### 🎯 **HOCs (Higher Order Components)**

#### **📋 Beginner:**
- **Q1:** What is a Higher Order Component (HOC)?
<details>
<summary>Answer</summary>
**HOC** is a function that takes a component and returns a new enhanced component.


```jsx
// Basic HOC pattern
function withLogger(WrappedComponent) {
  return function EnhancedComponent(props) {
    console.log('Rendering with props:', props);
    
    return <WrappedComponent {...props} />;
  };
}

// Usage
const Button = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);

const LoggedButton = withLogger(Button);

// In use
<LoggedButton label="Click me" onClick={handleClick} />
```

**Key concepts:**
- **Higher-order function** - Function that returns function
- **Component enhancement** - Add functionality without modifying original
- **Props forwarding** - Pass through all props
- **Composition over inheritance** - React pattern
</details>

- **Q2:** How do you create a simple HOC?
<details>
<summary>Answer</summary>

```jsx
// 1. Simple authentication HOC
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { user } = useContext(AuthContext);
    
    if (!user) {
      return <div>Please log in</div>;
    }
    
    return <WrappedComponent {...props} user={user} />;
  };
}

// 2. Loading HOC
function withLoading(WrappedComponent) {
  return function LoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    }
    
    return <WrappedComponent {...props} />;
  };
}

// 3. Error boundary HOC
function withErrorBoundary(WrappedComponent) {
  return class extends Component {
    constructor(props) {
      super(props);
      this.state = { hasError: false };
    }
    
    static getDerivedStateFromError(error) {
      return { hasError: true };
    }
    
    render() {
      if (this.state.hasError) {
        return <div>Something went wrong</div>;
      }
      
      return <WrappedComponent {...this.props} />;
    }
  };
}

// Usage
const ProtectedProfile = withAuth(Profile);
const LoadingProfile = withLoading(Profile);
const SafeProfile = withErrorBoundary(Profile);

// Composition
const EnhancedProfile = withAuth(withLoading(withErrorBoundary(Profile)));
```
</details>

- **Q3:** What problems do HOCs solve?
<details>
<summary>Answer</summary>
**HOCs solve cross-cutting concerns:**


```jsx
// 1. Authentication
function withAuth(WrappedComponent) {
  return function(props) {
    const { user } = useAuth();
    return user ? <WrappedComponent {...props} /> : <Login />;
  };
}

// 2. Data fetching
function withData(url) {
  return function(WrappedComponent) {
    return function(props) {
      const [data, setData] = useState(null);
      const [loading, setLoading] = useState(true);
      
      useEffect(() => {
        fetch(url).then(res => res.json()).then(data => {
          setData(data);
          setLoading(false);
        });
      }, []);
      
      return <WrappedComponent {...props} data={data} loading={loading} />;
    };
  };
}

// 3. Theme injection
function withTheme(WrappedComponent) {
  return function(props) {
    const theme = useContext(ThemeContext);
    return <WrappedComponent {...props} theme={theme} />;
  };
}

// 4. Analytics tracking
function withAnalytics(eventName) {
  return function(WrappedComponent) {
    return function(props) {
      useEffect(() => {
        analytics.track(eventName);
      }, []);
      
      return <WrappedComponent {...props} />;
    };
  };
}

// Multiple enhancements
const EnhancedComponent = withAuth(
  withTheme(
    withData('/api/user')(
      withAnalytics('page_view')(UserProfile)
    )
  )
);
```

**Benefits:**
- **Code reuse** - Share logic across components
- **Separation of concerns** - Keep components focused
- **Composition** - Combine multiple behaviors
- **Clean components** - Remove boilerplate
</details>

#### **🚀 Intermediate:**
- **Q1:** What are the drawbacks of HOCs?
<details>
<summary>Answer</summary>
**HOC problems:**


```jsx
// 1. Wrapper hell
const EnhancedComponent = withAuth(
  withTheme(
    withLoading(
      withData(
        withAnalytics(MyComponent)
      )
    )
  )
);

// 2. Props collision
function withUser(WrappedComponent) {
  return function(props) {
    return <WrappedComponent {...props} name="HOC User" />;
  };
}

function withAdmin(WrappedComponent) {
  return function(props) {
    return <WrappedComponent {...props} name="Admin User" />; // Collision!
  };
}

// 3. Ref forwarding issues
const withLogging = (WrappedComponent) => {
  return function LoggingComponent(props) {
    // Ref doesn't get forwarded automatically
    return <WrappedComponent {...props} />;
  };
};

// Fix with forwardRef
const withLogging = (WrappedComponent) => {
  return React.forwardRef((props, ref) => {
    console.log('Rendering...');
    return <WrappedComponent {...props} ref={ref} />;
  });
};

// 4. Static methods not copied
class MyComponent extends Component {
  static displayName = 'MyComponent';
  static defaultProps = { theme: 'light' };
}

const Enhanced = withTheme(MyComponent);
// Enhanced.displayName is undefined
// Enhanced.defaultProps is undefined

// Fix with hoist-non-react-statics
import hoistNonReactStatics from 'hoist-non-react-statics';

function withTheme(WrappedComponent) {
  const ThemedComponent = function(props) {
    return <WrappedComponent {...props} theme="dark" />;
  };
  
  return hoistNonReactStatics(ThemedComponent, WrappedComponent);
}
```

**Modern alternatives:**
- **Custom hooks** - Better composition
- **Render props** - More explicit
- **Context** - Global state
- **Component composition** - Children patterns
</details>

- **Q2:** How do HOCs compare to hooks and render props?
<details>
<summary>Answer</summary>

```jsx
// 1. Same functionality, different patterns

// HOC approach
function withCounter(WrappedComponent) {
  return function(props) {
    const [count, setCount] = useState(0);
    return <WrappedComponent {...props} count={count} setCount={setCount} />;
  };
}

const CounterButton = withCounter(({ count, setCount }) => (
  <button onClick={() => setCount(count + 1)}>Count: {count}</button>
));

// Custom hook approach
function useCounter() {
  const [count, setCount] = useState(0);
  return { count, setCount };
}

function CounterButton() {
  const { count, setCount } = useCounter();
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}

// Render props approach
function Counter({ render }) {
  const [count, setCount] = useState(0);
  return render({ count, setCount });
}

function App() {
  return (
    <Counter 
      render={({ count, setCount }) => (
        <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      )}
    />
  );
}

// 2. Comparison
const comparison = {
  HOCs: {
    pros: ['Reusable', 'Clean component interface'],
    cons: ['Wrapper hell', 'Props collision', 'Static hoisting']
  },
  Hooks: {
    pros: ['Simple composition', 'No wrapper components', 'Better TypeScript'],
    cons: ['Only in functional components', 'Rules of hooks']
  },
  RenderProps: {
    pros: ['Explicit dependencies', 'No wrapper hell'],
    cons: ['Callback hell', 'Less reusable']
  }
};
```

**Modern recommendation:** Use **custom hooks** for most use cases
</details>

- **Q2:** How do HOCs compare to hooks and render props?
<details>
<summary>Answer</summary>

```jsx
// 1. Same functionality, different patterns

// HOC approach
function withCounter(WrappedComponent) {
  return function(props) {
    const [count, setCount] = useState(0);
    return <WrappedComponent {...props} count={count} setCount={setCount} />;
  };
}

const CounterButton = withCounter(({ count, setCount }) => (
  <button onClick={() => setCount(count + 1)}>Count: {count}</button>
));

// Custom hook approach
function useCounter() {
  const [count, setCount] = useState(0);
  return { count, setCount };
}

function CounterButton() {
  const { count, setCount } = useCounter();
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}

// Render props approach
function Counter({ render }) {
  const [count, setCount] = useState(0);
  return render({ count, setCount });
}

function App() {
  return (
    <Counter 
      render={({ count, setCount }) => (
        <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      )}
    />
  );
}

// 2. Comparison
const comparison = {
  HOCs: {
    pros: ['Reusable', 'Clean component interface'],
    cons: ['Wrapper hell', 'Props collision', 'Static hoisting']
  },
  Hooks: {
    pros: ['Simple composition', 'No wrapper components', 'Better TypeScript'],
    cons: ['Only in functional components', 'Rules of hooks']
  },
  RenderProps: {
    pros: ['Explicit dependencies', 'No wrapper hell'],
    cons: ['Callback hell', 'Less reusable']
  }
};
```

**Modern recommendation:** Use **custom hooks** for most use cases
</details>

---

### 🎯 **Render props pattern**

#### **📋 Beginner:**
- **Q1:** What is the render props pattern?
<details>
<summary>Answer</summary>
**Render props** is a pattern where a component receives a function as a prop that returns JSX.


```jsx
// Basic render props pattern
function DataProvider({ render }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetchData().then(result => {
      setData(result);
      setLoading(false);
    });
  }, []);
  
  return render({ data, loading });
}

// Usage
function App() {
  return (
    <DataProvider
      render={({ data, loading }) => (
        <div>
          {loading ? <div>Loading...</div> : <div>{data.message}</div>}
        </div>
      )}
    />
  );
}

// Alternative: children as function
function DataProvider({ children }) {
  const [data, setData] = useState(null);
  return children({ data });
}

// Usage
<DataProvider>
  {({ data }) => <div>{data?.message}</div>}
</DataProvider>
```

**Key concept:** Component controls **what** to render, consumer controls **how** to render
</details>

- **Q2:** How do you implement a component using render props?
<details>
<summary>Answer</summary>

```jsx
// Mouse position tracker
function Mouse({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    const handleMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    
    window.addEventListener('mousemove', handleMove);
    return () => window.removeEventListener('mousemove', handleMove);
  }, []);
  
  return render(position);
}

// Usage
function App() {
  return (
    <Mouse
      render={({ x, y }) => (
        <div>Mouse position: {x}, {y}</div>
      )}
    />
  );
}

// Toggle component
function Toggle({ children }) {
  const [isOn, setIsOn] = useState(false);
  
  return children({
    isOn,
    toggle: () => setIsOn(!isOn),
    turnOn: () => setIsOn(true),
    turnOff: () => setIsOn(false)
  });
}

// Usage
<Toggle>
  {({ isOn, toggle }) => (
    <div>
      <button onClick={toggle}>
        {isOn ? 'ON' : 'OFF'}
      </button>
      {isOn && <div>Content is visible!</div>}
    </div>
  )}
</Toggle>

// API data fetcher
function Fetcher({ url, children }) {
  const [state, setState] = useState({
    data: null,
    loading: true,
    error: null
  });
  
  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => setState({ data, loading: false, error: null }))
      .catch(error => setState({ data: null, loading: false, error }));
  }, [url]);
  
  return children(state);
}

// Usage
<Fetcher url="/api/users">
  {({ data, loading, error }) => {
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;
    return <UserList users={data} />;
  }}
</Fetcher>
```
</details>

- **Q3:** What are the benefits of render props?
<details>
<summary>Answer</summary>

```jsx
// 1. Flexible rendering
function DataList({ data, children }) {
  return (
    <div>
      {data.map(item => children(item))}
    </div>
  );
}

// Different rendering styles
<DataList data={users}>
  {user => <UserCard key={user.id} user={user} />}
</DataList>

<DataList data={users}>
  {user => <UserRow key={user.id} user={user} />}
</DataList>

// 2. Logic reuse without wrapper components
function WindowSize({ children }) {
  const [size, setSize] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    const updateSize = () => {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    };
    
    window.addEventListener('resize', updateSize);
    updateSize();
    
    return () => window.removeEventListener('resize', updateSize);
  }, []);
  
  return children(size);
}

// No wrapper divs!
<WindowSize>
  {({ width, height }) => (
    <div>Screen: {width}x{height}</div>
  )}
</WindowSize>

// 3. Conditional rendering
function Permission({ role, children }) {
  const { user } = useAuth();
  
  return children({
    hasPermission: user?.role === role,
    user
  });
}

<Permission role="admin">
  {({ hasPermission, user }) => (
    hasPermission ? <AdminPanel user={user} /> : <AccessDenied />
  )}
</Permission>
```

**Benefits:**
- **Flexible rendering** - Consumer controls UI
- **Logic reuse** - Share behavior across components
- **No wrapper hell** - Unlike HOCs
- **Explicit dependencies** - Clear what's being passed
</details>

#### **🚀 Intermediate:**
- **Q1:** How do render props compare to HOCs and hooks?
<details>
<summary>Answer</summary>

```jsx
// Same counter logic, different patterns

// 1. Render Props
function Counter({ children }) {
  const [count, setCount] = useState(0);
  return children({ count, increment: () => setCount(c => c + 1) });
}

<Counter>
  {({ count, increment }) => (
    <button onClick={increment}>Count: {count}</button>
  )}
</Counter>

// 2. HOC
const withCounter = (Component) => (props) => {
  const [count, setCount] = useState(0);
  return <Component {...props} count={count} increment={() => setCount(c => c + 1)} />;
};

const CounterButton = withCounter(({ count, increment }) => (
  <button onClick={increment}>Count: {count}</button>
));

// 3. Custom Hook
function useCounter() {
  const [count, setCount] = useState(0);
  return { count, increment: () => setCount(c => c + 1) };
}

function CounterButton() {
  const { count, increment } = useCounter();
  return <button onClick={increment}>Count: {count}</button>;
}

// Comparison table
const patterns = {
  renderProps: {
    pros: ['Explicit deps', 'Flexible UI', 'No wrapper components'],
    cons: ['Callback hell', 'Indent nesting', 'Performance overhead']
  },
  hocs: {
    pros: ['Clean usage', 'Reusable', 'Component-like'],
    cons: ['Wrapper hell', 'Props collision', 'Ref issues']
  },
  hooks: {
    pros: ['Simple', 'Composable', 'No nesting', 'Great TypeScript'],
    cons: ['Function components only', 'Rules of hooks']
  }
};
```

**When to use:**
- **Hooks** - Modern default choice
- **Render props** - When you need flexible rendering
- **HOCs** - Legacy codebases or specific wrapper needs
</details>

- **Q2:** How do you avoid callback hell with render props?
<details>
<summary>Answer</summary>

```jsx
// ❌ Callback hell problem
<UserProvider>
  {user => (
    <ThemeProvider>
      {theme => (
        <DataProvider>
          {data => (
            <LoadingProvider>
              {loading => (
                <div style={{ color: theme.color }}>
                  {loading ? 'Loading...' : `Hello ${user.name}: ${data.message}`}
                </div>
              )}
            </LoadingProvider>
          )}
        </DataProvider>
      )}
    </ThemeProvider>
  )}
</UserProvider>

// ✅ Solution 1: Compose render props
function compose(...providers) {
  return ({ children }) => {
    return providers.reduceRight((acc, Provider) => (
      <Provider>{acc}</Provider>
    ), children);
  };
}

const AppProviders = compose(UserProvider, ThemeProvider, DataProvider);

<AppProviders>
  {({ user, theme, data, loading }) => (
    <div style={{ color: theme.color }}>
      {loading ? 'Loading...' : `Hello ${user.name}: ${data.message}`}
    </div>
  )}
</AppProviders>

// ✅ Solution 2: Custom hook combination
function useAppData() {
  const user = useUser();
  const theme = useTheme();
  const { data, loading } = useData();
  
  return { user, theme, data, loading };
}

function App() {
  const { user, theme, data, loading } = useAppData();
  
  return (
    <div style={{ color: theme.color }}>
      {loading ? 'Loading...' : `Hello ${user.name}: ${data.message}`}
    </div>
  );
}

// ✅ Solution 3: Render prop composer
function RenderPropComposer({ providers, children }) {
  const renderNext = (index, accumulated = {}) => {
    if (index >= providers.length) {
      return children(accumulated);
    }
    
    const Provider = providers[index];
    return (
      <Provider>
        {(props) => renderNext(index + 1, { ...accumulated, ...props })}
      </Provider>
    );
  };
  
  return renderNext(0);
}

// Usage
<RenderPropComposer
  providers={[UserProvider, ThemeProvider, DataProvider]}
>
  {({ user, theme, data, loading }) => (
    <div style={{ color: theme.color }}>
      {loading ? 'Loading...' : `Hello ${user.name}: ${data.message}`}
    </div>
  )}
</RenderPropComposer>
```
</details>

---

### 🎯 **Portals**

#### **📋 Beginner:**
- **Q1:** What are React Portals and when would you use them?
<details>
<summary>Answer</summary>
**Portals** render children into a DOM node outside the parent component's hierarchy.


```jsx
import { createPortal } from 'react-dom';

function Modal({ children, isOpen }) {
  if (!isOpen) return null;
  
  return createPortal(
    <div className="modal-overlay">
      <div className="modal">
        {children}
      </div>
    </div>,
    document.getElementById('modal-root') // Render outside React tree
  );
}

// HTML
<div id="root">
  <div id="modal-root"></div>
</div>

// Usage
function App() {
  const [showModal, setShowModal] = useState(false);
  
  return (
    <div style={{ overflow: 'hidden' }}>
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      
      <Modal isOpen={showModal}>
        <h2>Modal Content</h2>
        <button onClick={() => setShowModal(false)}>Close</button>
      </Modal>
    </div>
  );
}
```

**Use cases:**
- **Modals** - Above page content
- **Tooltips** - Escape overflow constraints
- **Dropdowns** - Avoid z-index issues
- **Notifications** - Fixed positioning
</details>

- **Q2:** How do you create a portal?
<details>
<summary>Answer</summary>

```jsx
import { createPortal } from 'react-dom';

// Basic portal
function Portal({ children, container }) {
  return createPortal(children, container);
}

// Custom hook for portal
function usePortal(id) {
  const [container, setContainer] = useState(null);
  
  useEffect(() => {
    let element = document.getElementById(id);
    
    if (!element) {
      element = document.createElement('div');
      element.id = id;
      document.body.appendChild(element);
    }
    
    setContainer(element);
    
    return () => {
      if (element.parentNode) {
        element.parentNode.removeChild(element);
      }
    };
  }, [id]);
  
  return container;
}

// Usage
function Tooltip({ children, content, isVisible }) {
  const portalContainer = usePortal('tooltip-root');
  
  if (!isVisible || !portalContainer) return children;
  
  return (
    <>
      {children}
      {createPortal(
        <div className="tooltip">{content}</div>,
        portalContainer
      )}
    </>
  );
}

// Notification system
function NotificationProvider({ children }) {
  const [notifications, setNotifications] = useState([]);
  const portalContainer = usePortal('notifications');
  
  const addNotification = (message) => {
    const id = Date.now();
    setNotifications(prev => [...prev, { id, message }]);
    
    setTimeout(() => {
      setNotifications(prev => prev.filter(n => n.id !== id));
    }, 3000);
  };
  
  return (
    <NotificationContext.Provider value={{ addNotification }}>
      {children}
      {portalContainer && createPortal(
        <div className="notifications">
          {notifications.map(n => (
            <div key={n.id} className="notification">
              {n.message}
            </div>
          ))}
        </div>,
        portalContainer
      )}
    </NotificationContext.Provider>
  );
}
```
</details>

- **Q3:** Do portals affect event propagation?
<details>
<summary>Answer</summary>
**Events bubble through React tree, not DOM tree.**


```jsx
function App() {
  const handleClick = (e) => {
    console.log('App clicked'); // This will fire!
  };
  
  return (
    <div onClick={handleClick}>
      <PortalModal />
    </div>
  );
}

function PortalModal() {
  return createPortal(
    <div onClick={() => console.log('Modal clicked')}>
      <button>Click me</button>
      {/* Clicking button triggers both modal and app handlers */}
    </div>,
    document.body // Rendered in body, not in app div
  );
}

// Prevent event bubbling
function PortalModal() {
  const handleModalClick = (e) => {
    e.stopPropagation(); // Stops React event bubbling
    console.log('Modal clicked');
  };
  
  return createPortal(
    <div onClick={handleModalClick}>
      <button>Click me</button>
    </div>,
    document.body
  );
}

// Context still works through portals
function App() {
  return (
    <ThemeProvider>
      <PortalComponent /> {/* Can access ThemeContext */}
    </ThemeProvider>
  );
}

function PortalComponent() {
  const theme = useContext(ThemeContext); // Works!
  
  return createPortal(
    <div style={{ color: theme.color }}>Portal content</div>,
    document.body
  );
}
```

**Key points:**
- Events bubble through **React component tree**
- Context is preserved through portals
- DOM structure doesn't affect React event system
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you handle portal cleanup and memory leaks?
<details>
<summary>Answer</summary>

```jsx
// Custom portal hook with cleanup
function usePortal(elementId) {
  const [portalElement, setPortalElement] = useState(null);
  
  useEffect(() => {
    // Create or find element
    let element = document.getElementById(elementId);
    let shouldRemove = false;
    
    if (!element) {
      element = document.createElement('div');
      element.id = elementId;
      document.body.appendChild(element);
      shouldRemove = true;
    }
    
    setPortalElement(element);
    
    // Cleanup function
    return () => {
      if (shouldRemove && element.parentNode) {
        element.parentNode.removeChild(element);
      }
    };
  }, [elementId]);
  
  return portalElement;
}

// Portal manager for multiple portals
class PortalManager {
  constructor() {
    this.portals = new Map();
  }
  
  getPortal(id) {
    if (!this.portals.has(id)) {
      const element = document.createElement('div');
      element.id = id;
      document.body.appendChild(element);
      this.portals.set(id, element);
    }
    return this.portals.get(id);
  }
  
  removePortal(id) {
    const element = this.portals.get(id);
    if (element && element.parentNode) {
      element.parentNode.removeChild(element);
      this.portals.delete(id);
    }
  }
  
  cleanup() {
    this.portals.forEach((element, id) => {
      this.removePortal(id);
    });
  }
}

const portalManager = new PortalManager();

// Portal with ref forwarding
const Portal = forwardRef(({ children, containerId }, ref) => {
  const [container, setContainer] = useState(null);
  
  useEffect(() => {
    const element = portalManager.getPortal(containerId);
    setContainer(element);
    
    return () => {
      // Only remove if no other components using it
      if (element && element.children.length === 0) {
        portalManager.removePortal(containerId);
      }
    };
  }, [containerId]);
  
  if (!container) return null;
  
  return createPortal(
    <div ref={ref}>{children}</div>,
    container
  );
});

// Global cleanup on app unmount
function App() {
  useEffect(() => {
    return () => {
      portalManager.cleanup();
    };
  }, []);
  
  return <div>App content</div>;
}
```
</details>

- **Q2:** How do portals work with SSR?
<details>
<summary>Answer</summary>

```jsx
// Portals don't work with SSR - they're client-only
function Modal({ children, isOpen }) {
  const [isMounted, setIsMounted] = useState(false);
  
  useEffect(() => {
    setIsMounted(true);
  }, []);
  
  // Don't render portal on server
  if (!isMounted || !isOpen) return null;
  
  return createPortal(
    <div className="modal">{children}</div>,
    document.body
  );
}

// SSR-safe portal hook
function useSSRSafePortal(elementId) {
  const [portalElement, setPortalElement] = useState(null);
  
  useEffect(() => {
    // Only run on client
    if (typeof window !== 'undefined') {
      let element = document.getElementById(elementId);
      
      if (!element) {
        element = document.createElement('div');
        element.id = elementId;
        document.body.appendChild(element);
      }
      
      setPortalElement(element);
    }
  }, [elementId]);
  
  return portalElement;
}

// Alternative: Use div with fixed positioning
function SSRModal({ children, isOpen }) {
  if (!isOpen) return null;
  
  return (
    <div
      style={{
        position: 'fixed',
        top: 0,
        left: 0,
        width: '100%',
        height: '100%',
        zIndex: 1000,
        backgroundColor: 'rgba(0,0,0,0.5)'
      }}
    >
      <div className="modal">{children}</div>
    </div>
  );
}

// Next.js dynamic import for client-only portals
import dynamic from 'next/dynamic';

const ClientOnlyPortal = dynamic(
  () => import('./Portal'),
  { ssr: false }
);
```

**SSR considerations:**
- Portals are **client-only** features
- Use `useEffect` to detect client-side
- Consider CSS alternatives for SSR
- Use dynamic imports in Next.js
</details>

---

### 🎯 **Fragments**

#### **📋 Beginner:**
- **Q1:** What are React Fragments and why are they useful?
<details>
<summary>Answer</summary>
**Fragments** let you group elements without adding extra DOM nodes.


```jsx
// ❌ Without Fragments - adds unnecessary div
function UserInfo() {
  return (
    <div> {/* Extra wrapper div */}
      <h1>John Doe</h1>
      <p>Software Engineer</p>
    </div>
  );
}

// ✅ With Fragments - no extra wrapper
function UserInfo() {
  return (
    <React.Fragment>
      <h1>John Doe</h1>
      <p>Software Engineer</p>
    </React.Fragment>
  );
}

// ✅ Short syntax
function UserInfo() {
  return (
    <>
      <h1>John Doe</h1>
      <p>Software Engineer</p>
    </>
  );
}

// Before Fragments, this would break
function Table() {
  return (
    <table>
      <tbody>
        <TableRow /> {/* Must return <tr> elements, not wrapped in div */}
      </tbody>
    </table>
  );
}

function TableRow() {
  return (
    <>
      <tr><td>Row 1</td></tr>
      <tr><td>Row 2</td></tr>
    </>
  );
}
```

**Benefits:**
- **Cleaner DOM** - No wrapper elements
- **Better performance** - Fewer DOM nodes
- **Valid HTML** - Proper table/list structure
- **CSS compatibility** - No layout interference
</details>

- **Q2:** What are the different ways to use Fragments?
<details>
<summary>Answer</summary>

```jsx
// 1. React.Fragment syntax
function Component() {
  return (
    <React.Fragment>
      <div>Item 1</div>
      <div>Item 2</div>
    </React.Fragment>
  );
}

// 2. Short syntax (most common)
function Component() {
  return (
    <>
      <div>Item 1</div>
      <div>Item 2</div>
    </>
  );
}

// 3. Fragment with key (for lists)
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <React.Fragment key={todo.id}>
          <li>{todo.title}</li>
          <li>{todo.description}</li>
        </React.Fragment>
      ))}
    </ul>
  );
}

// 4. Nested Fragments
function App() {
  return (
    <>
      <Header />
      <>
        <nav>Navigation</nav>
        <main>Content</main>
      </>
      <Footer />
    </>
  );
}

// 5. Conditional Fragments
function ConditionalComponent({ showExtra }) {
  return (
    <>
      <div>Always shown</div>
      {showExtra && (
        <>
          <div>Extra item 1</div>
          <div>Extra item 2</div>
        </>
      )}
    </>
  );
}
```
</details>

- **Q3:** When should you use Fragments vs div wrappers?
<details>
<summary>Answer</summary>

```jsx
// ✅ Use Fragments when:

// 1. HTML structure matters
function TableRows() {
  return (
    <>
      <tr><td>Row 1</td></tr>
      <tr><td>Row 2</td></tr>
    </>
  ); // div would break table structure
}

// 2. CSS layout would be affected
function FlexItems() {
  return (
    <>
      <div className="flex-item">Item 1</div>
      <div className="flex-item">Item 2</div>
    </>
  ); // div wrapper would interfere with flex layout
}

// 3. Avoiding unnecessary DOM nesting
function SimpleGroup() {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  ); // No need for extra wrapper
}

// ✅ Use div wrappers when:

// 1. You need styling or event handling
function StyledGroup() {
  return (
    <div className="card" onClick={handleClick}>
      <h1>Title</h1>
      <p>Description</p>
    </div>
  );
}

// 2. Semantic grouping
function Article() {
  return (
    <article>
      <h1>Article Title</h1>
      <p>Article content</p>
    </article>
  );
}

// 3. Layout containers
function Layout() {
  return (
    <div className="container">
      <Header />
      <Main />
    </div>
  );
}

// Comparison
const guidelines = {
  useFragments: [
    'HTML structure integrity',
    'CSS layout preservation',
    'Performance (fewer DOM nodes)',
    'Clean markup'
  ],
  useDivs: [
    'Need styling/classes',
    'Event handling on container',
    'Semantic meaning',
    'Layout positioning'
  ]
};
```
</details>

#### **🚀 Intermediate:**
- **Q1:** Can Fragments have keys? When is this useful?
<details>
<summary>Answer</summary>

```jsx
// ✅ React.Fragment can have keys (short syntax cannot)
function CommentList({ comments }) {
  return (
    <div>
      {comments.map(comment => (
        <React.Fragment key={comment.id}>
          <h3>{comment.author}</h3>
          <p>{comment.text}</p>
          <time>{comment.date}</time>
          <hr />
        </React.Fragment>
      ))}
    </div>
  );
}

// ❌ Short syntax doesn't support keys
function BadExample({ items }) {
  return (
    <div>
      {items.map(item => (
        <> {/* Error: Cannot have key */}
          <div>{item.title}</div>
          <div>{item.description}</div>
        </>
      ))}
    </div>
  );
}

// ✅ Use React.Fragment for keyed lists
function ProductList({ products }) {
  return (
    <div className="product-grid">
      {products.map(product => (
        <React.Fragment key={product.id}>
          <img src={product.image} alt={product.name} />
          <h3>{product.name}</h3>
          <p>${product.price}</p>
          <button>Add to Cart</button>
        </React.Fragment>
      ))}
    </div>
  );
}

// Complex example with nested fragments
function ChatMessage({ message }) {
  return (
    <React.Fragment key={message.id}>
      <div className="message-header">
        <span className="author">{message.author}</span>
        <span className="time">{message.timestamp}</span>
      </div>
      <div className="message-body">
        {message.text}
      </div>
      {message.attachments.map(attachment => (
        <React.Fragment key={attachment.id}>
          <div className="attachment">
            <img src={attachment.url} alt={attachment.name} />
          </div>
        </React.Fragment>
      ))}
    </React.Fragment>
  );
}
```

**When keyed Fragments are useful:**
- **List items with multiple elements** - Group related elements
- **Complex list rendering** - Maintain React's reconciliation
- **Dynamic content** - Items can be reordered
</details>

- **Q2:** How do Fragments affect CSS styling and layout?
<details>
<summary>Answer</summary>

```jsx
// CSS Grid example
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

// ❌ Wrapper div breaks grid layout
function GridItems() {
  return (
    <div className="grid-container">
      <div> {/* This wrapper breaks the grid! */}
        <div className="grid-item">Item 1</div>
        <div className="grid-item">Item 2</div>
      </div>
      <div className="grid-item">Item 3</div>
    </div>
  );
}

// ✅ Fragment preserves grid layout
function GridItems() {
  return (
    <div className="grid-container">
      <>
        <div className="grid-item">Item 1</div>
        <div className="grid-item">Item 2</div>
      </>
      <div className="grid-item">Item 3</div>
    </div>
  );
}

// Flexbox example
.flex-container {
  display: flex;
  justify-content: space-between;
}

// ❌ Wrapper interferes with flex
function FlexExample() {
  return (
    <div className="flex-container">
      <div> {/* Wrapper changes flex behavior */}
        <div>Flex item 1</div>
        <div>Flex item 2</div>
      </div>
    </div>
  );
}

// ✅ Fragment preserves flex layout
function FlexExample() {
  return (
    <div className="flex-container">
      <>
        <div>Flex item 1</div>
        <div>Flex item 2</div>
      </>
    </div>
  );
}

// Table structure preservation
function DataTable({ rows }) {
  return (
    <table>
      <tbody>
        {rows.map(row => (
          <React.Fragment key={row.id}>
            <tr>
              <td>{row.name}</td>
              <td>{row.value}</td>
            </tr>
            {row.hasDetails && (
              <tr>
                <td colSpan="2">{row.details}</td>
              </tr>
            )}
          </React.Fragment>
        ))}
      </tbody>
    </table>
  );
}

// CSS selector considerations
// Fragments don't create DOM nodes, so CSS selectors work as expected
.parent > .child {
  /* This works with Fragments */
}

.parent .nested {
  /* Fragment doesn't interrupt descendant selectors */
}
```

**CSS impact:**
- **No DOM nodes** - Fragments don't appear in rendered HTML
- **Layout preservation** - CSS Grid, Flexbox work correctly
- **Selector behavior** - No interference with CSS selectors
- **Performance** - Fewer DOM nodes to style
</details>

---

### 🎯 **Keys in lists**

#### **📋 Beginner:**
- **Q1:** Why are keys required when rendering lists in React?
<details>
<summary>Answer</summary>
**Keys** help React identify which items have changed, been added, or removed in lists.


```jsx
// ❌ Without keys - performance issues and bugs
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li>{user.name}</li> // No key - React warning
      ))}
    </ul>
  );
}

// ✅ With keys - proper reconciliation
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// State preservation example
function TodoList({ todos }) {
  return (
    <div>
      {todos.map(todo => (
        <TodoItem 
          key={todo.id} // ✅ Stable key preserves component state
          todo={todo}
        />
      ))}
    </div>
  );
}

function TodoItem({ todo }) {
  const [isEditing, setIsEditing] = useState(false);
  
  return (
    <div>
      {isEditing ? (
        <input defaultValue={todo.text} />
      ) : (
        <span>{todo.text}</span>
      )}
      <button onClick={() => setIsEditing(!isEditing)}>
        {isEditing ? 'Save' : 'Edit'}
      </button>
    </div>
  );
}
```

**Why keys matter:**
- **Performance** - Efficient reconciliation
- **State preservation** - Component state maintained
- **Correct behavior** - No unexpected side effects
- **Animation** - Smooth transitions
</details>

- **Q2:** What happens if you use array indices as keys?
<details>
<summary>Answer</summary>

```jsx
// ❌ Using index as key - problematic for dynamic lists
function BadTodoList({ todos, onDelete }) {
  return (
    <div>
      {todos.map((todo, index) => (
        <div key={index}> {/* Bad! Index changes when items removed */}
          <input type="checkbox" defaultChecked={todo.completed} />
          <span>{todo.text}</span>
          <button onClick={() => onDelete(index)}>Delete</button>
        </div>
      ))}
    </div>
  );
}

// Problem demonstration:
// Initial: [A, B, C] with indices [0, 1, 2]
// Delete B: [A, C] but indices are now [0, 1]
// React thinks item at index 1 changed from B to C!

// ✅ Using stable unique keys
function GoodTodoList({ todos, onDelete }) {
  return (
    <div>
      {todos.map(todo => (
        <div key={todo.id}> {/* Good! Stable unique key */}
          <input type="checkbox" defaultChecked={todo.completed} />
          <span>{todo.text}</span>
          <button onClick={() => onDelete(todo.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}

// Index key problems:
const indexKeyIssues = {
  reordering: 'Items appear to change when list reordered',
  stateCorruption: 'Form inputs keep wrong values',
  performance: 'Unnecessary re-renders and DOM updates',
  animations: 'Incorrect element transitions'
};

// When index keys are OK:
const okayToUseIndex = [
  'Static lists that never change',
  'Lists that only append items',
  'No user interaction with list items',
  'No sorting, filtering, or reordering'
];
```
</details>

- **Q3:** What makes a good key for list items?
<details>
<summary>Answer</summary>

```jsx
// ✅ GOOD: Stable unique identifiers
function ProductList({ products }) {
  return (
    <div>
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

// ✅ GOOD: Composite keys when needed
function OrderItems({ order }) {
  return (
    <div>
      {order.items.map(item => (
        <div key={`${order.id}-${item.productId}`}>
          {item.name} x {item.quantity}
        </div>
      ))}
    </div>
  );
}

// ✅ GOOD: Content-based keys when no ID
function MessageList({ messages }) {
  return (
    <div>
      {messages.map(message => (
        <div key={`${message.timestamp}-${message.userId}`}>
          {message.text}
        </div>
      ))}
    </div>
  );
}

// ❌ BAD: Non-unique or unstable keys
function BadExamples({ items }) {
  return (
    <div>
      {/* ❌ Random values - new key every render */}
      {items.map(item => (
        <div key={Math.random()}>{item.name}</div>
      ))}
      
      {/* ❌ Non-unique values */}
      {items.map(item => (
        <div key={item.category}>{item.name}</div>
      ))}
      
      {/* ❌ Object reference - changes every render */}
      {items.map(item => (
        <div key={item}>{item.name}</div>
      ))}
    </div>
  );
}

// Key selection criteria
const goodKeyTraits = {
  unique: 'No two items have same key',
  stable: 'Key doesn\'t change between renders',
  predictable: 'Key doesn\'t change with data updates',
  meaningful: 'Key represents item identity'
};

// Key generation strategies
function generateKeys(items) {
  // If items have IDs
  if (items[0]?.id) {
    return items.map(item => item.id);
  }
  
  // Composite key from multiple fields
  if (items[0]?.userId && items[0]?.timestamp) {
    return items.map(item => `${item.userId}-${item.timestamp}`);
  }
  
  // Hash content if no natural key
  return items.map(item => 
    btoa(JSON.stringify(item)).substring(0, 10)
  );
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do keys affect component state during list reordering?
<details>
<summary>Answer</summary>

```jsx
// Demonstration of key impact on component state

function EditableItem({ item }) {
  const [isEditing, setIsEditing] = useState(false);
  const [value, setValue] = useState(item.text);
  
  return (
    <div>
      {isEditing ? (
        <input 
          value={value}
          onChange={e => setValue(e.target.value)}
        />
      ) : (
        <span>{item.text}</span>
      )}
      <button onClick={() => setIsEditing(!isEditing)}>
        {isEditing ? 'Save' : 'Edit'}
      </button>
    </div>
  );
}

// ❌ Bad keys - state gets mixed up during reordering
function BadReorderExample({ items, onReorder }) {
  return (
    <div>
      {items.map((item, index) => (
        <EditableItem key={index} item={item} />
      ))}
      <button onClick={() => onReorder([1, 0, 2])}>
        Move first item to second position
      </button>
    </div>
  );
}

/*
Problem:
- User starts editing first item (index 0)
- Items get reordered: [A, B, C] → [B, A, C]  
- Component at index 0 now shows item B but keeps editing state from A
- User's editing session is lost or applied to wrong item!
*/

// ✅ Good keys - state follows the correct item
function GoodReorderExample({ items, onReorder }) {
  return (
    <div>
      {items.map(item => (
        <EditableItem key={item.id} item={item} />
      ))}
      <button onClick={() => onReorder([1, 0, 2])}>
        Move first item to second position
      </button>
    </div>
  );
}

// Advanced example: Drag and drop list
function DragDropList({ items, onReorder }) {
  const [draggedItem, setDraggedItem] = useState(null);
  
  return (
    <div>
      {items.map(item => (
        <DraggableItem
          key={item.id} // State preserved during drag operations
          item={item}
          onDrag={setDraggedItem}
        />
      ))}
    </div>
  );
}

function DraggableItem({ item, onDrag }) {
  const [isDragging, setIsDragging] = useState(false);
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  // Component state is preserved because of stable key
  return (
    <div
      draggable
      style={{ transform: `translate(${position.x}px, ${position.y}px)` }}
      onDragStart={() => {
        setIsDragging(true);
        onDrag(item);
      }}
      onDragEnd={() => setIsDragging(false)}
    >
      {item.text}
    </div>
  );
}

// State preservation comparison
const statePreservation = {
  withGoodKeys: [
    'Component state stays with correct item',
    'Form inputs maintain values',
    'Focus state preserved',
    'Animation state continues'
  ],
  withBadKeys: [
    'State attached to wrong items',
    'Form data gets mixed up',
    'Focus jumps to wrong elements',
    'Animations break or restart'
  ]
};
```
</details>

- **Q2:** How do you handle keys in dynamic lists?
<details>
<summary>Answer</summary>

```jsx
// Dynamic list with filtering, sorting, and CRUD operations

function DynamicUserList() {
  const [users, setUsers] = useState([
    { id: 1, name: 'Alice', role: 'admin', active: true },
    { id: 2, name: 'Bob', role: 'user', active: false },
    { id: 3, name: 'Charlie', role: 'user', active: true }
  ]);
  
  const [filter, setFilter] = useState('all');
  const [sortBy, setSortBy] = useState('name');
  
  // Filter users
  const filteredUsers = users.filter(user => {
    if (filter === 'active') return user.active;
    if (filter === 'inactive') return !user.active;
    return true;
  });
  
  // Sort users
  const sortedUsers = [...filteredUsers].sort((a, b) => {
    if (sortBy === 'name') return a.name.localeCompare(b.name);
    if (sortBy === 'role') return a.role.localeCompare(b.role);
    return 0;
  });
  
  const addUser = () => {
    const newUser = {
      id: Date.now(), // Simple ID generation
      name: `User ${users.length + 1}`,
      role: 'user',
      active: true
    };
    setUsers(prev => [...prev, newUser]);
  };
  
  const removeUser = (id) => {
    setUsers(prev => prev.filter(user => user.id !== id));
  };
  
  return (
    <div>
      <div>
        <button onClick={addUser}>Add User</button>
        <select value={filter} onChange={e => setFilter(e.target.value)}>
          <option value="all">All Users</option>
          <option value="active">Active</option>
          <option value="inactive">Inactive</option>
        </select>
        <select value={sortBy} onChange={e => setSortBy(e.target.value)}>
          <option value="name">Sort by Name</option>
          <option value="role">Sort by Role</option>
        </select>
      </div>
      
      <div>
        {sortedUsers.map(user => (
          <UserCard
            key={user.id} // Stable key through all operations
            user={user}
            onRemove={removeUser}
          />
        ))}
      </div>
    </div>
  );
}

// Nested dynamic lists
function NestedDynamicList({ categories }) {
  return (
    <div>
      {categories.map(category => (
        <div key={category.id}>
          <h3>{category.name}</h3>
          {category.items.map(item => (
            <div key={`${category.id}-${item.id}`}>
              {item.name}
            </div>
          ))}
        </div>
      ))}
    </div>
  );
}

// Optimistic updates with temporary IDs
function OptimisticList() {
  const [items, setItems] = useState([]);
  const [nextTempId, setNextTempId] = useState(-1);
  
  const addItemOptimistically = async (text) => {
    const tempId = nextTempId;
    setNextTempId(prev => prev - 1);
    
    // Add with temporary ID immediately
    const tempItem = { id: tempId, text, pending: true };
    setItems(prev => [...prev, tempItem]);
    
    try {
      // API call
      const savedItem = await saveItem(text);
      
      // Replace temp item with real item
      setItems(prev => 
        prev.map(item => 
          item.id === tempId ? savedItem : item
        )
      );
    } catch (error) {
      // Remove temp item on error
      setItems(prev => prev.filter(item => item.id !== tempId));
    }
  };
  
  return (
    <div>
      {items.map(item => (
        <div 
          key={item.id} // Works with both temp and real IDs
          style={{ opacity: item.pending ? 0.5 : 1 }}
        >
          {item.text}
        </div>
      ))}
    </div>
  );
}

// Key strategies for different scenarios
const keyStrategies = {
  simpleList: 'Use item.id',
  nestedList: 'Use composite keys: `${parentId}-${childId}`',
  tempItems: 'Use negative numbers for temp IDs',
  contentBased: 'Use hash of content when no ID available',
  timestamp: 'Use timestamp + userId for messages/logs'
};
```
</details>

---

### 🎯 **Hydration (SSR)**

#### **📋 Beginner:**
- **Q1:** What is hydration in the context of SSR?
<details>
<summary>Answer</summary>
**Hydration** is the process of attaching React event handlers and state to server-rendered HTML:
- **Server renders** static HTML for fast initial display
- **Client downloads** JavaScript bundle
- **React "hydrates"** the static HTML, making it interactive
- **Event listeners attached**, state restored, components become functional


```jsx
// Server renders this HTML
<div id="root">
  <button>Click me</button>
  <p>Count: 0</p>
</div>

// Client hydrates with React
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <p>Count: {count}</p>
    </div>
  );
}

// React 18 hydration
import { hydrateRoot } from 'react-dom/client';
hydrateRoot(document.getElementById('root'), <App />);
```
</details>

- **Q2:** What happens during the hydration process?
<details>
<summary>Answer</summary>
**Hydration process steps:**

1. **HTML already rendered** by server and displayed to user
2. **JavaScript bundle downloads** and executes
3. **React recreates** virtual DOM from components
4. **Compares** server HTML with client virtual DOM
5. **Attaches event listeners** to existing DOM nodes
6. **Initializes state** and makes components interactive
7. **Reports mismatches** if server/client don't match


```jsx
// During hydration React does:
function HydrationExample() {
  const [isClient, setIsClient] = useState(false);
  
  useEffect(() => {
    setIsClient(true); // This runs only after hydration
  }, []);
  
  return (
    <div>
      <h1>Server-rendered content</h1>
      {isClient && <p>This appears after hydration</p>}
    </div>
  );
}

// Hydration timeline:
// 1. Server HTML shows: "Server-rendered content"
// 2. Client hydrates and useEffect runs
// 3. Component re-renders with: "This appears after hydration"
```
</details>

- **Q3:** What are hydration mismatches?
<details>
<summary>Answer</summary>
**Hydration mismatches** occur when server-rendered HTML differs from client-rendered HTML:

**Common causes:**
- **Different data** between server and client
- **Date/time differences** (server vs client timezone)
- **Random values** (Math.random(), Date.now())
- **Browser-only APIs** used during SSR
- **Conditional rendering** based on client-only state


```jsx
// ❌ Causes hydration mismatch
function BadComponent() {
  const timestamp = new Date().toLocaleString(); // Different on server/client
  return <div>Current time: {timestamp}</div>;
}

// ❌ Browser API on server
function BadComponent() {
  const width = window.innerWidth; // window undefined on server
  return <div>Width: {width}</div>;
}

// ✅ Fix: Suppress hydration warning for dynamic content
function FixedComponent() {
  const [timestamp, setTimestamp] = useState('');
  
  useEffect(() => {
    setTimestamp(new Date().toLocaleString());
  }, []);
  
  return (
    <div suppressHydrationWarning={true}>
      Current time: {timestamp || 'Loading...'}
    </div>
  );
}

// ✅ Fix: Use client-only rendering
function ClientOnlyComponent() {
  const [isClient, setIsClient] = useState(false);
  
  useEffect(() => {
    setIsClient(true);
  }, []);
  
  if (!isClient) {
    return <div>Loading...</div>; // Server and initial client render
  }
  
  return <div>Width: {window.innerWidth}</div>; // Only after hydration
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you debug hydration issues?
<details>
<summary>Answer</summary>
**Debugging hydration issues:**

1. **Enable detailed warnings**:

```jsx
// React 18 - more detailed hydration errors
import { hydrateRoot } from 'react-dom/client';

const root = hydrateRoot(document.getElementById('root'), <App />);
// Console will show specific mismatch details
```

2. **Use development tools**:

```jsx
// Add debug logging
function DebugComponent() {
  console.log('Rendering on:', typeof window === 'undefined' ? 'server' : 'client');
  
  const [mounted, setMounted] = useState(false);
  
  useEffect(() => {
    console.log('Component hydrated');
    setMounted(true);
  }, []);
  
  return <div data-mounted={mounted}>Content</div>;
}
```

3. **Compare server vs client output**:

```jsx
// Custom hook to detect hydration
function useHydrated() {
  const [hydrated, setHydrated] = useState(false);
  
  useEffect(() => {
    setHydrated(true);
  }, []);
  
  return hydrated;
}

function DebuggingComponent() {
  const isHydrated = useHydrated();
  const serverData = getServerData();
  const clientData = getClientData();
  
  if (process.env.NODE_ENV === 'development') {
    console.log('Server data:', serverData);
    console.log('Client data:', clientData);
    console.log('Hydrated:', isHydrated);
  }
  
  return <div>{isHydrated ? clientData : serverData}</div>;
}
```

4. **Use suppressHydrationWarning strategically**:

```jsx
// Only suppress when you know why there's a mismatch
function TimestampComponent() {
  const [time, setTime] = useState(() => 
    typeof window === 'undefined' ? '' : new Date().toISOString()
  );
  
  useEffect(() => {
    setTime(new Date().toISOString());
  }, []);
  
  return (
    <div suppressHydrationWarning={true}>
      Last updated: {time}
    </div>
  );
}
```
</details>

- **Q2:** How do you implement progressive hydration?
<details>
<summary>Answer</summary>
**Progressive hydration** loads and hydrates components incrementally:


```jsx
// Custom hook for intersection-based hydration
function useIntersectionHydration(threshold = 0.1) {
  const [isVisible, setIsVisible] = useState(false);
  const ref = useRef();
  
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true);
          observer.disconnect();
        }
      },
      { threshold }
    );
    
    if (ref.current) {
      observer.observe(ref.current);
    }
    
    return () => observer.disconnect();
  }, [threshold]);
  
  return [ref, isVisible];
}

// Component with progressive hydration
function LazyHydratedComponent({ children, fallback }) {
  const [ref, isVisible] = useIntersectionHydration();
  const [isHydrated, setIsHydrated] = useState(false);
  
  useEffect(() => {
    if (isVisible) {
      // Trigger hydration when visible
      setIsHydrated(true);
    }
  }, [isVisible]);
  
  return (
    <div ref={ref}>
      {isHydrated ? children : fallback}
    </div>
  );
}

// Usage
function App() {
  return (
    <div>
      <Header /> {/* Hydrates immediately */}
      
      <LazyHydratedComponent fallback={<div>Loading widget...</div>}>
        <ExpensiveWidget /> {/* Hydrates when scrolled into view */}
      </LazyHydratedComponent>
      
      <Footer />
    </div>
  );
}

// Time-based progressive hydration
function TimeBasedHydration({ delay = 1000, children, fallback }) {
  const [shouldHydrate, setShouldHydrate] = useState(false);
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setShouldHydrate(true);
    }, delay);
    
    return () => clearTimeout(timer);
  }, [delay]);
  
  return shouldHydrate ? children : fallback;
}

// Priority-based hydration
const HydrationPriority = {
  IMMEDIATE: 0,
  HIGH: 1,
  NORMAL: 2,
  LOW: 3
};

function PriorityHydration({ priority, children, fallback }) {
  const [isHydrated, setIsHydrated] = useState(priority === HydrationPriority.IMMEDIATE);
  
  useEffect(() => {
    if (priority === HydrationPriority.IMMEDIATE) return;
    
    const delays = [0, 100, 500, 2000];
    const timer = setTimeout(() => {
      setIsHydrated(true);
    }, delays[priority]);
    
    return () => clearTimeout(timer);
  }, [priority]);
  
  return isHydrated ? children : fallback;
}
```

**Benefits of progressive hydration:**
- **Faster initial interactivity** - Critical components hydrate first
- **Reduced main thread blocking** - Spreads hydration work over time
- **Better user experience** - Above-the-fold content interactive sooner
- **Bandwidth optimization** - Can defer non-critical component JS
</details>

---

### 🎯 **Next.js basics (SSR, SSG, ISR)**

#### **📋 Beginner:**
- **Q1:** What is Next.js and what features does it provide?
<details>
<summary>Answer</summary>
**Next.js** is a React framework that provides production-ready features:

**Core features:**
- **Server-Side Rendering (SSR)** - Dynamic server rendering
- **Static Site Generation (SSG)** - Pre-built static pages
- **API Routes** - Backend API endpoints
- **File-based routing** - Automatic routing from file structure
- **Code splitting** - Automatic bundle optimization
- **Image optimization** - Built-in image component with optimization
- **TypeScript support** - First-class TypeScript integration


```jsx
// File structure creates routes automatically
pages/
  index.js          // Route: /
  about.js          // Route: /about
  blog/
    index.js        // Route: /blog
    [slug].js       // Route: /blog/[slug]
  api/
    users.js        // API: /api/users

// Basic Next.js page
function HomePage() {
  return (
    <div>
      <h1>Welcome to Next.js</h1>
      <Link href="/about">About</Link>
    </div>
  );
}

export default HomePage;
```
</details>

- **Q2:** What's the difference between SSR, SSG, and CSR?
<details>
<summary>Answer</summary>
| Rendering Method | When | Where | Performance | SEO | Use Case |
|------------------|------|-------|-------------|-----|----------|
| **SSR** | Request time | Server | Good | Excellent | Dynamic content |
| **SSG** | Build time | CDN | Excellent | Excellent | Static content |
| **CSR** | Runtime | Browser | Poor initial | Poor | Interactive apps |


```jsx
// SSG - getStaticProps (build time)
export async function getStaticProps() {
  const posts = await fetchPosts();
  return {
    props: { posts },
    revalidate: 3600 // ISR: revalidate every hour
  };
}

function BlogPage({ posts }) {
  return (
    <div>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.excerpt}</p>
        </article>
      ))}
    </div>
  );
}

// SSR - getServerSideProps (request time)
export async function getServerSideProps(context) {
  const { userId } = context.params;
  const user = await fetchUser(userId);
  
  return {
    props: { user }
  };
}

function UserProfile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>Last login: {user.lastLogin}</p>
    </div>
  );
}

// CSR - Client-side rendering
function Dashboard() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetchDashboardData().then(setData);
  }, []);
  
  if (!data) return <div>Loading...</div>;
  
  return <div>{/* Render dashboard */}</div>;
}
```
</details>

- **Q3:** What is ISR (Incremental Static Regeneration)?
<details>
<summary>Answer</summary>
**ISR** combines benefits of SSG and SSR by regenerating static pages on-demand:

**How ISR works:**
1. **Initial request** serves stale static page immediately
2. **Background regeneration** triggered if page is stale
3. **New version** replaces old static page
4. **Subsequent requests** get updated version


```jsx
// ISR with revalidate
export async function getStaticProps() {
  const posts = await fetchPosts();
  
  return {
    props: { posts },
    revalidate: 60 // Regenerate at most once every 60 seconds
  };
}

// ISR with on-demand revalidation (Next.js 12.2+)
// pages/api/revalidate.js
export default async function handler(req, res) {
  if (req.secret !== process.env.REVALIDATE_TOKEN) {
    return res.status(401).json({ message: 'Invalid token' });
  }
  
  try {
    await res.revalidate('/blog');
    await res.revalidate('/blog/[slug]');
    return res.json({ revalidated: true });
  } catch (err) {
    return res.status(500).send('Error revalidating');
  }
}

// ISR with fallback pages
export async function getStaticPaths() {
  const posts = await fetchPopularPosts();
  const paths = posts.map(post => ({ params: { slug: post.slug } }));
  
  return {
    paths,
    fallback: 'blocking' // Generate missing pages on first request
  };
}

export async function getStaticProps({ params }) {
  const post = await fetchPost(params.slug);
  
  if (!post) {
    return { notFound: true };
  }
  
  return {
    props: { post },
    revalidate: 3600 // Regenerate once per hour
  };
}
```

**ISR benefits:**
- **Fast initial load** - Serves cached static page
- **Fresh content** - Updates automatically when stale
- **Scalability** - CDN caching + dynamic updates
- **Cost-effective** - Less server compute than full SSR
</details>

#### **🚀 Intermediate:**
- **Q1:** How do you choose between getStaticProps, getServerSideProps, and getStaticPaths?
<details>
<summary>Answer</summary>
**Decision matrix:**


```jsx
// Use getStaticProps when:
// - Data doesn't change often
// - Same data for all users
// - SEO is important
// - Performance is critical
export async function getStaticProps() {
  const products = await fetchProducts();
  return {
    props: { products },
    revalidate: 3600 // ISR for occasional updates
  };
}

// Use getServerSideProps when:
// - Data changes frequently
// - User-specific data
// - Request-time data needed
// - Real-time requirements
export async function getServerSideProps(context) {
  const { req } = context;
  const user = await getCurrentUser(req);
  const notifications = await fetchUserNotifications(user.id);
  
  return {
    props: { user, notifications }
  };
}

// Use getStaticPaths when:
// - Dynamic routes with known possible values
// - Want to pre-generate popular pages
// - Combine with getStaticProps for ISR
export async function getStaticPaths() {
  const categories = await fetchCategories();
  
  return {
    paths: categories.map(cat => ({ params: { category: cat.slug } })),
    fallback: 'blocking' // Handle unknown categories
  };
}

// Decision flowchart:
const chooseDataFetching = (requirements) => {
  if (requirements.userSpecific) {
    return 'getServerSideProps';
  }
  
  if (requirements.dynamicRoutes) {
    return ['getStaticPaths', 'getStaticProps'];
  }
  
  if (requirements.frequentUpdates) {
    return requirements.seoImportant 
      ? 'getStaticProps with ISR'
      : 'client-side fetching';
  }
  
  return 'getStaticProps';
};

// Hybrid approach example
function ProductPage({ product, relatedProducts }) {
  const [reviews, setReviews] = useState([]);
  
  // Static: product data (SSG)
  // Client-side: reviews (real-time updates)
  useEffect(() => {
    fetchProductReviews(product.id).then(setReviews);
  }, [product.id]);
  
  return (
    <div>
      <ProductDetails product={product} />
      <RelatedProducts products={relatedProducts} />
      <ProductReviews reviews={reviews} />
    </div>
  );
}
```
</details>

- **Q2:** How do you implement API routes in Next.js?
<details>
<summary>Answer</summary>
**API routes** in Next.js create serverless functions:


```jsx
// pages/api/users.js - Basic API route
export default function handler(req, res) {
  if (req.method === 'GET') {
    const users = await getUsers();
    res.status(200).json(users);
  } else if (req.method === 'POST') {
    const newUser = await createUser(req.body);
    res.status(201).json(newUser);
  } else {
    res.setHeader('Allow', ['GET', 'POST']);
    res.status(405).end(`Method ${req.method} Not Allowed`);
  }
}

// pages/api/users/[id].js - Dynamic API route
export default async function handler(req, res) {
  const { id } = req.query;
  
  switch (req.method) {
    case 'GET':
      const user = await getUserById(id);
      if (!user) {
        return res.status(404).json({ message: 'User not found' });
      }
      res.status(200).json(user);
      break;
      
    case 'PUT':
      const updatedUser = await updateUser(id, req.body);
      res.status(200).json(updatedUser);
      break;
      
    case 'DELETE':
      await deleteUser(id);
      res.status(204).end();
      break;
      
    default:
      res.setHeader('Allow', ['GET', 'PUT', 'DELETE']);
      res.status(405).end(`Method ${req.method} Not Allowed`);
  }
}

// pages/api/auth/[...nextauth].js - Catch-all API route
import NextAuth from 'next-auth';
import GoogleProvider from 'next-auth/providers/google';

export default NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
  // Additional NextAuth configuration
});

// Middleware for API routes
export default function withAuth(handler) {
  return async (req, res) => {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (!token) {
      return res.status(401).json({ message: 'No token provided' });
    }
    
    try {
      const user = await verifyToken(token);
      req.user = user;
      return handler(req, res);
    } catch (error) {
      return res.status(401).json({ message: 'Invalid token' });
    }
  };
}

// Usage with middleware
export default withAuth(async function handler(req, res) {
  // req.user is available here
  const userPosts = await getUserPosts(req.user.id);
  res.status(200).json(userPosts);
});

// File upload API route
import formidable from 'formidable';

export const config = {
  api: {
    bodyParser: false,
  },
};

export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ message: 'Method not allowed' });
  }
  
  const form = formidable();
  
  try {
    const [fields, files] = await form.parse(req);
    const file = files.file[0];
    
    // Process file upload
    const uploadedFile = await uploadToS3(file);
    
    res.status(200).json({ url: uploadedFile.url });
  } catch (error) {
    res.status(500).json({ message: 'Upload failed' });
  }
}
```

**API route features:**
- **Serverless functions** - Auto-scaling backend endpoints
- **File-based routing** - URL structure matches file structure
- **Built-in middleware** - CORS, body parsing, etc.
- **Environment variables** - Secure configuration
- **Edge runtime** - Fast global execution
</details>

---

### 🎯 **Server Components (React 18+)**

#### **📋 Beginner:**
- **Q1:** What are React Server Components?
<details>
<summary>Answer</summary>
**React Server Components** run on the server and render to a special format that's sent to the client:

**Key characteristics:**
- **Run on server** - Execute during build time or request time
- **No client bundle** - Code doesn't ship to browser
- **Direct backend access** - Can access databases, file system, etc.
- **Zero client JavaScript** - Reduces bundle size
- **Seamless integration** - Work with client components


```jsx
// Server Component (runs on server)
import { db } from './database';

async function BlogPosts() {
  // Direct database access - no API needed
  const posts = await db.posts.findMany({
    orderBy: { createdAt: 'desc' }
  });
  
  return (
    <div>
      <h1>Latest Posts</h1>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.excerpt}</p>
          <PostInteractions postId={post.id} /> {/* Client Component */}
        </article>
      ))}
    </div>
  );
}

// Client Component (runs in browser)
'use client';

import { useState } from 'react';

function PostInteractions({ postId }) {
  const [liked, setLiked] = useState(false);
  
  return (
    <div>
      <button onClick={() => setLiked(!liked)}>
        {liked ? '❤️' : '🤍'} Like
      </button>
      <button>Share</button>
    </div>
  );
}
```
</details>

- **Q2:** How do Server Components differ from traditional SSR?
<details>
<summary>Answer</summary>
| Feature | Server Components | Traditional SSR |
|---------|-------------------|-----------------|
| **Execution** | Server only | Server + Client |
| **Bundle size** | No client JS | Full component JS |
| **Hydration** | Selective | Full page |
| **Data access** | Direct backend | API calls |
| **Re-rendering** | Server re-render | Client re-render |
| **State** | No client state | Full client state |


```jsx
// Traditional SSR
function SSRComponent({ posts }) {
  const [filter, setFilter] = useState(''); // Client state
  
  // This entire component ships to client
  useEffect(() => {
    // Client-side effect
  }, []);
  
  return (
    <div>
      <input 
        value={filter} 
        onChange={e => setFilter(e.target.value)}
      />
      {posts.filter(post => 
        post.title.includes(filter)
      ).map(post => (
        <PostCard key={post.id} post={post} />
      ))}
    </div>
  );
}

// Server Component approach
async function ServerPostList() {
  const posts = await fetchPosts(); // Server-only
  
  return (
    <div>
      <PostFilter /> {/* Client Component for interactivity */}
      <PostsList posts={posts} /> {/* Server Component for rendering */}
    </div>
  );
}

'use client';
function PostFilter() {
  const [filter, setFilter] = useState('');
  // Only this small component ships to client
  
  return (
    <input 
      value={filter}
      onChange={e => setFilter(e.target.value)}
    />
  );
}
```

**SSR process:**
1. Server renders HTML
2. Client downloads full component JS
3. Hydration makes entire page interactive

**Server Components process:**
1. Server renders components to special format
2. Client receives minimal JS for interactive parts
3. Only client components hydrate
</details>

- **Q3:** What are the benefits of Server Components?
<details>
<summary>Answer</summary>
**Server Components benefits:**

1. **Reduced bundle size**:

```jsx
// Before: Large chart library ships to client
import { LargeChartLibrary } from 'heavy-charts';

function Dashboard({ data }) {
  return <LargeChartLibrary data={data} />;
}

// After: Chart renders on server, no client JS
async function ServerDashboard() {
  const data = await fetchAnalytics();
  return <ServerChart data={data} />; // Renders to HTML/SVG
}
```

2. **Direct backend access**:

```jsx
// Before: Need API routes
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(setUser);
  }, [userId]);
  
  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}

// After: Direct database access
async function ServerUserProfile({ userId }) {
  const user = await db.users.findById(userId);
  return <div>{user.name}</div>;
}
```

3. **Better performance**:
- **Faster initial load** - Less JavaScript to download
- **Reduced waterfalls** - Data fetched in parallel on server
- **Better Core Web Vitals** - Smaller bundles, faster interaction

4. **Improved security**:
- **Server-only secrets** - API keys, database credentials
- **Sensitive logic** - Business rules stay on server
- **Reduced attack surface** - Less client-side code

5. **Simplified data fetching**:

```jsx
// Complex client-side data fetching
function ComplexComponent() {
  const [posts, setPosts] = useState([]);
  const [authors, setAuthors] = useState([]);
  const [categories, setCategories] = useState([]);
  
  useEffect(() => {
    Promise.all([
      fetchPosts(),
      fetchAuthors(),
      fetchCategories()
    ]).then(([posts, authors, categories]) => {
      setPosts(posts);
      setAuthors(authors);
      setCategories(categories);
    });
  }, []);
  
  // Complex loading states, error handling...
}

// Simple server component
async function ServerComponent() {
  const [posts, authors, categories] = await Promise.all([
    fetchPosts(),
    fetchAuthors(),
    fetchCategories()
  ]);
  
  return <ComplexUI posts={posts} authors={authors} categories={categories} />;
}
```
</details>

#### **🚀 Intermediate:**
- **Q1:** How do Server and Client Components work together?
<details>
<summary>Answer</summary>
**Server and Client Components composition patterns:**


```jsx
// Server Component (default)
async function BlogPage() {
  const posts = await fetchPosts();
  
  return (
    <div>
      <Header /> {/* Server Component */}
      <SearchBar /> {/* Client Component - needs interactivity */}
      <PostList posts={posts}> {/* Server Component */}
        {posts.map(post => (
          <PostCard key={post.id} post={post}> {/* Server Component */}
            <LikeButton postId={post.id} /> {/* Client Component */}
            <CommentSection postId={post.id} /> {/* Client Component */}
          </PostCard>
        ))}
      </PostList>
    </div>
  );
}

// Client Component boundary
'use client';

function SearchBar() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  const handleSearch = async () => {
    const results = await searchPosts(query);
    setResults(results);
  };
  
  return (
    <div>
      <input 
        value={query}
        onChange={e => setQuery(e.target.value)}
      />
      <button onClick={handleSearch}>Search</button>
      <SearchResults results={results} /> {/* Client Component */}
    </div>
  );
}

// Passing props between Server and Client Components
async function ServerParent() {
  const data = await fetchData();
  
  return (
    <ClientChild 
      data={data} // ✅ Serializable props
      timestamp={new Date().toISOString()} // ✅ Serializable
      // onCallback={() => {}} // ❌ Functions not serializable
      // complexObject={new Map()} // ❌ Non-serializable objects
    />
  );
}

'use client';
function ClientChild({ data, timestamp }) {
  return <div>{data.title} - {timestamp}</div>;
}

// Pattern: Server Component wrapping Client Component
async function ProductPage({ productId }) {
  const product = await fetchProduct(productId);
  const reviews = await fetchReviews(productId);
  
  return (
    <div>
      <ProductInfo product={product} /> {/* Server Component */}
      
      {/* Client Component with server-fetched data */}
      <InteractiveReviews 
        initialReviews={reviews}
        productId={productId}
      />
    </div>
  );
}

'use client';
function InteractiveReviews({ initialReviews, productId }) {
  const [reviews, setReviews] = useState(initialReviews);
  const [newReview, setNewReview] = useState('');
  
  const addReview = async () => {
    const review = await submitReview(productId, newReview);
    setReviews(prev => [review, ...prev]);
    setNewReview('');
  };
  
  return (
    <div>
      <ReviewForm 
        value={newReview}
        onChange={setNewReview}
        onSubmit={addReview}
      />
      <ReviewsList reviews={reviews} />
    </div>
  );
}
```
</details>

- **Q2:** What are the limitations of Server Components?
<details>
<summary>Answer</summary>
**Server Component limitations:**

1. **No client-side state**:

```jsx
// ❌ Can't use useState, useEffect, etc.
function ServerComponent() {
  const [count, setCount] = useState(0); // Error!
  
  useEffect(() => {
    // Error! No client effects
  }, []);
  
  return <div>{count}</div>;
}

// ✅ Use Client Components for state
'use client';
function ClientCounter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

2. **No browser APIs**:

```jsx
// ❌ No access to window, document, localStorage
function ServerComponent() {
  const width = window.innerWidth; // Error!
  const stored = localStorage.getItem('key'); // Error!
  
  return <div>{width}</div>;
}

// ✅ Use Client Components for browser APIs
'use client';
function ClientComponent() {
  const [width, setWidth] = useState(0);
  
  useEffect(() => {
    setWidth(window.innerWidth);
  }, []);
  
  return <div>{width}</div>;
}
```

3. **No event handlers**:

```jsx
// ❌ No onClick, onChange, etc.
function ServerComponent() {
  const handleClick = () => console.log('clicked'); // Error!
  
  return <button onClick={handleClick}>Click</button>;
}

// ✅ Wrap interactive parts in Client Components
function ServerWrapper() {
  return (
    <div>
      <h1>Server Content</h1>
      <ClientButton />
    </div>
  );
}

'use client';
function ClientButton() {
  return (
    <button onClick={() => console.log('clicked')}>
      Click me
    </button>
  );
}
```

4. **Serialization constraints**:

```jsx
// ❌ Can't pass functions, classes, or complex objects
async function ServerParent() {
  const callback = () => console.log('hello'); // Error!
  const date = new Date(); // Error!
  const map = new Map(); // Error!
  
  return (
    <ClientChild 
      callback={callback}
      date={date}
      map={map}
    />
  );
}

// ✅ Pass serializable data only
async function ServerParent() {
  const data = await fetchData();
  
  return (
    <ClientChild 
      title={data.title} // ✅ String
      count={data.count} // ✅ Number
      items={data.items} // ✅ Array
      meta={{ id: data.id, timestamp: data.createdAt }} // ✅ Plain object
    />
  );
}
```

5. **No context consumption**:

```jsx
// ❌ Can't use useContext in Server Components
function ServerComponent() {
  const theme = useContext(ThemeContext); // Error!
  return <div className={theme}>Content</div>;
}

// ✅ Pass context values as props or use Client Components
function ServerWithTheme({ theme }) {
  return <div className={theme}>Content</div>;
}
```

**Working around limitations:**
- **Compose strategically** - Server for data, Client for interaction
- **Pass serializable props** - Convert complex objects to plain data
- **Use children pattern** - Server Components can render Client Components
- **Lift state up** - Manage state in Client Components, pass to Server Components
</details>

---
