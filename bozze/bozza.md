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

# Gestore degli eventi
il gestore degli eventi contiene i metadati relativi agli eventi dell'intero programma, 
ovvero un dizionario Evento-Lista di listeners da testare con le loro prioritá, 
il gestore degli eventi si occupa quindi di fornire al processore una lista di funzioni da eseguire
con tutto il loro iter di require/triggers/inputs/effects ordinata.

# Reference Registrar
si occupa di mantenere tutte le referenze create tramite la new keyword e il loro conto, quando 
il conto scende a zero o segna la reference da trashare / la elimina dal registrar.
viene utilizzato come data container dal processore per recuperare in modo efficiente tutti gli oggetti 
di un certa tipologia per avere degli input efficienti.