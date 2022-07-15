# Reti combinatorie

- **Tri-state**:
  - $2$ ingressi, $in$ ed $en$;
  - $1$ uscita.
  - Tabella di verità:

	| In | En | Out |
	|---|---|---|
	| 0 | 0 | Z |
	| 1 | 0 | Z |
	| 0 | 1 | 0 |
	| 1 | 1 | 1 |

- **Decoder**:
	- $N$ ingressi: codifica in base $2$ dell'uscita;
	- $2^N$ uscite: $1$ nell'uscita, $0$ altrove.

- **Demultiplexer**:
	- $N+1$ ingressi: ($x$) ed $N$ variabili di comando, codifica in base $2$ dell'uscita;
	- $2^N$ uscite: ($x$) dalla porta corrispondente.

- **Multiplexer**:
	- $N+2^N$ ingresssi: $2^N$ ($x$), $N$ variabili di comando, codifica in base $2$;
	- $1$ uscita: $z$ prende il valore di ($x_i$) selezionato.

# Reti sequenziali
- **Latch SR** detto anche **flip-flop SR**:
	- $2$ Ingressi, $s$ ed $r$ rispettivamente "set" e "reset", entrambe attive alte;
	- $2$ Uscite, $q$ e $qN$, la seconda la negazione della prima.
	- Tabella di verità:
		| s | r | q | qN |
		|---|---|---|----|
		| 0 | 0 | q | $\bar{q}$ |
		| 0 | 1 | 0 | 1  |
		| 1 | 1 | - | -  |
		| 1 | 0 | 1 | 0  |

	- Tabella di applicazione (che voglio ottenere e come):
		| q | q' | s | r |
		|---|----|---|---|
		| 0 | 0  | 0 | - |
		| 0 | 1  | 1 | 0 |
		| 1 | 0  | 0 | 1 |
		| 1 | 1  | - | 0 |

	- È necessario applicare alla rete altri due ingressi attivi bassi, /reset e /preclear:
    	- /preset = /preclear = 1, l'elemento funziona normalmente;
    	- /preset = 0, la rete va in S1;
    	- /preclear = 0, la rete si porta in S1.

- **D-Latch trasparente**:
  - $2$ Ingressi, $d$ ed $c$;
  - $1$ Uscita, $q$.
  - Quando la $c$ è settata, la rete è trasparente, quando $c$ vale $0$, memorizza l'ultimo bit che è passato in $d$.
  - Sintetizzabili con un Latch SR e una rete combinatoria.
  - Il pilotaggio deve avvenire nel rispetto di alcune regole:
    - Si deve tenere $d$ costante a cavallo della transizione di $c$ da $1$ a $0$. I tempi per cui deve essere costante (prima e dopo) sono chiamati rispettivamente $T_{setup}$ e $T_{hold}$;

- **D flip-flop**:
  - $2$ Ingressi, $d$ ed $p$;
  - $1$ Uscita, $q$.
  - Quando $p$ ha un fronmte in salita, memorizza d, attendi un po' ed adegua l'uscita.
  - Si prende un D-latch, e si premette alla variabile $c$ un formatore di impulsi, in modo tale che, al fronte di salita di $p$, il D-latch vada brevemente in trasparenza e memorizzi $d$. Poi si ritarda l'uscita di un ritardo $\Delta$ maggiore dell'intervallo del $P+$.
  - Il pilotaggio deve avvenire nel rispetto di alcune regole:
    - A cavallo del fronte di salita di $p$, la variabile $d$ deve rimanere costante. I nomi dei tempi per cui deve rimanere costante prima e dopo il fronte di salita sono rispettivamente $T_{setup}$ e $T_{hold}$;
    - Tra le due transiazioni in salita della variabile $p$ deve passare abbastanza tempo perché l'uscita si possa adeguare;
  - Il ritardo con cui si adegua l'uscita, misurato a partire dal fronte di salita di $p$, si chiama $T_{prop}$ ed è $T_{prop} > T_{hold}$, da cui la non trasparenza della rete;
  - L'uscita di un D-FF **non** oscilla mai;
  - Per la sintesi si possono usare anche due D-latch, in configurazione master-slave:
    - $p = 0$, il **master campiona e lo slave conserva**;
    - $p = 1$, il **master conserva e lo slave campiona**;
