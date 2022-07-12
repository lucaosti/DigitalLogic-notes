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
  - Descriviamo la temporizzazione del ciclo di lettura della memoria.
    - 