# 🔁 OOP — Method Overloading & Overriding

## 🔷 Method Overloading (Compile-Time Polymorphism)

**Definition:** Two or more methods in the **same class** with the **same name** but **different parameter lists**.

```java
class Calculator {
    int add(int a, int b)          { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c)   { return a + b + c; }
}
```

### What CAN differentiate overloaded methods:
| Factor | Example |
|--------|---------|
| Number of parameters | `test(int)` vs `test(int, int)` |
| Type of parameters | `test(int)` vs `test(double)` |
| Order of parameters | `test(int, double)` vs `test(double, int)` |

### What CANNOT differentiate overloaded methods:
- **Return type alone** — Java cannot distinguish overloaded methods by return type.

```java
int test(int a) { }
double test(int a) { }  // ❌ COMPILE ERROR — same parameter, different return type
```

---

## 🔷 Automatic Type Promotion in Overloading

If an exact match is not found, Java tries **automatic type widening**:

```java
class OverloadDemo {
    void test(double a) {
        System.out.println("test(double): " + a);
    }
}

OverloadDemo ob = new OverloadDemo();
int i = 88;
ob.test(i);      // int → auto-promoted to double ✅
ob.test(123.2);  // exact match ✅
```

> Type promotion order: `byte → short → int → long → float → double`

---

## 🔷 Returning Objects from Methods

```java
class Test {
    int a;

    Test(int i) { a = i; }

    Test incrByTen() {
        return new Test(a + 10);  // Returns a new object
    }
}

Test ob1 = new Test(2);
Test ob2 = ob1.incrByTen();
// ob1.a = 2, ob2.a = 12
```

- Each call to `incrByTen()` creates a **new object** on the heap.
- The object lives as long as a reference to it exists.
- Garbage collected when no references remain.

---

## 🔷 Constructor Overloading

```java
class Box {
    double w, h, d;

    Box()                            { w = h = d = 0; }
    Box(double side)                 { w = h = d = side; }
    Box(double w, double h, double d){ this.w = w; this.h = h; this.d = d; }
    Box(Box ob)                      { w = ob.w; h = ob.h; d = ob.d; }
}
```

---

## 🔷 Method Overriding (Runtime Polymorphism)

**Definition:** A subclass provides a **specific implementation** of a method already defined in its superclass, with the **same name and type signature**.

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Woof!"); }  // Overrides Animal's version
}
```

### Rules for Overriding:
| Rule | Details |
|------|---------|
| Same name & signature | Must match exactly |
| IS-A relationship | Subclass must extend the superclass |
| Access modifier | Same or **wider** (e.g., `protected` → `public`) |
| Return type | Same or **covariant** (subtype of original) |
| Exceptions | Can throw fewer/narrower checked exceptions |

---

## 🔷 Covariant Return Types

If `B extends A`, a method in subclass can return `B` even if the superclass method returns `A`.

```java
class A {
    A getInstance() { return new A(); }
}
class B extends A {
    @Override
    B getInstance() { return new B(); }  // Covariant return type ✅
}
```

---

## 🔷 Dynamic Method Dispatch (Runtime Polymorphism)

The mechanism by which an **overridden method call is resolved at runtime**.

```java
Animal a = new Dog();   // Superclass reference → subclass object
a.makeSound();          // Calls Dog's makeSound() — resolved at RUNTIME
```

> It is the **type of the actual object** (not the reference variable type) that determines which version executes.

### How it works:
1. At compile time → Java checks if method exists in the reference type (`Animal`).
2. At runtime → JVM looks at the **actual object type** (`Dog`) and calls its version.

---

## 🔷 Overloading vs Overriding — Side-by-Side

| Feature | Overloading | Overriding |
|---------|------------|-----------|
| Also called | Compile-time polymorphism | Runtime polymorphism |
| Location | Same class | Parent & Child class |
| Method signature | Must differ | Must be same |
| Return type | Can differ | Same or covariant |
| `static` methods | Can be overloaded | Cannot be overridden (hidden) |
| `private` methods | Can be overloaded | Cannot be overridden |
| `final` methods | Can be overloaded | Cannot be overridden |
| Resolution | Compile time | Runtime |

---

## 🔷 `@Override` Annotation

```java
@Override
void makeSound() { ... }
```

- Not mandatory, but **strongly recommended**.
- Tells the compiler to verify this method actually overrides something.
- Catches typos in method names at compile time.

---

## 🔷 Cannot Override:
- `private` methods (not visible to subclass)
- `static` methods (they are hidden, not overridden)
- `final` methods
- Constructors (they are not inherited)

---

## 🔷 Access Modifier Rule in Overriding

```
Overriding access modifier must be SAME or MORE permissive:
private < default < protected < public
```

```java
class Parent {
    protected void show() {}
}
class Child extends Parent {
    public void show() {}    // ✅ More permissive
    // private void show() {}  ❌ Less permissive — COMPILE ERROR
}
```

---

## 🔷 Calling Super's Overridden Method

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override
    void makeSound() {
        super.makeSound();           // Call parent's version
        System.out.println("Woof!");
    }
}
```

---

## ✅ Key Takeaways

| Concept | Key Rule |
|---------|----------|
| Overloading | Same name, different parameters, same class |
| Return type alone | NOT enough to overload |
| Auto type promotion | Java promotes type if exact match not found |
| Overriding | Same name + signature, subclass overrides superclass |
| Dynamic dispatch | Actual object type determines method at runtime |
| `@Override` | Always use it — catches mistakes at compile time |
| Access modifier | Must be same or wider when overriding |
| Covariant return | Can return subtype in overriding method |
