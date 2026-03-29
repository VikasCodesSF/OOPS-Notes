# 📦 OOP — Classes & Objects

## 🔷 What is a Class?

- A **class** is a **template / blueprint** for creating objects.
- A class creates a **new data type** that can be used to create objects.
- A class is a **logical construct** — it has no physical reality until an object is created.

```java
class Box {
    double width, height, depth;
}
```

---

## 🔷 What is an Object?

- An object is an **instance of a class**.
- An object has **physical reality** — it occupies space in memory.

### Three Essential Properties of an Object:
| Property | Description |
|----------|-------------|
| **State** | Value from its data type (instance variables) |
| **Identity** | Distinguishes one object from another (memory address) |
| **Behavior** | Effect of operations defined by methods |

---

## 🔷 Declaring & Instantiating Objects

```java
Box mybox;           // Step 1: Declare a reference variable
mybox = new Box();   // Step 2: Allocate memory and assign reference
```

- **`mybox`** → reference variable (holds memory address) — checked by **compiler**
- **`new Box()`** → actual object in heap memory — handled by **JVM**

> **Key Rule:** `Box b2 = b1;` does NOT copy the object — both `b1` and `b2` point to the **same object**. Changes via `b2` affect `b1`.

---

## 🔷 The `new` Keyword

```java
classname classVar = new classname();
```

- Dynamically allocates memory **at runtime** (on the heap).
- Returns a **reference** (not a raw pointer) to the allocated object.
- Java does **not** allow arbitrary pointer manipulation — ensures safety.

> **Why primitives don't use `new`?**  
> Primitives (`int`, `char`, `boolean`, etc.) are **NOT objects** in Java. They are stored directly on the stack for efficiency. Wrapper classes (`Integer`, `Character`, etc.) are their object counterparts.

---

## 🔷 Parameters vs Arguments

```java
int square(int i) { return i * i; }  // i is a PARAMETER
square(100);                          // 100 is an ARGUMENT
```

---

## 🔷 Dot Operator (`.`)

- Links the **object name** to its **instance variable or method**.
- Formally called a **separator** in the Java specification (not an operator).

```java
mybox.width = 10;
mybox.display();
```

---

## 🔷 Object Reference Assignment

```java
Box b1 = new Box();
Box b2 = b1;   // b2 points to SAME object as b1
```

- **No new memory** is allocated.
- Modifying through `b2` also changes what `b1` sees.
- To create a true copy, you need to implement `clone()` or a copy constructor.

---

## 🔷 Garbage Collection

- Java has automatic garbage collection.
- When no references point to an object, it becomes eligible for GC.
- The `finalize()` method (deprecated since Java 9) was called before GC reclaimed an object.

```java
protected void finalize() {
    // cleanup code (legacy - avoid in modern Java)
}
```

> **Modern Java:** Use `try-with-resources` and `AutoCloseable` instead of `finalize()`.

---

## 🔷 Class vs Object — Quick Comparison

| Aspect | Class | Object |
|--------|-------|--------|
| Nature | Blueprint / Template | Instance of class |
| Memory | No memory at declaration | Memory allocated at runtime |
| Existence | Logical | Physical |
| Creation | `class Box {}` | `new Box()` |

---

## 🔷 Missing Topics (Supplemented)

### Inner Classes
```java
class Outer {
    class Inner {  // non-static inner class
        void show() { System.out.println("Inner class"); }
    }
}
```
- Inner class has access to all members (including private) of the outer class.
- Cannot have static members unless it's a `static` nested class.

### Anonymous Classes
```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Anonymous class");
    }
};
```
- Used when you need a **one-time use** class.
- Commonly used with interfaces and abstract classes.

### Record Classes (Java 16+)
```java
record Point(int x, int y) {}  // Immutable data class
Point p = new Point(3, 4);
System.out.println(p.x());  // 3
```

---

## ✅ Key Takeaways

- Class = blueprint (logical), Object = instance (physical, in heap memory)
- `new` allocates memory at runtime and returns a reference
- Assigning one object reference to another copies the **reference**, not the object
- Java primitives are NOT objects — stored on stack for efficiency
- Dot operator links object to its members
