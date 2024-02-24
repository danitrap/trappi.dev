---
layout: post
title: Type Level TypeScript | Filter [Italian]
excerpt: "Esploriamo l'utilizzo della funzione filter in JavaScript e la sua implementazione con tipi condizionali ricorsivi in TypeScript"
date: 2024-02-21
updatedDate: 2024-02-21
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Filtrare elementi in un array è un'operazione comune in programmazione. In JavaScript, la funzione `filter` fornisce un modo semplice per creare un nuovo array contenente solo gli elementi che soddisfano una determinata condizione. Vediamo come utilizzarla:

### JavaScript: Utilizziamo `filter`

```javascript
const list = ["ciao", "come", "stai"];

const filteredList = list.filter((item) => item.startsWith("c"));
console.log(filteredList); // Output: ["ciao", "come"]
```

In questo esempio, abbiamo un array di stringhe `list` e vogliamo creare un nuovo array contenente solo le stringhe che iniziano con la lettera "c". Utilizzando la funzione `filter`, definiamo una callback che restituisce `true` se l'elemento corrente inizia con "c", altrimenti restituisce `false`.

### Conditional Types ricorsivi per filtrare

Ora, esploreremo come possiamo ottenere lo stesso risultato utilizzando i tipi condizionali ricorsivi in TypeScript. Qui di seguito è riportata un'implementazione che filtra le stringhe all'interno di un tipo di array:

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

In questo codice, definiamo un tipo `StartsWithC` che accetta un tipo di array. Utilizzando i tipi condizionali ricorsivi, verifichiamo se la testa dell'array corrente inizia con la lettera "c". Se sì, includiamo la testa nell'array risultante e chiamiamo ricorsivamente `StartsWithC` con la coda dell'array. Se no, escludiamo la testa e chiamiamo ricorsivamente `StartsWithC` con la coda. Questo processo continua fino a quando non abbiamo esaminato tutti gli elementi dell'array originale.

#### Caso Generale

In generale possiamo definire un tipo `Filter` che accetta un tipo di array e una condizione di filtro:

```typescript
type Filter<List, Condition> = List extends [infer Head, ...infer Tail]
  ? Head extends Condition
    ? [Head, ...Filter<Tail, Condition>]
    : Filter<Tail, Condition>
  : [];
```

### Conclusione

In questo articolo abbiamo esplorato la potenza della programmazione type-level in TypeScript, utilizzando i tipi condizionali ricorsivi per emulare il comportamento della funzione `filter` di JavaScript. Questo è solo un esempio delle molte funzionalità avanzate che TypeScript offre per la manipolazione dei tipi. Spero che tu abbia trovato utile questo articolo e ti abbia ispirato a esplorare ulteriormente la programmazione type-level in TypeScript.
