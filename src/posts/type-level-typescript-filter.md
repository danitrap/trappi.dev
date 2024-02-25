---
layout: post
title: Type Level TypeScript | Filter
excerpt: "Exploring the use of the filter function in JavaScript and its implementation with recursive conditional types in TypeScript"
date: 2024-02-21
updatedDate: 2024-02-21
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Filtering elements in an array is a common operation in programming. In JavaScript, the `filter` function provides a simple way to create a new array containing only the elements that meet a certain condition. Let's see how to use it:

### JavaScript: Using `filter`

```javascript
const list = ["ciao", "come", "stai"];

const filteredList = list.filter((item) => item.startsWith("c"));
console.log(filteredList); // Output: ["ciao", "come"]
```

In this example, we have an array of strings `list` and we want to create a new array containing only the strings that start with the letter "c". Using the `filter` function, we define a callback that returns `true` if the current item starts with "c", otherwise it returns `false`.

### Recursive Conditional Types for Filtering

Now, let's explore how we can achieve the same result using recursive conditional types in TypeScript. Below is an implementation that filters strings within an array type:

```typescript
type List = ["ciao", "come", "stai"];

type StartsWithC<List> = List extends [infer Head, ...infer Tail]
  ? Head extends `c${string}`
    ? [Head, ...StartsWithC<Tail>]
    : StartsWithC<Tail>
  : [];

type CList = StartsWithC<List>;
//   ^? type CList = ["ciao", "come"]
```

In this code, we define a type `StartsWithC` that takes an array type. Using recursive conditional types, we check if the head of the current array starts with the letter "c". If yes, we include the head in the resulting array and recursively call `StartsWithC` with the tail of the array. If not, we exclude the head and recursively call `StartsWithC` with the tail. This process continues until we have examined all the elements of the original array.

#### General Case

In general, we can define a `Filter` type that takes an array type and a filter condition:

```typescript
type Filter<List, Condition> = List extends [infer Head, ...infer Tail]
  ? Head extends Condition
    ? [Head, ...Filter<Tail, Condition>]
    : Filter<Tail, Condition>
  : [];
```

### Conclusion

In this article, we explored the power of type-level programming in TypeScript, using recursive conditional types to emulate the behavior of JavaScript's `filter` function. This is just one example of the many advanced features that TypeScript offers for type manipulation. I hope you found this article useful and that it has inspired you to further explore type-level programming in TypeScript.
