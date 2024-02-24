---
layout: post
title: Type Level TypeScript | IndexOf [Italian]
excerpt: "Esploriamo l'utilizzo della funzione indexOf in JavaScript e la sua implementazione con tipi condizionali ricorsivi in TypeScript"
date: 2024-02-24
updatedDate: 2024-02-24
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

La ricerca dell'indice di un elemento in un array è un'operazione comune in programmazione. In JavaScript, la funzione `indexOf` fornisce un modo per trovare l'indice del primo elemento corrispondente a un valore specificato. Vediamo come utilizzarla:

### JavaScript: Utilizzo della funzione `indexOf`

```javascript
const list = ["ciao", "come", "stai"];

const foundIndex = list.indexOf("come");
console.log(foundIndex); // Output: 1

const notFoundIndex = list.indexOf("404");
console.log(notFoundIndex); // Output: -1
```

In questo esempio, abbiamo un array di stringhe `list` e vogliamo trovare l'indice del primo elemento uguale a "come". Utilizzando la funzione `indexOf`, otteniamo l'indice del primo elemento corrispondente a "come". Se l'elemento non viene trovato, viene restituito -1.

### Conditional Types ricorsivi per trovare l'indice

Ora, esploreremo come possiamo ottenere lo stesso risultato utilizzando i tipi condizionali ricorsivi in TypeScript. Qui di seguito è riportata un'implementazione che cerca l'indice di un elemento all'interno di un tipo di array:

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

In questo codice, definiamo un tipo `IndexOf` che accetta un tipo di array `Haystack`, un elemento da cercare `Needle`, e un contatore opzionale `Counter` che tiene traccia della lunghezza dell'array. Utilizzando i tipi condizionali ricorsivi, verifichiamo se la testa dell'array corrente è uguale all'elemento cercato. Se sì, restituiamo la lunghezza del contatore, che rappresenta l'indice dell'elemento trovato. Se no, chiamiamo ricorsivamente `IndexOf` con la coda dell'array e incrementiamo il contatore. Questo processo continua fino a quando non troviamo l'elemento o esauriamo l'array, nel qual caso restituiamo -1.

### Conclusione

In questo articolo, abbiamo esplorato la potenza della programmazione type-level in TypeScript, utilizzando i tipi condizionali ricorsivi per emulare il comportamento della funzione `indexOf` di JavaScript. Questo è solo un esempio delle molte funzionalità avanzate che TypeScript offre per la manipolazione dei tipi. Spero che tu abbia trovato utile questo articolo e ti abbia ispirato a esplorare ulteriormente la programmazione type-level in TypeScript.
