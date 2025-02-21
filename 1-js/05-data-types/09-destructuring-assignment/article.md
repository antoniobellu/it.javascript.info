# Assegnamento di destrutturazione

Le due strutture dati più utilizzate in JavaScritp sono `Object` e `Array`.

Gli oggetti ci consentono di raccogliere molti pezzi di informazione in una singola entità, mentre gli array ci consentono di memorizzare collezioni ordinate. Possiamo quindi costruire un oggetto o un array e gestirlo come singola entità, oppure passarlo ad una funzione.

*L'assegnamento di destrutturazione* è una speciale sintassi che ci consente di "spacchettare" oggetti o array in un insieme di variabili, che in molti casi possono risultare più comode. 
La destrutturazione inoltre funziona molto bene con le funzione complesse che richiedono molti parametri, valori di default, molto presto vedremo come gestire anche questi.

## Destrutturazione di un array

Un esempio di come un array viene destrutturato in variabili:

```js
// we have an array with the name and surname
let arr = ["Ilya", "Kantor"]

*!*
// destructuring assignment
let [firstName, surname] = arr;
*/!*

alert(firstName); // Ilya
alert(surname);  // Kantor
```

Ora possiamo lavorare con le variabili piuttosto che con i membri dell'array.

Risulta utile se combinata con `split` o altri metodi che ritorna un array:

```js
let [firstName, surname] = "Ilya Kantor".split(' ');
```

````smart header="\"Destrutturazione\" non significa \"distruzione\"."
Viene chiamato "assegnamento di destrutturazione", perché "destrutturizza" copiando gli elementi all'interno di variabili. Ma l'array stesso non viene modificato.

E' solo un modo breve per scrivere:
```js
// let [firstName, surname] = arr;
let firstName = arr[0];
let surname = arr[1];
```
````

````smart header="Ignora gli elementi con la virgola"
Possono essere ignorati degli elementi dell'array inserendo una virgola:

```js run
*!*
// second element is not needed
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];
*/!*

alert( title ); // Consul
```

Nel codice sopra, il secondo elemento viene ignorato, il terzo viene assegnato a `title`, il resto dell'array viene ignorato.
````

````smart header="Funziona con qualsiasi itarabile"

... In realtà, possiamo utilizzarlo con qualsiasi iterabile, non solamente con array:

```js
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1, 2, 3]);
```

````


````smart header="Assegna a qualsiasi cosa ci sia dalla parte sinistra"

Possiamo inserire qualsiasi cosa sia "assegnabile" alla sinistra.

Ad esempio, la proprietà di un oggetto:
```js run
let user = {};
[user.name, user.surname] = "Ilya Kantor".split(' ');

alert(user.name); // Ilya
```

````

````smart header="Cicla con .entries()"

Nel capitolo precedente abbiamo visto il metodo [Object.entries(obj)](mdn:js/Object/entries).

Possiamo utilizzarlo con la destrutturazione per eseguire cicli su chaivi/valore di un oggetto:

```js run
let user = {
  name: "John",
  age: 30
};

// loop over keys-and-values
*!*
for (let [key, value] of Object.entries(user)) {
*/!*
  alert(`${key}:${value}`); // name:John, then age:30
}
```

...Lo stesso vale per map:

```js run
let user = new Map();
user.set("name", "John");
user.set("age", "30");

*!*
for (let [key, value] of user.entries()) {
*/!*
  alert(`${key}:${value}`); // name:John, then age:30
}
```
````
### Il resto '...'

Se invece vogliamo destrutturare tutto, non solamente il primo elemento, ma raccogliere anche quello che segue -- passiamo un ulteriore parametro nella forma `"..."`:

```js run
let [name1, name2, *!*...rest*/!*] = ["Julius", "Caesar", *!*"Consul", "of the Roman Republic"*/!*];

alert(name1); // Julius
alert(name2); // Caesar

*!*
// Note that type of `rest` is Array.
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
*/!*
```

La variabile `rest` è un array dei valori dell'array rimanenti. Possiamo utilizzare qualsiasi altro nome di variabile al posto di `rest`, è sufficiente accertarsi di inserire i tre punti prima del nome.

### Valori di default

Se ci sono meno valori nell'array delle variabili da assegnare, non ci sarà alcun errore. I valori assenti vengono considerati undefined:

```js run
*!*
let [firstName, surname] = [];
*/!*

alert(firstName); // undefined
alert(surname); // undefined
```

Se volessimo utilizzare un nostro valore di "default", potremmo fornirlo con la sintassi `=`:

```js run
*!*
// default values
let [name = "Guest", surname = "Anonymous"] = ["Julius"];
*/!*

alert(name);    // Julius (from array)
alert(surname); // Anonymous (default used)
```

