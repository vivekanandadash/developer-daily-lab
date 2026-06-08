# Java Streams and Comparator Notes

## Why This Works

```java
employeeList.stream()
            .sorted(
                Comparator.comparing(Employee::getSalary)
                          .reversed()
            );
```

### Step 1

```java
Comparator.comparing(Employee::getSalary)
```

returns:

```java
Comparator<Employee>
```

### Step 2

`reversed()` is a method of `Comparator`.

```java
Comparator<Employee> c =
    Comparator.comparing(Employee::getSalary);

c.reversed();   // ✅ Valid
```

### Step 3

The reversed comparator is passed into `sorted()`.

```java
employeeList.stream()
            .sorted(c.reversed());
```

---

## Why This Does NOT Work

```java
employeeList.stream()
            .sorted(Comparator.comparing(Employee::getSalary))
            .reversed();
```

### What happens?

```java
employeeList.stream()
            .sorted(...)
```

returns:

```java
Stream<Employee>
```

Now Java sees:

```java
Stream<Employee>.reversed()
```

But `Stream` does not have a `reversed()` method.

So the compiler throws an error.

---

## Important Rule

### `reversed()` belongs to Comparator

```java
Comparator<Employee> c =
    Comparator.comparing(Employee::getSalary);

c.reversed();   // ✅
```

### `reversed()` does NOT belong to Stream

```java
Stream<Employee> s =
    employeeList.stream();

s.reversed();   // ❌ Compile Error
```

---

## Remember the Return Types

### comparing()

```java
Comparator.comparing(Employee::getSalary)
```

returns:

```java
Comparator<Employee>
```

### sorted()

```java
employeeList.stream().sorted(...)
```

returns:

```java
Stream<Employee>
```

---

## Easy Interview Answer

Why do we write:

```java
employeeList.stream()
            .sorted(
                Comparator.comparing(Employee::getSalary)
                          .reversed()
            );
```

instead of:

```java
employeeList.stream()
            .sorted(Comparator.comparing(Employee::getSalary))
            .reversed();
```

Answer:

Because `reversed()` is a method of `Comparator`, not `Stream`. `Comparator.comparing(...)` returns a `Comparator`, so `reversed()` can be called on it. But `sorted(...)` returns a `Stream`, and `Stream` has no `reversed()` method.
