# 🧩 Type Guards vs Assertion Functions in TypeScript (Simplified Guide)

This guide explains **user-defined type guards** and **assertion functions** in simple, beginner-friendly terms with relatable examples.

---

## 🕵️‍♂️ 1. User-Defined Type Guards

### 🧠 What They Are
Type guards help **TypeScript figure out the type of something at runtime**, especially when you have *union types* (like `Dog | Cat`).

They tell TypeScript:
> “If this condition is true, treat this variable as type X.”

---

### ⚙️ Example

```ts
type Dog = { bark: () => void };
type Cat = { meow: () => void };

function makeSound(animal: Dog | Cat) {
  animal.bark(); // ❌ Error: not every animal can bark
}
```

TypeScript says: “Wait, `animal` could be a Cat — it might not have a `bark()` method!”

---

### ✅ Fix — Create a Type Guard

```ts
function isDog(animal: Dog | Cat): animal is Dog {
  return (animal as Dog).bark !== undefined;
}

function makeSound(animal: Dog | Cat) {
  if (isDog(animal)) {
    animal.bark(); // ✅ TypeScript knows it's a Dog here
  } else {
    animal.meow(); // ✅ TypeScript knows it's a Cat here
  }
}
```

---

### 💡 When to Use Type Guards
Use a **type guard** when:
- You have **union types** (e.g., `Dog | Cat`)
- You want to **check and handle types safely**
- You don’t need to throw an error

➡️ **They return `true` or `false` — they do not throw.**

---

## 🚨 2. Assertion Functions

### 🧠 What They Are
Assertion functions are like *strict bouncers* at a club.

They **throw an error** if something isn’t valid, and tell TypeScript:
> “If I didn’t throw an error, you can safely assume the value is correct.”

---

### ⚙️ Example

```ts
function printName(name?: string) {
  console.log(name.toUpperCase()); // ❌ Error: name could be undefined
}
```

---

### ✅ Fix — Create an Assertion Function

```ts
function assertIsDefined<T>(value: T): asserts value is NonNullable<T> {
  if (value === null || value === undefined) {
    throw new Error("Value must be defined");
  }
}

function printName(name?: string) {
  assertIsDefined(name);
  console.log(name.toUpperCase()); // ✅ Safe now
}
```

---

### 💡 When to Use Assertion Functions
Use an **assertion function** when:
- You want to **validate data** and **stop execution** if it’s invalid
- You need to **enforce** that a variable is defined or of a specific type
- You want both **runtime validation** + **type narrowing**

➡️ **They throw an error** if the condition fails.

---

## 🧭 Comparison Table

| Feature | User-Defined Type Guard 🕵️‍♂️ | Assertion Function 🚨 |
|----------|-------------------------------|------------------------|
| What it does | Checks type and returns `true`/`false` | Checks type and **throws error** if invalid |
| Used for | Branching logic safely | Enforcing correctness (stop execution) |
| Return type | `x is Type` | `asserts x is Type` |
| Throws error? | ❌ No | ✅ Yes |
| Common use | Distinguish between two possible types | Validate and ensure something is correct |

---

## 🧠 Think of it Like This

| Scenario | What to Use |
|-----------|--------------|
| “I want to check if this is a Dog or a Cat and handle both.” | **Type Guard** |
| “I want to make sure this *is definitely* a Dog — otherwise crash.” | **Assertion Function** |

---

## 🐶 Example Using Both

```ts
type Dog = { bark: () => void };
type Cat = { meow: () => void };

function isDog(animal: Dog | Cat): animal is Dog {
  return (animal as Dog).bark !== undefined;
}

function assertIsDog(animal: Dog | Cat): asserts animal is Dog {
  if (!isDog(animal)) throw new Error("Not a Dog!");
}

function play(animal: Dog | Cat) {
  if (isDog(animal)) {
    animal.bark(); // ✅ Safe branching
  }

  assertIsDog(animal); // Throws if not a Dog
  animal.bark(); // ✅ Definitely a Dog here
}
```

---

## 🪄 Summary in Plain English

| Concept | What it’s Like | What it Does |
|----------|----------------|---------------|
| **Type Guard** | Asking politely: “Are you a Dog?” | Returns true/false so TS can handle both |
| **Assertion Function** | Demanding: “You better be a Dog or I’ll throw an error!” | Stops execution if type doesn’t match |

---

### ✅ Quick Recap
- **Type Guard** → Helps TypeScript *decide* between types.
- **Assertion Function** → *Enforces* that a type is correct.

That’s the key difference!
