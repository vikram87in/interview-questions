# 🧠 TypeScript Basic Interview Questions & Answers

A quick guide to help you prepare for beginner-level TypeScript interviews.

---

## 1. What is TypeScript?
TypeScript is a **superset of JavaScript** that adds **static typing**, **interfaces**, and **type checking**. It compiles down to plain JavaScript.

---

## 2. Why use TypeScript over JavaScript?
- Detects errors at **compile time**  
- Improves **readability and maintainability**  
- Provides **better IDE support**  
- Enables **modern features** (classes, modules, async/await)

---

## 3. How do you define types for variables?
```ts
let name: string = "Vikram";
let age: number = 30;
let isActive: boolean = true;
```

---

## 4. What are types in TypeScript?
Types describe what kind of value a variable can hold.  
Examples: `string`, `number`, `boolean`, `any`, `unknown`, `void`, `object`, `array`, `tuple`, etc.

---

## 5. What is the difference between `any` and `unknown`?
- `any`: disables all type checking.  
- `unknown`: requires type checking before use.

```ts
let a: any = 10;      // can be used freely
let b: unknown = 10;  // must be type-checked
```

---

## 6. Difference between `interface` and `type`
| Feature | interface | type |
|----------|------------|------|
| Extension | Can be extended or merged | Cannot be merged |
| Flexibility | Limited to object shapes | Can represent unions, intersections, primitives |

```ts
interface Person { name: string }
type ID = string | number;
```

---

## 7. What is type inference?
TypeScript automatically determines the type of a variable based on its value.

```ts
let message = "Hello"; // inferred as string
```

---

## 8. What are union types?
Allow a variable to hold more than one type.

```ts
let value: string | number;
value = "Hi";
value = 10;
```

---

## 9. What are intersection types?
Combine multiple types into one.

```ts
type Person = { name: string };
type Employee = { id: number };
type Staff = Person & Employee; // must have both name and id
```

---

## 10. What are optional properties in interfaces?
Properties that may or may not be present.

```ts
interface User {
  name: string;
  age?: number;
}
```

---

## 11. What are function types?
Define types for parameters and return values.

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

---

## 12. What is type assertion?
Manually tell TypeScript what type a value is.

```ts
let value: unknown = "hello";
let length = (value as string).length;
```

---

## 13. What is the `readonly` modifier?
Prevents reassignment after initialization.

```ts
interface Car {
  readonly brand: string;
}
```

---

## 14. What are enums?
Enums define a set of named constants.

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
```

---

## 15. Difference between `null` and `undefined`
- `undefined`: variable declared but not assigned  
- `null`: explicitly assigned to indicate “no value”

---

## 16. What are generics?
Used to create reusable components that work with any type.

```ts
function identity<T>(value: T): T {
  return value;
}
```

---

## 17. What are access modifiers in classes?
| Modifier | Scope |
|-----------|--------|
| public | Accessible everywhere |
| private | Accessible only inside the class |
| protected | Accessible in class and subclasses |

```ts
class Car {
  private brand: string;
  protected speed: number;
  public color: string;
}
```

---

## 18. Difference between `interface` and `abstract class`
| Feature | Interface | Abstract Class |
|----------|------------|----------------|
| Implementation | Only structure | Can have implementation |
| Inheritance | Multiple allowed | Single inheritance |
| Use case | Define shape | Provide base functionality |

---

## 19. What is function overloading?
Define multiple signatures for a function with different parameter types.

```ts
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: any, b: any) {
  return a + b;
}
```

---

## 20. What is the `never` type?
Represents values that **never occur**, e.g., a function that always throws an error.

```ts
function throwError(): never {
  throw new Error("Error!");
}
```

---

### ✅ Summary
TypeScript adds **type safety**, **better tooling**, and **modern syntax** on top of JavaScript, making it ideal for scalable applications.

---
