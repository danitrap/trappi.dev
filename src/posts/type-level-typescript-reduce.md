---
layout: post
title: Type Level TypeScript | Reduce [Italian]
excerpt: "Esploriamo l'utilizzo della funzione reduce in JavaScript e la sua implementazione con tipi condizionali ricorsivi in TypeScript"
date: 2024-02-22
updatedDate: 2024-02-22
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

La riduzione di un array in una stringa concatenata è un'operazione comune in programmazione. In JavaScript, la funzione `reduce` fornisce un modo versatile per ridurre gli elementi di un array in un singolo valore. Vediamo come utilizzarla:

### JavaScript: Utilizzo della funzione `reduce`

```javascript
const list = ["ciao", "come", "stai"];

const joinedString = list.reduce((acc, curr) => `${acc} ${curr}`, "").trim();
console.log(joinedString); // Output: "ciao come stai"
```

In questo esempio, abbiamo un array di stringhe `list` e vogliamo concatenare tutte le stringhe in un'unica stringa separata da uno spazio. Utilizzando la funzione `reduce`, definiamo una callback che aggiunge l'elemento corrente all'accumulatore, separato da uno spazio. Infine, utilizziamo il metodo `trim()` per rimuovere eventuali spazi in eccesso all'inizio e alla fine della stringa risultante.

### Conditional Types ricorsivi per ridurre

Ora, esploreremo come possiamo ottenere lo stesso risultato utilizzando i tipi condizionali ricorsivi in TypeScript. Qui di seguito è riportata un'implementazione che concatena le stringhe all'interno di un tipo di array:

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

In questo codice, definiamo un tipo `Join` che accetta un tipo di array, un separatore opzionale e una stringa di output opzionale. Utilizzando i tipi condizionali ricorsivi, concateniamo le stringhe dell'array con il separatore specificato. Quando raggiungiamo la fine dell'array, rimuoviamo il separatore finale dalla stringa risultante. Il risultato è una stringa che rappresenta l'array concatenato.

#### Caso Generale

In generale possiamo definire un tipo `Reduce` che accetta un tipo di array e un valore iniziale:

```typescript
type Reduce<List, Result = /* valore iniziale */> = List extends [
  infer Head,
  ...infer Tail
]
  ? Reduce<Tail, /* logica di riduzione qui */>
  : Result;
```

### Conclusione

In questo articolo abbiamo esplorato la potenza della programmazione type-level in TypeScript, utilizzando i tipi condizionali ricorsivi per emulare il comportamento della funzione `reduce` di JavaScript. Questo è solo un esempio delle molte funzionalità avanzate che TypeScript offre per la manipolazione dei tipi. Spero che tu abbia trovato utile questo articolo e ti abbia ispirato a esplorare ulteriormente la programmazione type-level in TypeScript.

```

```
