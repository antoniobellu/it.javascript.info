
# Polyfills

Il linguaggio JavaScript si evolte costantemente. Nuove proposte per il linguaggio arrivano regolarmente, vengono analizzatte, e successivamente se ritenute valide vengono aggiunte alla lista <https://tc39.github.io/ecma262/> fino a diventare delle [specifiche](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

I team che stanno dietro il motore di JavaScript hanno le loro personali idee riguardo cosa implementare. Potrebbero quindi decidere di implementare delle proposte recenti e posticipare quelle più vecchie a causa di difficoltà nell'implementazione.

Quindi per un motore di script è naturale implementare solo le cose che si trovano nello standard.

Se si vuole rimanere aggiornati riguardo lo stato di supporto delle caratteristiche si può controllare la pagina <https://kangax.github.io/compat-table/es6/> (è molt grande, dovremmo studiare ancora molto).

## Babel

Quando utilizziamo caratteristiche moderne del linguaggio, alcuni motori potrebbero non supportarle. Infatti, non tutte le caratteristiche sono implementate ovunque.

Qui Babel ci viene in soccorso.

[Babel](https://babeljs.io) è un [transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler). Riscrive il codice da JavaScript moderno ad uno standard precedente.

Babel è composto da due parti:

1. Primo, il programma che esegue la traduzione, che riscrive quindi il codice. Lo sviuppatore lo esegue sul computer personale. Questo si occuperà di tradurre il codice nei vecchi standard. Questo verrà poi fornito al sito per gli utenti. Esistono dei progetti di sistemi come [webpack](http://webpack.github.io/) o [brunch](http://brunch.io/) che si occupano di eseguire la traduzione ad ogni cambiamente, evitando perdite di tempo al programmatore.

2. Secondo, il polyfill.

    Il transpiler riscrive il codice, quindi la sintassi utilizzata è riconosciuta. Ma per le nuove funzionalità abbiamo bisogno di scrivere uno speciale script che le implementi. JavaScript è un linguaggio altamente dinamico, gli script potrebbero non solo aggiungere nuove funzionalità, ma modificare anche quelle integrate, queste devono quindi essere conformi allo standard.

    Esiste un termine "polyfill" per gli script che "riempiono" il vuoto aggiungendo funzionalità mancanti.

    Due polyfill importanti sono:
    - [babel polyfill](https://babeljs.io/docs/usage/polyfill/) che ne supporta molti, ma è molto pesante.
    - [polyfill.io](http://polyfill.io) servizio che consente di caricare costrutti polyfills, in base alle nostre necessità.

Quindi è necessario settare il transplier e aggiungere polyfill per i vecchi motori di JavaScript, per far si che supportino le caratteristiche moderne.

Se ci spostiamo nella direzione dei moderni motori e non utilizziamo altre caratteristiche se non quelle supportate ovunque, allora non abbiamo bisogno di Babel.

## Esempi nel tutorial


````online
Molti esempi sono eseguibili sul posto, come questi:

```js run
alert('Press the "Play" button in the upper-right corner to run');
```

Gli esempio che utilizzano il moderno JS funzioneranno solo se il vostr browser li supporta.
````

```offline
Se satate leggendo la versione offline, gli esempi non sono eseguibili. Ma solitamente funzionano :)
```

[Chrome Canary](https://www.google.com/chrome/browser/canary.html) è ottimo per tutti gli esempi, ma vanno bene anche gli altri browser moderni.

Da notare che in produzione possiamo utilizzare Babel per tradurre il codice moderno in codice adatto a tutti i browser, senza alcune limitazioni, il codice eseguirà ovunque.
