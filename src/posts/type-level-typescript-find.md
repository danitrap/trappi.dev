---
layout: post
title: Type Level TypeScript | Find [Italian]
excerpt: "Esploriamo l'utilizzo della funzione find in JavaScript e la sua implementazione con tipi condizionali ricorsivi in TypeScript"
date: 2024-02-23
updatedDate: 2024-02-23
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Oggi esploreremo l'utilizzo della funzione `find` in JavaScript e la sua implementazione con tipi condizionali ricorsivi in TypeScript. La funzione `find` è un metodo utile per trovare il primo elemento in un array che soddisfa una determinata condizione. Vediamo come possiamo utilizzarla in JavaScript e replicarne il comportamento in TypeScript utilizzando i tipi condizionali ricorsivi.

### JavaScript: Utilizzo della funzione `find`

```javascript
const list = ["ciao", "come", "stai"];

const found = list.find((a) => a === "come");
console.log(found); // Output: "come"

const notFound = list.find((a) => a === "404");
console.log(notFound); // Output: undefined
```

In questo esempio, abbiamo un array di stringhe `list` e vogliamo trovare il primo elemento uguale a "come". Utilizzando la funzione `find`, definiamo una callback che restituisce `true` se l'elemento corrente è uguale a "come", altrimenti restituisce `false`. Se l'elemento viene trovato, viene restituito, altrimenti viene restituito `undefined`.

### Conditional Types ricorsivi per cercare

Ora, esploreremo come possiamo ottenere lo stesso risultato utilizzando i tipi condizionali ricorsivi in TypeScript. Qui di seguito è riportata un'implementazione che cerca un elemento all'interno di un tipo di array:

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

In questo codice, definiamo un tipo `Find` che accetta un tipo di array `Haystack` e un elemento da cercare `Needle`. Utilizzando i tipi condizionali ricorsivi, verifichiamo se la testa dell'array corrente è uguale all'elemento cercato. Se sì, restituiamo la testa dell'array. Se no, chiamiamo ricorsivamente `Find` con la coda dell'array. Questo processo continua fino a quando non troviamo l'elemento o esauriamo l'array, nel qual caso restituiamo `undefined`.

### Conclusione

In questo articolo, abbiamo esplorato la potenza della programmazione type-level in TypeScript, utilizzando i tipi condizionali ricorsivi per emulare il comportamento della funzione `find` di JavaScript. Questo è solo un esempio delle molte funzionalità avanzate che TypeScript offre per la manipolazione dei tipi. Spero che tu abbia trovato utile questo articolo e ti abbia ispirato a esplorare ulteriormente la programmazione type-level in TypeScript.

```

```
