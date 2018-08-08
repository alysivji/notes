# Fluent Python

by Luciano Ramalho

<!-- TOC -->

- [Chapter 1: The Basics](#chapter-1-the-basics)
- [Chapter 2: Control Structures and Functions](#chapter-2-control-structures-and-functions)

<!-- /TOC -->

---
---

## Chapter 1: The Basics

* `val` is for constant, `var` is for variable
    * use `val` unless you need to change contents
* do not need to specify type of value or variable, it is inferred from the expression type
* semicolons are only required if you have multiple statements on the same line

* Scala has 7 numeric types:
    * `Byte`, `Char`, `Short`, `Int`, `Long`, `Float`, `Double`, and `Boolean`

* operators are actualy methods
```java
a + b

// same as
a.+(b)
```

* does not have `++` or `--` operators, use `+=1` or `-=1`
* *Rule of Thumb:* parameterless method that doesn't modify the object has no parentheses

---

## Chapter 2: Control Structures and Functions

*
