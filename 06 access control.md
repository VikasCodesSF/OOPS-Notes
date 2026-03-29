# 🔐 OOP — Access Control & Modifiers

## 🔷 Why Access Control?

- To **restrict access** to data members — only allow access via methods (encapsulation).
- To define methods that are **private to a class**.
- To control what is **visible outside** a package.

---

## 🔷 The Four Access Levels

| Modifier | Class | Package | Subclass (same pkg) | Subclass (diff pkg) | World |
|----------|:-----:|:-------:|:-------------------:|:-------------------:|:-----:|
| `public` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ✅ | ❌ |
| *(default/package-private)* | ✅ | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |

> **Default** = no modifier keyword. Accessible within the same package only.

---

## 🔷 `public`

```java
public class MyClass {
    public int x = 10;
    public void show() { }
}
```
- Accessible from **anywhere** — any class, any package.
- Class-level `public` means the file must be named `MyClass.java`.

---

## 🔷 `private`

```java
class BankAccount {
    private double balance;   // Accessible only within BankAccount

    public double getBalance() { return balance; }   // Getter
    public void setBalance(double b) { balance = b; } // Setter
}
```
- Most restrictive — accessible **only within the same class**.
- Subclasses **inherit** private members but **cannot access** them directly.
- Private members are accessible only through `public`/`protected` methods (getters/setters).

---

## 🔷 `protected`

```java
// packageOne/Base.java
package packageOne;
public class Base {
    protected void display() {
        System.out.println("Base display");
    }
}

// packageTwo/Derived.java
package packageTwo;
public class Derived extends packageOne.Base {
    public void show() {
        display();               // ✅ Allowed — called on current instance
        new Derived().display(); // ✅ Allowed — Derived object
        new Base().display();    // ❌ NOT allowed — Base object in different package
    }
}
```

### The `protected` Rule Explained:
- `protected` is accessible in:
  - Same class ✅
  - Same package ✅
  - Subclass (same package) ✅
  - **Subclass (different package) — ONLY through the subclass's own reference** ✅
- You **cannot** access `protected` members through a **superclass reference** from a different package.

> **Why?** If you access `new Base().display()` from `packageTwo`, you're bypassing the protection boundary. The rule ensures only the subclass can manipulate its own inherited protected members.

---

## 🔷 Default (Package-Private)

```java
class Helper {           // No modifier = package-private class
    void doWork() { }    // No modifier = package-private method
}
```
- Accessible only within the **same package**.
- Also called "package-private" or "package-level access".
- If no modifier is used on a **class**, it can only be used within its package.

---

## 🔷 Access Modifiers for Classes

| Level | Top-Level Class | Nested Class |
|-------|----------------|--------------|
| `public` | ✅ | ✅ |
| `protected` | ❌ | ✅ |
| *(default)* | ✅ | ✅ |
| `private` | ❌ | ✅ |

> Top-level classes can only be `public` or *default*. Nested classes can use all four.

---

## 🔷 Encapsulation Best Practice

```java
class Person {
    private String name;   // Private field
    private int age;

    // Public getter
    public String getName() { return name; }

    // Public setter with validation
    public void setAge(int age) {
        if (age > 0) this.age = age;
    }
}
```

### Benefits of Encapsulation:
- Hides implementation details
- Validates data before accepting it
- Makes code maintainable — change internals without affecting users

---

## 🔷 protected — Detailed Tricky Scenario

```java
class C {
    protected int member;
}

// In a different package:
class S extends C {
    void test(S obj, C obj2) {
        obj.member = 10;    // ✅ obj is of type S (subclass of C) — allowed
        obj2.member = 10;   // ❌ obj2 is of type C — not allowed from different package
    }
}
```

**Why this restriction?** If `obj2` were allowed, `S` could tamper with the internals of **any** `C` object (even another subclass `S2` of `C`). This would break `S2`'s ability to reason about its own state.

---

## 🔷 Access Modifier + Inheritance (Overriding Rule)

When overriding, the access modifier can be **same or more permissive**:

```
private < default < protected < public
```

```java
class Parent {
    protected void display() {}
}
class Child extends Parent {
    @Override
    public void display() {}    // ✅ More permissive
    // private void display() {}  ❌ More restrictive — COMPILE ERROR
}
```

---

## 🔷 Quick Memory Aid

```
PUBLIC   → Everywhere
PROTECTED → Package + Subclass (subclass ref only for diff package)
DEFAULT  → Package only
PRIVATE  → Class only
```

---

## ✅ Key Takeaways

| Modifier | Scope |
|----------|-------|
| `public` | Accessible everywhere |
| `protected` | Package + subclass (via subclass reference in different package) |
| *(default)* | Same package only |
| `private` | Same class only |
| Overriding rule | Access modifier must be **same or wider** |
| Encapsulation | Use `private` fields + `public` getters/setters |
