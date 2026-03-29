# ⚡ OOP — The `static` Keyword


## 🔷 What is `static`?

When a member is declared `static`, it **belongs to the class** — not to any specific object.

- Can be accessed **before any object is created**
- Does not require an object reference
- Shared across **all instances** of the class

```java
class Counter {
    static int count = 0;  // Shared by all instances

    Counter() { count++; }
}
Counter a = new Counter();
Counter b = new Counter();
System.out.println(Counter.count);  // 2
```

---

## 🔷 Static Variables (Class Variables)

```java
class Human {
    static String species = "Homo Sapiens";  // Shared by all
    String name;                              // Unique per instance
}
```

- One copy per class, shared by all objects.
- Accessed via `ClassName.variable` (preferred) or object reference.
- Stored in the **Method Area** of JVM memory.

---

## 🔷 Static Methods

```java
class MathUtils {
    static int square(int n) { return n * n; }
}
MathUtils.square(5);  // Called without creating an object
```

### Rules for Static Methods:
| Rule | Details |
|------|---------|
| Access | Can access **only static** data |
| Non-static data | Cannot access instance variables directly |
| `this` / `super` | Cannot use these keywords |
| Overriding | Cannot be overridden (can be hidden) |
| Called via | ClassName or object reference |

### Why `main()` is static:
```java
public static void main(String[] args) { }
```
- Called before any object of the class exists.
- JVM needs to call it without instantiating the class.

---

## 🔷 Accessing Non-Static from Static Context

You **cannot** directly access instance variables from a static method, but you **can** by explicitly passing an object reference:

```java
class Human {
    String message = "Hello";

    public static void display(Human human) {
        System.out.println(human.message);  // ✅ via object reference
    }

    public static void main(String[] args) {
        Human kunal = new Human();
        kunal.message = "Kunal's message";
        Human.display(kunal);
    }
}
```

---

## 🔷 Static Block (Static Initializer)

Used to perform **complex initialization** of static variables. Runs **exactly once** when the class is first loaded.

```java
class UseStatic {
    static int a = 3;
    static int b;

    static {
        System.out.println("Static block executed");
        b = a * 4;  // b = 12
    }

    static void meth(int x) {
        System.out.println("x=" + x + ", a=" + a + ", b=" + b);
    }

    public static void main(String[] args) {
        meth(42);
    }
}

// Output:
// Static block executed
// x=42, a=3, b=12
```

### Execution Order:
1. Static variables initialized (in declaration order)
2. Static blocks executed (in order of appearance)
3. `main()` called

---

## 🔷 Static Nested Classes

```java
public class Static {
    static class Test {       // ✅ Only nested classes can be static
        String name;

        Test(String name) { this.name = name; }
    }

    public static void main(String[] args) {
        Test a = new Test("Kunal");
        Test b = new Test("Rahul");
        System.out.println(a.name);  // Kunal
        System.out.println(b.name);  // Rahul
    }
}
```

> **Only nested classes can be static.** Top-level classes cannot be `static`.

### Static Nested vs Inner Class:

| Feature | Static Nested Class | Inner Class (Non-static) |
|---------|--------------------|-----------------------|
| Outer class instance needed? | ❌ No | ✅ Yes |
| Can have static members? | ✅ Yes | ❌ No |
| Accesses outer class members? | Only static | All (including private) |

---

## 🔷 Static and Inheritance

- Static methods **can be inherited** but **cannot be overridden**.
- They can be **hidden** — if subclass defines same static method, it hides the parent's.

```java
class Parent {
    static void greet() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void greet() { System.out.println("Child"); }  // HIDES, not overrides
}

Parent obj = new Child();
obj.greet();   // "Parent" — resolved at compile time by reference type
```

> **Static Interface Methods** are NOT inherited by implementing classes or sub-interfaces.

---

## 🔷 Static Import

```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

double area = PI * r * r;
double val = sqrt(25);  // No need to write Math.sqrt
```

---

## 🔷 Common Mistakes

```java
class Test {
    int x = 10;

    static void show() {
        System.out.println(x);   // ❌ ERROR — cannot access instance var from static context
        System.out.println(this); // ❌ ERROR — 'this' not available in static context
    }
}
```

---

## 🔷 Memory Layout Summary

| Member Type | Stored In | One Per |
|------------|-----------|---------|
| Instance variable | Heap (with object) | Object |
| Static variable | Method Area | Class |
| Local variable | Stack | Method call |
| Static method | Method Area | Class |

---

## ✅ Key Takeaways

| Concept | Rule |
|---------|------|
| Static variable | Shared across all instances; class-level |
| Static method | Belongs to class; no `this`/`super`; only accesses static members |
| Static block | Runs once when class loads; for complex static initialization |
| Static nested class | No outer class instance needed; can have static members |
| Static + inheritance | Can be inherited but not overridden (hidden) |
| `main()` is static | Must be callable before any object exists |
