# 🧩 Index Signatures in TypeScript

## 💡 What Are Index Signatures?

An **index signature** allows you to define the **type of keys and their corresponding values** when you don’t know all the property names in advance.

It’s like saying:  
> “This object can have any number of properties, as long as the keys and values are of specific types.”

---

## 🧱 Syntax

```ts
type Example = {
  [key: string]: number;
};
```

- `key` → the name used for each property key (any valid identifier)  
- `string` → the type of key  
- `number` → the type of value for each key  

---

## 🎯 Example 1 — Dynamic Object Keys

```ts
interface Scores {
  [subject: string]: number;
}

const marks: Scores = {
  math: 90,
  english: 85,
  science: 92,
  // history: "A" ❌ Error — must be a number
};
```

✅ You can add any property as long as its key is a `string` and its value is a `number`.

---

## 🎯 Example 2 — Mixed Known + Dynamic Keys

```ts
interface User {
  id: number;
  name: string;
  [extraProp: string]: string | number;
}

const user: User = {
  id: 1,
  name: "Vikram",
  city: "Delhi",   // OK
  age: 30,         // OK
};
```

Here, `id` and `name` are known properties,  
but `city` and `age` are allowed due to the index signature.

---

## ⚠️ Things to Watch Out For

1. **All property types must be compatible** with the index signature’s value type.  
   → If `[key: string]: number`, then every property must be a number or subtype of number.  
2. **Key types can only be `string`, `number`, or `symbol`.**

---

## ✅ When to Use

- When your object has **dynamic keys** (e.g., dictionaries, maps).  
- When building **APIs or configs** where property names aren’t known ahead of time.

---

**In short:**  
> Use index signatures when you want to describe objects with flexible or dynamic property names but consistent value types.
