# ⚙️ TypeScript Intermediate Interview Questions & Answers

A collection of intermediate-level TypeScript questions that test your understanding beyond the basics.

---

## 1. What are Type Guards in TypeScript?
Type Guards help TypeScript determine the type of a variable at runtime using checks.

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

➡️ Here, `typeof` acts as a **type guard**.

---

## 2. What is the difference between `interface` merging and `type` aliases?
Interfaces **can merge**, while `type` aliases cannot.

```ts
interface Person { name: string; }
interface Person { age: number; }

// Merged as:
const user: Person = { name: "Vikram", age: 30 };
```

Type aliases throw an error if redefined.

---

## 3. What is the purpose of the `keyof` operator?
It returns a union of all property names of a type.

```ts
interface User {
  name: string;
  age: number;
}

type UserKeys = keyof User; // "name" | "age"
```

---

## 4. What does the `typeof` operator do in TypeScript?
In **types**, `typeof` extracts the type of a variable or object.

```ts
const person = { name: "Vikram", age: 30 };
type PersonType = typeof person; // { name: string; age: number }
```

---

## 5. What is the `in` operator used for in mapped types?
It is used to iterate over keys in a type.

```ts
type Options = "dark" | "light";
type Settings = { [K in Options]: boolean };

// Equivalent to:
type Settings = { dark: boolean; light: boolean };
```

---

## 6. Explain `Partial`, `Pick`, and `Omit` utility types.
These are **built-in utility types** for transforming existing types.

```ts
interface User {
  name: string;
  age: number;
  email: string;
}

type PartialUser = Partial<User>; // all optional
type PickUser = Pick<User, "name" | "email">; // select few keys
type OmitUser = Omit<User, "email">; // remove specific keys
```

---

## 7. What is a discriminated union?
It’s a pattern combining unions with a shared literal property for type narrowing.

```ts
interface Square { kind: "square"; size: number; }
interface Circle { kind: "circle"; radius: number; }

type Shape = Square | Circle;

function area(shape: Shape) {
  switch (shape.kind) {
    case "square": return shape.size ** 2;
    case "circle": return Math.PI * shape.radius ** 2;
  }
}
```

---

## 8. What is the difference between `unknown` and `never`?
- `unknown`: value type not known at compile time  
- `never`: value that **never occurs**

```ts
function fail(): never {
  throw new Error("Error");
}
```

---

## 9. Explain `extends` with generics.
Used to **constrain** what types can be passed to a generic.

```ts
function logLength<T extends { length: number }>(arg: T) {
  console.log(arg.length);
}
logLength("Hello"); // ✅ valid
logLength([1, 2, 3]); // ✅ valid
```

---

## 10. What are Conditional Types?
Conditional types choose one type or another based on a condition.

```ts
type IsString<T> = T extends string ? "Yes" : "No";

type A = IsString<string>; // "Yes"
type B = IsString<number>; // "No"
```

---

## 11. How do you create a custom mapped type?
```ts
type ReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};

interface User {
  name: string;
  age: number;
}

type ReadonlyUser = ReadOnly<User>;
```

---

## 12. What is the difference between `abstract class` and `interface`?
- Abstract classes can contain **implementation**
- Interfaces only describe **structure**
- A class can **extend one** abstract class but **implement many** interfaces

```ts
abstract class Animal {
  abstract speak(): void;
  move() { console.log("Moving..."); }
}
```

---

## 13. What is the difference between `as const` and normal objects?
`as const` makes the entire object **immutable and literal**.

```ts
const COLORS = { RED: "red", BLUE: "blue" } as const;

type Color = typeof COLORS[keyof typeof COLORS];
// "red" | "blue"
```

---

## 14. How to ensure exhaustive checks in a `switch` statement?
Use the `never` type.

```ts
function getShapeArea(shape: Shape) {
  switch (shape.kind) {
    case "square": return shape.size ** 2;
    case "circle": return Math.PI * shape.radius ** 2;
    default:
      const _exhaustive: never = shape;
      return _exhaustive;
  }
}
```

---

## 15. What is a `namespace` in TypeScript?
Namespaces group related code together (less common today, replaced by ES modules).

```ts
namespace Utils {
  export function log(msg: string) {
    console.log("LOG:", msg);
  }
}

Utils.log("Hello");
```

---

### ✅ Summary
Intermediate TypeScript concepts include **type guards, mapped types, generics, discriminated unions**, and **utility types**. Mastering these gives you strong control over complex type systems.

---
