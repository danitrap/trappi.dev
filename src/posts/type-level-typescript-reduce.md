---
layout: post
title: Type Level TypeScript | Reduce
excerpt: "Exploring the use of the reduce function in JavaScript and its implementation with recursive conditional types in TypeScript"
date: 2024-02-22
updatedDate: 2024-02-22
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Reducing an array into a concatenated string is a common operation in programming. In JavaScript, the `reduce` function provides a versatile way to reduce the elements of an array into a single value. Let's see how to use it:

### JavaScript: Using the `reduce` function

```javascript
const list = ["ciao", "come", "stai"];

const joinedString = list.reduce((acc, curr) => `${acc} ${curr}`, "").trim();
console.log(joinedString); // Output: "ciao come stai"
```

In this example, we have an array of strings `list` and we want to concatenate all the strings into a single string separated by a space. Using the `reduce` function, we define a callback that adds the current item to the accumulator, separated by a space. Finally, we use the `trim()` method to remove any excess spaces at the beginning and end of the resulting string.

### Recursive Conditional Types for Reduction

Now, let's explore how we can achieve the same result using recursive conditional types in TypeScript. Below is an implementation that concatenates strings within an array type:

```typescript
type List = ["ciao", "come", "stai"];

type Join<
  List,
  Separator extends string = " ",
  Out extends string = ""
> = List extends [infer Head extends string, ...infer Tail]
  ? Join<Tail, Separator, `${Out}${Head}${Separator}`>
  : Out extends `${infer Cleaned}${Separator}`
  ? Cleaned
  : Out;

type Joined = Join<List>;
//   ^? type Joined = "ciao come stai"
```

In this code, we define a type `Join` that takes an array type, an optional separator, and an optional output string. Using recursive conditional types, we concatenate the strings of the array with the specified separator. When we reach the end of the array, we remove the final separator from the resulting string. The result is a string that represents the concatenated array.

#### General Case

In general, we can define a `Reduce` type that takes an array type and an initial value:

```typescript
type Reduce<List, Result = /* initial value */> = List extends [
  infer Head,
  ...infer Tail
]
  ? Reduce<Tail, /* reduction logic here */>
  : Result;
```

### Conclusion

In this article, we explored the power of type-level programming in TypeScript, using recursive conditional types to emulate the behavior of JavaScript's `reduce` function. This is just one example of the many advanced features that TypeScript offers for type manipulation. I hope you found this article useful and that it has inspired you to further explore type-level programming in TypeScript.
