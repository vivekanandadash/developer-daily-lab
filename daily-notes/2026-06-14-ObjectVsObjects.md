# 📘 Java Notes - Object vs Objects

## 🚀 Object

`Object` is the root class of all classes in Java.

Every class directly or indirectly inherits from `Object`.

### Example

```java
Object obj1 = "Java";
Object obj2 = 100;
Object obj3 = true;
```

### Common Methods

```java
toString()
equals()
hashCode()
getClass()
```

### Use Cases

- Generic references
- Storing different types of objects
- Framework APIs

---

## 🚀 Objects

`Objects` is a utility class present in:

```java
java.util.Objects
```

It provides static helper methods for object operations and null handling.

### Import

```java
import java.util.Objects;
```

### Common Methods

#### Objects.equals()

```java
Objects.equals(str1, str2);
```

Null-safe comparison.

#### Objects.isNull()

```java
Objects.isNull(obj);
```

Checks if object is null.

#### Objects.nonNull()

```java
Objects.nonNull(obj);
```

Checks if object is not null.

#### Objects.requireNonNull()

```java
Objects.requireNonNull(name);
```

Throws `NullPointerException` if value is null.

#### Objects.hash()

```java
Objects.hash(name, age);
```

Generates hash code.

---

## 🔥 Object vs Objects

| Object | Objects |
|----------|----------|
| Root class of Java | Utility class |
| Package: `java.lang` | Package: `java.util` |
| Parent of all classes | Helper methods |
| Non-static methods | Static methods |
| Used for generic references | Used for null-safe operations |

---

## 🎯 Interview Answer

**What is the difference between Object and Objects?**

- `Object` is the parent class of all Java classes.
- `Objects` is a utility class that provides helper methods like `equals()`, `isNull()`, `nonNull()`, and `requireNonNull()`.

---

## 📝 Quick Revision

✅ Object = Parent Class

✅ Objects = Utility Class

✅ Object → `java.lang.Object`

✅ Objects → `java.util.Objects`

✅ Use `Object` for generic references

✅ Use `Objects` for null-safe operations
