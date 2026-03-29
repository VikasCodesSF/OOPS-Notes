# 📚 OOP — Master Index

## 📂 Files in this Series

| # | File | Topics Covered |
|---|------|---------------|
| 01 | `01_Classes_and_Objects.md` | Class definition, object instantiation, `new`, reference assignment, dot operator, garbage collection |
| 02 | `02_Constructors_this_final_finalize.md` | Constructors (types, chaining), `this` keyword, `final` (variable/method/class), `finalize()` |
| 03 | `03_Inheritance.md` | `extends`, `super`, multilevel hierarchy, `final` with inheritance, static + polymorphism |
| 04 | `04_Overloading_and_Overriding.md` | Method overloading, type promotion, overriding, `@Override`, dynamic dispatch, covariant returns |
| 05 | `05_Static_Keyword.md` | Static variables/methods/blocks, static nested classes, static + inheritance |
| 06 | `06_Access_Control.md` | `public`, `private`, `protected`, default access, encapsulation best practices |
| 07 | `07_Packages.md` | Package declaration, imports, CLASSPATH, sub-packages, naming conventions |
| 08 | `08_Abstract_Classes.md` | Abstract methods/classes, rules, template method pattern, partial implementation |
| 09 | `09_Interfaces.md` | Interface syntax, default/static/private methods, functional interfaces, marker interfaces |
| 10 | `10_Enums.md` | Enum class type, methods (values/ordinal/valueOf), constructor, switch, EnumSet/EnumMap |
| 11 | `11_Encapsulation_and_Abstraction.md` | Four pillars, encapsulation vs abstraction, data hiding, immutability |

---

## 🗺️ OOP Concept Map

```
                        OBJECT-ORIENTED PROGRAMMING
                                    │
          ┌─────────────────────────┼─────────────────────────┐
          │                         │                         │
   ENCAPSULATION             ABSTRACTION              INHERITANCE
   (private + getters)    (abstract/interface)         (extends)
          │                         │                         │
          │                   ┌─────┘                         │
          │            Abstract Class        ┌────────────────┤
          │            Interface             │                │
          │            Enum                  │         POLYMORPHISM
          │                           Overloading      (overriding +
          │                           Overriding        dispatch)
          │
    Access Modifiers
    public / private
    protected / default
          │
       Packages
    (namespace +
     visibility)
```

---

## 🔑 Quick Reference — Core Rules

### Inheritance
- Java supports **single class inheritance** only
- `super()` must be the **first statement** in a constructor
- Private members are **inherited but inaccessible** directly

### Overriding
- Same name + same type signature required
- Access modifier must be **same or wider**
- Return type can be **covariant** (subtype)
- `static` methods are **hidden**, not overridden

### Abstract Classes
- Cannot be instantiated
- Cannot be both `abstract` and `final`
- `abstract static` methods are NOT allowed

### Interfaces
- Variables: implicitly `public static final`
- Methods: implicitly `public abstract`
- Implementing methods must be `public`
- Static interface methods are NOT inherited

### Access Modifiers (Widest → Narrowest)
```
public → protected → default (package-private) → private
```

### Static
- Cannot access instance variables or `this`
- Static methods: class-level, compile-time resolved
- Only nested classes can be `static`

---

## 📖 Source References
- Java SE Documentation — [docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/concepts/)
- "The Complete Reference" — Herbert Schildt
