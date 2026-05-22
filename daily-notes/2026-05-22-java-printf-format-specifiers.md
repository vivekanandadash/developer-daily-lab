# Java printf() Format Specifiers

`printf()` is used to print formatted output in Java.

---

## Syntax

```java
System.out.printf("format string", values);

System.out.printf(
    "Name: %s, Age: %d, Salary: %.2f%n",
    "Alice",
    34,
    90000.0
);

Output
Name: Alice, Age: 34, Salary: 90000.00
```

## Common Format Specifiers

| Specifier | Meaning | Example |
|:----------|:--------|:--------|
| `%s` | String | "Alice" |
| `%d` | Integer | 34 |
| `%f` | Floating-point number | 90000.0 |
| `%.2f` | 2 decimal places | 90000.00 |
| `%c` | Character | 'A' |
| `%b` | Boolean | true |
| `%n` | New line | Line break |

## Notes to remember
* %.2f → .2 means show 2 digits after decimal.
* %n is preferred over \n for platform-independent new lines.
* printf() helps create clean and readable console output.


