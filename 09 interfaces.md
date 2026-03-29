# 🔌 OOP — Interfaces

## 🔷 What is an Interface?

An interface is a **fully abstract contract** that specifies **what a class must do**, not **how it does it**.

```java
interface Drawable {
    void draw();              // Implicitly public abstract
    int MAX_SIZE = 100;       // Implicitly public static final
}
```

---

## 🔷 Why Interfaces? (Problem Solved)

Java doesn't support multiple class inheritance due to the **Diamond Problem**:

```
If class A and class B both define method X differently,
and class C extends both — which version of X does C use?
```

**Interfaces solve this** — multiple interfaces can be implemented, and the class provides the single concrete implementation.

---

## 🔷 Interface Syntax

```java
interface InterfaceName {
    // All variables: public static final (implicit)
    int CONSTANT = 42;

    // All methods: public abstract (implicit)
    void abstractMethod();

    // Default method (Java 8+)
    default void defaultMethod() {
        System.out.println("Default implementation");
    }

    // Static method (Java 8+)
    static void staticMethod() {
        System.out.println("Interface static method");
    }

    // Private method (Java 9+)
    private void helperMethod() { }
}
```

---

## 🔷 Implementing an Interface

```java
class Circle implements Drawable {
    @Override
    public void draw() {           // Must be PUBLIC when implementing
        System.out.println("Drawing circle");
    }
}
```

> **Rule:** Implementing methods **must be declared `public`** — cannot be less permissive than the interface definition.

---

## 🔷 Multiple Interface Implementation

```java
interface Flyable {
    void fly();
}
interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly()  { System.out.println("Duck flying"); }
    @Override
    public void swim() { System.out.println("Duck swimming"); }
}
```

---

## 🔷 Interface Variables

```java
interface Config {
    int MAX_USERS = 100;   // Implicitly: public static final int MAX_USERS = 100
}
```

- Always `public static final` — cannot be changed.
- **Must be initialized** at declaration.
- Accessed as `Config.MAX_USERS` or directly by name in implementing class.

---

## 🔷 Interface as Reference (Polymorphism)

```java
Drawable d = new Circle();    // Interface reference → implementing class object
d.draw();                     // Resolved at RUNTIME based on actual object
```

- Dynamic method lookup at runtime.
- The calling code doesn't need to know the actual implementing class.
- Enables **loose coupling**.

> ⚠️ **Performance Note:** Dynamic lookup via interface has slightly more overhead than direct method calls. Avoid in performance-critical inner loops.

---

## 🔷 Default Methods (Java 8+)

Allows adding new methods to interfaces **without breaking existing implementations**.

```java
interface Vehicle {
    void start();

    default void fuelUp() {
        System.out.println("Fueling up...");  // Default implementation
    }
}

class Car implements Vehicle {
    @Override
    public void start() { System.out.println("Car starting"); }
    // fuelUp() inherited from interface — no need to override
}
```

### Default Method Conflict Resolution:

| Scenario | Resolution |
|----------|-----------|
| Class implements two interfaces with same default method | **Class must override** — otherwise COMPILE ERROR |
| Class provides its own implementation | **Class takes priority** over interface default |
| Interface A extends Interface B, both define default | **Inheriting interface (A) takes priority** |

```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }

class C implements A, B {
    @Override
    public void show() {
        A.super.show();   // Explicitly call A's default
    }
}
```

---

## 🔷 Static Interface Methods (Java 8+)

```java
interface MathOps {
    static int square(int n) { return n * n; }
}

MathOps.square(5);     // ✅ Called via interface name
// new MyClass().square();  ❌ Cannot call on object
```

> **Key:** Static interface methods are **NOT inherited** by implementing classes or sub-interfaces. They must have a body.

---

## 🔷 Private Interface Methods (Java 9+)

```java
interface Logger {
    default void logInfo(String msg)  { log("INFO", msg); }
    default void logError(String msg) { log("ERROR", msg); }

    private void log(String level, String msg) {   // Shared helper
        System.out.println("[" + level + "] " + msg);
    }
}
```
- Reduces code duplication in default/static methods.
- Not accessible to implementing classes.

---

## 🔷 Extending Interfaces

```java
interface A {
    void methodA();
}
interface B extends A {
    void methodB();
}

class C implements B {
    @Override
    public void methodA() { }  // Must implement both
    @Override
    public void methodB() { }
}
```

- Interfaces can **only extend** other interfaces (not classes).
- An interface can extend **multiple** interfaces.

---

## 🔷 Nested Interfaces

```java
class A {
    public interface NestedIF {
        boolean isNotNegative(int x);
    }
}

class B implements A.NestedIF {
    @Override
    public boolean isNotNegative(int x) {
        return x >= 0;
    }
}

A.NestedIF nif = new B();
nif.isNotNegative(10);   // ✅
```

- A nested interface can be `public`, `private`, or `protected`.
- Top-level interfaces can only be `public` or *default*.

---

## 🔷 Functional Interfaces (Java 8+)

An interface with **exactly one abstract method** is a **functional interface** — usable with lambdas.

```java
@FunctionalInterface
interface Greeting {
    void greet(String name);
}

Greeting g = name -> System.out.println("Hello, " + name);
g.greet("Raj");   // Hello, Raj
```

Common built-in functional interfaces in `java.util.function`:
| Interface | Method | Use |
|-----------|--------|-----|
| `Runnable` | `run()` | No args, no return |
| `Callable<T>` | `call()` | No args, returns T |
| `Predicate<T>` | `test(T)` | Takes T, returns boolean |
| `Function<T,R>` | `apply(T)` | Takes T, returns R |
| `Consumer<T>` | `accept(T)` | Takes T, no return |
| `Supplier<T>` | `get()` | No args, returns T |

---

## 🔷 Marker Interfaces

An interface with **no methods** — used to "tag" classes.

```java
interface Serializable { }   // java.io.Serializable — marks class as serializable
interface Cloneable { }      // java.lang.Cloneable — marks class as cloneable
```

---

## 🔷 Abstract Class vs Interface — Decision Chart

```
Does the class need to share IMPLEMENTATION (code/state)?
  → YES → Abstract Class

Does the class need MULTIPLE inheritance?
  → YES → Interface

Is it a CAPABILITY that unrelated classes can share?
  → YES → Interface (e.g., Serializable, Comparable)

Is it a strong IS-A relationship with shared base?
  → YES → Abstract Class
```

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| Interface variables | Implicitly `public static final` |
| Interface methods | Implicitly `public abstract` |
| Implementing methods | Must be `public` |
| Default methods | Java 8+; class overrides take priority |
| Static interface methods | NOT inherited; must have body |
| Multiple interfaces | A class can implement many |
| Interface reference | Enables runtime polymorphism |
| Functional interface | Exactly one abstract method; used with lambdas |