I valori di default possono essere anche espressioni complesse o anche delle chiamate a funzione. Verranno presi in considerazione solamente se non verrà fornito alcun valore.

Ad esempio, qui usiamo la funzione `prompt` come default. La chiamata avverrà solamente nel secondo caso:

```js run
// runs only prompt for surname
let [name = prompt('name?'), surname = prompt('surname?')] = ["Julius"];

alert(name);    // Julius (from array)
alert(surname); // whatever prompt gets
```



## Destrutturazione di oggetti

L'assegnamento di destrutturazione funziona allo stesso modo con gli oggetti.

La sintassi è:

```js
let {var1, var2} = {var1:…, var2…}
```

Abbiamo un oggetto alla destra dell'assegnazione, che vogliamo dividere in variabili. Nel lato sinistro abbiamo un "pattern" di proprietà corrispondenti. In questo semplice caso, abbiamo una lista di variabili raggruppate tra parentesi `{...}`.

Ad esempio:

```js run
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

*!*
let {title, width, height} = options;
*/!*

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

Le proprietà `options.title`, `options.width` e `options.height` vengono assegnate alle variabili corrispondenti. L'ordine non ha importanza. Questo codice funzionerebbe allo stesso modo:

```js
// changed the order of properties in let {...}
let {height, width, title} = { title: "Menu", height: 200, width: 100 }
```

Il pattern alla sinistra potrebbe essere anche più complesso e specificare una mappatura tra proprietà e variabili.

Se volessimo assegnare una proprietà ad una variabile con un altro nome, ad esempio, la proprietà `options.width` vogliamo inserirla in una variabile chiamata `w`, allora possiamo specificarlo con i due punti:

```js run
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

*!*
// { sourceProperty: targetVariable }
let {width: w, height: h, title} = options;
*/!*

// width -> w
// height -> h
// title -> title

alert(title);  // Menu
alert(w);      // 100
alert(h);      // 200
```

I due punti specificano "cosa : va dove". Nell'esempio sopra la proprietà `width` va in `w`, la proprietà `height` va in `h`, e `title` viene assegnata ad una variabile con lo stesso nome.

Per delle potenziali proprietà mancanti possiamo impostare dei valori di default utilizzando `"="`, come nell'esempio:

```js run
let options = {
  title: "Menu"
};

*!*
let {width = 100, height = 200, title} = options;
*/!*

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

Proprio come nel caso degli array o dei parametri di funzione, i valori di default possono essere espressioni più complesse o chiamate a funzioni. Questi verranno valutati solo nel caso in cui il valore non verrà fornito.

Il codice sotto richiederà la larghezza (width), ma non il titolo (title).

```js run
let options = {
  title: "Menu"
};

*!*
let {width = prompt("width?"), title = prompt("title?")} = options;
*/!*

alert(title);  // Menu
alert(width);  // (whatever you the result of prompt is)
```

Possiamo anche combinare entrambi, i due punti e l'uguaglianza:

```js run
let options = {
  title: "Menu"
};

*!*
let {width: w = 100, height: h = 200, title} = options;
*/!*

alert(title);  // Menu
alert(w);      // 100
alert(h);      // 200
```

### L'operatore di resto

Cosa succede se l'oggetto possiede più proprietà delle variabili da noi fornite? Possiamo prendere solamente alcune ed assegnare tutto ciò che avanza da un'altra parte?

La specifica per l'utilizzo dell'operatore resto (tre punti) fa quasi parte dello standard, ma molti browser non lo supportano ancora.

Appare cosi:

```js run
let options = {
  title: "Menu",
  height: 200,
  width: 100
};

*!*
let {title, ...rest} = options;
*/!*

// now title="Menu", rest={height: 200, width: 100}
alert(rest.height);  // 200
alert(rest.width);   // 100
```



````smart header="Catturare senza `let`"
Negli esempi sopra le variabili vengono dichiarate appena prima di essere assegnate: `let {…} = {…}`. Ovviamente, potremmo anche utilizzare delle variabili già esistenti. Ma c'è un tranello.

Questo non funzionerebbe:
```js run
let title, width, height;

// error in this line
{title, width, height} = {title: "Menu", width: 200, height: 100};
```

Il problema è che JavaScript tratta `{...}` come un blocco di codice. Questo blocco di codice può essere utilizzato per raggruppare istruzioni, come nell'esempio:

```js run
{
  // a code block
  let message = "Hello";
  // ...
  alert( message );
}
```

Per informare JavaScript che non ci troviamo in un blocco di codice, possiamo raggruppare l'intera assegnazione tra parentesi `(...)`:

```js run
let title, width, height;

// okay now
*!*(*/!*{title, width, height} = {title: "Menu", width: 200, height: 100}*!*)*/!*;

alert( title ); // Menu
```

````

## Destrutturazione annidata

Se un oggetto o un array contiene altri oggetti o array, possiamo utilizzare sequenze (pattern) di estrazione più complesse per andare più in profondità con l'estrazione.

Nel codice sotto `options` possiede un ulteriore oggetto nella proprietà `size` ed un array nella proprietà `items`. Il pattern alla sinistra dell'assegnazione ha la stessa struttura:

```js run
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true    // something extra that we will not destruct
};

// destructuring assignment on multiple lines for clarity
let {
  size: { // put size here
    width,
    height
  },
  items: [item1, item2], // assign items here
  title = "Menu" // not present in the object (default value is used)
} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
alert(item1);  // Cake
alert(item2);  // Donut
```

L'intero oggetto `options` ad eccezione di `extra` il quale non viene menzionato, viene assegnato alle corrispondenti variabili.

Note that `size` and `items` itself is not destructured.

![](destructuring-complex.png)

Infine, abbiamo `width`, `height`, `item1`, `item2` e `title` che assumo il valore di default.

Questo accade spesso con l'assegnamento di destrutturazione. Abbiamo un oggetto complesso e vogliamo estrarre solamente ciò di cui abbiamo bisogno.

Anche qui accade lo stesso:
```js
// take size as a whole into a variable, ignore the rest
let { size } = options;
```

## Parametri di funzione intelligenti

Ci sono casi in cui una funzione può accettare più parametri, molti dei quali opzionali. Questo è vero specialmente per le interfacce utente. Immaginate una funzione che crea un menu. Può avere una larghezza, un'altezza, un titolo, una lista di elementi e molto altro.

Vediamo un pessimo modo per scrivere questo tipo di funzioni:

```js
function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
  // ...
}
```

Nella vita reale, il problema è ricordarsi l'ordine degli argomenti. Solitamente gli IDE ci aiutano in questo, specialmente se il codice è ben documentato, ma ancora... Un ulteriore problema è quello di chiamare una funzione nel caso in cui molti parametri ci vadano bene di default.

Come qui?

```js
showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])
```

E' brutto a vedersi. E diventa illeggibile quando il numero di parametri aumenta.

La destrutturazione ci viene in soccorso!

Possiamo passare i parametri come un oggetto, e la funzione immediatamente lo destrutturizza in variabili:

```js run
// we pass object to function
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

// ...and it immediately expands it to variables
function showMenu(*!*{title = "Untitled", width = 200, height = 100, items = []}*/!*) {
  // title, items – taken from options,
  // width, height – defaults used
  alert( `${title} ${width} ${height}` ); // My Menu 200 100
  alert( items ); // Item1, Item2
}

showMenu(options);
```

Possiamo anche utilizzare una destrutturazione più complessa con oggetti annidati e diverse mappature:

```js run
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

*!*
function showMenu({
  title = "Untitled",
  width: w = 100,  // width goes to w
  height: h = 200, // height goes to h
  items: [item1, item2] // items first element goes to item1, second to item2
}) {
*/!*
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```

La sintassi è la stessa dell'assegnamento di destrutturazione:
```js
function({
  incomingProperty: parameterName = defaultValue
  ...
})
```

Da notare che la destrutturazione assume che `showMenu()` abbia un argomento. Se vogliamo tutti i valori di default, allora dovremmo specificare un oggetto vuoto:

```js
showMenu({});


showMenu(); // this would give an error
```

Possiamo farlo ponendo `{}` il valore di default per l'intera destrutturazione:


```js run
// simplified parameters a bit for clarity
function showMenu(*!*{ title = "Menu", width = 100, height = 200 } = {}*/!*) {
  alert( `${title} ${width} ${height}` );
}

showMenu(); // Menu 100 200
```

Nel codice sopra, tutti gli argomenti dell'oggetto sono `{}` di default, quindi ci sarà sempre qualcosa da destrutturare.

## Riepilogo

- L'assegnamento di destrutturazione ci consente di mappare un oggetto o un array su alcune variabili.
- La sintassi per gli oggetti:
    ```js
    let {prop : varName = default, ...} = object
    ```

    Questo significa che la proprietà `prop` dovrebbe andare nella variabile `varName` e, se non esiste alcuan proprietà, allora verrà utilizzato il valore di `default`.

- La sintassi per gli array:

    ```js
    let [item1 = default, item2, ...rest] = array
    ```

    Il primo elemento va in `item1`; il secondo va in `item2`, tutti gli altri finiscono nell'array `rest`.

- Per casi più complessi, la parte sinistra deve possedere la stessa struttura di quella destra.
