# Analisi comparativa tra Os161 e Xv6

## Introduzione

Questo studio si concentra principalmente sull'analisi del sistema operativo Xv6, confrontandolo con Os161. L'obiettivo di questa analisi comparativa è offrire una panoramica delle capacità di Os161 e Xv6, mettendo in evidenza le differenze e le somiglianze tra questi due sistemi operativi open source.

## Xv6

Così come Os161, Xv6 è un sistema operativo nato con scopi didattici, sviluppato presso il MIT, che si propone come un sistema operativo semplice e leggero, ma allo stesso tempo completo e funzionale. É basato su UNIX V6, da cui prende il nome, realizzato in C e assembly. La prima versione del 2006 è progettata per essere eseguibile su architetture x86, mentre nella versione del 2020 è stata realizzata una conversione per RISC-V. 
La versione che abbiamo scelto di analizzare è quella del 2006 per x86.

Le funzioni chiave oggetto di studio includono system calls, meccanismi di sincronizzazione, gestione della memoria virtuale e della MMU, algoritmi di scheduling, oltre a esaminare altre caratteristiche rilevanti per il corretto funzionamento di un sistema operativo. 

### Organizzazione del Kernel

Il sistema operativo è organizzato in modo che risieda tutto all'interno del kernel in modo che le implementazioni di tutte le chiamate di sistema vengano eseguite in modalità kernel. Questa organizzazione è chiamata kernel monolitico.

<div align="center">
    <figure>
     <img src="Immagini\xv6\monolithic.jpg" width="291" height="189">
         <figcaption>Figura 1: Kernel monolitico</figcaption>
   </figure> 
</div>

In questa organizzazione, l'intero sistema operativo viene eseguito con pieno privilegio hardware. Questa organizzazione è conveniente perché il progettista del sistema operativo non deve decidere quale parte del sistema operativo non ha bisogno di pieno privilegio hardware. Inoltre, è facile per diverse parti del sistema operativo cooperare. Ad esempio, un sistema operativo potrebbe avere una cache di buffer che può essere condivisa sia dal sistema di file che dal sistema di memoria virtuale.

Un inconveniente dell'organizzazione monolitica è che le interfacce tra le diverse parti del sistema operativo sono spesso complesse, ed è quindi facile per uno sviluppatore del sistema operativo commettere un errore. In un kernel monolitico, un errore è fatale, perché un errore in modalità kernel spesso comporta il fallimento del kernel. Se il kernel fallisce, il computer smette di funzionare e, di conseguenza, tutte le applicazioni falliscono. Il computer deve essere riavviato per ripartire.

Per ridurre il rischio di errori nel kernel, i progettisti del sistema operativo possono minimizzare la quantità di codice del sistema operativo che viene eseguita in modalità kernel ed eseguire la maggior parte del sistema operativo in modalità utente. Questa organizzazione del kernel è chiamata microkernel. 

<div align="center">
    <figure>
     <img src="Immagini\xv6\microkernel.jpg" width="340" height="134">
        <figcaption>Figura 2: Microkernel</figcaption>
   </figure> 
</div>

Nella figura, il sistema di file viene eseguito come processo a livello utente. I servizi del sistema operativo eseguiti come processi sono chiamati server. Per consentire alle applicazioni di interagire con il file server, il kernel fornisce un meccanismo di comunicazione tra processi per inviare messaggi da un processo a livello utente a un altro.

In un microkernel, l'interfaccia del kernel consiste in alcune funzioni a basso livello per avviare applicazioni, inviare messaggi, accedere all'hardware del dispositivo, ecc. Questa organizzazione consente al kernel di essere relativamente semplice, poiché la maggior parte del sistema operativo risiede nei server a livello utente.

Xv6 è implementato come un kernel monolitico, seguendo la maggior parte dei sistemi operativi Unix. Pertanto, in xv6, l'interfaccia del kernel corrisponde all'interfaccia del sistema operativo e il kernel implementa l'intero sistema operativo. Poiché xv6 non fornisce molti servizi, il suo kernel è più piccolo di alcuni microkernel.

#### Processi

