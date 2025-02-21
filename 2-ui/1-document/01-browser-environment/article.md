# Il Browser come ambiente, specifiche

Il linguaggio JavaScript è stato inizialmente creato per i browser. Da allora, si è evoluto fino a diventare un linguaggio adatto a molte piattaforme e differenti usi.

La piattaforma di utilizzo può essere un browser, un web-server, una lavatrice o un qualunque altro tipo di *host*. Ognuno di essi fornisce delle funzionalità specifiche alla piattaforma stessa. Secondo la specifica JavaScript questa è la definizione di *ambiente host*

<<<<<<< HEAD
Un ambiente host, oltre alle funzionalità core del linguaggio, fornisce oggetti e funzioni specifiche della piattaforma. I browser web, ad esempio, permettono di interagire con le pagine web, mentre Node.JS fornisce funzionalità dedicate al server e così via.
=======
A host environment provides platform-specific objects and functions additional to the language core. Web browsers give a means to control web pages. Node.js provides server-side features, and so on.
>>>>>>> 273e47b70a14ae7a8b882b8d2543e581b000eefb

Di seguito una panoramica di cosa succede quando JavaScript viene eseguito nel browser:

![](windowObjects.png)

C'è un oggetto "padre" chiamato `window` che ha due ruoli:

1. Il primo è quello di essere un oggetto globale per JavaScript, come descritto nel capitolo <info:global-object>.
2. Il secondo è quello di rappresentare "la finestra del browser" e fornire metodi per controllarla.

In questo caso lo utilizziamo come oggetto globale:

```js run
function sayHi() {
  alert("Hello");
}

// global functions are accessible as properties of window
window.sayHi();
```

E qui invece come finestra del browser, vogliamo visualizzarne l'altezza:

```js run
alert(window.innerHeight); // inner window height
```

Ci sono molte altre funzionalità dell'oggetto `window`, ma le vedremo più avanti.

## Document Object Model (DOM)

L'oggetto `document` dà accesso al contenuto della pagina e ci permette di cambiare o creare qualunque cosa all'interno della stessa.

Ad esempio:

```js run
// change the background color to red
document.body.style.background = "red";

// change it back after 1 second
setTimeout(() => document.body.style.background = "", 1000);
```

In questo caso abbiamo usato `document.body.style`, ma c'è molto di più. Proprietà e metodi sono descritti all'interno delle specifiche. Due sono i gruppi che hanno contribuito al loro sviluppo:

1. [W3C](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) -- la documentazione disponibile su <https://www.w3.org/TR/dom>.
2. [WhatWG](https://en.wikipedia.org/wiki/WHATWG), pubblicato su <https://dom.spec.whatwg.org>.

Come normalmente accade, i due gruppi non sono sempre d'accordo, questo fa sì che esistano due tipi di standard. Fortunatamente le differenze non sono mai così distanti tra loro e si riesce a unirle sotto un'unica voce. La documentazione reperibile ai link indicati è al 99% simile. Le differenze sono talmente minime da non accorgersene nemmeno.

Personalmente, trovo più gradevole utilizzare <https://dom.spec.whatwg.org> 

In tempi antichi non esisteva uno standard comune, ogni browser aveva il proprio e lo implementava a proprio piacimento. Browser differenti avevano metodi e proprietà differenti per svolgere lo stesso compito, così gli sviluppatori erano costretti a scrivere codice per ogni browser. Tempi bui e incasinati.

Anche oggi può capitare di incappare in vecchio codice che aggira delle incompatibilità e sfrutta delle proprietà specifiche del browser. In questo tutorial utilizzeremo strumenti moderni, non ci sarà bisogno di imparare codice obsoleto a meno che non sia strettamente necessario (le probabilità che questo accada sono molto basse).

A questo punto fa la sua comparsa lo "standard" del DOM, nel tentativo di mettere tutti d'accordo. La prima versione era la "DOM Level 1", poi c'è stata la "DOM Level 2", la "DOM Level 3" e ad oggi abbiamo la "DOM Level 4". Il gruppo WhatWG, stufo di numerare le versioni, ha deciso di chiamarlo solamente "DOM", senza numerazione. Noi faremo lo stesso.

```smart header="Il DOM non è solo per i browser"
La specifica del DOM spiega la struttura di un documento e fornisce oggetti per la sua manipolazione. Esistono strumenti al di fuori del browser che usano comunque il DOM.

Per esempio, strumenti lato server che scaricano e processano le pagine HTML sfruttano il DOM. In questo caso però potrebbero supportare solo una parte delle specifiche.
```

```smart header="CSSOM per lo stile"
Regole CSS e fogli di stile non sono strutturati come l'HTML. Esiste una specifica separata [CSSOM](https://www.w3.org/TR/cssom-1/) che spiega come essi siano rappresentati come oggetti, come leggerli e scriverli.

CSSOM è usato in concomitanza con il DOM quando modifichiamo delle regole di stile per il documento. In pratica però, CSSOM è raramente richiesto poiché di solito le regole CSS sono statiche. Di rado esiste la necessità di aggiungere/rimuovere regole CSS attraverso JavaScript, quindi non tratteremo l'argomento al momento.
```

## BOM (parte della specifica HTML)

Il Browser Object Model (BOM) rappresenta gli oggetti aggiuntivi forniti dal browser (ambiente host) per interagire con qualunque cosa a eccezione del documento.

Per esempio:

- L'oggetto [navigator](mdn:api/Window/navigator) fornisce informazioni riguardo il browser e il sistema operativo. Ci sono molte proprietà a disposizione, ma quelle largamente conosciute sono: `navigator.userAgent` -- informazioni riguardo il browser corrente, e `navigator.platform` -- informazioni sulla piattaforma (può essere utile differenziare Windows/Linux/Mac etc).
- L'oggetto [location](mdn:api/Window/location) permette di leggere l'URL corrente e reindirizzare il browser verso un altro URL.

Ecco come possiamo usare l'oggetto `location`:

```js run
alert(location.href); // shows current URL
if (confirm("Go to wikipedia?")) {
  location.href = "https://wikipedia.org"; // redirect the browser to another URL
}
```

Le funzioni `alert/confirm/prompt` fanno anch'esse parte del BOM: non sono direttamente correlate con il documento, ma rappresentano dei semplici metodi del browser per interagire con l'utente.

```smart header="Specifiche HTML"
Il BOM fa parte della [specifica HTML](https://html.spec.whatwg.org).

Sì, avete capito bene. La specifica HTML presente al link <https://html.spec.whatwg.org> non è solo per il "linguaggio HTML" (tags, attributi), ma copre anche una miriade di oggetti, metodi ed estensioni del DOM relative ai browser. Questa è la definizione di "HTML a grandi linee".
```

## Riepilogo

Ricapitolando gli standard, abbiamo:

Specifica DOM: Descrive la struttura del documento, sua manipolazione ed eventi, vedi <https://dom.spec.whatwg.org>.

Specifica CSSOM: Descrive i fogli e le regole di stile, le loro manipolazioni e il loro legame con i documenti, vedi <https://www.w3.org/TR/cssom-1/>.

Specifica HTML: Descrive il linguaggio HTML language (e.g. tags) e il BOM (browser object model) -- varie funzioni del browser: `setTimeout`, `alert`, `location` e così via, vedi <https://html.spec.whatwg.org>. Prende la specifica del DOM e l'arricchisce di ulteriori proprietà e metodi.

Adesso approfondiremo il DOM, il documento gioca un ruolo centrale nell'UI.

Per favore prendi nota dei link indicati in precedenza perché c'è talmente tanto da imparare che è impossibile trattare e ricordare ogni argomento.

Quando avrai voglia di informarti su una proprietà o un metodo, il manuale Mozilla <https://developer.mozilla.org/en-US/search> è sicuramente una buona risorsa, ma leggere la corrispondente specifica potrebbe essere addirittura meglio: è più complessa e lunga da leggere, ma consoliderà e completerà le tue conoscenze.
