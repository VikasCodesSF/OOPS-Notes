# 🎯 OOP — Enumerations (`enum`)

## 🔷 What is an Enum?

An **enumeration** is a list of named constants. In Java, `enum` defines a **class type** — making enumerations far more powerful than in C/C++.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

> Under the hood, the compiler converts each constant to a `public static final` object:
> ```java
> // What the compiler actually generates (conceptually):
> class Day {
>     public static final Day MONDAY = new Day();
>     public static final Day TUESDAY = new Day();
>     // ...
> }
> ```

---

## 🔷 Declaring and Using Enums

```java
enum Color { RED, GREEN, BLUE }

Color c = Color.RED;
System.out.println(c);  // RED
```

### Declaration Rules:
- Can be declared **outside** a class or **inside** a class.
- **Cannot** be declared inside a **method**.
- Can have a `main()` method — invocable directly from command line.

---

## 🔷 Enum and Inheritance

- All enums **implicitly extend** `java.lang.Enum`.
- Since a class can only extend one parent, **enums cannot extend anything else**.
- **An enum cannot be a superclass** (cannot be extended).
- Enums **can implement interfaces**.

```java
interface Describable {
    String describe();
}

enum Planet implements Describable {
    EARTH, MARS;

    @Override
    public String describe() {
        return "Planet: " + this.name();
    }
}
```

---

## 🔷 Important Enum Methods

| Method | Source | Description |
|--------|--------|-------------|
| `values()` | `java.lang.Enum` | Returns array of all enum constants |
| `ordinal()` | `java.lang.Enum` | Returns 0-based index of the constant |
| `valueOf(String)` | `java.lang.Enum` | Returns constant matching the string |
| `name()` | `java.lang.Enum` | Returns constant name as String |
| `toString()` | Overridden in `Enum` | Same as `name()` by default |

```java
enum Season { SPRING, SUMMER, AUTUMN, WINTER }

Season[] seasons = Season.values();   // [SPRING, SUMMER, AUTUMN, WINTER]
Season s = Season.valueOf("SUMMER");  // Season.SUMMER
int idx = Season.AUTUMN.ordinal();    // 2
```

---

## 🔷 Comparing Enum Constants

```java
Color c1 = Color.RED;
Color c2 = Color.RED;

// Option 1: == operator (recommended)
if (c1 == c2) { }          // ✅ Fast, safe for enums

// Option 2: equals() method
if (c1.equals(c2)) { }     // ✅ Works too, but == is preferred

// Note: equals() only returns true if both refer to the SAME constant
// in the SAME enumeration — ordinal value alone is NOT enough
```

---

## 🔷 Enum in Switch Statement

```java
Day today = Day.WEDNESDAY;

switch (today) {
    case MONDAY:
    case TUESDAY:
        System.out.println("Early week");
        break;
    case WEDNESDAY:
        System.out.println("Midweek");
        break;
    default:
        System.out.println("Later in the week");
}
```

> Java 14+ enhanced switch with enums:
> ```java
> String result = switch (today) {
>     case MONDAY, TUESDAY -> "Early week";
>     case WEDNESDAY -> "Midweek";
>     default -> "Later in the week";
> };
> ```

---

## 🔷 Enum with Constructor and Fields

```java
enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6);

    private final double mass;     // in kilograms
    private final double radius;   // in meters

    Planet(double mass, double radius) {  // Constructor — MUST be private or default
        this.mass = mass;
        this.radius = radius;
    }

    double surfaceGravity() {
        final double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}

System.out.println(Planet.EARTH.surfaceGravity());  // ~9.80
```

### Enum Constructor Rules:
- **Must be `private` or default** — never `public` or `protected`.
- Called **automatically** for each constant when the enum class loads.
- Cannot be called explicitly by the user.

> **Why private?** Allowing public would let users create new enum instances — breaking the guarantee that only the declared constants exist.

---

## 🔷 Enum with Abstract Methods

```java
enum Operation {
    ADD {
        @Override
        public int apply(int a, int b) { return a + b; }
    },
    SUBTRACT {
        @Override
        public int apply(int a, int b) { return a - b; }
    };

    public abstract int apply(int a, int b);
}

System.out.println(Operation.ADD.apply(5, 3));       // 8
System.out.println(Operation.SUBTRACT.apply(5, 3));  // 2
```

---

## 🔷 EnumSet and EnumMap (java.util)

```java
import java.util.*;

// EnumSet — high-performance Set implementation for enums
EnumSet<Day> weekend = EnumSet.of(Day.SATURDAY, Day.SUNDAY);

// EnumMap — high-performance Map with enum keys
EnumMap<Day, String> schedule = new EnumMap<>(Day.class);
schedule.put(Day.MONDAY, "Team meeting");
```

---

## 🔷 Enum Methods — Only Concrete

```java
enum Color {
    RED, GREEN, BLUE;

    // ✅ Concrete method — allowed
    public String describe() {
        return "Color: " + this.name();
    }

    // ❌ Abstract method — only allowed if each constant provides body (like above example)
}
```

> Enums **cannot have abstract methods** unless each constant provides the implementation (constant-specific class bodies).

---

## 🔷 `toString()` and `name()`

```java
enum Color { RED, GREEN, BLUE }

Color c = Color.RED;
System.out.println(c.toString());   // "RED"
System.out.println(c.name());       // "RED"
System.out.println(c);             // "RED" (uses toString())
```

- `toString()` is **overridable** — you can customize the output.
- `name()` is `final` — always returns the exact declared name.

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| `enum` | Defines a set of named constants as a class type |
| Extends | Implicitly extends `java.lang.Enum` — cannot extend anything else |
| Implements | Can implement interfaces |
| Constructor | Must be `private` or default; auto-called per constant |
| `values()` | Returns all constants as an array |
| `ordinal()` | Returns 0-based index of the constant |
| `valueOf()` | Returns constant by name string |
| Comparison | Use `==` (preferred) or `equals()` |
| In switch | Works natively with switch/switch expressions |
