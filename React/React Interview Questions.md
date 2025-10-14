# 📚 React Interview Questions Bank

---

## 🔹 **Basics**

---

### 🎯 **What is React and why use it**

#### **📋 Beginner:**
- **Q1:** What is React and what problem does it solve?
- **Q2:** What are the main advantages of using React over vanilla JavaScript?
- **Q3:** What is the difference between a library and a framework? Where does React fit?

#### **🚀 Intermediate:**
- **Q1:** How does React's virtual DOM improve performance compared to direct DOM manipulation?
- **Q2:** What are the trade-offs of using React in a project?

---

### 🎯 **JSX**

#### **📋 Beginner:**
- **Q1:** What is JSX and why is it used in React?
- **Q2:** Can browsers understand JSX directly? If not, how is it processed?
- **Q3:** What are the basic rules for writing JSX?

#### **🚀 Intermediate:**
- **Q1:** What does this JSX code compile to?
```jsx
const element = <h1 className="greeting">Hello, world!</h1>;
```
- **Q2:** How do you handle events in JSX and what are synthetic events?

---

### 🎯 **Components (functional vs class)**

#### **📋 Beginner:**
- **Q1:** What is a React component and what are the two main types?
- **Q2:** How do you create a simple functional component?
- **Q3:** What is the basic structure of a class component?

#### **🚀 Intermediate:**
- **Q1:** What are the advantages of functional components over class components?
- **Q2:** Convert this class component to a functional component:
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

---

### 🎯 **Props vs state**

#### **📋 Beginner:**
- **Q1:** What are props in React and how do you pass them to components?
- **Q2:** What is state and how is it different from props?
- **Q3:** Can you modify props inside a component? Why or why not?

#### **🚀 Intermediate:**
- **Q1:** What happens when you try to modify props directly in a component?
- **Q2:** Explain the concept of "lifting state up" with an example scenario.

---

### 🎯 **Rendering elements**

#### **📋 Beginner:**
- **Q1:** How do you render a React element to the DOM?
- **Q2:** What is the root element in a React application?
- **Q3:** Can you have multiple React apps on the same page?

#### **🚀 Intermediate:**
- **Q1:** What is the difference between ReactDOM.render() and ReactDOM.createRoot()?
- **Q2:** How does React decide when to re-render components?

---

### 🎯 **Conditional rendering**

#### **📋 Beginner:**
- **Q1:** How do you conditionally render elements in React?
- **Q2:** What are three different ways to implement conditional rendering?
- **Q3:** What will this code render if `isLoggedIn` is false?
```jsx
{isLoggedIn && <WelcomeMessage />}
```

#### **🚀 Intermediate:**
- **Q1:** What's the difference between using `&&` operator and ternary operator for conditional rendering?
- **Q2:** How do you handle multiple conditions in JSX efficiently?

---

### 🎯 **Lists & keys**

#### **📋 Beginner:**
- **Q1:** How do you render a list of items in React?
- **Q2:** What are keys in React and why are they important?
- **Q3:** What happens if you don't provide keys when rendering lists?

#### **🚀 Intermediate:**
- **Q1:** Why should you avoid using array indices as keys?
- **Q2:** What makes a good key for list items?

---

## 🔹 **Component Lifecycle**

---

### 🎯 **Class component lifecycle methods**
#### **📋 Beginner:**
- **Q1:** What are the three main phases of a component lifecycle?
- **Q2:** When does componentDidMount() execute and what is it commonly used for?
- **Q3:** What is componentWillUnmount() used for?

#### **🚀 Intermediate:**
- **Q1:** What's the difference between componentDidUpdate() and componentDidMount()?
- **Q2:** How do you prevent unnecessary re-renders using lifecycle methods?

---

## 🔹 **Hooks**

---

### 🎯 **useState**

#### **📋 Beginner:**
- **Q1:** What is the useState hook and how do you use it?
- **Q2:** What does useState return?
- **Q3:** How do you update state using useState?

#### **🚀 Intermediate:**
- **Q1:** What will happen when this button is clicked?
```jsx
const [count, setCount] = useState(0);
const handleClick = () => {
  setCount(count + 1);
  setCount(count + 1);
  console.log(count);
};
```
- **Q2:** How do you update state based on previous state correctly?

