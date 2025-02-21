# Metodi degli oggetti,"this"

Gli oggetti solitamente vengono creati per rappresentare entità del mondo reale, come utenti, prodotti e molto altro:

```js
let user = {
  name: "John",
  age: 30
};
```

Inoltre, nel mondo reale, un utente può *agire*: selezionare qualcosa dalla lista degli acquisti, effettuare login, logout etc.

In JavaScript le azioni vengono rappresentate tramite le funzioni.

## Esempio di un metodo

Per iniziare, insegniamo a `user` a salutare:

```js run
let user = {
  name: "John",
  age: 30
};

*!*
user.sayHi = function() {
  alert("Hello!");
};
*/!*

user.sayHi(); // Hello!
```

Qui abbiamo appena utilizzato un espressione di funzione per creare una funzione ed assegnarla alla proprietà `user.sayHi` dell'oggetto.

Successivamente possiamo chiamarla. Ora l'utente potrà parlare!

Una funzione che è una proprietà di un oggetto si chiama *metodo*.

Quindi, nell'esempio abbiamo un metodo `sayHi` per l'oggetto `user`.

Ovviamente possiamo utilizzare una funzione già dichiarata come metodo:

```js run
let user = {
  // ...
};

*!*
// first, declare
function sayHi() {
  alert("Hello!");
};

// then add as a method
user.sayHi = sayHi;
*/!*

user.sayHi(); // Hello!
```

```smart header="Programmazione orientata agli oggetti"
Quando scriviamo codice utilizzando gli oggetti per rappresentare le entità, questa viene definita [programmazione orientata agli oggetti](https://en.wikipedia.org/wiki/Object-oriented_programming), in breve: "OOP".

OOP è una grande cosa, un ambito di interesse con i propri studi. Come scegliere le giuste entità? Come organizzare le interazioni tra loro? Questa è l'architettura di un codice, e ci sono molti libri importanti che trattano questo argomento, come "Design Patterns: Elements of Reusable Object-Oriented Software" by E.Gamma, R.Helm, R.Johnson, J.Vissides or "Object-Oriented Analysis and Design with Applications" by G.Booch. Noi cercheremo di capire i concetti chiave nel capitolo <info:object-oriented-programming>.
```
### La forma breve dei metodi

Esiste una sintassi più breve per i metodi in un oggetto letterale:

```js
// these objects do the same

let user = {
  sayHi: function() {
    alert("Hello");
  }
};

// method shorthand looks better, right?
let user = {
*!*
  sayHi() { // same as "sayHi: function()"
*/!*
    alert("Hello");
  }
};
```

Come possiamo notare, si può omettere `"function"` e scrivere solamente `sayHi()`.

A dire la verità, la notazione non è proprio uguale. Ci sono delle sottili differenze legate all'ereditarietà degli oggetti (le studieremo più avanti), per ora non hanno importanza. Nella maggior parte dei casi la forma breve viene preferita.

## "this" nei metodi

E' molto comune che un metodo abbia necessità di accedere alle informazioni memorizzate nell'oggetto, per eseguire determinate azioni.

Ad esempio, il codice dentro `user.sayHi()` potrebbe aver bisogno del nome dell'`user`.

**Per accedere all'oggetto, un metodo può utilizzare la parola chiave `this`.**

Il valore di `this` è l'oggetto "prima del punto", quello che ha eseguito la chiamata del metodo.

Ad esempio:

```js run
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    alert(this.name);
*/!*
  }

};

user.sayHi(); // John
```

Quindi in fase di esecuzione quando viene chiamato il metodo `user.sayHi()`, il valore di `this` sarà `user`.

Tecnicamente, è possibile accedere all'oggetto anche senza `this`, tramite riferimento come variabile esterna:

```js
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    alert(user.name); // "user" instead of "this"
*/!*
  }

};
```

...Questo codice è instabile. Se decidessimo di copiare `user` in un'altra variabile, ad esempio `admin = user` e sovrascrivere `user` con qualcos'altro, allora verrebbe effettuato l'accesso all'oggetto sbagliato.

Lo dimostriamo:

```js run
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    alert( user.name ); // leads to an error
*/!*
  }

};


let admin = user;
user = null; // overwrite to make things obvious

admin.sayHi(); // Whoops! inside sayHi(), the old name is used! error!
```

Se scriviamo `this.name` piuttosto di `user.name` all'interno di `alert`, il codice funzionerà.

## "this" non ha limiti

In JavaScript, la parola chiave "this" si comporta diversamente da molti altri linguaggi di programmazione. Primo, può essere utilizzata in ogni funzione.

Non c'è alcun errore di sintassi in un codice come questo:

```js
function sayHi() {
  alert( *!*this*/!*.name );
}
```

Il valore di `this` viene valutato in esecuzione. E può essere un valore qualsiasi.

Ad esempio, la stessa funzione potrebbe avere diversi "this" quando viene chiamata da oggetti diversi:

```js run
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

*!*
// use the same functions in two objects
user.f = sayHi;
admin.f = sayHi;
*/!*

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

In realtà potremmo chiamare la funzione anche senza un oggetto:

```js run
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

In questo caso `this`  è `undefined` in modalità strict. Se tentiamo di accedere a `this.name`, ci sarà un errore.

Se non è attiva la modalità strict (quindi se ci dimentichiamo `use strict`) il valor di `this` in questo sarà *l'oggetto globale* (`window` in un browser, lo studieremo più avanti nel capitolo [](info:global-object)). Questo strano comportamento ha delle motivazioni storiche, che `"use strict"` risolve.

Va detto che solitamente una chiamata a funzione che utilizza `this` senza un oggetto non è comune, nella maggior part dei casi è un errore di programmazione. Se una funzione utilizza `this`, ha senso che venga invocata nel contesto di un oggetto.

