# 🧩 Abstract Classes and Methods in TypeScript

## 💡 What Is “Abstract” in TypeScript?

The keyword **`abstract`** is used to define:
- **Abstract classes** → Classes that **can’t be instantiated** directly.  
- **Abstract methods** → Methods that **must be implemented** by child classes.

They act as **blueprints** for other classes — defining a structure but leaving implementation details for subclasses.

---

## 🏗️ Example — Abstract Class

```ts
abstract class Animal {
  abstract makeSound(): void; // 👈 must be implemented by subclass

  move(): void {
    console.log("The animal moves");
  }
}

// ❌ Error: cannot create an instance of an abstract class
// const a = new Animal();

class Dog extends Animal {
  makeSound(): void {
    console.log("Woof!");
  }
}

const dog = new Dog();
dog.makeSound(); // Woof!
dog.move();      // The animal moves
```

### 🔍 What’s Happening:
- `Animal` is **abstract** — it provides a **general idea** of what animals can do.
- It defines `makeSound()` as **abstract**, meaning every subclass **must** define it.
- You can’t create `new Animal()`, but you can extend it.

---

## 🧱 Why Abstract Classes Are Useful
They are useful when you want:
1. **Shared logic** between classes (like `move()` above).  
2. **Certain methods to be mandatory** for all subclasses (like `makeSound()`).  
3. To enforce a **consistent design** for derived classes.

---

## ⚙️ Example — Real-World Scenario

```ts
abstract class PaymentProcessor {
  abstract process(amount: number): void;

  validate(amount: number) {
    if (amount <= 0) throw new Error("Invalid amount");
  }
}

class CreditCardPayment extends PaymentProcessor {
  process(amount: number): void {
    this.validate(amount);
    console.log(`Processing ₹${amount} via Credit Card`);
  }
}

class UpiPayment extends PaymentProcessor {
  process(amount: number): void {
    this.validate(amount);
    console.log(`Processing ₹${amount} via UPI`);
  }
}

const upi = new UpiPayment();
upi.process(500);
```

### ✅ Benefits:
- You ensure **every payment method** has a `process()` method.
- Shared logic (like `validate()`) stays in the base class.
- Prevents misuse (you can’t create a `PaymentProcessor` directly).

---

## 🧠 In Short

| Concept | Meaning | Can Be Instantiated? | Must Be Implemented? |
|----------|----------|----------------------|----------------------|
| **Abstract Class** | Blueprint for other classes | ❌ No | Only if abstract methods exist |
| **Abstract Method** | Declared but not implemented | — | ✅ Yes, by subclasses |

---

**In summary:**  
Use **abstract classes** when you want to define a **base structure** for other classes to follow — enforcing some methods and optionally sharing common logic.