---

### 🎯 **useEffect (dependencies, cleanup)**

#### **📋 Beginner:**
- **Q1:** What is useEffect and when does it run?
- **Q2:** How do you make useEffect run only once?
- **Q3:** What is the purpose of the dependency array in useEffect?

#### **🚀 Intermediate:**
- **Q1:** How do you clean up effects (like event listeners or subscriptions)?
- **Q2:** What will happen in this useEffect?
```jsx
useEffect(() => {
  console.log('Effect runs');
  setCount(count + 1);
}, [count]);
```

---

---

### 🎯 **useContext**

#### **📋 Beginner:**
- **Q1:** What is useContext and how do you use it?
- **Q2:** How do you create a context in React?
- **Q3:** What is a Context Provider?

#### **🚀 Intermediate:**
- **Q1:** How do you avoid unnecessary re-renders when using Context?
- **Q2:** When should you split contexts instead of using one large context?

---

### 🎯 **useReducer**

#### **📋 Beginner:**
- **Q1:** What is useReducer and when would you use it instead of useState?
- **Q2:** What are the arguments that useReducer takes?
- **Q3:** What is a reducer function?

#### **🚀 Intermediate:**
- **Q1:** How do you implement useReducer for a counter with increment, decrement, and reset actions?
- **Q2:** What are the benefits of using useReducer for complex state logic?

---

### 🎯 **useRef**

#### **📋 Beginner:**
- **Q1:** What is useRef and what are its common use cases?
- **Q2:** How do you access DOM elements using useRef?
- **Q3:** Does changing a ref's current value cause a re-render?

#### **🚀 Intermediate:**
- **Q1:** How can you use useRef to store previous values?
- **Q2:** What's the difference between useRef and useState for storing values?

---

### 🎯 **useMemo**

#### **📋 Beginner:**
- **Q1:** What is useMemo and when should you use it?
- **Q2:** How does useMemo help with performance?
- **Q3:** What happens if you don't provide a dependency array to useMemo?

#### **🚀 Intermediate:**
- **Q1:** When can useMemo be an anti-pattern?
- **Q2:** How do you decide what to include in useMemo's dependency array?

---

### 🎯 **useCallback**

#### **📋 Beginner:**
- **Q1:** What is useCallback and how is it related to useMemo?
- **Q2:** When should you use useCallback?
- **Q3:** What does useCallback return?

#### **🚀 Intermediate:**
- **Q1:** Why doesn't this optimization work?
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
- **Q2:** How can overusing useCallback hurt performance?

---

### 🎯 **useLayoutEffect**

#### **📋 Beginner:**
- **Q1:** What's the difference between useEffect and useLayoutEffect?
- **Q2:** When should you use useLayoutEffect?
- **Q3:** What are the performance implications of useLayoutEffect?

#### **🚀 Intermediate:**
- **Q1:** How would you measure DOM elements before the browser paints?
- **Q2:** Why might useLayoutEffect cause issues in SSR?

---

### 🎯 **useImperativeHandle**

#### **📋 Beginner:**
- **Q1:** What is useImperativeHandle and with which hook is it typically used?
- **Q2:** When would you need to use useImperativeHandle?
- **Q3:** How do you expose methods from a child component to its parent?

#### **🚀 Intermediate:**
- **Q1:** Why should useImperativeHandle be used sparingly?
- **Q2:** How do you create a custom input component that exposes a focus method?

---

### 🎯 **Custom hooks**

#### **📋 Beginner:**
- **Q1:** What are custom hooks and what naming convention must they follow?
- **Q2:** Why would you create a custom hook?
- **Q3:** Can custom hooks use other hooks?

#### **🚀 Intermediate:**
- **Q1:** Create a custom hook called useLocalStorage that manages localStorage state.
- **Q2:** How do you test custom hooks?

---

## 🔹 **Advanced Hooks Patterns**

---

### 🎯 **Rules of hooks**

#### **📋 Beginner:**
- **Q1:** What are the two main Rules of Hooks?
- **Q2:** Why can't you call hooks inside loops or conditions?
- **Q3:** Where can hooks be called?

