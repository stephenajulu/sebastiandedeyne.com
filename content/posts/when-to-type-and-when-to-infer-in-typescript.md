---
title: "When to type and when to infer in TypeScript"
slug: when-to-type-and-when-to-infer-in-typescript
date: 2020-11-12
categories: ["articles"]
keywords:
  - TypeScript
---

Type inference is the ability to deduce types from other pieces of code. TypeScript's type inference is very powerful, even a minimal amount of typing adds a lot of assertions.

Just because you aren't required to add types, doesn't mean you shouldn't. Here's how I decide when or when not to explicitly add types in TypeScript.

<!--more-->

In this example, the least amount of types possible were added: the parameters.

```js
function divide(a: number, b: number) {
    return a / b;
}

const result = divide(10, 2);
```

TypeScript infers that `result` is a number despite not adding a return type to `divide`, or a type declaration to the `result` variable.

TypeScript knows that `a / b` will return a number, so `divide` returns a number. TypeScript knows that `result` has the same type of whatever `divide` returns, which is a number.

A more explicit version would have a lot more `number`s.

```js
function divide(a: number, b: number): number {
    return a / b;
}

const five: number = divide(10, 2);
```

So how to decide when to type explicitly, and when to infer? I follow one simple rule: **Add types to all function declarations.**

My version of the snippet lands somewhere between the two.

```js
function divide(a: number, b: number): number {
    return a / b;
}

const five = divide(10, 2);
```

I type function declarations for two reasons: readability and contract.

**Readability:** by typing the declaration, I exactly what to expect of the function without looking at its body.

While we may assume this will return a number, we can't be sure. And assumptions are exactly what we want to avoid by using types.

```js
function divide(a: number, b: number)
```

**Contract:** by typing the declaration, I'll catch unexpected types in the function body sooner.

This would implicitly set the return type as `void | number`.

```js
function divide(a: number, b: number) {
    if (b === 0) {
      return;
    }

    return a / b;
}
```

While in reality, I'd rather ensure a number is returned.

```js
function divide(a: number, b: number): number {
    if (b === 0) {
      throw new Error("Can't divide by 0");
    }

    return a / b;
}
```

While unexpectdd types like the above are easy to spot in small examples, they can be very subtle in large function bodies.
