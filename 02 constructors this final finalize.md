# 🏗️ OOP — Constructors, `this`, `final` & `finalize`

## 🔷 Constructors

A **constructor** is a special method that is automatically called when an object is created.

### Key Rules:
- **Same name** as the class
- **No return type** (not even `void`) — implicit return type is the class itself
- Called automatically when `new` is used
- If no constructor is defined, Java provides a **default no-arg constructor**

```java
class Box {
    double width, height, depth;

    Box(double w, double h, double d) {  // Parameterized constructor
        width = w;
        height = h;
        depth = d;
    }
}

Box b = new Box(10, 20, 30);  // Constructor called here
```

---

## 🔷 Types of Constructors

| Type | Description |
|------|-------------|
| **Default** | No parameters. Auto-provided by Java if none defined |
| **Parameterized** | Accepts arguments to initialize fields |
| **Copy Constructor** | Takes an object of the same class as parameter |

### Copy Constructor Example:
```java
class Box {
    double width, height;

    Box(Box ob) {          // Copy constructor
        width = ob.width;
        height = ob.height;
    }
}
```

---

## 🔷 Constructor Chaining with `this()`

- `this()` calls another constructor in the **same class**.
- Must be the **first statement** in the constructor.

```java
class Box {
    double width, height;

    Box() {
        this(10, 20);   // Calls the parameterized constructor
    }

    Box(double w, double h) {
        width = w;
        height = h;
    }
}
```

---

## 🔷 Constructors and Inheritance

- When a subclass object is created, the **superclass constructor is called first**.
- Java auto-inserts `super()` (no-arg) at the start of derived class constructor.
- If the superclass has **only parameterized** constructors, you must explicitly call `super(args)`.

```java
class Base {
    Base() { System.out.println("Base Constructor"); }
}

class Derived extends Base {
    Derived() {
        // super() is implicitly called here
        System.out.println("Derived Constructor");
    }
}

// Output when: new Derived()
// Base Constructor
// Derived Constructor
```

> **Rule:** Constructors complete in **order of derivation** — base class first, then derived.

---

## 🔷 The `this` Keyword

`this` is a reference to the **current object** (the object that invoked the method).

### Use Case 1 — Resolve Name Conflicts:
```java
class Box {
    double width;

    Box(double width) {
        this.width = width;  // this.width = field, width = parameter
    }
}
```

### Use Case 2 — Pass Current Object:
```java
class Chain {
    Chain doSomething() {
        return this;  // returns current object for method chaining
    }
}
```

### Use Case 3 — Constructor Chaining (`this()`):
```java
Box() { this(0, 0, 0); }
```

---

## 🔷 The `final` Keyword

### 1. Final Variable (Constant)
```java
final int MAX_SIZE = 100;  // Cannot be reassigned
```
- Convention: use ALL_CAPS for final constants.
- `final` on a **primitive** → value cannot change.
- `final` on a **reference** → the reference cannot change (still points to same object), but the **object's contents** can change.

```java
final int[] arr = {1, 2, 3};
arr[0] = 99;   // ✅ Allowed — contents changed
arr = new int[5];  // ❌ NOT allowed — reference changed
```

### 2. Final Method
```java
class Parent {
    final void show() { System.out.println("Cannot override"); }
}
```
- Cannot be **overridden** by subclasses.
- Enables **early binding** (compile-time resolution) → performance optimization.
- Compiler may **inline** the call, eliminating method call overhead.

### 3. Final Class
```java
final class Immutable { }  // Cannot be extended/inherited
```
- All methods of a `final` class are implicitly `final`.
- Example: `String`, `Integer`, `Math` in Java are `final` classes.
- A class **cannot be both `abstract` and `final`**.

---

## 🔷 The `finalize()` Method (Legacy — Deprecated in Java 9+)

```java
protected void finalize() {
    // Cleanup resources before GC reclaims the object
}
```

- Called by **Garbage Collector** just before reclaiming the object.
- **Not recommended** in modern Java — execution not guaranteed.

> **Modern Alternative:** Use `try-with-resources` and implement `AutoCloseable`:

```java
class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("Resource released");
    }
}

try (MyResource r = new MyResource()) {
    // use resource
}  // close() auto-called here
```

---

## 🔷 Blank Final Variables

```java
class Demo {
    final int VALUE;  // Blank final

    Demo(int v) {
        VALUE = v;   // Must be initialized in constructor
    }
}
```

- A blank final **instance variable** must be assigned in every constructor.
- A blank final **static variable** must be assigned in a static block.

---

## 🔷 static final (Constants)

```java
static final double PI = 3.14159;
```
- Shared across all instances (static).
- Value cannot change (final).
- Accessed as `ClassName.PI`.

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| Constructor | No return type, auto-called with `new`, name = class name |
| Default Constructor | Auto-provided only if NO constructor is defined |
| `this` | Reference to current object; `this()` for constructor chaining |
| `final` variable | Value/reference cannot be reassigned after initialization |
| `final` method | Cannot be overridden; allows compile-time (early) binding |
| `final` class | Cannot be extended (e.g., `String`) |
| `finalize()` | Deprecated; GC called it before reclaiming object |
