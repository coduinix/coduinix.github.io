---
title: Functional Types
excerpt: Types can provide a safety net by making illegal states unrepresentable in code. 
  When applied correctly it can save a bunch of (unit) tests too.
---

I've been using JAVA classes and objects throughout my whole career as a developer.
In the past couple of years I did projects in Scala as well. 
These projects showed me additional ways of using types and compile-time safety to help me prevent mistakes.

## Example
Suppose we have the following data structure (a `record` in this case):
```java
record User(
    String firstName,
    String lastName,
    String email,
    boolean emailVerified) {
}
```

This looks quite okay and won't probably raise eyebrows during a code review.

### The Problem
Upon closer inspection, there is an interesting twist with the `emailVerified` field.
If a user gets a new email address, that new email address should be verified, so the `boolean` should be reset to `false`.
This is easy to forget.

## Solution
One way to make it harder to forget to update the `emailVerified` boolean would be to introduce new types.