```smart header="Le conseguenze della libertà di `this`"
Se avete utilizzato altri linguaggi di programmazione, probabilmente la vostra idea è di un "`this` limitato", quando viene definito un metodo in un oggetto questo avrà sempre in `this` il riferimento all'oggetto.

In JavaScript `this` è "libero", il suo valore viene prelevato durante l'esecuzione e non dipende da dove il metodo è stato definito, ma piuttosto dall'oggetto "prima del punto".

Il concetto di valutare `this` durante l'esecuzione ha i suoi pregi e difetti. Da una parte una funzione può essere riutilizzata per oggetti diversi. Questa grande flessibilità può essere fonte di molti errori.

In questa guida non vogliamo giudicare le scelte di design effettuate. Ci concentriamo su come le cose funzionano, come trarre benefici ed evitare errori.
```

## Le parti interne: il tipo riferimento

```warn header="Caratteristiche del linguaggio più profonde"
Questa sezione coprirà un argomento avanzato, per poter capire meglio alcuni casi limite.

Se volete potete saltare questa sezione o posticiparne la lettura.
```

Una chiamata complessa può perde `this`, ad esmpio:

```js run
let user = {
  name: "John",
  hi() { alert(this.name); },
  bye() { alert("Bye"); }
};

user.hi(); // John (the simple call works)

*!*
// now let's call user.hi or user.bye depending on the name
(user.name == "John" ? user.hi : user.bye)(); // Error!
*/!*
```

Nell'ultima riga c'è un operatore ternario che deve decidere tra `user.hi` o `user.bye`. In questo caso `user.hi`.

Il metodo viene immediatamente chiamato con le parentesi `()`. Ma non funziona!

Questo funziona (oggetto punto metodo):
```js
user.hi();
```

Questo non funziona (valutazione del metodo):
```js
(user.name == "John" ? user.hi : user.bye)(); // Error!
```

Perché? Se voglia capire perché accade questo, dobbiamo capire come funziona la chiamata `obj.method()`.

Guardandola da più vicino, possiamo notare due operazioni in `obj.method()`:

1. Prima, il punto `'.'` ritorna la proprietà `obj.method`.
2. Poi la parentesi `()` la esegue.

Quindi, come vengono passate le informazioni riguardo `this` dalla prima alla seconda parte?

Se poniamo queste operazioni in due righe separate, allora `this` verrà certamente perso:

```js run
let user = {
  name: "John",
  hi() { alert(this.name); }
}

*!*
// split getting and calling the method in two lines
let hi = user.hi;
hi(); // Error, because this is undefined
*/!*
```

Qui `hi = user.hi` mette la funzione nella variabile, e nella riga successiva viene chiamata da sola, non c'è nessun `this`.

**Per rendere funzionante `user.hi()`, JavaScript usa un trucco -- il punto `'.'` non ritorna una funzione, ma un speciale valore di [tipo riferimento](https://tc39.github.io/ecma262/#sec-reference-specification-type).**

Il tipo riferimento è un "tipo specifico". Noi non possiamo utilizzarlo esplicitamente, viene usato internamente dal linguaggio.

Il valore del tipo riferimento è una combinazione di tre valori `(base, name, strict)`, dove:

- `base` è l'oggetto.
- `name` è la proprietà.
- `strict` è vero se `use strict` è attivo.

Il risultato dell'accesso ad un proprietà `user.hi` non è una funzione, ma un valore di tipo riferimento. Per `user.hi` con la modalità strict attiva:

```js
// Reference Type value
(user, "hi", true)
```

Quando le parentesi `()` vengono chiamate sul tipo riferimento, queste ricevono le informazioni riguardo l'oggetto e i suoi metodi, e possono settare il  `this` (`=user` in questo caso).

Qualsiasi operazione come l'assegnazione `hi = user.hi` scartano il tipo riferimento, prendono il valore di `user.hi` (una funzione) e lo ritornano per l'assegnazione. Quindi qualsiasi operazione ulteriore sul tipo riferimento perde il `this`.

Quindi il valore `this` viene passato solamente chiamando la funzione direttamente usando la notazione puntata `obj.method()` o le parentesi quadre `obj['method']()`    (vanno bene entrambe). Più avanti in questa guida, studieremo vari modi per risolvere il problema come [func.bind()](/bind#solution-2-bind).

## Le funzioni freccia non hanno "this"

Le funzioni freccia sono speciali: non hanno un proprio `this`. Se proviamo a riferirci a `this` all'interno di una funzione di questo tipo, verrà preso dal contesto esterno.

Ad esempio, qui `arrow()` usa `this` preso dal metodo esterno `user.sayHi()`:

```js run
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```

Questa è una speciale caratteristica delle funzioni freccia, è utile quando non vogliamo avere un ulteriore `this`, ma utilizzare quello del contesto esterno. Più avanti nel capitolo <info:arrow-functions> studieremo più in dettaglio le funzioni freccia.


## Riepilogo

- Le funzioni che vengono memorizzate come proprietà di un oggetto vengono dette "metodi".
- I metodi consentono agli oggetti di "agire" come `object.doSomething()`.
- I metodi possono riferirsi all'oggetto tramite `this`.

Il valore `this` viene definito durante l'esecuzione (run-time).
- Quando una funzione viene dichiarata, può utilizzare `this`, ma questo `this` non avrà alcun valore fino a che la funzione non verrà chiamata.
- La funzione può essere copiata in vari oggetti.
- Quando una funzione viene chiamata come "metodo": `object.method()`, il valore di `this` durante la chiamata vale `object`.

Da notare che le funzioni freccia sono speciali: non hanno `this`. Quando si prova ad accedere a `this` in una funzione freccia, questo verrà preso dal contesto esterno.