#### **🚀 Intermediate:**
- **Q1:** How does React keep track of hook state between renders?
- **Q2:** What tools help enforce the Rules of Hooks?

---

### 🎯 **Sharing logic with custom hooks**

#### **📋 Beginner:**
- **Q1:** How do custom hooks help with code reuse?
- **Q2:** What's the difference between custom hooks and HOCs for sharing logic?
- **Q3:** Can custom hooks return JSX?

#### **🚀 Intermediate:**
- **Q1:** Design a custom hook for handling form input state.
- **Q2:** How do you compose multiple custom hooks together?

---

## 🔹 **State Management**

---

### 🎯 **Context API**

#### **📋 Beginner:**
- **Q1:** How do you create and provide context values?
- **Q2:** What causes Context consumers to re-render?
- **Q3:** How do you consume context in functional components?

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Context to prevent unnecessary re-renders?
- **Q2:** When should you use multiple contexts vs one large context?

---

### 🎯 **Redux (actions, reducers, store, middleware)**

#### **📋 Beginner:**
- **Q1:** What are the three core principles of Redux?
- **Q2:** What is an action in Redux?
- **Q3:** What is a reducer and what rules must it follow?

#### **🚀 Intermediate:**
- **Q1:** How does middleware work in Redux?
- **Q2:** What is the purpose of normalizing state in Redux?

---

### 🎯 **Redux Toolkit**

#### **📋 Beginner:**
- **Q1:** What problems does Redux Toolkit solve?
- **Q2:** What is createSlice and what does it generate?
- **Q3:** How does Redux Toolkit handle immutability?

#### **🚀 Intermediate:**
- **Q1:** What is createAsyncThunk and when would you use it?
- **Q2:** How does RTK Query compare to other data fetching solutions?

---

## 🔹 **Rendering & Performance**

---

### 🎯 **Virtual DOM**

#### **📋 Beginner:**
- **Q1:** What is the Virtual DOM and why does React use it?
- **Q2:** How does the Virtual DOM improve performance?
- **Q3:** Is the Virtual DOM always faster than direct DOM manipulation?

#### **🚀 Intermediate:**
- **Q1:** Describe the process from state change to DOM update in React.
- **Q2:** What are the limitations of the Virtual DOM approach?

---

### 🎯 **Reconciliation**

#### **📋 Beginner:**
- **Q1:** What is reconciliation in React?
- **Q2:** What triggers the reconciliation process?
- **Q3:** How does React handle different element types during reconciliation?

#### **🚀 Intermediate:**
- **Q1:** How does React's reconciliation algorithm achieve O(n) complexity?
- **Q2:** What happens when component types change during reconciliation?

---

### 🎯 **Diffing algorithm**

#### **📋 Beginner:**
- **Q1:** What assumptions does React's diffing algorithm make?
- **Q2:** How does React handle list reconciliation?
- **Q3:** Why are keys important for the diffing algorithm?

#### **🚀 Intermediate:**
- **Q1:** What are the performance implications of changing all keys in a list?
- **Q2:** How can poor key choices lead to bugs?

---

### 🎯 **Controlled vs uncontrolled components**

#### **📋 Beginner:**
- **Q1:** What's the difference between controlled and uncontrolled components?
- **Q2:** How do you create a controlled input component?
- **Q3:** When might you use an uncontrolled component?

#### **🚀 Intermediate:**
- **Q1:** What are the trade-offs between controlled and uncontrolled components?
- **Q2:** How do you handle file inputs (which are always uncontrolled)?

---

### 🎯 **Memoization (React.memo, useMemo, useCallback)**

#### **📋 Beginner:**
- **Q1:** What is React.memo and when should you use it?
- **Q2:** How do React.memo, useMemo, and useCallback work together?
- **Q3:** When does React.memo not prevent re-renders?

#### **🚀 Intermediate:**
- **Q1:** When can memoization actually hurt performance?
- **Q2:** How do you properly memoize a component with object props?

---

### 🎯 **Code splitting & lazy loading**

#### **📋 Beginner:**
- **Q1:** What is code splitting and why is it useful?
- **Q2:** How do you implement lazy loading with React.lazy?
- **Q3:** What is Suspense and how does it work with lazy loading?

