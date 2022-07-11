# Reti combinatorie

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
		|---|---|---|---|
		| 0 | 0 | q  | $\bar{q}$ |
		| 0 | 1 | 0 | 1 |
		| 1 | 1 | - | - |
		| 1 | 0 | 1 | 0 |

	- Tabella di applicazione (che voglio ottenere e come)
		| q | q' | s | r |
		|---|---|---|---|
		|0|0|0|-|
		|0|1|1|0|
		|1|0|0|1|
		|1|1|-|0|

	- È necessario applicare alla rete altri due ingressi attivi bassi, /reset e /preclear:
    	- /preset = /preclear = 1, l'elemento funziona normalmente;
    	- /preset = 0, la rete va in S1;
    	- /preclear = 0, la rete si porta in S1.

- **D-Latch trasparente**:
  - $2$ Ingressi, $d$ ed $c$;
  - $1$ Uscita, $q$.
  - Quando la c è settata, la rete è trasparente, quando c vale 0, memorizza l'ultimo bit che è passato in d.
  - Sintetizzabili con un Latch SR e una rete combinatoria

- **D flip-flop**:
  - 
