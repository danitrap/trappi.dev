---
layout: post
title: Type Level TypeScript | Map [Italian]
excerpt: "Esploriamo la programmazione type-level in TypeScript utilizzando i tipi condizionali ricorsivi e i tipi mappati"
date: 2024-02-20
updatedDate: 2024-02-20
tags:
  - post
  - typescript
  - type-level-programming
draft: false
---

Ricomincio a scrivere un po' sul blog, questo è il primo post di una serie sulla programmazione type level in TypeScript. TypeScript offre un'ampia gamma di funzionalità avanzate, tra cui i Conditional Types riscorsivi ed i Mapped Types. In questo articolo, esploreremo due approcci diversi per ottenere lo stesso risultato: trasformare una lista di stringhe in una lista dove ogni elemento è scritto in maiuscolo e termina con tre punti esclamativi.

#### JavaScript: il punto di partenza

Prima di addentrarci nei dettagli di TypeScript, diamo uno sguardo al codice JavaScript che vogliamo replicare utilizzando i tipi di TypeScript.

```javascript
const list = ["ciao", "come", "stai"];

const shoutedList = list.map((item) => `${item.toUpperCase()}!!!`);
```

Questo semplice snippet di codice prende un array di stringhe e applica una trasformazione a ciascun elemento: lo rende maiuscolo e aggiunge tre punti esclamativi alla fine. Il risultato è un nuovo array chiamato `shoutedList`.

#### Conditional Types Ricorsivi

Il primo approccio che esamineremo coinvolge l'uso di tipi ricorsivi condizionali. In TypeScript, è possibile definire tipi che si comportano in modo ricorsivo, applicando condizioni durante la loro espansione. Ecco come il nostro codice potrebbe apparire utilizzando questa tecnica:

```typescript
type List = ["ciao", "come", "stai"];

type Shout<List> = List extends [infer Head extends string, ...infer Tail]
  ? [`${Uppercase<Head>}!!!`, ...Shout<Tail>]
  : [];
type ShoutedList = Shout<List>;
//   ^? type ShoutedList = ["CIAO!!!", "COME!!!", "STAI!!!"]
```

In questo codice, definiamo un tipo `Shout` che accetta un tipo di lista. Se la lista è vuota, restituiamo un array vuoto. Altrimenti, estraiamo il primo elemento della lista e lo trasformiamo in maiuscolo aggiungendo i punti esclamativi. Quindi, chiamiamo ricorsivamente il tipo `Shout` con il resto della lista. Questo processo continua fino a quando non abbiamo esaminato tutti gli elementi della lista originale.

#### Caso Generale con Conditional Types Ricorsivi

Generalmente parlando, per eseguire un `map` su un array possiamo fare così:

```typescript
type Map<T extends any[]> = T extends [infer Head, ...infer Tail]
  ? [/* trasforma qui Head */, ...Map<Tail, U>]
  : [];
```

### Mapped Types

Un altro approccio per ottenere lo stesso risultato è utilizzare i tipi mappati di TypeScript. Con i tipi mappati, è possibile iterare attraverso i tipi esistenti e costruirne di nuovi. Infatti, non funzionano solo con gli oggetti, ma anche con gli array! Ecco come possiamo implementare la stessa trasformazione utilizzando i mapped types:

```typescript
type List = ["ciao", "come", "stai"];

type Shout<List extends string[]> = {
  [k in keyof List]: `${Uppercase<List[k]>}!!!`;
};
type ShoutedList = Shout<List>;
//   ^? type ShoutedList = ["CIAO!!!", "COME!!!", "STAI!!!"]
```

In questo caso, definiamo un tipo `Shout` che accetta una lista di stringhe. Utilizzando un mapped type, iteriamo attraverso ogni elemento della lista originale e applichiamo la trasformazione desiderata: convertiamo la stringa in maiuscolo e aggiungiamo i punti esclamativi. Il risultato è un nuovo tipo che rappresenta la lista trasformata.

#### Caso Generale Mapped Types

In generale, per eseguire un `map` su un array con i Mapped Types possiamo fare così:

```typescript
type Map<T extends any[]> = {
  [k in keyof T]: /* trasforma qui T[k] */;
};
```

### Conclusioni

Entrambi gli approcci ci consentono di ottenere lo stesso risultato: una lista di stringhe in maiuscolo con i punti esclamativi aggiunti. Mentre il primo utilizza la ricorsione condizionale, il secondo sfrutta i tipi mappati. La scelta tra i due dipenderà dalle esigenze specifiche del progetto e dalle preferenze personali dello sviluppatore. In entrambi i casi, ciò che emerge è la potenza e la flessibilità dei tipi in TypeScript, che ci consentono di esprimere concetti complessi in modo chiaro e sicuro durante la progettazione delle nostre applicazioni.
