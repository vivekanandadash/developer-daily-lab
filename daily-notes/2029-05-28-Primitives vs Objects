# 🗓️ Developer Daily Lab — May 28, 2026

## Topic: Primitives vs Objects — `getClass()`, `instanceof`, `valueOf()`, and `parseInt()`

---

## 🔴 The Root Misunderstanding — Primitives Are NOT Objects

In Java, there are two worlds: **primitives** and **objects**. This single distinction causes a lot of confusion for beginners.

```java
int num = 2;
System.out.println(num.getClass()); // ❌ COMPILE ERROR
```

**Why does this fail?**

Because `int` is a **primitive type**, not an object. Primitives live directly in memory as raw values — they have **no methods**, no class, no identity. Only objects (instances of classes) have methods you can call.

| Type | Kind | Has Methods? |
|------|------|-------------|
| `int` | Primitive | ❌ No |
| `double` | Primitive | ❌ No |
| `boolean` | Primitive | ❌ No |
| `Integer` | Object (Wrapper) | ✅ Yes |
| `Double` | Object (Wrapper) | ✅ Yes |
| `String` | Object | ✅ Yes |

**The fix — use the wrapper class:**

```java
Integer num = Integer.valueOf("2");
System.out.println(num.getClass()); // ✅ Works
// Output: class java.lang.Integer
```

---

## ⚠️ Why Your Previous Code Did NOT Work

```java
Consumer<String> conv = n -> System.out.println(Integer.parseInt(n));

for (Integer string : conv) { // ❌ COMPILE ERROR
    System.out.println(string.getClass());
}
```

**The problem:**

`conv` is of type `Consumer<String>` — it is just a **functional interface** (a lambda / function object).

It is **NOT**:
- a `List`
- an `Array`
- a `Collection`
- `Iterable`

Java's enhanced `for-each` loop **only works on**:
- Arrays
- Anything that implements `Iterable` (like `List`, `Set`, etc.)

So Java throws:
```
for-each not applicable to type 'Consumer<String>'
```

**The correct way to use a `Consumer`:**

```java
List<String> numbers = Arrays.asList("1", "2", "3");
Consumer<String> conv = n -> System.out.println(Integer.parseInt(n));

numbers.forEach(conv); // ✅ Correct
// Output:
// 1
// 2
// 3
```

You pass the consumer **into** `forEach()` — you do not iterate over the consumer itself.

---

## ✅ Concept 1 — `getClass()` : Getting an Object's Class at Runtime

`getClass()` is a method defined on **every Java object** (inherited from `Object`). It returns the **runtime class** of that object.

```java
Integer num = Integer.valueOf("42");
System.out.println(num.getClass());        // class java.lang.Integer
System.out.println(num.getClass().getName()); // java.lang.Integer

String text = "Hello";
System.out.println(text.getClass());       // class java.lang.String
```

**Rule: Only call `getClass()` on object references, NEVER on primitives.**

```java
int a = 5;
a.getClass();       // ❌ Compile error — int is primitive

Integer b = 5;
b.getClass();       // ✅ Works — Integer is an object
```

---

## ✅ Concept 2 — `instanceof` : Checking an Object's Type

Use `instanceof` when you want to **check** what type an object is at runtime.

**Basic example:**

```java
Object obj = "Hello";
if (obj instanceof String) {
    System.out.println("It is a String");
}
```

**Another example:**

```java
Object obj = 10;
if (obj instanceof Integer) {
    System.out.println("It is an Integer");
}
```

**When is `instanceof` useful?**

- Variable is declared as `Object` or a supertype
- You have mixed types in a collection
- Working with polymorphism

**Real-world example — mixed-type list:**

```java
List<Object> list = Arrays.asList("A", 10, 20.5);

for (Object obj : list) {
    if (obj instanceof String) {
        System.out.println(obj + " → String");
    } else if (obj instanceof Integer) {
        System.out.println(obj + " → Integer");
    } else if (obj instanceof Double) {
        System.out.println(obj + " → Double");
    }
}
// Output:
// A → String
// 10 → Integer
// 20.5 → Double
```

---

## ✅ Concept 3 — `parseInt()` vs `valueOf()` : Conversion

Both convert a `String` to a number, but they return **different types**.

### `Integer.parseInt()` → returns a **primitive** `int`

```java
int num = Integer.parseInt("123");
System.out.println(num);           // 123
// num is a primitive — no methods available
```

### `Integer.valueOf()` → returns an **Integer object**

```java
Integer num = Integer.valueOf("123");
System.out.println(num);           // 123
System.out.println(num.getClass()); // class java.lang.Integer
// num is an object — methods available
```

**Side-by-side comparison:**

```java
// parseInt — primitive result
int a = Integer.parseInt("50");
// a.getClass(); ❌ Not allowed

// valueOf — object result
Integer b = Integer.valueOf("50");
System.out.println(b.getClass()); // ✅ class java.lang.Integer
```

**When to use which:**

| Scenario | Use |
|----------|-----|
| Simple arithmetic | `parseInt()` → faster, no object overhead |
| Need object features (generics, collections, methods) | `valueOf()` → returns wrapper object |
| Storing in `List<Integer>` | Either works (autoboxing handles `parseInt`) |

---

## 📋 Quick Reference Table

| Goal | Tool | Returns |
|------|------|---------|
| Check what type an object is | `instanceof` | `boolean` |
| Convert `String` → primitive `int` | `Integer.parseInt()` | `int` (primitive) |
| Convert `String` → `Integer` object | `Integer.valueOf()` | `Integer` (object) |
| Get the class of an object at runtime | `.getClass()` | `Class<?>` object |

---

## 🔥 Final Combined Example

```java
import java.util.Arrays;
import java.util.List;

public class DailyLab {
    public static void main(String[] args) {

        List<Object> values = Arrays.asList("10", 20, "30");

        for (Object obj : values) {

            // Step 1 — Check type with instanceof
            if (obj instanceof String) {
                System.out.println(obj + " is a String");

                // Step 2 — Convert String → Integer object using valueOf()
                Integer num = Integer.valueOf((String) obj);

                // Step 3 — Call getClass() on the object
                System.out.println("Class: " + num.getClass());

            } else if (obj instanceof Integer) {
                System.out.println(obj + " is an Integer");

                // Can call getClass() directly — already an object
                System.out.println("Class: " + obj.getClass());
            }

            System.out.println("---");
        }
    }
}
```

**Output:**

```
10 is a String
Class: class java.lang.Integer
---
20 is an Integer
Class: class java.lang.Integer
---
30 is a String
Class: class java.lang.Integer
---
```

---

## 🧠 Key Takeaways

1. **Primitives (`int`, `double`, etc.) have NO methods** — you cannot call `.getClass()` on them.
2. **Wrapper classes (`Integer`, `Double`, etc.) are objects** — they wrap primitives and add methods.
3. **`instanceof`** is for type-checking, not conversion.
4. **`parseInt()`** gives you a raw primitive; **`valueOf()`** gives you a full object.
5. **`Consumer`** is a function — you cannot loop over it with `for-each`. You pass it to `forEach()`.
6. **`getClass()`** is a runtime introspection tool — only callable on object references.

---

*Added to Developer Daily Lab — May 29, 2026*
