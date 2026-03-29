# 📦 OOP — Packages

## 🔷 What is a Package?

A **package** is a container for classes that provides:
- **Naming control** — Avoids class name collisions across large projects
- **Visibility control** — Controls access between classes
- **Organization** — Groups related classes together

```java
package MyPackage;   // Declares this file belongs to MyPackage
```

---

## 🔷 How Packages Are Stored

- Packages are stored as **directories** in the file system.
- The directory name **must match** the package name exactly (case-sensitive).

```
package java.awt.image;
→ stored at: java/awt/image/  (Linux/Mac)
→ stored at: java\awt\image\  (Windows)
```

> ⚠️ You **cannot rename** a package without renaming the directory.

---

## 🔷 How Java Finds Packages

Java runtime looks for packages in three ways:

| Priority | Method |
|----------|--------|
| 1 | **Current working directory** — default starting point |
| 2 | **CLASSPATH** environment variable — explicitly set paths |
| 3 | **-classpath / -cp** flag with `java` / `javac` commands |

```bash
javac -classpath /path/to/classes MyProgram.java
java -cp /path/to/classes MyProgram
```

---

## 🔷 Importing Packages

### Import a specific class:
```java
import java.util.ArrayList;
```

### Import all public classes in a package (wildcard):
```java
import java.util.*;
```

> Wildcard `*` imports all **classes** in the package — not sub-packages.

### No import needed for:
- `java.lang` package (auto-imported)
- Classes in the same package

---

## 🔷 Fully Qualified Class Name (without import)

```java
java.util.ArrayList<String> list = new java.util.ArrayList<>();
```
- Useful when two packages have a class with the same name:

```java
import java.util.Date;
// java.sql.Date must be used with full name to avoid conflict
java.sql.Date sqlDate = new java.sql.Date(System.currentTimeMillis());
```

---

## 🔷 Package Access Rules

- When a package is imported, only **`public`** members are visible to non-subclasses in the importing code.
- **Default (package-private)** members are not accessible from outside the package.

```java
package animals;

class Dog {               // default class — not visible outside 'animals' package
    public void bark() {}
}

public class Animal {     // public class — visible everywhere
    void breathe() {}     // default method — only within 'animals' package
}
```

---

## 🔷 Common Built-in Packages

| Package | Contains |
|---------|---------|
| `java.lang` | Core classes: `String`, `Math`, `Object`, `System` (auto-imported) |
| `java.util` | Collections, `ArrayList`, `HashMap`, `Scanner`, `Date` |
| `java.io` | File I/O, streams |
| `java.net` | Networking, `URL`, `Socket` |
| `java.sql` | JDBC database access |
| `java.awt` | Abstract Window Toolkit (GUI) |
| `javax.swing` | Swing GUI components |

---

## 🔷 Creating and Using a Package — Example

```
project/
├── animals/
│   ├── Animal.java
│   └── Dog.java
└── Main.java
```

```java
// animals/Animal.java
package animals;

public class Animal {
    public void breathe() {
        System.out.println("Breathing...");
    }
}
```

```java
// Main.java
import animals.Animal;

public class Main {
    public static void main(String[] args) {
        Animal a = new Animal();
        a.breathe();
    }
}
```

---

## 🔷 Static Import

```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

double area = PI * r * r;      // No need to write Math.PI
double val = sqrt(16);         // No need to write Math.sqrt
```

- Allows accessing static members of a class **without** class name prefix.
- Use sparingly — can reduce readability if overused.

---

## 🔷 Sub-packages

```java
package com.company.project.module;
```
- Java packages can be **hierarchical** using dots.
- Each dot represents a subdirectory.
- Sub-packages are **NOT automatically imported** by importing the parent package.

```java
import java.util.*;           // Imports classes in java.util
// Does NOT import java.util.concurrent classes — need separate import
import java.util.concurrent.*;
```

---

## 🔷 Naming Conventions

| Convention | Example |
|------------|---------|
| All lowercase | `com.mycompany.project` |
| Reverse domain name | `com.google.android` |
| Avoid Java reserved words | Don't use `int`, `class`, etc. as package names |

---

## ✅ Key Takeaways

| Concept | Key Point |
|---------|-----------|
| Package | Namespace container; maps to directory on filesystem |
| `package` statement | Must be the **first statement** in a Java source file |
| Import | Brings in public members of a class/package |
| Wildcard import `*` | Imports all classes, not sub-packages |
| Default access | Only visible within the same package |
| `java.lang` | Auto-imported — no explicit import needed |
| CLASSPATH | Tells JVM where to find packages |
