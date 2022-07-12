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