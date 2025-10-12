# Understanding `extends` and `implements` in TypeScript

In TypeScript, **`extends`** and **`implements`** are both used for *inheritance* and *type contracts*, but they serve different purposes depending on what you’re working with — **classes**, **interfaces**, or **types**.

---

## 🧩 1. `extends`

`extends` is used for **inheritance** — it means *“derive from”* or *“build upon”* another class or interface.

---

### ✅ When used with **classes**

It means one class inherits properties and methods from another class.

```ts
class Animal {
  move() {
    console.log("Moving...");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof!");
  }
}

const dog = new Dog();
dog.move(); // ✅ from Animal
dog.bark(); // ✅ from Dog
```

➡️ Here:
- `Dog` **inherits** from `Animal`
- It can use everything from `Animal` (public/protected members)

---

### ✅ When used with **interfaces**

It means one interface is **extending** another interface — i.e., combining their definitions.

```ts
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
}

const emp: Employee = {
  name: "Vikram",
  employeeId: 101
};
```

➡️ Here:
- `Employee` includes everything from `Person` plus its own members.

---

### ✅ You can also extend multiple interfaces

```ts
interface A { a: string; }
interface B { b: number; }

interface C extends A, B {
  c: boolean;
}

const obj: C = { a: "hi", b: 10, c: true };
```

---

## ⚙️ 2. `implements`

`implements` is used when a **class promises to follow the structure** of an interface (or type).  
It’s a *contract enforcement*, not inheritance.

---

### ✅ Example

```ts
interface Greetable {
  name: string;
  greet(): void;
}

class Person implements Greetable {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}
```

➡️ Here:
- `Person` must **define all properties and methods** from `Greetable`.
- But it does **not** inherit any actual code — only enforces structure.

---

### 🚫 Note

You can’t use `implements` to inherit functionality.  
You must explicitly write out the methods.

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log("Log:", message);
  }
}
```

---

## ⚖️ Summary Table

| Feature | `extends` | `implements` |
|----------|------------|--------------|
| Used with | Classes, Interfaces | Classes only |
| Purpose | Inherit from base class or interface | Enforce structure from interface/type |
| Code Reuse | ✅ Yes (when used with classes) | ❌ No |
| Multiple inheritance | ❌ (only one class) / ✅ (multiple interfaces) | ✅ (multiple interfaces) |
| Example | `class Dog extends Animal {}` | `class Dog implements Pet {}` |

---

### 🧠 Pro Tip: Combine Both!

You can mix both `extends` and `implements` together.

```ts
interface CanRun {
  run(): void;
}

class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Dog extends Animal implements CanRun {
  run() {
    console.log("Running...");
  }
}
```

➡️ Here `Dog` inherits **implementation** from `Animal` and **structure contract** from `CanRun`.

---
