---
layout: talk
id: jspecify
title: Never a Null Moment with JSpecify
---

Since the beginning of Java we're used to dealing with NullPointerExceptions.
We tried to prevent `null` references using project conventions and sprinkling (a lot of) annotations throughout our codebases.

But even with all this tedious work we still run into "null moments", unexpected NullPointerExceptions showing up at runtime.

One of the reasons this problem has been hard to solve is that Java never had a standard well-defined way to express nullness.
Different annotation libraries and tools exist, with slightly different semantics.
As a result, annotations often act more as hints than as guarantees.

JSpecify aims to change this.
It's a collaborative effort by major Java ecosystem stakeholders to define clear, consistent nullness semantics.
With JSpecify, you can set default nullness per package via the `@NullMarked` annotation, making your intent explicit.

Because of this precision, JSpecify offers a solid foundation to provide null-safety in your libraries and applications.

You don’t need to annotate everything or rewrite existing code to get started.
In this session you will learn what makes JSpecify different from other attempts.
You'll also discover how you can start applying JSpecify incrementally in your own production code right away.