#### **🚀 Intermediate:**
- **Q1:** What are the best strategies for code splitting in a React app?
- **Q2:** How do you handle loading errors with lazy components?

---

### 🎯 **Suspense**

#### **📋 Beginner:**
- **Q1:** What is Suspense and what can it be used for?
- **Q2:** How do you use Suspense with React.lazy?
- **Q3:** What is a fallback in Suspense?

#### **🚀 Intermediate:**
- **Q1:** How will Suspense work with data fetching in the future?
- **Q2:** How do you handle nested Suspense boundaries?

---

## 🔹 **Routing**

---

### 🎯 **React Router basics (BrowserRouter, Route, Link)**

#### **📋 Beginner:**
- **Q1:** What is React Router and why is it needed?
- **Q2:** What's the difference between BrowserRouter and HashRouter?
- **Q3:** How do you create basic routes using Route components?

#### **🚀 Intermediate:**
- **Q1:** What's the difference between Link and regular anchor tags?
- **Q2:** How do you implement nested routing layouts?

---

### 🎯 **Route parameters**

#### **📋 Beginner:**
- **Q1:** How do you define route parameters in React Router?
- **Q2:** How do you access route parameters in your components?
- **Q3:** What happens if a required parameter is missing?

#### **🚀 Intermediate:**
- **Q1:** How do you handle optional route parameters?
- **Q2:** How do you validate route parameters?

---

### 🎯 **Nested routes**

#### **📋 Beginner:**
- **Q1:** What are nested routes and how do you implement them?
- **Q2:** How does the Outlet component work?
- **Q3:** How do you structure components for nested routes?

#### **🚀 Intermediate:**
- **Q1:** How do you implement breadcrumbs with nested routes?
- **Q2:** How do you handle data loading for nested routes?

---

### 🎯 **Redirects**

#### **📋 Beginner:**
- **Q1:** How do you redirect users to different routes?
- **Q2:** What's the difference between Navigate and useNavigate?
- **Q3:** How do you implement protected routes?

#### **🚀 Intermediate:**
- **Q1:** How do you preserve the intended destination after login redirects?
- **Q2:** How do you handle programmatic navigation with state?

---

### 🎯 **Hooks in React Router (useParams, useNavigate)**

#### **📋 Beginner:**
- **Q1:** What does useParams return and how do you use it?
- **Q2:** How do you navigate programmatically using useNavigate?
- **Q3:** What other React Router hooks are commonly used?

#### **🚀 Intermediate:**
- **Q1:** How do you access query parameters and location state?
- **Q2:** How do you implement navigation guards with React Router hooks?

---

## 🔹 **Error Handling**

---

### 🎯 **Error boundaries**

#### **📋 Beginner:**
- **Q1:** What are Error Boundaries and what errors do they catch?
- **Q2:** How do you create an Error Boundary?
- **Q3:** What errors do Error Boundaries NOT catch?

#### **🚀 Intermediate:**
- **Q1:** How do you implement error logging in Error Boundaries?
- **Q2:** How do you reset Error Boundaries after an error?

---

### 🎯 **componentDidCatch**

#### **📋 Beginner:**
- **Q1:** What is componentDidCatch and when is it called?
- **Q2:** What's the difference between componentDidCatch and getDerivedStateFromError?
- **Q3:** Can functional components implement error boundaries?

#### **🚀 Intermediate:**
- **Q1:** How do you integrate error boundaries with error monitoring services?
- **Q2:** How do you test error boundaries?

---

### 🎯 **Handling async errors**

#### **📋 Beginner:**
- **Q1:** Why don't Error Boundaries catch async errors?
- **Q2:** How do you handle errors in async operations?
- **Q3:** How do you handle promise rejections in useEffect?

#### **🚀 Intermediate:**
- **Q1:** How do you create a global error handler for async operations?
- **Q2:** How do you implement retry logic for failed async operations?

---

## 🔹 **Context & Prop Drilling**

---

### 🎯 **Context API usage**

#### **📋 Beginner:**
- **Q1:** What problem does Context API solve?
- **Q2:** How do you avoid prop drilling using Context?
- **Q3:** When should you use Context vs props?

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Context usage for large applications?
- **Q2:** How do you implement multiple contexts efficiently?

