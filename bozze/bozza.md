# Processore di gioco
si occupa di interagire con:
- gestore degli eventi
- registro dei riferimenti 
- aggiornamenti e gestione della gui
  
si occupa di eseguire le istruzioni, durante l'esecuzione del blocco di isturzioni
salva gli eventi che sono generati dal blocco nell'ordine in cui sono generati
per passarli al gestore degli eventi che gli restituisce in che ordine eseguire 
le azioni associate.

prima che un azione controlli il filtro, viene valutato il require se negativo
blocca la esecuzione.
si effettua il filtro su i trigger se positivo continua con il recupero degli input:
1. richiesta al registro dei riferimenti tutti i riferimenti di un certo tipo
2. avvio del filtro dei tipi
3. richiesta dell'input in base al modificatore 
    - auto: filtra e popola se tutto ok ( se non lista e piú risultati errore ect... , se lista filtra ma controlli sulla lista potrebbe dare errore in abse alla gui, come gestire l'input delle liste é questione della gui, appena il filtro sulla lista fallisce potrebbe smettere di chiedere input invece che fallire l'azione)
    - player: dopo aver filtrato tutte le possibli reference chiede all'utente di scegliere, l'operazione é equivalente a quella considerata per auto, cambia solo che non fallisce nel caso ci siano piú possibili risultati e un tipo non collezione perché chiede al giocatore di scegliere uno tra i vari tipi, direi mostrandogli i valori / nome sempre questione della gui
4. dopo aver ricevuto e popolato tutti gli input avvia l'effetto che potrebbe generare degli eventi a suo volta, se genera degli eventi sono salvati in un buffer apposito fino al termine del blocco dove poi saranno 
gestiti chiendendo informazioni al gestore degli eventi in base alla loro prioritá. 
5. alla fine della risoluzione degli eventi avviene un possibile passaggio di turno che é trattato come un evento speciale e quindi incodato a tutti gli eventi 

## Eventi predefiniti
- Passaggio di fase all'interno del turno
    ```
    {
        string turn
        string OldPhase;
        string NewPhase;
        player OldPlayer;
        player NewPlayer;
    }
    ```
- Attivazione di un nuovo turno: 
    ```
    {
        string Old;
        string New;
    }
    ```

l'evento serve solo per astrarre l'esecuzione di molteplici funzioni e fornire dei punti di estensione per 
programmi futuri senza dover intaccare dirattemente il codice sorgente.
aggiungere un flag speciale alla generazione dell'evento che permetta di eseguirli in modo asincrono con il 
default modalitá sincrona immediata stile funzione glorificata che permette di astrarre la chiamata di molteplici X funzioni o azioni diverse.
incodare subito dopo il blocco di codice permette di generare eventi all'interno di 
trigger / requires e input (non so se vale la pena peró si puó lasciare).
il blocco sincrono potrebbe avere problemi di performance e/o bloccare l'esecuzione 
se la fila degli eventi é molto lunga / ha un ciclo, quindi é da usare con piú 
attenzione.

# Gestore degli eventi
il gestore degli eventi contiene i metadati relativi agli eventi dell'intero programma, 
ovvero un dizionario Evento-Lista di listeners da testare con le loro prioritá, 
il gestore degli eventi si occupa quindi di fornire al processore una lista di funzioni da eseguire
con tutto il loro iter di require/triggers/inputs/effects ordinata.

diagramma come se fosse una dizionario:

evento1 - { handler, prio } , { handler, prio }, {hanlder,prio} ( lista in verticale )
evento2 - { handler, prio } , { handler, prio }
...

handler sono al Func<EventBase, bool> e sono le funzioni di filtro 

oggetto con 1 funzione => GetHandlers(EventMetadata metadata) -> List<Handlers>

# Reference Registrar
si occupa di mantenere tutte le referenze create tramite la new keyword e il loro conto, quando 
il conto scende a zero o segna la reference da trashare / la elimina dal registrar.
viene utilizzato come data container dal processore per recuperare in modo efficiente tutti gli oggetti 
di un certa tipologia per avere degli input efficienti.

classe con tre funzioni:
    - CreateRef()
    - RemoveRef()
    - GetTypeRefs(Type t)

fare che viene transpiled in c# e mostare alcuni dei pezzi come processore ( alcuni pezzi ), event manager e il reference registrar.

menzionare che il metodo di conversione del codice normale tipo
class => class
function => global class with static functions
utils.functions => utils global class 
global class => singleton pattern
group class => class con lista interna dei giocatori assegnati
local class +> class on riferimento interno dei giocatori assegnati
action => diventato una classe contentente tutte le informazioni necessarie:
- possiede liste di require func Func<bool>
- possisede liste dei triggers Func<EventBase, bool>
- possiede liste degli input proprietá 
- possiede la func effetto Action() 
- metodo Validate() che controlla se il require é valido
- metodo Triggered() chec ontrolla se il trigger é ok
- metodo RetrieveInputs() che contatta la guid / input injector per prendere i parametri di cui ha bisogno
- metodo Execute() per avviare l'effetto

i metadati sono considerati come attributi e vengono messi in cache nel processore / altro componente dopo la prima volta che vengono incontrati, alcuni metadati possono essere:
- i modificatori degli input

EventBase
{
    string Identifier;
}

usato per 

EventGenerated ( creato dal transpiler )
{
    // attributi dell'evento
}

classe => 
{
    attributi,
    funzioni sono passate come 
}

le funzioni azioni prendeono in input in InputInjector da cui possono ricavare 
un oggetto object che viene ricavato da un dizionare tramite il suo identificatore
e castato come variabile all'interno del codice 

esempio:
class Test
{
    action TestAction
    {
        input player Classe classe { ... }
        trigger OnChange { }
        effect { classe.Call(); }
    }
}

public class OnChange : EventBase { }
public class Test
{
    double test = 0;

    [EventTrigger(typeof(OnChange), 0)] // Type, priority
    [Input(typeof(Classe), "classe"]
    Action TestAction
    {
        List<Func<bool>> Require; // possono prendere solo globali / catturare gli attributi della classe
        List<Func<EventBase, bool>> Triggers;
        List<InputInjector, input> Inputs;
        Action<InputInjector> Action;
    }
}

il gestore eventi a tempo di avvio si salva tutte le azioni con EventTrigger attribute.

problema class Action generica: come recupero i suoi metadati per input
