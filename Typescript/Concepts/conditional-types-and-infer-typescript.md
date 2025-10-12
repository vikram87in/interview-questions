# Understanding Conditional Types and `infer` in TypeScript

## 🎯 Introduction
TypeScript has some powerful features that allow you to write smarter, more flexible types.  
Two of these features are **conditional types** and the **`infer`** keyword.

This guide will explain both in simple terms with clear, easy-to-understand examples.

---

## 🧠 Conditional Types

Conditional types allow you to express logic in types — like an *if-else statement* for types.

### 📘 Syntax
```ts
T extends U ? X : Y
```
This means:
> If `T` is assignable to `U`, use type `X`; otherwise, use type `Y`.

---

### 🧩 Example 1: Basic Usage
```ts
type IsString<T> = T extends string ? "yes" : "no";

type A = IsString<string>; // "yes"
type B = IsString<number>; // "no"
```

If `T` is a `string`, the result is `"yes"`. Otherwise, `"no"`.

---

### 🧩 Example 2: Conditional Return Type
```ts
type Result<T> = T extends boolean ? "Boolean Result" : "Other Result";

function getResult<T>(input: T): Result<T> {
  return (typeof input === "boolean" ? "Boolean Result" : "Other Result") as Result<T>;
}

let r1 = getResult(true);  // "Boolean Result"
let r2 = getResult(123);   // "Other Result"
```

The return type changes automatically depending on the input.

---

### 🧩 Example 3: Filtering Types
```ts
type ExtractString<T> = T extends string ? T : never;

type MyTypes = string | number | boolean;
type OnlyStrings = ExtractString<MyTypes>; 
// OnlyStrings = string
```

This removes all types from a union except `string`.

---

### 🧩 Example 4: Built-in Example — `Exclude`
```ts
type Example = Exclude<string | number | boolean, boolean>;
// Result: string | number
```

`Exclude` is defined using conditional types internally:
```ts
type Exclude<T, U> = T extends U ? never : T;
```

---

### 🧩 Example 5: Conditional Function Example
```ts
type ToArray<T> = T extends any[] ? T : T[];

function ensureArray<T>(input: T): ToArray<T> {
  return (Array.isArray(input) ? input : [input]) as ToArray<T>;
}

const arr1 = ensureArray(5);        // number[]
const arr2 = ensureArray(["a"]);    // string[]
```

If the input is already an array, return it. Otherwise, wrap it inside an array.

---

## 🧩 Real-World Example — API Response
```ts
type ApiResponse<T> = T extends { success: true }
  ? { data: string }
  : { error: string };

type Success = ApiResponse<{ success: true }>;   // { data: string }
type Failure = ApiResponse<{ success: false }>;  // { error: string }
```

---

## 💡 Why Use Conditional Types?

| Benefit | Description |
|----------|--------------|
| ✅ Type Safety | Automatically adapts to input type |
| ⚙️ Reusable | One flexible type covers multiple cases |
| 💡 Smarter Inference | Editor infers correct type |
| 🧩 Foundation | Used in built-ins like `Exclude`, `Extract`, etc. |

---

## 🔍 `infer` in TypeScript

`infer` is used **inside conditional types** to capture (or “extract”) a type.

### 📘 Syntax
```ts
T extends SomeType<infer U> ? U : never
```

Read as:
> If `T` matches `SomeType<something>`, infer that `something` as `U`.

---

### 🧩 Example 1: Inferring Array Element Type
```ts
type ElementType<T> = T extends (infer U)[] ? U : T;

type A = ElementType<string[]>; // string
type B = ElementType<number[]>; // number
type C = ElementType<boolean>;  // boolean
```

---

### 🧩 Example 2: Inferring Function Return Type
```ts
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R : never;

function greet() {
  return "Hello Vikram!";
}

type GreetReturn = ReturnTypeOf<typeof greet>; // string
```

This is exactly how TypeScript’s built-in `ReturnType<T>` works.

---

### 🧩 Example 3: Inferring Promise Result
```ts
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;

type A = UnwrapPromise<Promise<string>>; // string
type B = UnwrapPromise<number>;          // number
```

---

### 🧩 Example 4: Inferring Tuple Parts
```ts
type Head<T> = T extends [infer First, ...any[]] ? First : never;
type Tail<T> = T extends [any, ...infer Rest] ? Rest : never;

type Example = [string, number, boolean];

type FirstElem = Head<Example>; // string
type RestElems = Tail<Example>; // [number, boolean]
```

---

### 🧩 Example 5: Inferring Function Parameters
```ts
type FirstParameter<T> = T extends (arg1: infer P, ...args: any[]) => any ? P : never;

type MyFunc = (id: number, name: string) => void;

type Param = FirstParameter<MyFunc>; // number
```

---

## 🎯 Why Use `infer`?

| Benefit | Description |
|----------|--------------|
| 🧩 Extract Types | Pull out part of a complex type |
| ⚙️ Dynamic Typing | Adapts to different shapes of types |
| 💡 Reduces Repetition | No need to explicitly define inner types |
| 🚀 Enables Advanced Utilities | Used in built-ins like `ReturnType`, `Parameters`, `Awaited`, etc. |

---

## ✅ Summary

| Concept | Example | Extracted Type |
|----------|----------|----------------|
| Array element | `T extends (infer U)[]` | `U` (element) |
| Function return | `T extends (...args) => infer R` | `R` (return type) |
| Function param | `T extends (arg1: infer P)` | `P` (first param) |
| Promise inner | `T extends Promise<infer U>` | `U` (resolved value) |
| Tuple head/tail | `[infer F, ...infer R]` | `F` or `R` |

---

### 🌟 In Short
- **Conditional types** are like “if-else” for types.  
- **`infer`** helps TypeScript *extract* types automatically from complex structures.

Together, they make TypeScript’s type system extremely powerful and expressive.