---

### 🎯 **Comparison: Context vs Redux**

#### **📋 Beginner:**
- **Q1:** What are the main differences between Context and Redux?
- **Q2:** When would you choose Context over Redux?
- **Q3:** When would you choose Redux over Context?

#### **🚀 Intermediate:**
- **Q1:** How do the performance characteristics of Context and Redux compare?
- **Q2:** How do you decide between Context, Redux, and other state management solutions?

---

## 🔹 **Asynchronous React**

---

### 🎯 **Data fetching (fetch, axios)**

#### **📋 Beginner:**
- **Q1:** How do you fetch data in React components?
- **Q2:** Why shouldn't you fetch data directly in the render method?
- **Q3:** What's the difference between fetch and axios for React applications?

#### **🚀 Intermediate:**
- **Q1:** How do you handle race conditions when fetching data?
- **Q2:** How do you implement request cancellation?

---

### 🎯 **Async in useEffect**

#### **📋 Beginner:**
- **Q1:** Can you make the useEffect callback function async?
- **Q2:** How do you properly handle async operations in useEffect?
- **Q3:** How do you clean up async operations in useEffect?

#### **🚀 Intermediate:**
- **Q1:** How do you handle component unmounting during async operations?
- **Q2:** How do you implement proper error handling for async useEffect operations?

---

### 🎯 **Suspense for data fetching**

#### **📋 Beginner:**
- **Q1:** How will Suspense work with data fetching?
- **Q2:** What is concurrent rendering and how does it relate to Suspense?
- **Q3:** What are the current limitations of Suspense for data fetching?

#### **🚀 Intermediate:**
- **Q1:** How do you implement data fetching libraries that work with Suspense?
- **Q2:** How do you handle error boundaries with Suspense data fetching?

---

## 🔹 **React 18 Features**

---

### 🎯 **Automatic batching**

#### **📋 Beginner:**
- **Q1:** What is automatic batching in React 18?
- **Q2:** How does automatic batching improve performance?
- **Q3:** What changed from React 17 to React 18 regarding batching?

#### **🚀 Intermediate:**
- **Q1:** How do you opt out of automatic batching when needed?
- **Q2:** What are the implications of automatic batching for testing?

---

### 🎯 **Transitions (useTransition)**

#### **📋 Beginner:**
- **Q1:** What is useTransition and what problem does it solve?
- **Q2:** How do you mark updates as transitions?
- **Q3:** What is the isPending value in useTransition?

#### **🚀 Intermediate:**
- **Q1:** How do transitions improve user experience in large lists?
- **Q2:** When should you avoid using transitions?

---

### 🎯 **useDeferredValue**

#### **📋 Beginner:**
- **Q1:** What is useDeferredValue and how does it work?
- **Q2:** How is useDeferredValue different from useTransition?
- **Q3:** When would you use useDeferredValue?

#### **🚀 Intermediate:**
- **Q1:** How do you implement debounced search with useDeferredValue?
- **Q2:** What are the performance implications of useDeferredValue?

---

### 🎯 **Concurrent rendering basics**

#### **📋 Beginner:**
- **Q1:** What is concurrent rendering in React 18?
- **Q2:** How does concurrent rendering improve user experience?
- **Q3:** Do you need to change your code to benefit from concurrent rendering?

#### **🚀 Intermediate:**
- **Q1:** How does concurrent rendering affect component lifecycle?
- **Q2:** What are the implications of concurrent rendering for third-party libraries?

---

### 🎯 **Streaming server rendering**

#### **📋 Beginner:**
- **Q1:** What is streaming server rendering?
- **Q2:** How does streaming SSR improve performance?
- **Q3:** What role does Suspense play in streaming SSR?

#### **🚀 Intermediate:**
- **Q1:** How do you implement selective hydration with streaming SSR?
- **Q2:** What are the challenges of streaming SSR implementation?

---

## 🔹 **Build & Deployment**

---

### 🎯 **Webpack basics**

#### **📋 Beginner:**
- **Q1:** What is Webpack and why is it used in React projects?
- **Q2:** What are the main concepts in Webpack (entry, output, loaders, plugins)?
- **Q3:** How does Webpack handle different file types?

