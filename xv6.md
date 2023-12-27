# Analisi comparativa tra Os161 e Xv6

#### Sources:
https://clownote.github.io/2021/02/27/xv6/Xv6-operating-system-organization/
https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf

## Introduzione

Questo studio si concentra principalmente sull'analisi del sistema operativo Xv6, confrontandolo con Os161. L'obiettivo di questa analisi comparativa è offrire una panoramica delle capacità di Os161 e Xv6, mettendo in evidenza le differenze e le somiglianze tra questi due sistemi operativi open source.

## Xv6

Così come Os161, Xv6 è un sistema operativo nato con scopi didattici, sviluppato presso il MIT, che si propone come un sistema operativo semplice e leggero, ma allo stesso tempo completo e funzionale. É basato su UNIX V6, da cui prende il nome, realizzato in C e assembly. La prima versione del 2006 è progettata per essere eseguibile su architetture x86, mentre nella versione del 2020 è stata realizzata una conversione per RISC-V. 
La versione che abbiamo scelto di analizzare è quella del 2006 per x86.

Le funzioni chiave oggetto di studio includono system calls, meccanismi di sincronizzazione, gestione della memoria virtuale e della MMU, algoritmi di scheduling, oltre a esaminare altre caratteristiche rilevanti per il corretto funzionamento di un sistema operativo. 

### Organizzazione del Kernel

Il sistema operativo è organizzato in modo che risieda tutto all'interno del kernel in modo che le implementazioni di tutte le chiamate di sistema vengano eseguite in modalità kernel. Questa organizzazione è chiamata kernel monolitico.

![Kernel Monolitico](Immagini\xv6\monolithic.jpg)

In questa organizzazione, l'intero sistema operativo viene eseguito con pieno privilegio hardware. Questa organizzazione è conveniente perché il progettista del sistema operativo non deve decidere quale parte del sistema operativo non ha bisogno di pieno privilegio hardware. Inoltre, è facile per diverse parti del sistema operativo cooperare. Ad esempio, un sistema operativo potrebbe avere una cache di buffer che può essere condivisa sia dal sistema di file che dal sistema di memoria virtuale.

Un inconveniente dell'organizzazione monolitica è che le interfacce tra le diverse parti del sistema operativo sono spesso complesse, ed è quindi facile per uno sviluppatore del sistema operativo commettere un errore. In un kernel monolitico, un errore è fatale, perché un errore in modalità kernel spesso comporta il fallimento del kernel. Se il kernel fallisce, il computer smette di funzionare e, di conseguenza, tutte le applicazioni falliscono. Il computer deve essere riavviato per ripartire.

Per ridurre il rischio di errori nel kernel, i progettisti del sistema operativo possono minimizzare la quantità di codice del sistema operativo che viene eseguita in modalità kernel ed eseguire la maggior parte del sistema operativo in modalità utente. Questa organizzazione del kernel è chiamata microkernel. 

![Microkernel](Immagini\xv6\microkernel.jpg)

Nella figura, il sistema di file viene eseguito come processo a livello utente. I servizi del sistema operativo eseguiti come processi sono chiamati server. Per consentire alle applicazioni di interagire con il file server, il kernel fornisce un meccanismo di comunicazione tra processi per inviare messaggi da un processo a livello utente a un altro.

In un microkernel, l'interfaccia del kernel consiste in alcune funzioni a basso livello per avviare applicazioni, inviare messaggi, accedere all'hardware del dispositivo, ecc. Questa organizzazione consente al kernel di essere relativamente semplice, poiché la maggior parte del sistema operativo risiede nei server a livello utente.

Xv6 è implementato come un kernel monolitico, seguendo la maggior parte dei sistemi operativi Unix. Pertanto, in xv6, l'interfaccia del kernel corrisponde all'interfaccia del sistema operativo e il kernel implementa l'intero sistema operativo. Poiché xv6 non fornisce molti servizi, il suo kernel è più piccolo di alcuni microkernel.

#### Processi

I processi vanno in xv6 a costituire l'unità fondamentale di isolamento. L'astrazione del processo fornisce l'illusione a un programma che esso abbia la sua stessa macchina privata. Un processo fornisce a un programma ciò che sembra essere un sistema di memoria privato, o spazio degli indirizzi, che altri processi non possono leggere o scrivere. Un processo fornisce anche al programma ciò che sembra essere la sua stessa CPU per eseguire le istruzioni del programma.

Xv6 utilizza le tabelle delle pagine (implementate dall'hardware) per dare a ogni processo il suo spazio degli indirizzi. La tabella delle pagine x86 mappa un indirizzo virtuale in un indirizzo fisico.

###

### Riassunto delle principali caratteristiche


- **Struttura Simplice**:

      Xv6 è progettato con un'architettura relativamente semplice e compatta, facilitando la comprensione e lo studio dei principi fondamentali dei sistemi operativi.

- **Unix V6 Heritage**:
        
      Xv6 trae ispirazione da Unix V6, che è stato uno dei primi sistemi operativi Unix sviluppati presso i laboratori Bell nel 1975. Mantenendo una certa coerenza con il suo predecessore, Xv6 offre una prospettiva storica dei concetti fondamentali dei sistemi operativi.

- **Kernel Monolitico**:
            
      Xv6 adotta un'architettura monolitica, dove il kernel è una singola immagine eseguibile che gestisce direttamente tutte le funzioni del sistema operativo. Questo facilita l'avvio e la comprensione, ma potrebbe limitare la modularità in confronto ad altri design.

- **Gestione della Memoria**:
  
      Implementa un sistema di gestione della memoria basato su paging. Xv6 include concetti come l'allocazione dinamica della memoria e la gestione degli spazi degli indirizzi per i processi.

- **Gestione dei Processi**:
     
      Utilizza un modello di gestione dei processi basato su fork e exec, seguendo il modello di Unix. La creazione e l'esecuzione dei processi sono concettualmente simili a quanto presente in Unix.

- **File System**:
 
      Fornisce un sistema di file semplificato, ispirato a quello presente in Unix V6. Include concetti come la gestione di directory, la lettura e scrittura di file, e altre operazioni di base di un sistema di file.

- **Interfaccia di Sistema (System Call)**:
 
      Xv6 implementa un insieme essenziale di chiamate di sistema (system call) che consentono ai processi di interagire con il kernel. Queste system call includono operazioni di I/O, gestione dei processi e gestione della memoria.

- **Sincronizzazione e Semafori**:
 
      Contiene meccanismi di sincronizzazione, come semafori, che possono essere utilizzati per coordinare l'accesso concorrente alle risorse condivise.

- **Scheduler**:
 
      Implementa un semplice scheduler a round-robin per la gestione dei processi. Anche se di base, fornisce una base per la sperimentazione con diversi algoritmi di scheduling.

- **Portabilità**:

      Xv6 è progettato per essere eseguibile su architetture x86, facilitando la sua adozione e utilizzo su diverse piattaforme hardware.
