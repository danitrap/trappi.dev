---
layout: post
title: Type Level TypeScript | Map
excerpt: "Exploring type-level programming in TypeScript using recursive conditional types and mapped types"
date: 2024-02-20
updatedDate: 2024-02-20
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

I'm starting to write a bit on the blog again, this is the first post in a series on type-level programming in TypeScript. TypeScript offers a wide range of advanced features, including recursive Conditional Types and Mapped Types. In this article, we will explore two different approaches to achieve the same result: transforming a list of strings into a list where each element is written in uppercase and ends with three exclamation points.

#### JavaScript: The Starting Point

Before diving into the details of TypeScript, let's take a look at the JavaScript code we want to replicate using TypeScript types.

```javascript
const list = ["ciao", "come", "stai"];

const shoutedList = list.map((item) => `${item.toUpperCase()}!!!`);
```

This simple code snippet takes an array of strings and applies a transformation to each element: it makes it uppercase and adds three exclamation points at the end. The result is a new array called `shoutedList`.

#### Recursive Conditional Types

The first approach we will examine involves the use of recursive conditional types. In TypeScript, it is possible to define types that behave recursively, applying conditions during their expansion. Here's how our code might look using this technique:

```typescript
type List = ["ciao", "come", "stai"];

type Shout<List> = List extends [infer Head extends string, ...infer Tail]
  ? [`${Uppercase<Head>}!!!`, ...Shout<Tail>]
  : [];
type ShoutedList = Shout<List>;
//   ^? type ShoutedList = ["CIAO!!!", "COME!!!", "STAI!!!"]
```

In this code, we define a type `Shout` that takes a list type. If the list is empty, we return an empty array. Otherwise, we extract the first element of the list and transform it into uppercase adding the exclamation points. Then, we recursively call the type `Shout` with the rest of the list. This process continues until we have examined all the elements of the original list.

#### General Case with Recursive Conditional Types

Generally speaking, to perform a `map` on an array we can do it like this:

```typescript
type Map<T extends any[]> = T extends [infer Head, ...infer Tail]
  ? [/* transform Head here */, ...Map<Tail>]
  : [];
```

### Mapped Types

Another approach to achieve the same result is to use TypeScript's mapped types. With mapped types, it is possible to iterate through existing types and build new ones. Indeed, they work not only with objects but also with arrays! Here's how we can implement the same transformation using mapped types:

```typescript
type List = ["ciao", "come", "stai"];

type Shout<List extends string[]> = {
  [k in keyof List]: `${Uppercase<List[k]>}!!!`;
};
type ShoutedList = Shout<List>;
//   ^? type ShoutedList = ["CIAO!!!", "COME!!!", "STAI!!!"]
```

In this case, we define a type `Shout` that takes a list of strings. Using a mapped type, we iterate through each element of the original list and apply the desired transformation: we convert the string to uppercase and add the exclamation points. The result is a new type that represents the transformed list.

#### General Case Mapped Types

In general, to perform a `map` on an array with Mapped Types we can do it like this:

```typescript
type Map<T extends any[]> = {
  [k in keyof T]: /* transform T[k] here */;
};
```

### Conclusions

Both approaches allow us to achieve the same result: a list of strings in uppercase with exclamation points added. While the first uses conditional recursion, the second leverages mapped types. The choice between the two will depend on the specific needs of the project and the personal preferences of the developer. In both cases, what emerges is the power and flexibility of types in TypeScript, which allow us to express complex concepts clearly and safely during the design of our applications.
