# logisim-train-station
##  Specifica di progetto: Stazione Ferroviaria					
l progetto prevede la realizzazione di un circuito che simuli il funzionamento di un monitor delle partenze di una stazione ferroviaria.

Il circuito permetterà di:

- visualizzare il nome della destinazione del treno
- visualizzare dei LED che lampeggiano quando il treno sta per arrivare
- visualizzare l'orario in secondi, minuti e ore
- aumentare e diminuire il ritardo del treno

All'accensione del circuito si attiva un orologio che rappresenta un orario virtuale. 2 minuti prima dell'arrivo del treno, dei LED luminosi iniziano a lampeggiare. Quando l'orario del treno e l'ora attuale coincidono, il tabellone cambia scritta. Si può aumentare e diminuire il ritardo del treno fino a un massimo di 4 minuti. Quando si prova ad aggiungere ulteriore ritardo, il treno viene cancellato e si passa alla stazione successiva.
					
# Relazione 
## il funzionamento in sintesi
Il circuito viene attivato tramite un apposito tasto power e il clock attivo, ciò viene confermato da dei led verdi. quando il circuito è spento sono accesi dei led rossi. 

All’accensione del circuito si attiva un orologio che rappresenta un orario virtuale, e quando l’orario del treno in arrivo coincide con l’ora virtuale la scritta sul tabellone delle destinazioni cambia. 2 minuti prima dell’arrivo, quest’ultimo viene segnalato grazie a una matrice di led che lampeggia a intermittenza a ritmo di clock. 

l’utente può inoltre aumentare (e diminuire) il ritardo di un treno fino a un massimo di 4 minuti. Quando si prova ad aggiungere ulteriore ritardo, il treno viene cancellato e si passa alla stazione successiva.

# I circuti
## Accensione e spegnimento					
l’accensione/spegnimento del circuito è costituito da un flip-flop con input un tasto power e il segnale di clock. l’output del flip-flop entra dentro un decoder e può essere : 
0 ->  attiva dei led di colore rosso indicando che il circuito è spento.
1 -> accende il canale ON e dei led diventa di colore verde indicando che il circuito è acceso.

<img width="734" alt="power" src="https://github.com/kellygreta/logisim-train-station/assets/91347408/093442aa-3855-4308-9aba-1508ad8fe40b">

## Timer
il clock invia il segnale al primo counter, aumentando ogni secondo il proprio valore fino a nove, in questo modo sull’hex digit display vengono segnati dieci secondi, successivamente si resetta e riparte da zero facendo così scattare il secondo counter che aumenta il proprio valore fino ad un massimo di cinque indicando così che sono passati 60 secondi.
lo stesso ragionamento vale per il counter di minuti e delle ore.	

<img width="550" alt="timer" src="https://github.com/kellygreta/logisim-train-station/assets/91347408/550460ac-0c0a-4a13-b4e0-4d90c5d500fe">

## Next
<img width="777" alt="next" src="https://github.com/kellygreta/logisim-train-station/assets/91347408/6cb9488f-48d6-486e-ae78-d30b6e32ccf3">

INPUT(turchese)
- orario in uscita dalla ROM
- decine dei secondi dell’orario virtuale
-  unità dei secondi dell’orario virtuale
-  skip
-  clock

OUTPUT(rosso)
- stazione successiva
- indice per la ROM
- led

## prossima stazione
il circuito ha l’obiettivo di cambiare la scritta del tabellone confrontando con un comparator l’orario virtuale, esteso di segno da 4 bit a 6, a cui bisogna moltiplicare per 10 il numero delle decine dei secondi, e sommato con le unità dei secondi sempre in uscita dall’orario virtuale, con l’orario salvato nella ROM.  
Se i due orari coincidono, o viene attivato il segnale di skip, si aumenta un counter che va da 0 a 2 e cambia la scritta e un counter che va da 0 a 11 il cui segnale andrà a incidere sulla scelta del prossimo orario nella ROM.

## in arrivo
se l’orario virtuale, sottratto di 3 minuti, è minore dell’orario salvato nella ROM, il segnale va in and con il clock, creando delle intermittenze a fronte di clock che entreranno nei led per segnalare l’arrivo imminente del treno.

## Orario

il circuito ha l’obiettivo di selezionare l’orario da confrontare con con l’orario virtuale. Un segnale seleziona un orario salvato nella ROM secondo questa tabella oraria:  a cui può essere aggiunto un altro input proveniente dal circuito di ritardo.

## Ritardo
## LED 