***
- **Memorie RAM statiche** (RAM statiche o S-RAM):
  - Sono composte da D-latch montati a matrice: una riga costituisce una locazione di memoria che può essere sia **letta o scritta** ma **non** simultaneamente;
  - Dal punto di vista dell'utente, una memoria è dotata dei seguenti collegamenti:
    - un certo numero di **fili di indirizzo**, che sono ingressi;
    - un certo numero di **fili di dati**, che sono fili di ingresso/uscita (andranno forchettati con porte tri-state);
    - Due segnali **attivi bassi** di **memory read** e **memory write**;
    - Un segnale attivo basso di **select**, che viene attivato quando la memoria è selezionata: quando $/s$ vale $1$, la memoria è insensibile a tutti gli ingressi; quando vale $0$, la memoria è selezionata e quindi reagisce agli ingressi.
    - Il comportamento della memoria è deciso da $/s, /mw, /mr$:
  
		| /s | /mr | /mw | Azione                                                     | b | c |
		|----|-----|-----|------------------------------------------------------------|---|---|
		| 1  | -   | -   | Nessuna azione (memoria non selezionata)                   | 0 | 0 |
		| 0  | 1   | 1   | Nessuna azione (memoria selezionata, nessun ciclo in corso | 0 | 0 |
		| 0  | 0   | 1   | Ciclo di lettura in corso                                  | 1 | 0 |
		| 0  | 1   | 0   | Ciclo di scrittura in corso                                | 0 | 1 |
		| 0  | 0   | 0   | Non definito                                               | - | - |

  - Vediamo come è realizzata:
  	- Disegnare la matrice di D-latch. Una riga è una locazione, bia 0 a destra, bit 3 a sinistra;
  	- Le uscite dei D-latch dovranno essere selezionate una riga alla volta, per finire sui fili dei dati in uscita. Ci vuole un multiplexer per ogni bit, in cui:
      	- gli ingressi sono le uscite dei D-latch;
      	- le variabili di comando sono i **fili di indirizzo**;
  	- Le uscite di ciascuno dei multiplexer vanno bloccate con porte tri-state. Dovranno essere abilitate quando sto leggendo dalla memoria;
  	- Per quanto riguarda gli ingressi: posso portare a ciascuna colonna di D-latch i fili di dati sull'ingresso $d$. Basta fare in modo che quando voglio scrivere, soltanto una riga di D-latch sia abilitata ($c = 1$).
  - Descriviamo la temporizzazione del ciclo di lettura della memoria:
    - Ad un certo istante, gli indirizzi si stabilizzano al valore della cella che voglio leggere ed arriva il comando di $/mr$.
	- Nel frattempo, in quando funzione combinatoria di altri bit, $/s$ balla e arriva con un po' di ritardo.
	- Quando sia $/mr$ che $/s$ sono a $0$, le porte tri-state vanno in conduzione e i multiplexer sulle uscite vanno a regime.
	- Da quel punto in poi i dati sono buoni.
	- Quando $/mr$ viene tirato su, i dati tornano in alta impedenza e a quel punto gli indirizzi di $/s$ possono ballare a piacimento, tanto non succede niente.
  - Descriviamo la temporizazione del ciclo di scrittura della memoria:
    - Devo attendere che $/s$ e gli indirizzi siano stabili prima di portare giù $/mw$.
    - I dati possono ballare a piacimento ma devono essere stabili prima di portare giù $/mw$, poiché corrisponde, con un minimo ritardo, al fronte di discesa di $c$ all'interno dei D-latch.

- **Collegamento al bus e maschere**:
  - I fili di indirizzo della memoria provengono da un bus indirizzi, dove il processore ne impostano il valore.
  - Il piedino $/s$ di un modulo di RAM serve appunto a poter realizzare uno spazio di memoria grande usando moduli di memoria più piccoli.
  - Supponiamo di avere un bus indirizzi a 32 bit e di voler montare un modulo di RAM 256Mx8 bit a partire dall'indirizzo 0xE0000000. Il modulo avrà 28 fili di indirizzo ed un filo di select $/s$ e dovrà rispondere agli indirizzi nell'intervallo [0xE0000000 - 0xEFFFFFFF]:
    - I 28 fili di indirizzo meno significativi del bus andranno in ingresso al modulo di RAM;
    - I restanti 4 fili di indirizzo più significativi andranno in ingresso ad una maschera, che genera il select per il modulo di RAM.
  - La maschera deve riconoscere la configurazione di bit richiesta (0xE == B1110), pertanto deve essere $/s = \bar{a_{31}}+\bar{a_{30}}+\bar{a_{29}}+a_{28}$.

- **Memomorie Read-Only** (ROM):
  - Sono circuiti combinatori, infatti cisacuna locazione contiene dei valori costanti inseriti in modo indelebile.
  - Costituiscono la parte non volatile dello spazio di memoria.
  - Possono essere descritti come memorie RAM tolto tutto quello necessario alla scrittura dei dati.
  - I D-latch sono sostituiti da generatori di costante. Si distinguono 3 tipi:
    - PROM;
    - EPROM;
    - EEPROM;<br>
	A seconda del metodo di programmazione.

- **ROM programmabili**:
  - PROM: La matrice di connesione è fatta da fusibili, che possono essere fatti saltare in modo selettivo in modo da inserire in ciascuna cella il lvalore desiderato. [NON PUÒ ESSERE RIPETUTA]
  - EPROM: Le connessioni sono fatte non con fusibili, ma con dispositivi elettronici, che sono programmabili per via elettrica e cancellabile tramite esposizione a raggi ultravioletti. [PUÒ ESSERE RIPETUTA]
  - EEPROM: Possono essere programmate e cancellate tramite segnali elettrici appositi.
***
# Reti sequenziali sincronizzate (RSS)
Si evolvono soltanto in corrispondenza di istanti temporali ben precisi, detti _istanti di sincronizzazione_.<br>
L'evento che sincornizza è, di solito, il fronte di salita del clock;

- **Registri**:
  - Definisco un registro a $W$ bit come una collezione di $W$ D-flip-flop positive-edge-triggered, che hanno:
    - ingressi $d_i$ ed uscite $q_i$ separati (indipendenti);
    - ingresso $p$ comune (di sincronizzazione), che posso evitare di annoverare tra gli ingressi.
  - Un registro può essere visto come una rete sequenziale sincronizzata, in cui l'ingresso $p$ funge da segnale di sincronizzazione.
- **Temporizzazione RSS**:
  - L'unica regola di pilotaggio che dobbiamo garantire è:
    - Detto $t_i$ l'i-esimo fronte di salita del clock, lo stato di ingresso ai registri deve essere stabile in [$t_i - T_{setup}$ , $t_i + T_{hold}$], $\forall i$.
  - Questa regola ci dice che non posso fare il clock veloce quanto voglio. <br>
  Se voglio che un stato di ingresso attraverso le reti combinatorie, concorra a formare gli ingressi ai registri, dovrò dare il tempo a chi pilota la rete di:
    - produrre un nuovo stato di ingresso;
    - farlo 
    - arrivare, attraverso le reti combinatorie, fino agli ingressi dei registri.
  - Definiamo i seguenti ritardi:
    - $T_{in-to-reg}$: il tempo di attraversamento della più lunga catena fatta di sole reti combinatorie che si trovi tra un piedino di ingresso fino all'imgresso di un registro;
    - $T_{reg-to-reg}$: (...) l'uscita di un registro e l'ingresso di un registro;
    - $T_{in-to-out}$: (...) un piedino di ingresso e un piedino di uscita;
    - $T_{reg-to-out}$: (...) l'uscita di un registro e un piedino di uscita.
  - Ho $3$ vincoli temporali:
    - ingressi costanti [$t_i-T_{setup}$ , $t_i+T_{hold}$];
    - vincolo di pilotaggio in ingresso: chi pilota gli ingressi deve avere almeno un $T_{a-monte}$ per poterli cambiare;
    - vincolo di pilotaggio in uscita: chi usa le uscite deve averle stabili per un tempo $T_{a-valle}$ per poterci fare qualcosa.
<br>
<br>

![](img/1.png)

<br>


- **Contatori**:
  - Un contatore è una RSS il cui stato di uscita può essere visto come un numero naturale ad $n$ cifre in base $\beta$, secondo una qualche codifica.
  - Ad ogni ciclo di clock, il contatore fa la seguente cosa:
    - incrementa di 1 (modulo $\beta^n$), il valore di uscita (contatore up);
    - decrementa di 1 (modulo $\beta^n$), il valore di uscita (contatore down);
    - incrementa o decrementa di 1 (modulo $\beta^n$), il valore di uscita a seconda del valore di una variabile di comando (contatore up/down).
- **Registri multifunzionali**:
  - È una rete che, all'arrivo del clock, memorizza nel registro stesso una tra K funzioni combinatorie possibili, scelta impostando un certo numero di variabili di comando $W = \lceil \log_2K \rceil$. Si realizza con un multiplexer a $K$ ingressi;
- **Modello di MOORE**:
  1. un insieme di $N$ variabili logiche in ingresso;
  2. un insieme di $M$ variabili logiche di uscita;
  3. un _meccanismo di marcatura_, che ad ogni istante marca uno stato interno presente, scelto tra un insieme finito di $K$ stati interni $S = \{S_0,...,S_{K-1} \}$;
  4. Una legge di _evoluzione del tempo_ del tipo $A : X \times S \rightarrow S$, che mappa quindi una coppia (stato di ingresso, stato interno) in un nuovo stato interno;
  5. Una legge di _evoluzione del tempo_ del tipo $B : S \rightarrow Z$, che decide lo stato di uscita basandosi sullo stato interno;
  6. La rete riceve segnali di sincronizzazione, come transizioni da 0 a 1 del segnale di clock;
  7. Si adegua alla seguente **legge di temporizzazione**:
     - "Dato $S$, stato interno marcato ad un certo istante, e dato $X$ ingresso ad un certo istante immediatamente precedente all'arrivo di un segnale di sincronizzazione,
     - individuare il nuovo stato interno da marcare $S' = A(S,X)$;
     - attendere $T_{prop}$ dopo l'arrivo del segnale di sincronizzazione;
     - promuovere $S'$ al rango di stato interno marcato;
     - E, inoltre, individuare continuamente $Z=B(S)$ e presentarlo in uscita.

|$S[t_{i+1}] = A(X[t_i], S[t_i])$|
|---|
|$Z[t_i] = B(S[t_i])$|

<br>

![](img/2.png)

<br>

- **Flip-Flop JK**:
  - Il FF JK è una rete sequenziale sincronizzata con due ingressi ed un'uscita che, all'arrivo del clock, valuta i suoi due ingressi $j$ e $k$ e si comporta come segue:

    | jk | Azione in uscita |
    |----|------------------|
    | 00 | Conserva         |
    | 10 | Setta            |
    | 01 | Resetta          |
    | 11 | Commuta          |
  - Tabella di applicazione:

    | q  | q' | j  | k  |
    |----|----|----|----|
    | 0  | 0  | 0  | -  |
    | 0  | 1  | 1  | -  |
    | 1  | 0  | -  | 1  |
    | 1  | 1  | -  | 0  |

  - FF JK è una rete di Moore poiché l'uscita non è funzione combinatoria degli ingressi ma del registro.

- **Modello di Mealy**:
  - Nel modello di Moore, l'uscita è funzione soltanto dello stato interno presente, tramite la legge $B:S \rightarrow Z$. Se si consente a tale legge di essere più generale, scrivendo $B : X \times S \rightarrow Z$, si ottengono reti realizzate secondo il modello di Mealy.
  - Il vantaggio di questo modello è che al variare dell'ingresso posso produrre un nuovo stato di uscita senza dover aspettare il succesivo fronte del clock.

|$S[t_{i+1}] = A(X[t_i], S[t_i])$|
|---|
|$Z[t_i] = B(X[t_i], S[t_i])$|

<br>

![](img/3.png)

<br>

- **Modello di Mealy ritardato**:
  - Si parte da una rete di Mealy, e si mette in uscita un registro OUTR. In questo modo, le uscite:
    - variano sempre all'arrivo del clock, dopo un tempo $T_{prop}$ ;
    - variano in maniera netta, senza oscillazioni;
    - rimangono stabili per l'intero ciclo di clock;
    - sono non trasparenti.
  - Le proprietà:
    - Ha una legge di evoluzione del tempo del tipo $A:X \times S \rightarrow S$, che mappa quindi una coppia (stato d'ingresso, stato interno) in un nuovo stato di uscita;
    - Una legge di evoluzione nel tempo del tipo $B: X \times S \rightarrow Z$, che mappa quindi una coppia (stato ingresso, stato interno) in un nuovo stato interno;
    - Si adegua alla seguente legge di temporizzazione:<br>
    Dato $S$, stato interno presente (marcato) ad un certo istante, e dato $X$ stato di ingresso ad un certo istante precedente l'arrivo di un segnale di sincronizzazione,
      1. individuare sia il nuovo stato interno da marcare $S'=A(S,X)$, sia il nuovo stato di uscita $Z = B(S,X)$;
      2. attendere $T_{prop}$ dopo l'arrivo del segnale di sincronizzazione;
      3. promuovere $S'$ al rango di stato interno marcato, e promuovere $Z$ al rango di nuovo stato di uscita.

|$S[t_{i+1}] = A(X[t_i], S[t_i])$|
|---|
|$Z[t_{i+1}] = B(X[t_i], S[t_i])$|

<br>

![](img/4.png)

<br>

***

# La struttura del calcolatore
Scopo del prossimo blocco di lezioni è la descrizione in verilog di un sistema-calcolatore completo di processore, memoria, interfacce e dispositivi di ingresso-uscita.
- **Il sottostistema di I/O**: 
  - Si occupa di gestire la codifica delle informazioni ed il loro scambio con il mondo esterno;
  - All'interno di questo sottosistema distinguiamo interfacce e dispositivi;
    - Le interfacce gestiscono i vari dispositivi;
    - I dispositivi effettuano la codifica;
- **La memoria principale**:
  - Contiene in ogni momento le istruzioni e i dati che il processore elabora;
- **Il processore**: 
  - Ciclicamente preleva un'istruzione dalla memoria e la esegue, finché non trova "HLT";
  - Al reset deve partire in modo consistente, cioé:
    - Deve iniziare a leggere la memoria da una locazione ben precisa;
    - In quella locazione ci deve essere già scritto del codice, in maniera indelebile;
- **La rete di interconnessione**:
  - Mette in comunicazione tutti questi moduli, trasportando i segnali generati da uno verso l'altro;

## Visione da parte del programmatore
- La memoria appare come uno spazio lineare di $2^{24}$ locazioni di un byte, per un totale di $16$ MB;
- Lo spazio di I/O, cioè l'insieme dei registri d'interfaccia che il processore può teoricamente indirizzare appare al programmatore come uno spazio lineare di $2^{16}$ locazioni o porte;
- Il processore ha 3 tipi di registro:
  - Registri accumulatore: destinati a contenere operandi di elaborazione. [AH ed AL]
  - Registro dei flag: sarà ad $8$ bit e di questi 4 sono:
    - CF(0);
    - ZF(1);
    - SF(2);
    - OF(3);
  - Registri puntatore:
    - IP (instruction pointer): contiene l'indirizzo della prossima istruzione da eseguire;
    - SP (stack pointer): contiene l'indirizzo del top della pila;
    - DP (data pointer): contiene l'indirizzo di operandi, a seconda della modalità di indirizzamento;

## Linguaggio macchina 
Istruzioni assembler:
$$OPCODE\ source,\ destination$$
$source$ potrebbe mancare.

- Per le **istruzioni operative**:
  - Indirizzamento a registro: uno o entrambi gli operandi sono registri;
  - Indirizzamento immediato: l'operando $sorgente$ è specificato direttamente nell'istruzione come costante (es: $0x10$);
  - Indirizzamento di memoria:<br>
  valido per sorgente **o** destinatario, MAI entrambi
    - diretto: l'indirizzo è specificato direttamente nell'istruzione;
    - indiretto: la locazione di memoria ha indirizzo contenuto nel registro DP;
  - Indirizzamento porte I/O:
    - si indirizzano in modo diretto, specificando l'offset della porta dentro l'istruzione stessa;
- Per le **istruzioni di conttrollo**:
  - Alterano il flusso dell'esecuzione del programma, che normalmente procederebbe in sequenza;

Ciascuna istruzione macchina è lunga almeno un byte, che codifica:
- Il tipo di operazione, rilevante in fase di esecuzione;
- Il modo in cui si devono recuperare gli operando, detto formato dell'istruzione, che è invece rilevante in fase di fetch;

I formati possibili per il processore sono 8, il che vuol dire che nel primo byte:
- i primi $3$ bit codificano il formato;
- i restanti cinque codificano il codice operativo [32 possibili $OPCODE$];

In particolare, i formati sono:
- F0 (000): pesano **1 byte** e rientrano tutte le istruzioni per le quali il processore non deve compiere nessuna azione per procurarsi gli operandi, in quanto:
  - gli operandi sono regitri, oppure;
  - le istruzioni non hanno operandi (HTL, NOP, RET);
- Formato F2 (010): pesano **1 byte** e raggruppano le istruzioni in cui l'operando sorgente si trova in memoria, indirizzato in maniera indiretta tramite il registro puntatore DP;
- Formato F3 (011): pesano **1 byte** e raggruppano le istrusioni in cui l'operando destinatario è indirizzato in modo indiretto, usando il registro puntatore DP;
- Formato F4 (100): pesano **2 byte** e raggruppano le istruzioni in cui l'operando sorgente è indirizzato in modo immediato, e sta du 8 bit;
- Formato F5 (101): pesano **4 byte** e raggruppano tutte le istruzioni in cui l'operando sorgente è indirizzato in modo diretto;
- Formato F6 (110): pesano **4 byte** e raggruppano tutte le istruzioni in cui l'operando destinatario è in memoria, indirizzato in modo diretto.
- Formato F7 (111): pesano **4 byte** e raggruppano tutte le istruzioni di controllo (CALL, JMP, Jcon) in cui ho un indirizzo di salto;
- Formato F1 (001): raggruppa tutte le istruzioni che mancano. Istruzioni relative allo spazio di I/O, per le quali è necessario prelevare in memoria l'indirizzo a 16 bit della porta di I/O sorgente/destinatario.

## Architettura del calcolatore
- **Fili di indirizzo**: 24, sono uscite per il processore, il quale imposterà gli indirizzi delle locazioni di memoria o delle porte di I/O dove vuole leggere e scrivere, ed ingressi per il resto del mondo;
- **Fili di dati**: tutti attivi bassi, /mr, /mw (per leggere e scrivere in memoria), /ior, /iow (per leggere e scrivere in I/O); sono uscite per il processore ingressi per gli altri;
- **Segnale di clock**;
- **Fili di comunicazione tra la memoria video e l'adattatore grafico**;

## Spazio di memoria
Lo spazio di memoria fisica, grande 16MB, è implementato con tecnologia RAM, in parte EPROM ed in parte come memoria video;
La logica combinatoria che genera il segnale di abilitazione (/s) per un modulo a partire dagli indirizzi prende il nome di maschera:
- Sul bus non c'è nessun gilo di select;
- Il chip RAM copre anche gli indirizzi coperti da EPROM e dalla memoria video.

## Spazio di I/O
Lo spazio di I/O è realizzato tramite interfacce, che fungono da raccordo tra il bus e i dispositivi di I/O.<br>
Saranno simili anche le temporizzazioni per i cicli di lettura e scrittura:
- in una RAM si può leggere e scrivere qualunque locazione;
- se un'interfaccia implementa una sola porta, non sono necessari i fili di indirizzo (basta /s);

Dal lato dispositivo, i collegamenti variano da interfaccia a interfaccia. Il motivo poer cui al bus si attaccano le interfacce, è duplice:
- i dispositivi hanno velocità molto diversa tra loro, e sono spesso più lenti del processore;
- i dispositivi hanno modalità di trasferimento dati molto diverse tra loro;

## Processore
Contiene un certo numero di registri, tra cui:
- STAR, registro di stato, essento il processore una RSS;
- MJR;
- Instraction registers (OPCODE, SOURCE; DEST_ADDR), vengono riempiti in fase di fetch:
  - OPCODE: codice operativo dell'istruzione;
  - SOURCE: l'operando sorgente;
  - DEST_ADDR: indirizzo dell'operando destinatario;
- Ho dei registri che sostengono le uscite, come deve essere in una RSS;
- Un registro DIR, per abilitare la tri-state quando il processore deve effettuare scritture sul bus;
- Dei registri di appoggio APPx e NUMLOC, che servono per i cicli di lettura/scrittura;

Al reset, si inizializzano:
- i registri IP ed F, in modo da partire con un'evoluzione consistente;
- tutti i registri che hanno a che fare con variabili di controllo del bus dovranno essere inizializzati in modo coerente: /MR, /MW, /IOR, /IOW dovranno contenere tutti 1;
- I fili di dati vanno posti in alta impedenza. DIR deve contenere 0;
- STAR verrà inizializzato con l'etichetta del promo statement della fase di fetch;

Fase di Fetch:
- Preleva un byte, quello indicato da IP;
- Incrementa IP, modulo $2^{24}$;
- controlla che quel byte corrisponda ad un opcode, sennò di ferma;
- inserisce il byte letto nel registro OPCODE, e valuta il formato dell'istruzione, a seconda di questo:
  - si procura un operando sorgente a 8 bit e lo inserisce in SOURCE;
  - si procura l'indirizzo dell'operando destinatario e lo inserisce in DEST_ADDR;

## Lettura e scrittura in I/O
Durante la fase di fetch, il processore legge in memoria; durante quella esecuzione, il processore dovrà leggere e scrivere in memoria (MOV) o nello spazio di I/O (IN,OUT);
I cicli di scrittura e lettura nello spazio di I/O sono simili, ma non identici: ci sono importanti differenze:
- Gli indirizzi sono a 16 bit, e si usano /ior e /iow non /mr e /mw;
- Nel ciclo di lettura gli indirizzi devono esser eprinti un clock prima del comando di lettura;
- Nel ciclo di scrittura i dati devono essere già pronti sul fronte di discesa si /iow;

***
# Interfacce
Sono di 3 tripi:
- **Parallele**, che sono in grado di inviare/ricevere $1$ byte alla volta;
- **Seriali**, che sono in grado di inviare/ricevere $1$ bit alla volta;
- **conversione analogica/digitale [lenta] e digitale/analogico [veloce]**, che trasformano gruppi di bit in tensioni e viceversa;

## Interfacce parallele
Prendiamo il tipo più semplice di interfaccia parallela in **ingresso**.<br>
Un'interfaccia che da corpo ad una sola porta, dalla quale si può soltanto leggere.<br>
Dal punto di vista dei collegamenti con il processore essa avrà bisogno di:
- un segnale di **select**, al quale va l'uscita della maschera, tramite la quale il progettista decide quale deve essere l'offset della porta di ingresso dell'interfaccia;
- un filo di /ior;
- otto fili di dati;
- nessun filo di indirizzo, poiché ha;
- una porta sola;

Dal lato del dispositivo con il quale si interfaccia, ci saranno 8 fili di ingresso, che chiamiamo _"byte_in"_, tramite i quali il dispositivo interno fa arrivare i dati. <br>
Questi dati saranno inseriti dal dispositivo nel registro _RBR_.

Dualmente, il tipo più semplice di interfaccia parallela di uscita è un'interfaccia che da corpo ad una sola porta, nella quale si può solo scrivere; dal punto di vista dei collegamenti col processore, abbiamo:
- un segnale di **select**, al quale va l'uscita della maschera tramite la quale il progettista decide quale deve essere l'offset della porta di uscita dell'interfaccia;
- un filo di /iow;
- otto fili di dati;
- nessun filo di indirizzo, poiché ha;
- una porta sola;

Dal lato del dispositivo con il quale si interfaccia, ci saranno 8 fili di uscita, che chiamiamo _"byte_out"_, tramite i quali il interfaccia fa arrivare i dati al dispositivo. <br>
Questi dati saranno scritti dal processore nel registro _TBR_.

### Intefaccie parallele con HandShake - Ingresso
Sono dotate di meccanismi di sincronizzazione: dal lato del processore, hanno un flag di ingresso pieno, che il processore accede a controllo di programma. <br>
Dal lato del dispositivo, hanno dei fili di handshake in più, uguali a quelli già visti nell'esempio di "produttore e consumatore"; In questo l'**interfaccia è un consumatore** rispetto al dispositivo. <br>
L'interfaccia di ingresso avrà solo /ior (non /iow) ed un filo di dati per distinguere gli accessi a RSR e RBR;<br>
La RC interna deve, per prima cosa, generare i segnali di abilitazione per le tri-state quando il processore accede in lettura a RBR o RSR.
Le due tri-state non sono mai in conduzione contemporaneamente, e sono entrambe in alta impedenza quando non ci sono accessi all'interfaccia.


### Intefaccie parallele con HandShake - Uscita
Il flag FO vale uno quando nel registro TBR può essere scritto un nuovo dato. Ci vuole un filo di indirizzo, perché ci sono due registri, e quindi è necessario distinguerli.<br>
Vediamo come è fatta l'interfaccia al suo interno:
- C'è una rete combinatoria che ha un ruolo analogo a quella dell'interfaccia di ingresso. L'unica differenza è che stavolta $e_B$ non serve ad abilitare le tri-state, perché i dati vanno nella direzione opposta.
- Si gestisce prima l'handshake con il processore e, finito quello, quello con il dispositivo; si noti che il contenuto di TBR balla, ma /dav viene tenuto a 1, quindi il dispositivo non può leggerlo.

## Interfaccia parallela di ingresso-uscita
In questo caso i registri RBR e TBR sono mappati sullo stesso indirizzo interno, e sono acceduti rispettivamente in lettura e scrittura. I due flag FI e FO danno corpo a due bit in un unico registro di controllo, detto RTSR.

# Interfaccia seriale start/stop
- Riceve dal bus un byte (perché il processore scrive byte in opportuni registri di I/O) e trasmette all'esterno sequenza di bit;
- Riceve dall'esterno sequenze di bit e presenta al processore byte componendo dquelle sequenze di bit in un registro che si possa leggere;

Da un punto di vista fisico, il mezzo trasmissivo sul quale esce l'informazione si presenta come un insieme di due linee: una linea di massa, che funge da riferimento, ed una che porta una tensione riferita a massa; Sono leciti due valori sulla linea:
- **Marking**, 1 logico;
- **Spacing**, 0 logico.

La trasmissione di un bit consiste nel tenere la linea in uno stato di marking o spacing per un determinato tempo T, detto tempo di bit;
Un insieme di bit scambiato si chiama trama o frame; per adesso supponiamo che una trama sia costituita da un byte, trasmesso dal meno significativo al più.

Affinché il mezzo trasmissivo possa sotenere trasmissione in entrambe le direzioni contemporaneamente, sono necessari tre fili, due dei quali portano le tensioni riferite a massa;

Come funziona la sincronizzazione? È possibile utilizzare 2 metodi:
- Condividere un clock: è quello che si fa sul bus;
- Aggiungere linee dedicate alla sincronizzazione (/dav, rfd);

Per implementare una di queste soluzioni ci vogliono più di due fili, e noi vogliamo usarne soltanto due. Il problema si risolve in questo modo:
- Entrambi i lati della comunicazione devono concordare sul tempo di bit T;
- Entrambi i lati della comunicazione devono concordare sul numero di bit di cui si compone una trama;
- Una trama deve essere resa riconoscibile. In particolare, è necessario che entrambi concordino sul modo di rendere noto che una trama è iniziata:
  - La linea sta, normalmente, in uno stato di marking; Quando voglio iniziare la trasmissione di una trama, la porto nello stato di spacing. Ciò significa che ogni trama inizia con il bit 0, che non fa parte della trama: bit di start;
  - Analogamente, quando ho trasmesso l'ultimo bit di una trama, devo riportare la linea in uno stato di marking per almeno un tempo di bit: bit di stop;

Per campionare i bit, mi conviene farlo a circa metà del tempo di bit, per evitare di incappare in errori dovuti alla salita o alla discesa; quindi dovrò:
- aspettare 3/2 T da quando la linea transisce a spacing per la prima volta (bit di start, più metà primo da campionare);
- campionare il valore della linea;
- aspettare di nuovo T, e così via per tutti i bit utili della trama;

> Non si può aumentare a dismisura i bit all'interno di una trama, poiché si creerebbero problemi dovuti all'imprecisione dei clock.

<br>

## FI e FO

![FI e FO](img/5.png)

<br>

# Conversione analogico/digitale e digitale/analogico
Nel mondo fisico, di norma, l'informazione è associata a grandezze analogiche che variano con continuità; <br>
All'interno del computer, invece, le informazioni sono associate a stringhe di bit, cioé grandezze digitali, che variano in modo discreto.

La tensione *v* da convertire sarà su una scala di FSR volts (Full-Scale Range). Il numero *x* nel quale la tensione sarà convertita è su N bit; tipicamente 8 o 16, mentre FSR tra 5 e 30.

Si possono distinguere:
- la **conversione unipolare**: $v \in [0,\ FSR],\ x \in [0,\ 2^N-1]$;
- la **conversione bipolare**: $v \in [-\frac{FSR}{2},\ \frac{FSR}{2}],\ x \in [-2^{N-1},\ 2^{N-1}-1]$;

Definiamo $K = \frac{FSR}{2^N}$, costante di proporzionalità tra i due intervalli. Una conversione *ideale*, sarebbe $v = K \cdot x$.<br>
In realtà, dovremo accontentarci di $|v-K \cdot x| \leq err$; con $err$ detto errore di conversione.

Gli errori di conversione possono essere:
- imprecisione a livello circuitale: i convertitori sono circuiti con resistenze, fili, reattanze, che non si comportano in maniera ideale; Ci sarà un'imprecisione dovuta alla non idealità dei componenti. Presente in **entrambi**, si chiama problema di **non linearità**;
- **Quantizzazione**: nella coversione **A/D** (e soltanto in questa!), devo convertire una grandezza continua in una discreta. Facendo questo si perde dell'informazione a causa dell'arrotondamento.

L'errore di non **linearità** deve essere più piccolo di $\frac{K}{2}$.<br>
L'errore massimo di **quantizzazione** è indipendente dalla natura del convertitore (A/D). Data una costante K, è pari a $\frac{K}{2}$. Infatti, se divido il FSR in $2^N$ intervalli larghi $K$ e converto tutto un intervallo nello stesso numero, la conversione sarà:
- esatta, per la tensione al centro dell'intervallo;
- errata, di $\plusmn \frac{K}{2}$ per le tensioni agli estremi.

Riassumento, abbiamo:
- conversione D/A: $err \leq \frac{K}{2}$ (soltanto errore di linearità);
- conversione A/D: $err \leq \frac{K}{2} + \frac{K}{2} = K$ (errore di non linearità e di quantizzazione);

A livello di tempi di risposta, i convertitori hanno le seguenti performance:
- D/A: essendo circuiti combinatori, sono velocissimi;
- A/D: hanno tempi di risposta variabili, perché sono circuiti sequenziali che possono avere architetture diverse.

> I convertitori bipolari lavorano rappresentando i numeri interi in traslazione, ossia l'intero $x$ è rappresentato $X = x+2^{N-1}$: per trasformarlo in complemento a 2, basta negare il bit più significativo.

