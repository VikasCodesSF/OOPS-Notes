# 🎭 c

## 🔷 What is an Abstract Class?

An **abstract class** defines a **generalized form** shared by all subclasses, leaving specific implementations to each subclass.

```java
abstract class Shape {
    abstract double area();      // Abstract method — no body
    void display() {             // Concrete method — has body
        System.out.println("I am a shape");
    }
}
```

---

## 🔷 Abstract Method

```java
abstract returnType methodName(parameterList);
```

- Declared **without a body** (no `{ }`)
- A class with **even one** abstract method **must** be declared `abstract`
- Subclasses **must override** all abstract methods — or also be declared `abstract`

---

## 🔷 Rules for Abstract Classes

| Rule | Details |
|------|---------|
| Cannot instantiate | `new Shape()` → ❌ COMPILE ERROR |
| Can have constructors | Used by subclasses via `super()` |
| Can have concrete methods | Mix of abstract and implemented methods |
| Can have instance variables | Both static and non-static |
| Cannot be `final` | `abstract final` → ❌ Contradiction |
| Cannot have abstract static | `abstract static void method()` → ❌ |
| CAN have static methods | With a body — allowed |
| Object references allowed | `Shape s = new Circle();` → ✅ |

---

## 🔷 Why No Abstract Static Methods?

- `static` methods are called via the class name, without an object.
- An `abstract` method has no body.
- Calling an **abstract static** method would invoke an **empty method via class name** — meaningless and impossible to resolve polymorphically.

```java
abstract class Bad {
    abstract static void method();  // ❌ COMPILE ERROR
    static void okay() { }          // ✅ Static with body is fine
}
```

---

## 🔷 Abstract Class with Concrete Methods

```java
abstract class Animal {
    String name;

    Animal(String name) {           // Constructor in abstract class
        this.name = name;
    }

    abstract void makeSound();      // Subclass must implement

    void breathe() {                // Concrete method
        System.out.println(name + " is breathing");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);                // Calls abstract class constructor
    }

    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}
```

---

## 🔷 Abstract Class as Reference

Even though you can't instantiate an abstract class, you can use it as a **reference type** for polymorphism:

```java
Animal a = new Dog("Rex");    // ✅ Valid — superclass reference to subclass object
a.makeSound();                 // Calls Dog's makeSound() at runtime
a.breathe();                   // Calls Animal's concrete method
```

This is how **runtime polymorphism** works with abstract classes.

---

## 🔷 Constructor in Abstract Class

```java
abstract class Vehicle {
    int speed;

    Vehicle(int speed) {            // Constructor — called via super()
        this.speed = speed;
    }
}

class Car extends Vehicle {
    Car(int speed) {
        super(speed);               // Required — calls abstract class constructor
    }
}
```

> A **`public` constructor** on an abstract class doesn't make sense semantically (you can't call it directly), but Java allows it. It's called through subclass constructors via `super()`.

---

## 🔷 Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Methods | Abstract + concrete | Abstract + default + static (Java 8+) |
| Variables | All types (final, non-final, static, non-static) | Only `public static final` |
| Constructors | ✅ Yes | ❌ No |
| Multiple inheritance | ❌ One only (`extends`) | ✅ Multiple (`implements`) |
| Access modifiers | All (private, protected, public) | `public` by default |
| IS-A relationship | Stronger — represents a type | Weaker — represents a capability |
| Usage | Base class with shared state/behavior | Contract/capability definition |

---

## 🔷 When to Use Abstract Class vs Interface

| Use Abstract Class when... | Use Interface when... |
|---------------------------|----------------------|
| Sharing **code** among related classes | Defining a **contract** for unrelated classes |
| Classes share **state** (instance variables) | No shared state needed |
| Want to use **non-public** members | All members can be public |
| Template Method Pattern | Multiple inheritance of type needed |

---

## 🔷 Template Method Pattern (Design Pattern with Abstract Class)

```java
abstract class DataProcessor {
    // Template method — defines the algorithm skeleton
    final void process() {
        readData();
        processData();   // Abstract — subclass defines
        writeOutput();
    }

    abstract void processData();   // Subclass fills in

    void readData() { System.out.println("Reading data..."); }
    void writeOutput() { System.out.println("Writing output..."); }
}

class CSVProcessor extends DataProcessor {
    @Override
    void processData() { System.out.println("Processing CSV"); }
}
```

---

## 🔷 Partial Implementation Scenario

```java
abstract class A {
    abstract void method1();
    abstract void method2();
}

// B implements only method1 — must also be abstract
abstract class B extends A {
    @Override
    void method1() { System.out.println("B's method1"); }
    // method2 still abstract
}

// C must implement remaining abstract methods
class C extends B {
    @Override
    void method2() { System.out.println("C's method2"); }
}
```

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| `abstract` class | Cannot be instantiated; may have abstract + concrete methods |
| `abstract` method | No body; subclass must override or be abstract itself |
| Constructor | Allowed in abstract class; called via `super()` |
| `abstract static` | NOT allowed |
| Static methods | Allowed in abstract class (with body) |
| Reference usage | `Abstract ref = new ConcreteSubclass()` → valid for polymorphism |
| `abstract` + `final` | ❌ Contradiction — illegal |
