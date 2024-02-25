---
layout: post
title: Type Level TypeScript | IndexOf
excerpt: "Exploring the use of the indexOf function in JavaScript and its implementation with recursive conditional types in TypeScript"
date: 2024-02-24
updatedDate: 2024-02-24
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Searching for the index of an element in an array is a common operation in programming. In JavaScript, the `indexOf` function provides a way to find the index of the first element that matches a specified value. Let's see how to use it:

### JavaScript: Using the `indexOf` function

```javascript
const list = ["ciao", "come", "stai"];

const foundIndex = list.indexOf("come");
console.log(foundIndex); // Output: 1

const notFoundIndex = list.indexOf("404");
console.log(notFoundIndex); // Output: -1
```

In this example, we have an array of strings `list` and we want to find the index of the first element equal to "come". Using the `indexOf` function, we obtain the index of the first element that matches "come". If the element is not found, -1 is returned.

### Recursive Conditional Types for Finding the Index

Now, let's explore how we can achieve the same result using recursive conditional types in TypeScript. Below is an implementation that searches for the index of an element within an array type:

```typescript
type List = ["ciao", "come", "stai"];

type IndexOf<Haystack, Needle, Counter extends any[] = []> = Haystack extends [
  infer Head,
  ...infer Tail
]
  ? Head extends Needle
    ? Counter["length"]
    : IndexOf<Tail, Needle, [...Counter, 1]>
  : -1;

type FoundIndex = IndexOf<List, "come">;
//   ^? type FoundIndex = 1
type NotFoundIndex = IndexOf<List, "404">;
//   ^? type NotFoundIndex = -1
```

In this code, we define a type `IndexOf` that takes an array type `Haystack`, an element to search for `Needle`, and an optional counter `Counter` that tracks the length of the array. Using recursive conditional types, we check if the head of the current array is equal to the searched element. If yes, we return the length of the counter, which represents the index of the found element. If not, we recursively call `IndexOf` with the tail of the array and increment the counter. This process continues until we find the element or exhaust the array, in which case we return -1.

### Conclusion

In this article, we explored the power of type-level programming in TypeScript, using recursive conditional types to emulate the behavior of JavaScript's `indexOf` function. This is just one example of the many advanced features that TypeScript offers for type manipulation. I hope you found this article useful and that it has inspired you to further explore type-level programming in TypeScript.