I processi vanno in xv6 a costituire l'unità fondamentale di isolamento. L'astrazione del processo fornisce l'illusione a un programma che esso abbia la sua stessa macchina privata. Un processo fornisce a un programma ciò che sembra essere un sistema di memoria privato, o spazio degli indirizzi, che altri processi non possono leggere o scrivere. Un processo fornisce anche al programma ciò che sembra essere la sua stessa CPU per eseguire le istruzioni del programma.

Xv6 utilizza le tabelle delle pagine (implementate dall'hardware) per dare a ogni processo il suo spazio degli indirizzi. La tabella delle pagine x86 mappa un indirizzo virtuale in un indirizzo fisico.
Ogni spazio degli indirizzi di un processo mappa le istruzioni e i dati del kernel, così come la memoria del programma utente. Il kernel di xv6 mantiene molte informazioni di stato per ciascun processo, che vengono raccolte in una struttura chiamata struct proc. Gli elementi più importanti dello stato del kernel di un processo sono la sua tabella delle pagine, il suo stack del kernel e il suo stato di esecuzione.

Quando un processo effettua una chiamata di sistema, il processore passa allo stack del kernel, aumenta il livello di privilegio hardware e inizia a eseguire le istruzioni del kernel che implementano la chiamata di sistema. Quando la chiamata di sistema si completa, il kernel torna allo spazio utente: l'hardware abbassa il livello di privilegio, passa di nuovo allo stack utente e riprende l'esecuzione delle istruzioni utente subito dopo l'istruzione di chiamata di sistema.

<div align="center">
    <figure>
     <img src="Immagini\xv6\layout_virtual_address_space.jpg" width="426" height="212">
        <figcaption>Figura 3: Layout dello spazio degli indirizzi virtuali di un processo.</figcaption>
   </figure> 
</div>

### Page Tables

Le tabelle delle pagine sono il meccanismo attraverso il quale il sistema operativo controlla il significato degli indirizzi di memoria. Consentono a xv6 di multiplexare gli spazi degli indirizzi di processi diversi su una singola memoria fisica e di proteggere le memorie di processi diversi. Xv6 utilizza principalmente le tabelle delle pagine per multiplexare gli spazi degli indirizzi e proteggere la memoria. Utilizza anche alcune semplici astuzie con le tabelle delle pagine: mappare la stessa memoria (il kernel) in diversi spazi degli indirizzi, mappare la stessa memoria più di una volta in uno spazio degli indirizzi (ogni pagina utente è anche mappata nella vista fisica della memoria del kernel) e proteggere uno stack utente con una pagina non mappata.

<div align="center">
    <figure>
     <img src="Immagini\xv6\pagetable.jpg" width="426" height="212">
        <figcaption>Figura 4: Page table di x86</figcaption>
   </figure> 
</div>

le istruzioni x86 (sia utente che kernel) manipolano indirizzi virtuali. La RAM della macchina, o memoria fisica, è indicizzata con indirizzi fisici. L'hardware di paginazione x86 collega questi due tipi di indirizzi, mappando ogni indirizzo virtuale in un indirizzo fisico. Una tabella delle pagine x86 è logicamente un array di 2^20 (1.048.576) page table entries (PTEs). Ogni PTE contiene un numero di physical page number (PPN) di 20 bit e alcune bandiere. L'hardware di paginazione traduce un indirizzo virtuale utilizzando i suoi primi 20 bit per indicizzare la tabella delle pagine e trovare una PTE, sostituendo i primi 20 bit dell'indirizzo con il PPN nella PTE.

La traduzione effettiva avviene in due fasi. Una tabella delle pagine è memorizzata in memoria fisica come un albero a due livelli. La radice dell'albero è un page directory di 4096 byte che contiene 1024 riferimenti simili a PTE a pagine di tabella delle pagine. Ogni pagina di tabella delle pagine è un array di 1024 PTE da 32 bit. L'hardware di paginazione utilizza i primi 10 bit di un indirizzo virtuale per selezionare un ingresso del page directory. Se sia l'ingresso del page directory che la PTE non sono presenti, l'hardware di paginazione genera una eccezione. 

### Traps ans Interrupts

### System Calls

### Memory Management

### Synchronization

### Scheduling

### File System 

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



### Sources:
https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf
