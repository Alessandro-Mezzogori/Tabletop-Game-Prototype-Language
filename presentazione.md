# Appunti presentazione

1. prima facciata = prima pagina della tesi / copertina
2. obbiettivo dell'elaborato: fornire un linguaggio capace di 
3. descrivere facilmente i concetti dei giochi da tavolo
4. struttura evneti ad oggetti perché della decisione
5. eventi e cosa abilita una struttura a eventi:
    - molteplici ascoltatori (facile estendibilitá), non esistono dipendenze tra il generatore e gli handlers
6. alcuni contro
    - gestione della concomitanza delle azioni
    - estensione: \
      - al momento il modo migliore per estendere un codice giá scritto é tramite inheritance o tramite composizione, usando dei patter come strategy. questo significa che per fare una modifica non triviale di un codice giá scritto per esempio per delle modifiche dovute ad un espansione si é obbligati a ritoccare il codice del gioco principale ( in caso di modiche delle meccaniche ), oppure di costruire un codice prevedendo i possibili cambiementi, aggiundendo dei livelli di astrazione che in questi casi d'uso non servirebbero ad altro che complicare il flusso di lavoro del linguaggio. attualmente nessun meccanismo é stato inserito nella prima proposta dovuta alla complessitá dell'argomento.

# Stesura discorso

Salve, mi chiamo Alessandro Mezzogori, studente del corso di laurea in Ingegneria Informatica.
// parlare di mie passione per giochi da tavolo ? 
L'elaborato che presento si concentra sulla proposta di un linguaggio utilizzato per la prototipizzazione di giochi da tavolo, 
di fatti l'unica alternativa utilizzata attualmente con uno scopo diverso dalla prototipizzazione é Game Description Language, il cui é piú correlato alla descrizione di generici giochi in modo che degli agenti artificiali possano interagire con il gioco descritto.

Tabletop Game Prototype Language ha l'obbiettivo di fornire un unica interfaccia di prototipizzazione per i giochi da tavolo, in maniera 
leggibile, efficacie e facile da modificare durante la codifica.

Questo puó avvenire facilmente tramite l'architettura a eventi del linguaggio che associa un evento ad N possibile handlers, 
permettendo un disaccoppiamento quasi totale del codice generante l'evento dal consumante.
Gli eventi sono facilmente mappabili alle meccaniche dei giochi
e di conseguenza forniscono un interfaccia facilmente traducibile 
da un gioco al codice.

per un facile intuizione mentale, il linguaggio é principalmente concepito come ad oggetti poiché questo paradigma si é rivelato 
molto efficacie nel modellare la struttura di giochi e videogiochi
negli ultimi anni.
A testimonianza i due game engine piú utilizzati Unity e Unreal Engine, fanno uso del paradigma ad oggetti il primo tramite c# come linguaggio di scripting e unreal tramite la programmazione di classi in c++.

le azioni implementano il concetto dell'handler nell'architettura ad eventi, una azione é composta da 4 parti principali ed é una pseudo-funzione alla base della funzionalitá principale del linguaggio.

le quattro parti che compongono un aizone sono:
- requirements
- triggers 
- input
- effect 

spiegare velocemenet ognuno, fare cenni alle funzioni come 
sistema di grouping del codice.

in conclusione, TGPL fornisce un set sufficientemente completo 
di strumenti per la codifica di un gioco 

# Trascrittura audio
Buongiorno alla commissione, mi chiamo Alessandro Mezzogori, sono uno
studente del corso di laurea in ingegneria informatica.
L'elaborato che presento si concentra sulla proposta di un linguaggio
di programmazione per la prototipizzazione dei giochi da tavolo.
Facendo una piccola tangente, una delle poche alternative un qualsiasi gioco, é Gaem description language creato da Micheal Genesereth per il general gameplaying prioject.
grazie alla specializzazione di GDL facilmente capibile da degli agenti artificiali, ovvero la concenzione di un qualsiasi gioco come una macchina a stati finiti. 
Purtroppo consegue difficile da utilizzare in giochi non ben definiti, ovvero nella fase di prototipizzazione e design.
Questo porta ad avere una mancanza di un linguaggio per tale scopo, 
la cui mia proposta si porrebbe come una possibile soluzione iniziale, o almeno per ispirare la discussione sull'argomento.

TTPL ha l'obbiettivi di fornire un unica interfaccia per la  prototipizzazione dei giochi da tavolo, la cui deve avvenire
in maniera flessibile, efficacie e a supporto dello sviluppatore o designer per non rendere la codifica del gioco un impresa piú ardua.

questo avviene principalmente tramite le due principali scelte di design del linguaggio, fare uso del paradigma ad oggetti
e avere un archittettura ad eventi.

il paradigma ad oggetti é stato scelto poiché negli ultimi anni si é rilevato efficacie per modellare la struttura di giochi e videogiochi,
a testimonianza i due game engine piú utilizzati al mondo, Unity e Unreal Engine, rispettivamente utilizzano csharp come linguaggio di scripting 
e c++ come linguaggio di programmazione entrambi nel paradigma ad oggetti.

L'architettura ad eventi rend epossibile la disaccoppiare il codice generante gli eventi dal quello consumante,
gli eventi sono una struttura facilmente mappabile tra il mondo dei giochi e il mondo del codice 
poiché una qualsiasi azione, comando, cambio di turno ecc.. possono essere classificati come degli eventi  
nel lignuaggio le azioni di una classe sono gli handler dell'evento, un azione puó essere associata a piú di un evento e ogni evento
puó avere zero o piú handlers.
si compongono di quattro componenti principali:
- requirements
- triggers
- input 
- effetto

requirements indicano se l'azione é attiva oppure no, ovvero é una funzione che decide se si devono valutare i
trigger in base al valore booleano di ritorno, true per attivo, false per inattivo.

i triggers sono gli observers in se dell'evento, delle funzioni che riportano anche loro un valore booleano
che indica se l'effetto dell'azione deve essere eseguito oppure no.

gli input avvengono in precedenza all'effetto e servono per fornire all'effetto o ad altri input dell'azione
dei parametri dall'estenro dell'azione.
questo valori possono essere recuperati in due modi diversi:
- richiesta dal giocatori, precedemente filtrati da una serie di filtri definiti dallo sviluppatore
- calcolati automaticamente tramite il filtro definito

gli effetti sono la funzione vera a propria dell'azione, ha un tipo di ritorno void, fa uso degli input per 
eseguire una certa logica.

oltre alle azioni, menziono anche le funzioni che sono principalmente utilizzate per raggrupare il codice 
evitandone la ripetizione seguendo il principio DRY ( don't repeat yourself ).
al contrario delle azioni si possono definire anche all'esterno delle classi nello scope globale e possono 
avere un tipo di ritorno diverso da void.
possono essere viste come azioni particolari in cui si possono definire solamente gli input e 
l'effetto.

per concludere si puó constatare che TGPL fornisce un set di strumenti sufficiente per la prototipizzazione di
un gioco, abilitando lo sviluppatore o designed nella progettazione di un nuovo gioco.




