---
layout: post
title: Type Level TypeScript | Find
excerpt: "Exploring the use of the find function in JavaScript and its implementation with recursive conditional types in TypeScript"
date: 2024-02-23
updatedDate: 2024-02-23
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Today, we'll explore the use of the `find` function in JavaScript and its implementation with recursive conditional types in TypeScript. The `find` function is a useful method for finding the first element in an array that satisfies a given condition. Let's see how we can use it in JavaScript and replicate its behavior in TypeScript using recursive conditional types.

### JavaScript: Using the `find` function

```javascript
const list = ["ciao", "come", "stai"];

const found = list.find((a) => a === "come");
console.log(found); // Output: "come"

const notFound = list.find((a) => a === "404");
console.log(notFound); // Output: undefined
```

In this example, we have an array of strings `list` and we want to find the first element equal to "come". Using the `find` function, we define a callback that returns `true` if the current item is equal to "come", otherwise it returns `false`. If the item is found, it is returned; otherwise, `undefined` is returned.

### Recursive Conditional Types for Finding

Now, let's explore how we can achieve the same result using recursive conditional types in TypeScript. Below is an implementation that searches for an element within an array type:

```typescript
type List = ["ciao", "come", "stai"];

type Find<Haystack, Needle> = Haystack extends [infer Head, ...infer Tail]
  ? Head extends Needle
    ? Head
    : Find<Tail, Needle>
  : undefined;

type Found = Find<List, "come">;
//   ^? type Found = "come"
type NotFound = Find<List, "404">;
//   ^? type NotFound = undefined
```

In this code, we define a type `Find` that takes an array type `Haystack` and an element to search for `Needle`. Using recursive conditional types, we check if the head of the current array is equal to the searched element. If yes, we return the head of the array. If not, we recursively call `Find` with the tail of the array. This process continues until we find the element or exhaust the array, in which case we return `undefined`.

### Conclusion

In this article, we explored the power of type-level programming in TypeScript, using recursive conditional types to emulate the behavior of JavaScript's `find` function. This is just one example of the many advanced features that TypeScript offers for type manipulation. I hope you found this article useful and that it has inspired you to further explore type-level programming in TypeScript.
