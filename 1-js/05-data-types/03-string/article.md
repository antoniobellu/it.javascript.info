# Stringhe

In JavaScript, i dati di tipo testuale vengono memorizzati in stringhe. Non esiste un tipo separato per i caratteri singoli.

Il formato interno per le stringhe è sempre [UTF-16](https://en.wikipedia.org/wiki/UTF-16), non viene influenzato dalla codifica della pagina.

## Apici

Ricapitoliamo i tipi di apice.

Le stringhe possono essere racchiuse tra singoli apici, doppi apici o backticks:

```js
let single = 'single-quoted';
let double = "double-quoted";

let backticks = `backticks`;
```

Gli apici singoli e doppi sono essenzialmente uguali. I backticks, oltretutto, ci consentono di includere una qualsiasi espressione all'interno della stringa, incluse chiamate a funzioni:

```js run
function sum(a, b) {
  return a + b;
}

alert(`1 + 2 = ${sum(1, 2)}.`); // 1 + 2 = 3.
```

Un altro vantaggio nell'utilizzo di backticks è che consentono di dividere la stringa in più righe:

```js run
let guestList = `Guests:
 * John
 * Pete
 * Mary
`;

alert(guestList); // a list of guests, multiple lines
```

Se proviamo a utilizzare gli apici singoli o doppi allo stesso modo, otterremo un errore:
```js run
let guestList = "Guests:  // Error: Unexpected token ILLEGAL
  * John";
```

Gli apici singoli e doppi sono nati insieme al linguaggio, quando non era stato ancora messo in conto la possibilità di stringhe multilinea. Le backticks sono apparse più tardi, per questo risultano più versatili.

Le backticks ci consentono anche di specificare un "template di funzione" prima della backtick di apertura. La sintassi è: <code>func&#96;string&#96;</code>. La funzione `func` viene chiamata automaticamente, gli viene passata la "string", può essere cosi trattata dalla funzione. Potete approfondire leggendo la [documentazione](mdn:/JavaScript/Reference/Template_literals#Tagged_template_literals). Questo viene chiamata "funzione template". Con questa caratteristica diventa più facile raccogliere stringhe da passare a funzioni, ma è raramente utilizzata.


## Caratteri speciali

E' comunque possibile creare stringhe multilinea con singoli apici utilizzando il "carattere nuova riga", cioè `\n`, che significa appunto nuova riga:

```js run
let guestList = "Guests:\n * John\n * Pete\n * Mary";

alert(guestList); // a multiline list of guests
```

Ad esempio, queste due funzioni portano allo stesso risultato:

```js run
alert( "Hello\nWorld" ); // two lines using a "newline symbol"

// two lines using a normal newline and backticks
alert( `Hello
World` );
```

Ci sono altri caratteri "speciali" meno comuni. Qui una lista:

| Carattere | Descrizione |
|-----------|-------------|
|`\b`|Backspace|
|`\f`|Campo di un form|
|`\n`|Nuova riga|
|`\r`|Ritorno a capo|
|`\t`|Tab|
|`\uNNNN`|Un simbolo unicode con il codice `NNNN`, ad esempio `\u00A9` -- è il codice unicode per il simbolo del copyright `©`. Devono essere esattamente quattro cifre esadecimali. |
|`\u{NNNNNNNN}`|Alcuni caratteri speciali vengono codificati con due simboli unicode, quindi 4 byte. Sono richieste le parentesi graffe.|

Esempi di unicode:

```js run
alert( "\u00A9" ); // ©
alert( "\u{20331}" ); // 佫, a rare chinese hieroglyph (long unicode)
alert( "\u{1F60D}" ); // 😍, a smiling face symbol (another long unicode)
```

Tutti i caratteri speciali iniziano con un backslash `\`. Che viene anche chiamato "carattere di escape".

Dobbiamo utilizzarlo anche se abbiamo intenzione di inserire un apice all'interno della stringa.

Ad esempio:

```js run
alert( 'I*!*\'*/!*m the Walrus!' ); // *!*I'm*/!* the Walrus!
```

Avete visto che abbiamo inserito un backslash `\'` prima dell'apice interno, altrimenti questo avrebbe indicato la fine della stringa.

Ovviamente, questo è valido per un apice uguale a quello utilizzato in apertura. Quindi, possiamo optare per una soluzione più elegante, ad esempio i doppi apici o i backticks:

```js run
alert( `I'm the Walrus!` ); // I'm the Walrus!
```
Da notare che il backslash `\` ha l'unico scopo di aiutare JavaScript nella lettura della stringa, questo verrà poi rimosso. La stringa in memoria non avrà `\`. Lo avrete sicuramente notato con gli `alert` dei vari esempi sopra.

Ma se volessimo realmente mostrare un backslash `\` dentro la stringa?

E' possibile farlo, ma dobbiamo esplicitarlo con un doppio backslash `\\`:

```js run
alert( `The backslash: \\` ); // The backslash: \
```

## String lengt

La proprietà `length` (lunghezza) contiene la lunghezza della stringa:

```js run
alert( `My\n`.length ); // 3
```

Da notare che `\n` è contato come unico carattere "speciale", quindi la lunghezza risulta essere `3`.

```warn header="`length` è una proprietà"
Alcune persone abituate ad altri linguaggi possono confondere al chiamata `str.length()` con `str.length`. Questo è un errore.

Infatti `str.length` è una proprietà numerica, non una funzione. Non c'è alcun bisogno delle parentesi.
```

## Accesso ai caratteri

Per ottenere un carattere alla posizione `pos`, si utilizzano le parentesi quadre `[pos]` oppure la chiamata al metodo [str.charAt(pos)](mdn:js/String/charAt). Il primo carattere parte dalla posizione zero:

```js run
let str = `Hello`;

// the first character
alert( str[0] ); // H
alert( str.charAt(0) ); // H

// the last character
alert( str[str.length - 1] ); // o
```

L'utilizzo delle parentesi quadre è il modo più classico per accedere ad un carattere, mentre `charAt` esiste principalmente per ragioni storiche.

L'unica differenza sta nel comportamento in casi di carattere non trovato, `[]` ritorna `undefined`, e `charAt` ritorna una stringa vuota:

```js run
let str = `Hello`;

alert( str[1000] ); // undefined
alert( str.charAt(1000) ); // '' (an empty string)
```

Possiamo iterare sui caratteri utilizzando `for..of`:

```js run
for (let char of "Hello") {
  alert(char); // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
}
```

## Le stringhe sono immutabili

Le stringhe in JavaScript non possono essere modificate. Risulta impossibile cambiare anche un solo carattere.

Possiamo anche provare a modificarla per vedere che non funziona:

```js run
let str = 'Hi';

str[0] = 'h'; // error
alert( str[0] ); // doesn't work
```

Il metodo utilizzato per aggirare questo problema è creare una nuova stringa ed assegnarla a `str` sostituendo quella vecchia.

Ad esemepio:

```js run
let str = 'Hi';

str = 'h' + str[1];  // replace the string

alert( str ); // hi
```

Nelle prossime sezioni vedremo ulteriori esempi.

## Cambiare il timbro delle lettere

I metodi come [toLowerCase()](mdn:js/String/toLowerCase) e [toUpperCase()](mdn:js/String/toUpperCase) cambiano il timbro delle lettere:

```js run
alert( 'Interface'.toUpperCase() ); // INTERFACE
alert( 'Interface'.toLowerCase() ); // interface
```

Altrimenti, possiamo agire anche su un singolo carattere:

```js
alert( 'Interface'[0].toLowerCase() ); // 'i'
```

## Cercare una sotto-stringa

Ci sono diversi modi per cercare una sotto-stringa all'interno di una stringa.

### str.indexOf

Il primo metodo è [str.indexOf(substr, pos)](mdn:js/String/indexOf).

Quello che fa è cercare `substr` in `str`, ad iniziare dalla posizione `pos`, e ne ritorna la posizione una volta trovata, se non trova corrispondenze ritorna `-1`.

Ad esempio:

```js run
let str = 'Widget with id';

alert( str.indexOf('Widget') ); // 0, because 'Widget' is found at the beginning
alert( str.indexOf('widget') ); // -1, not found, the search is case-sensitive

alert( str.indexOf("id") ); // 1, "id" is found at the position 1 (..idget with id)
```

Il secondo parametro opzionale ci consente di cercare a partire dalla posizione fornita.

Ad esempio, la prima occorrenza di `"id"` è alla posizione `1`. Per trovare la successiva occorrenza, dovremmo iniziare a cercare dalla posizione `2`:

```js run
let str = 'Widget with id';

alert( str.indexOf('id', 2) ) // 12
```


Se siamo interessati a tutte le occorrenze, possiamo utilizzare `indexOf` in un ciclo. Ogni chiamata viene fatta a partire dalla posizione della precedente corrispondenza:


```js run
let str = 'As sly as a fox, as strong as an ox';

let target = 'as'; // let's look for it

let pos = 0;
while (true) {
  let foundPos = str.indexOf(target, pos);
  if (foundPos == -1) break;

  alert( `Found at ${foundPos}` );
  pos = foundPos + 1; // continue the search from the next position
}
```

Lo stesso algoritmo può essere applicato in manierà più breve:

```js run
let str = "As sly as a fox, as strong as an ox";
let target = "as";

*!*
let pos = -1;
while ((pos = str.indexOf(target, pos + 1)) != -1) {
  alert( pos );
}
*/!*
```

```smart header="`str.lastIndexOf(substr, position)`"
Un altro metodo simile è [str.lastIndexOf(substr, position)](mdn:js/String/lastIndexOf) che ricerca a partire dalla fine della stringa.

Elenca quindi le occorrenze in ordine inverso.
```

C'è solo un piccolo inconveniente con `indexOf` nei test `if`. Non possiamo inserirlo in un `if` in questo modo:

```js run
let str = "Widget with id";

if (str.indexOf("Widget")) {
    alert("We found it"); // doesn't work!
}
```

L' `alert` nell'esempio sopra non viene mostrato perché `str.indexOf("Widget")` ritorna `0` (significa che è stata trovata una corrispondenza nella posizione iniziale). Ed è corretto, ma `if` considera `0` come `false`.

Quindi dovremmo verificare il `-1`, in questo modo:

```js run
let str = "Widget with id";

*!*
if (str.indexOf("Widget") != -1) {
*/!*
    alert("We found it"); // works now!
}
```

````smart header=Il trucco del NOT bit a bit"
Uno dei trucchi più utilizzati è l'operatore di [NOT bit a bit](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_NOT) `~`. Questo converte il numero ad un intero in 32bit (rimuovendo la parte decimale se presente) e successivamente inverte tutti i bit.

Per gli interi in 32bit la chiamata `~n` ha lo stesso risultato di `-(n+1)` (a causa del formato IEEE-754).

Ad esempio:

```js run
alert( ~2 ); // -3, the same as -(2+1)
alert( ~1 ); // -2, the same as -(1+1)
alert( ~0 ); // -1, the same as -(0+1)
*!*
alert( ~-1 ); // 0, the same as -(-1+1)
*/!*
```

Come avete potuto osservare, `~n` vale zero solo se `n == -1`.

Quindi, il test `if ( ~str.indexOf("...") )` è vero allora il risultato di `indexOf` non è `-1`. In altre parole, è stata trovata una corrispondenza.

Le persone lo utilizzano per abbreviare i controlli con `indexOf`:

```js run
let str = "Widget";

if (~str.indexOf("Widget")) {
  alert( 'Found it!' ); // works
}
```

Solitamente è sconsigliato utilizzare caratteristiche del linguaggio per azioni che possono risultare poco ovvie, ma questo particolare trucco è ampiamente utilizzato, quindi è giusto conoscerlo.

Ricordatevi solo che: `if (~str.indexOf(...))` si legge come "se trovi".
````

### includes, startsWith, endsWith

Un metodo più moderno come [str.includes(substr, pos)](mdn:js/String/includes) ritorna `true/false` basandosi solo sull'aver trovato in `str` la `substr`.

E' la scelta migliore se abbiamo bisogno di verificarne solo la corrispondenza, senza dover necessariamente conoscerne la posizione:

```js run
alert( "Widget with id".includes("Widget") ); // true

alert( "Hello".includes("Bye") ); // false
```

Il secondo argomento opzionale di `str.includes` è la posizioni da cui iniziare a cercare:

```js run
alert( "Midget".includes("id") ); // true
alert( "Midget".includes("id", 3) ); // false, from position 3 there is no "id"
```

I metodi [str.startsWith](mdn:js/String/startsWith) e [str.endsWith](mdn:js/String/endsWith) fanno esattamente ciò che dicono i loro nomi:

```js run
alert( "Widget".startsWith("Wid") ); // true, "Widget" starts with "Wid"
alert( "Widget".endsWith("get") );   // true, "Widget" ends with "get"
```

## Estrarre una sotto-stringa

Ci sono 3 metodi in JavaScript per estrarre una sotto-stringa: `substring`, `substr` e `slice`.

`str.slice(start [, end])`
: Ritorna la parte di stringa che va da `start` fino a `end` (esclusa).

    Ad esempio:

    ```js run
    let str = "stringify";
    alert( str.slice(0, 5) ); // 'strin', the substring from 0 to 5 (not including 5)
    alert( str.slice(0, 1) ); // 's', from 0 to 1, but not including 1, so only character at 0
    ```

    Se non c'è un secondo argomento, allora `slice` si ferma alla fine della stringa:

    ```js run
    let str = "st*!*ringify*/!*";
    alert( str.slice(2) ); // ringify, from the 2nd position till the end
    ```

    Sono possibili anche valori negativi per `start/end`. Questo significa che la posizione verrà contata a partire dalla fine della stringa:

    ```js run
    let str = "strin*!*gif*/!*y";

    // start at the 4th position from the right, end at the 1st from the right
    alert( str.slice(-4, -1) ); // gif
    ```


`str.substring(start [, end])`
: Ritorna la parte di stringa *compresa tra* `start` e `end`.

    E' molto simile a `slice`, ma consente di avere `start` maggiore di `end`.

    Ad esempio:


    ```js run
    let str = "st*!*ring*/!*ify";

    // these are same for substring
    alert( str.substring(2, 6) ); // "ring"
    alert( str.substring(6, 2) ); // "ring"

    // ...but not for slice:
    alert( str.slice(2, 6) ); // "ring" (the same)
    alert( str.slice(6, 2) ); // "" (an empty string)

    ```

    Argomenti negativi (a differenza di slice) non sono supportati, vengono trattati come `0`.


`str.substr(start [, length])`
: Ritorna la parte di stringa a partire da `start`, e della lunghezza `length` data.

    Diversamente dai metodi precedenti, questo ci consente di specificare la `length` (lunghezza) piuttosto della posizione in cui terminare l'estrazione:

    ```js run
    let str = "st*!*ring*/!*ify";
    alert( str.substr(2, 4) ); // ring, from the 2nd position get 4 characters
    ```

    Il primo argomento può anche essere negativo come con slice:

    ```js run
    let str = "strin*!*gi*/!*fy";
    alert( str.substr(-4, 2) ); // gi, from the 4th position get 2 characters
    ```

Ricapitoliamo questi metodi per evitare confusione:

| metodo | selezione... | negativi |
|--------|-----------|-----------|
| `slice(start, end)` | da `start` a `end` (`end` escluso) | indici negativi ammessi |
| `substring(start, end)` | tra `start` e `end` | valori negativi valgono come `0` |
| `substr(start, length)` | da `start` per `length` caratteri | consente indice di `start` negativo |


```smart header="Quale scegliere?"
Tutti quelli esaminati possono portare a termine il lavoro. Formalmente, `substr` ha un piccolo inconveniente: non è descritto nelle specifiche del core JavaScript, ma in quelle di Annex B, che copre solo le caratteristiche utili nello sviluppo browser. Quindi ambienti diversi da browser potrebbero non supportarla. Ma nella pratica viene utilizzata ovunque.

L'autore della guida si trova spesso ad utilizzare `slice`.
```

## Confrontare stringhe

Come sappiamo dal capitolo <info:comparison>, le stringhe vengono confrontate carattere per carattere in ordine alfabetico.

Sebbene ci siano delle stranezze.

1. Una lettera minuscola è sempre maggiore di una maiuscola:

    ```js run
    alert( 'a' > 'Z' ); // true
    ```

2. Le lettere con simboli particolari (come quelle tedesche) non vengono considerate:

    ```js run
    alert( 'Österreich' > 'Zealand' ); // true
    ```

    Questo potrebbe portare a strani risultati se provassimo ad ordinare le città per nome. Solitamente ci si aspetta di trovare  `Zealand` dopo `Österreich`.

Per capire cosa succede, dobbiamo guardare alla rappresentazione interna delle stringhe in JavaScript.

Tutte le stringhe vengono codificate utilizzando [UTF-16](https://en.wikipedia.org/wiki/UTF-16). Cioè: ogni carattere ha un suo codice numerico. Ci sono alcuni metodi che consentono di ottenere il carattere dal codice (e viceversa).

`str.codePointAt(pos)`
: Ritorna il codice per il carattere alla posizione `pos`:

    ```js run
    // different case letters have different codes
    alert( "z".codePointAt(0) ); // 122
    alert( "Z".codePointAt(0) ); // 90
    ```

`String.fromCodePoint(code)`
: Crea un carattere preso dalla sua rappresentazione numerica `code`:

    ```js run
    alert( String.fromCodePoint(90) ); // Z
    ```

    Possiamo anche aggiungere caratteri unicode tramite il loro codice utilizzando `\u` seguito dal codice esadecimale:

    ```js run
    // 90 is 5a in hexadecimal system
    alert( '\u005a' ); // Z
    ```

Ora vediamo i caratteri con il codice compreso tra `65..220` (l'alfabeto latino e qualche extra) creando una stringa:

```js run
let str = '';

for (let i = 65; i <= 220; i++) {
  str += String.fromCodePoint(i);
}
alert( str );
// ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
// ¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

Visto? Le lettere maiuscole vengono prima, poi ci sono un po di caratteri speciali e successivamente le lettere minuscole.

Ora è molto più ovvio il motivo per cui `a > Z` risulta vero.

I caratteri vengono confrontati utilizzando il loro codice numerico. Un codice maggiore significa che il carattere è maggiore. Il codice di `a` (97) è maggiore del codice di `Z` (90).

- Tutte le lettere minuscole vengono dopo quelle maiuscole perché il loro codice è maggiore.
- Alcune lettere come `Ö` vengono escluse dall'alfabeto. Il suo codice viene considerato maggiore di qualsiasi lettera compresa tra `a` e `z`.


### Confronto tra stringhe corretto

Il "giusto" algoritmo per confrontare stringhe è più complesso di come possa sembrare, poiché l'alfabeto è diverso per ogni lingua. Lettere uguali possono avere posizioni diverse nei vari alfabeti.

Quindi il browser deve sapere quale lingua utilizzare nel confronto.

Fortunatamente, tutti i browser moderni (IE10 + richiede una libreria esterna [Intl.JS](https://github.com/andyearnshaw/Intl.js/)) supportano lo standard internazionale  [ECMA 402](http://www.ecma-international.org/ecma-402/1.0/ECMA-402.pdf).

Questo fornisce uno speciale metodo per confrontare stringhe in lingue diverse, seguendo delle regole.

La chiamata [str.localeCompare(str2)](mdn:js/String/localeCompare):

- Ritorna `1` se `str` è maggiore di `str2` seguendo le regole della lingua.
- Ritorna `-1` se `str` è minore di `str2`.
- Ritorna `0` se sono uguali.

Ad esempio:

```js run
alert( 'Österreich'.localeCompare('Zealand') ); // -1
```

Questo metodo in realtà ha due argomenti opzionali specificati nella [documentazione](mdn:js/String/localeCompare), che consentono di specificare la lingua (di default viene presa dall'ambiente) e impostare delle regole aggiuntive come il timbro delle lettere, oppure se `"a"` e `"á"` dovrebbero essere trattate ugualmente etc.

## Internamente, Unicode

```warn header="Apprendimento avanzato"
Questa sezione andrà più in profondità riguardo le stringhe. Quello che leggerai ti potrà essere utile se hai intenzione di utilizzare emoji, simboli matematici o geroglifici.

Puoi semplicemente saltare questa sezione se non hai in programma di utilizzarle.
```

### Coppie surrogate

Molti simboli hanno un codice composto da 2 byte. Molte lettere di alfabeti europei, numeri, e anche molti geroglifici, hanno una rappresentazione in 2 byte.

Ma con 2 byte sono consentite solamente 65536 combinazioni, non sono comunque sufficienti per ogni tipo di simbolo possibile. Quindi molti simboli vengono codificati con una coppia di 2 byte chiamati "coppia surrogata".

La lunghezza di questi simboli è `2`:

```js run
alert( '𝒳'.length ); // 2, MATHEMATICAL SCRIPT CAPITAL X
alert( '😂'.length ); // 2, FACE WITH TEARS OF JOY
alert( '𩷶'.length ); // 2, a rare chinese hieroglyph
```

Da notare che le coppie surrogate non esistevano al momento della creazione di JavaScript, non vengono quindi processate correttamente dal linguaggio!

In realtà abbiamo un solo simbolo in ogni stringa sopra, ma la `length` vale `2`.

`String.fromCodePoint` e `str.codePointAt` sono dei rari metodi che operano correttamente con le coppie surrogate. Sono apparsi di recente nel linguaggio. Prima di loro, esisteva solo [String.fromCharCode](mdn:js/String/fromCharCode) e [str.charCodeAt](mdn:js/String/charCodeAt). Sono esattamente la stessa cosa di `fromCodePoint/codePointAt`, semplicemente non funzionano con le coppie surrogate.

Però cercare di ottenere un simbolo può essere difficile, perché una coppia surrogata viene trattata come due caratteri:

```js run
alert( '𝒳'[0] ); // strange symbols...
alert( '𝒳'[1] ); // ...pieces of the surrogate pair
```

Da notare che un pezzo di una coppia surrogata non ha alcun senza l'altro. Quindi nell'esempio sopra verrà mostrata "spazzatura".

Tecnicamente, le coppie surrogate sono decifrabili anche per i loro codici: se il primo carattere ha un intervallo di codice di `0xd800..0xdbff`. Allora il successivo carattere (seconda parte) deve avere l'intervallo `0xdc00..0xdfff`. Questi intervalli sono riservati esclusivamente per coppie surrogate definite dallo standard.

Nell'esempio sopra:

```js run
// charCodeAt is not surrogate-pair aware, so it gives codes for parts

alert( '𝒳'.charCodeAt(0).toString(16) ); // d835, between 0xd800 and 0xdbff
alert( '𝒳'.charCodeAt(1).toString(16) ); // dcb3, between 0xdc00 and 0xdfff
```

Nel capitolo <info:iterable> troverete molti altri modi per operare con le coppie surrogate. Ci sono anche delle librerie dedicate, ma nulla di abbastanza completo da meritare di essere menzionato.

### Lettere speciali e normalizzazione

In molti lingue ci sono lettere composte da un carattere di base completato da un simbolo che può stare sopra/sotto.

Ad esempio, la lettera `a` può essere il carattere di base per: `àáâäãåā`. Molti dei caratteri "composti" hanno una loro rappresentazione nella tabella UTF-16. Però non tutte, poiché le combinazioni possibili sono veramente molte.

Per supportare composizioni arbitrarie, UTF-16 ci consente di utilizzare diversi caratteri unicode. Un carattere di base ed uno o più "simboli" con cui "decorare" il carattere di base.

Ad esempio, se abbiamo `S` con uno speciale "punto sopra" (codice `\u0307`), viene mostrato Ṡ.

```js run
alert( 'S\u0307' ); // Ṡ
```

Se abbiamo bisogno di un ulteriore segno sopra la lettera (o sotto) -- nessun problema, è succificiente aggiungere il simbolo necessario.

Ad esempio, se vogliamo aggiungere un "punto sotto" (codice `\u0323`), allora otterremo una "S con due punti, sopra e sotto": `Ṩ`.

Ad esempio:

```js run
alert( 'S\u0307\u0323' ); // Ṩ
```

Questo consente una grande flessibilità, ma crea anche un potenziale problema: due caratteri potrebbero sembrare uguali, ma differire per la loro composizione di codici unicode.

Ad esempio:

```js run
alert( 'S\u0307\u0323' ); // Ṩ, S + dot above + dot below
alert( 'S\u0323\u0307' ); // Ṩ, S + dot below + dot above

alert( 'S\u0307\u0323' == 'S\u0323\u0307' ); // false
```

Per risolvere questo, esiste un algoritmo di "normalizzazione unicode" che porta ogni stringa alla forma "normale".

Questo algoritmo viene implementato da [str.normalize()](mdn:js/String/normalize).

```js run
alert( "S\u0307\u0323".normalize() == "S\u0323\u0307".normalize() ); // true
```

E' divertente notare che nella nostra situzione `normalize()` trasforma una sequenza di 3 caratteri in una che ne contiene solo uno: `\u1e68` (S con due punti).

```js run
alert( "S\u0307\u0323".normalize().length ); // 1

alert( "S\u0307\u0323".normalize() == "\u1e68" ); // true
```

In realtà, non è sempre cosi. La ragione è che il simbolo `Ṩ` è "abbastanza comune", quindi la tabella UTF-16 lo contiene già.

Se volete apprendere di più riguardo la normalizzazione e le sue varianti -- vengono descritte nell'appendice dello standard Unicode: [Unicode Normalization Forms](http://www.unicode.org/reports/tr15/), nella pratica le informazioni fornite in questa sezione sono sufficienti.


## Riepilogo

- Ci sono 3 tipi di apici. Le backticks consentono stringhe multi-linea ed espressioni integrate.
- Le stringhe in JavaScript vengono codificate usando UTF-16.
- Possiamo utilizzare caratteri speciali come `\n` ed inserire lettere tramite il codice unicode `\u...`.
- Per ottenere un carattere, si utilizza: `[]`.
- Per ottenere una sotto-stringa, si utilizza: `slice` o `substring`.
- Per cambiare il timbro delle lettere di una stringa si utilizza: `toLowerCase/toUpperCase`.
- Per cercare una sotto-stringa, usate: `indexOf`, o `includes/startsWith/endsWith` for simple checks.
- Per confrontar stringhe seguendo le regole della lingua, usate: `localeCompare`, altrimenti verranno comparate in base al codice del singolo carattere.

Ci sono molti altri metodi utili per operare con le stringhe:

- `str.trim()` -- rimuove gli spazi all'inzio e alla fine della stringa.
- `str.repeat(n)` -- ripete la stringa `n` volte.
- ...e molto altro. Guarda il [manuale](mdn:js/String) per maggiori dettagli.

Le stringhe possiedono anche metodi per eseguire ricerche/rimpiazzi con le regular expression. Ma l'argomento merita un capitolo separato, quindi ci ritorneremo più avanti.
