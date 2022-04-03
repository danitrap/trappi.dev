---
layout: post
title: 'FP-TS Option [Italian]'
excerpt: 'Abbastanza fp-ts per essere pericolosi'
externalLink: https://acoustic-puma-9b5.notion.site/Option-347f6cdc8bda4522a4736cdd35fb1594
date: 2022-04-03
updatedDate: 2022-04-03
tags:
  - post
  - italian
  - fp-ts
  - functional programming
  - typescript
draft: false
---

Una [Option](https://gcanti.github.io/fp-ts/modules/Option.ts.html) è una struttura dati che rappresenta un valore che può essere presente, oppure no. Se il valore è presente allora sarà `Option.some(valore)`, se non lo è allora sarà `Option.none`. Puoi pensare alle option come una alterantiva safe a `null` e `undefined`.

## Costruire una Option

### Partendo da un valore

Il modo più semplice per costruire una Option è utilizzare i costruttori `some` e `none`, che a loro volta ritornano un valore incapsulato in una Option.

```ts
import * as O from 'fp-ts/Option';

const noneValue = O.none;
const someValue = O.some('value');
```

Se vuoi vincolare la creazione di una Option partendo da un valore che potrebbe essere `null` o `undefined`. Se il valore è nullable ottieni una `Option.none`, altrimenti una `Option.some(valore)`.

```ts
import * as O from 'fp-ts/Option';

const noneValue = O.fromNullable(null); // Option.None
const optionValue = O.fromNullable('value'); // Option.Some("value")
```

### Da una funzione predicato

Se vuoi vincolare, invece, la creazione di una Option partendo da una condizione, puoi usare il type constructor `fromPredicate`:

```ts
import * as O from 'fp-ts/Option';

const isEven = (number) => number % 2 === 0;

const noneValue = O.fromPredicate(isEven)(3); // Option.None
const optionValue = O.fromPredicate(isEven)(4); // Option.Some(4)
```

### Da un'altra struttura dati

Puoi costruire una `Option` anche partendo da un `Either`. Se l'Either è nello stato di errore (left) ottieni una `Option.none`, se è nello stato di successo (right), ottieni una `Option.some(value)` contenente il valore dell'Either:

```ts
import * as E from 'fp-ts/Either';
import * as O from 'fp-ts/Option';

const leftEither = E.left('whatever');
const rightEither = E.right('value');

const noneValue = O.fromEither(leftEither); // Option.None
const optionValue = O.fromEither(rightEither); // Option.Some("value")
```

## Estrarre il valore da una Option

Quando il tuo valore è incapsulato in una Option potresti voler uscire dal mondo funzionale ed estrarre il valore contenuto. Quando vuoi effettuare questa operazione il mondo funzionale ti obbliga a gestire tutti i casi possibili.

### Estrarre il valore con fallback a null o undefined

Il modo più veloce per estrarre il valore da una Option è usare `toNullable` o `toUndefined`. Sono molto semplici, se la tua `Option` contiene un valore allora viene restitutito, altrimenti ottieni `null` o `undefined`:

```ts
import * as O from 'fp-ts/Option';

const noneValue = O.none;
const someValue = O.of('value');

O.toUndefined(noneValue); // undefined
O.toUndefined(someValue); // "value"
O.toNullable(noneValue); // null
O.toNullable(someValue); // "value"
```

### Estrarre il valore, esplicitando il fallback

Per esplicitare il valore di fallback puoi usare `getOrElse` che prende come parametro una funzione che ritorna il tuo fallback in caso la `Option` fosse di tipo `None`.

È importante tenere in considerazione che `getOrElse` si aspetta che il tipo di valore di fallback sia dello stesso tipo del valore contenuto nella Option: nel caso di `Option<number>` la funzione passata come parametro a `getOrElse` deve ritornare `number`.

Se vuoi poter tornare un tipo differente usa `getOrElseW`.

```ts
import * as O from 'fp-ts/Option';

const noneValue = O.none;
const someValue = O.of('value');

O.getOrElse(() => 'default')(noneValue); // "default"
O.getOrElse(() => 'default')(someValue); // "value"

O.getOrElseW(() => 3)(noneValue); // 3
```

### Estrarre il valore, eseguendo computazioni aggiuntive

L'ultimo modo per estrarre il valore da una Option è `match` (di cui `fold` è un alias) che ti permette di eseguire ulteriori computazioni prima di ritornare il valore. Accetta due funzioni come parametri, la prima viene eseguita in caso la tua `Option` sia di tipo `None`, la seconda nel caso in cui sia di tipo `Some`:

```ts
import * as O from 'fp-ts/Option';

const noneValue = O.none;
const someValue = O.of(10);

const doubleOrZero = O.match(
  () => 0, // viene eseguito nel caso sia None
  (n: number) => 2 * n // eseguito nel caso in cui la tua Option contenga un valore
);

doubleOrZero(noneValue); // 0
doubleOrZero(someValue); // 20
```

Tradotto e rielaborato partendo da [https://github.com/inato/fp-ts-cheatsheet](https://github.com/inato/fp-ts-cheatsheet)
