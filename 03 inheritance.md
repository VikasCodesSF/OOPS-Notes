# 🧬 OOP  — Inheritance


## 🔷 What is Inheritance?

Inheritance allows a class (subclass/child) to **acquire the properties and behaviors** of another class (superclass/parent) using the `extends` keyword.

```java
class SubclassName extends SuperclassName {
    // body
}
```

### Benefits:
- **Code Reusability** — Don't repeat common logic
- **Method Overriding** — Customize inherited behavior
- **Polymorphism** — One interface, many implementations

---

## 🔷 Types of Inheritance in Java

| Type | Supported? | How |
|------|-----------|-----|
| Single | ✅ Yes | `class B extends A` |
| Multilevel | ✅ Yes | `class C extends B extends A` |
| Hierarchical | ✅ Yes | Multiple classes extend one class |
| Multiple | ❌ No (for classes) | Use interfaces instead |
| Hybrid | ❌ No (for classes) | Use interfaces instead |

> Java does **not** support multiple class inheritance to avoid the **Diamond Problem**.

---

## 🔷 Key Rules of Inheritance

1. A subclass includes **all members** of its superclass.
2. `private` members of the superclass are **NOT accessible** in the subclass (inherited but hidden).
3. A class **cannot be its own superclass**.
4. Only **one superclass** per subclass (single inheritance).

---

## 🔷 Superclass Reference → Subclass Object

```java
Box plainBox = new BoxWeight();  // Superclass ref = subclass object
```

- Only members defined in the **superclass type** are accessible through this reference.
- The actual behavior at runtime is determined by the **actual object type** (polymorphism).

```java
// Only Box methods/fields accessible via plainBox
// Even though it points to a BoxWeight object
```

---

## 🔷 The `super` Keyword

### Use 1: Call Superclass Constructor

```java
class BoxWeight extends Box {
    double weight;

    BoxWeight(double w, double h, double d, double m) {
        super(w, h, d);  // Call Box constructor — MUST be first statement
        weight = m;
    }
}
```

- `super()` must be the **first statement** in the constructor.
- Always refers to the **immediately above superclass**.
- If not specified, Java auto-calls the **default (no-arg)** super constructor.

### Use 2: Access Superclass Members (Hidden by Subclass)

```java
class A {
    int x = 10;
}
class B extends A {
    int x = 20;

    void show() {
        System.out.println(super.x);  // 10 — superclass x
        System.out.println(x);        // 20 — subclass x
    }
}
```

---

## 🔷 Multi-level Inheritance

```java
class A {}
class B extends A {}
class C extends B {}
```

- `super()` in C → calls B constructor
- `super()` in B → calls A constructor
- Constructors execute **top-down**: A → B → C

> If a superclass requires parameters, **all subclasses must pass them up the line**.

---

## 🔷 Pass Object to Superclass Constructor

```java
class Box {
    Box(Box ob) {
        width = ob.width;
        height = ob.height;
    }
}

class BoxWeight extends Box {
    BoxWeight(BoxWeight ob) {
        super(ob);          // Passing BoxWeight as Box — works!
        weight = ob.weight;
    }
}
```

> A superclass variable can reference any object derived from it — so `BoxWeight` can be passed where `Box` is expected.

---

## 🔷 `final` with Inheritance

### Final Method (Prevent Overriding):
```java
class Parent {
    final void display() { }  // Cannot be overridden
}
```
- Enables **early binding** (resolved at compile time → faster).

### Final Class (Prevent Inheritance):
```java
final class MyClass { }  // Cannot be extended
```
- All methods become implicitly `final`.
- Examples: `String`, `Math`, `Integer`.
- **Cannot** be both `abstract` and `final`.

---

## 🔷 Polymorphism Does NOT Apply to Instance Variables

```java
class A { int x = 10; }
class B extends A { int x = 20; }

A obj = new B();
System.out.println(obj.x);  // 10 — compile-time resolution (type of reference)
```

- **Methods** → resolved at **runtime** (dynamic dispatch)
- **Variables** → resolved at **compile time** (type of reference variable)

---

## 🔷 Static Methods and Inheritance

- Static methods **can be inherited** but **cannot be overridden** (they are hidden, not overridden).
- Static methods are resolved at **compile time** (early binding), not runtime.
- **Static interface methods** are NOT inherited by implementing classes or sub-interfaces.

```java
class Parent {
    static void greet() { System.out.println("Parent greet"); }
}
class Child extends Parent {
    static void greet() { System.out.println("Child greet"); }  // HIDES, not overrides
}

Parent obj = new Child();
obj.greet();  // "Parent greet" — resolved by reference type at compile time
```

---

## 🔷 `instanceof` Operator

```java
if (obj instanceof BoxWeight) {
    BoxWeight bw = (BoxWeight) obj;
}
```
- Checks if object is an instance of a specific class or its subclasses.
- Returns `false` if `obj` is `null`.
- **Pattern matching** (Java 16+):

```java
if (obj instanceof BoxWeight bw) {
    System.out.println(bw.weight);  // No cast needed
}
```

---

## 🔷 Object Class — Root of All Classes

- Every class in Java **implicitly extends** `java.lang.Object`.
- Common methods inherited from `Object`:

| Method | Purpose |
|--------|---------|
| `toString()` | String representation |
| `equals(Object obj)` | Logical equality |
| `hashCode()` | Hash value |
| `getClass()` | Runtime class info |
| `clone()` | Shallow copy |
| `finalize()` | Pre-GC cleanup (deprecated) |

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| `extends` | Keyword to inherit |
| Single inheritance | Only ONE superclass per class |
| `super()` | Call superclass constructor — must be first statement |
| `super.member` | Access superclass member hidden by subclass |
| `final` method | Prevents overriding; early binding |
| `final` class | Prevents inheritance |
| Polymorphism + variables | Does NOT apply — variables use compile-time type |
| Static methods | Inherited but NOT overridden (hidden) |