#### **🚀 Intermediate:**
- **Q1:** How do you optimize Webpack builds for production?
- **Q2:** How do you implement code splitting with Webpack?

---

### 🎯 **Babel & JSX compilation**

#### **📋 Beginner:**
- **Q1:** What is Babel and why is it needed for React?
- **Q2:** How does JSX get transformed into JavaScript?
- **Q3:** What are Babel presets and plugins?

#### **🚀 Intermediate:**
- **Q1:** How do you configure Babel for optimal React builds?
- **Q2:** What are the differences between different JSX transforms?

---

### 🎯 **Production build optimizations**

#### **📋 Beginner:**
- **Q1:** What optimizations should you apply for React production builds?
- **Q2:** How do you minify and compress React applications?
- **Q3:** What is tree shaking and how does it help?

#### **🚀 Intermediate:**
- **Q1:** How do you analyze and optimize bundle sizes?
- **Q2:** What are the best practices for caching strategies in React apps?

---

## 🔹 **Miscellaneous**

---

### 🎯 **HOCs (Higher Order Components)**

#### **📋 Beginner:**
- **Q1:** What is a Higher Order Component (HOC)?
- **Q2:** How do you create a simple HOC?
- **Q3:** What problems do HOCs solve?

#### **🚀 Intermediate:**
- **Q1:** What are the drawbacks of HOCs?
- **Q2:** How do HOCs compare to hooks for code reuse?

---

### 🎯 **Render props pattern**

#### **📋 Beginner:**
- **Q1:** What is the render props pattern?
- **Q2:** How do you implement a component using render props?
- **Q3:** What are the benefits of render props?

#### **🚀 Intermediate:**
- **Q1:** How do render props compare to HOCs and hooks?
- **Q2:** How do you avoid callback hell with render props?

---

### 🎯 **Portals**

#### **📋 Beginner:**
- **Q1:** What are React Portals and when would you use them?
- **Q2:** How do you create a portal?
- **Q3:** Do portals affect event propagation?

#### **🚀 Intermediate:**
- **Q1:** How do you handle portal cleanup and memory leaks?
- **Q2:** How do portals work with SSR?

---

### 🎯 **Fragments**

#### **📋 Beginner:**
- **Q1:** What are React Fragments and why are they useful?
- **Q2:** What are the different ways to use Fragments?
- **Q3:** When should you use Fragments vs div wrappers?

#### **🚀 Intermediate:**
- **Q1:** Can Fragments have keys? When is this useful?
- **Q2:** How do Fragments affect CSS styling and layout?

---

### 🎯 **Keys in lists**

#### **📋 Beginner:**
- **Q1:** Why are keys required when rendering lists in React?
- **Q2:** What happens if you use array indices as keys?
- **Q3:** What makes a good key for list items?

#### **🚀 Intermediate:**
- **Q1:** How do keys affect component state during list reordering?
- **Q2:** How do you handle keys in dynamic lists?

---

### 🎯 **Hydration (SSR)**

#### **📋 Beginner:**
- **Q1:** What is hydration in the context of SSR?
- **Q2:** What happens during the hydration process?
- **Q3:** What are hydration mismatches?

#### **🚀 Intermediate:**
- **Q1:** How do you debug hydration issues?
- **Q2:** How do you implement progressive hydration?

---

### 🎯 **Next.js basics (SSR, SSG, ISR)**

#### **📋 Beginner:**
- **Q1:** What is Next.js and what features does it provide?
- **Q2:** What's the difference between SSR, SSG, and CSR?
- **Q3:** What is ISR (Incremental Static Regeneration)?

#### **🚀 Intermediate:**
- **Q1:** How do you choose between getStaticProps, getServerSideProps, and getStaticPaths?
- **Q2:** How do you implement API routes in Next.js?

---

### 🎯 **Server Components (React 18+)**

#### **📋 Beginner:**
- **Q1:** What are React Server Components?
- **Q2:** How do Server Components differ from traditional SSR?
- **Q3:** What are the benefits of Server Components?

#### **🚀 Intermediate:**
- **Q1:** How do Server and Client Components work together?
- **Q2:** What are the limitations of Server Components?

---
