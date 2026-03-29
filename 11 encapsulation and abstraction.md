# 🔒 OOP — Encapsulation & Abstraction

## 🔷 The Four Pillars of OOP

| Pillar | Keyword | Core Idea |
|--------|---------|-----------|
| **Encapsulation** | `private` + getters/setters | Bundling data + behavior; hiding implementation |
| **Abstraction** | `abstract`, `interface` | Hiding complexity; showing only essentials |
| **Inheritance** | `extends` | Reusing code; IS-A relationship |
| **Polymorphism** | Overloading / Overriding | One interface, many forms |

---

## 🔷 Encapsulation

**Definition:** Bundling data (fields) and the methods that operate on that data into a single unit (class), and **restricting direct access** to the data.

```java
class BankAccount {
    private double balance;      // Data hidden from outside
    private String accountId;

    public BankAccount(String id, double initialBalance) {
        this.accountId = id;
        this.balance = initialBalance;
    }

    // Controlled access via methods
    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) balance += amount;   // Validation inside
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

### Why Encapsulation?
- **Data hiding** — Prevents external code from corrupting the state.
- **Validation** — Can reject invalid values in setters.
- **Maintainability** — Change internal implementation without affecting external code.
- **Flexibility** — Can make fields read-only (getter only) or write-only (setter only).

---

## 🔷 Encapsulation vs Abstraction — The Key Difference

| Aspect | Encapsulation | Abstraction |
|--------|--------------|-------------|
| **Focus** | HOW data is protected | WHAT information is shown |
| **Mechanism** | Access modifiers (`private`, `protected`) | `abstract` classes, `interface` |
| **Goal** | Hide implementation details | Hide complexity |
| **Example** | `private` field + getter/setter | `abstract void drive()` in `Vehicle` |
| **Level** | Implementation level | Design level |

> **Analogy:**
> - **Encapsulation** = A capsule that hides/wraps medicine inside
> - **Abstraction** = A TV remote — you press buttons without knowing how the signal is generated

---

## 🔷 Abstraction

**Definition:** Hiding internal complexity and exposing only the **essential features** of an object.

```java
abstract class Vehicle {
    abstract void start();    // What it does — not how
    abstract void stop();

    void refuel() {           // Common implementation
        System.out.println("Refueling...");
    }
}

class Car extends Vehicle {
    @Override
    void start() { System.out.println("Car engine started"); }
    @Override
    void stop()  { System.out.println("Car engine stopped"); }
}
```

### Abstraction via Interface:
```java
interface Payment {
    void processPayment(double amount);   // Contract — no "how"
}

class UPIPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing UPI payment: ₹" + amount);
    }
}

class CardPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing card payment: ₹" + amount);
    }
}
```

---

## 🔷 Abstraction vs Encapsulation — Visual

```
┌─────────────────────────────────────────────────────────┐
│                    ABSTRACTION                          │
│    Hides WHAT the complex logic is (at design level)    │
│                                                         │
│   interface Drive { void accelerate(); void brake(); }  │
│                    ↓ implemented by ↓                   │
│   ┌─────────────────────────────────────────────────┐   │
│   │               ENCAPSULATION                     │   │
│   │  Hides HOW the data is stored/managed           │   │
│   │                                                 │   │
│   │  class Car implements Drive {                   │   │
│   │      private int speed;  ← hidden               │   │
│   │      public void accelerate() { speed += 10; }  │   │
│   │  }                                              │   │
│   └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 🔷 Levels of Abstraction

| Level | Example |
|-------|---------|
| High-level (Interface) | `interface Database { void query(); }` |
| Mid-level (Abstract Class) | `abstract class RelationalDB implements Database { ... }` |
| Concrete (Implementation) | `class MySQL extends RelationalDB { ... }` |

---

## 🔷 Real-World Analogy Table

| Pillar | Real World | Java |
|--------|-----------|------|
| **Encapsulation** | ATM machine — internal workings hidden, only buttons exposed | `private` fields + public methods |
| **Abstraction** | Car steering wheel — you steer without knowing the mechanics | `interface` / `abstract class` |
| **Inheritance** | Child inherits traits from parents | `extends` |
| **Polymorphism** | A person can be a student, employee, and customer at once | Method overriding / overloading |

---

## 🔷 Data Hiding (Encapsulation in Practice)

```java
// BAD — No encapsulation
class Person {
    public String name;
    public int age;   // Anyone can set age = -5
}

// GOOD — Encapsulated
class Person {
    private String name;
    private int age;

    public void setAge(int age) {
        if (age > 0 && age < 150) this.age = age;  // Validated
    }
    public int getAge() { return age; }
}
```

---

## 🔷 Immutability (Strong Encapsulation)

```java
final class ImmutablePoint {
    private final int x;
    private final int y;

    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() { return x; }
    public int getY() { return y; }
    // No setters — immutable!
}
```

Java's `String` class is a classic example of an immutable, fully encapsulated class.

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| Encapsulation | Bundle data + methods; use `private` + getters/setters |
| Abstraction | Show only essential features; use `abstract` / `interface` |
| Key difference | Encapsulation = HOW to protect; Abstraction = WHAT to expose |
| Getter/Setter | Controlled access + validation for private fields |
| Immutability | Extreme encapsulation — `final` fields, no setters |
| Both work together | Abstraction at design level, Encapsulation at implementation level |
