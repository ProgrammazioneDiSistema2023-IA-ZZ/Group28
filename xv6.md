# Analisi comparativa tra OS161 e xv6

#### Sources:
https://clownote.github.io/2021/02/27/xv6/Xv6-operating-system-organization/
https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf

## Introduzione
Attraverso questa analisi comparativa, si mira a fornire una panoramica delle capacità di OS161 e xv6, ponendo una particolare attenzione sulle differenze e sulle similarità di questi due sistemi operativi open source.

## xv6

Così come OS161, xv6 è un sistema operativo nato con scopi didattici, sviluppato presso il MIT, che si propone come un sistema operativo semplice e leggero, ma allo stesso tempo completo e funzionale. É basato su UNIX V6, da cui prende il nome, realizzato in C e assembly. La prima versione del 2006 è progettata per essere eseguibile su architetture x86, mentre nella versione del 2020 è stata realizzata una conversione per RISC-V. 
La versione che abbiamo scelto di analizzare è quella del 2006 per x86.

Le funzioni chiave oggetto di studio includono system calls, meccanismi di sincronizzazione, gestione della memoria virtuale e della MMU, algoritmi di scheduling, oltre a esaminare altre caratteristiche rilevanti per il corretto funzionamento di un sistema operativo. 

![Alt text](Immagini\xv6\os.jpg)

Le principali caratteristiche di xv6 sono:

- **Struttura Simplice**:

      xv6 è progettato con un'architettura relativamente semplice e compatta, facilitando la comprensione e lo studio dei principi fondamentali dei sistemi operativi.

- **Unix V6 Heritage**:
        
      xv6 trae ispirazione da Unix V6, che è stato uno dei primi sistemi operativi Unix sviluppati presso i laboratori Bell nel 1975. Mantenendo una certa coerenza con il suo predecessore, xv6 offre una prospettiva storica dei concetti fondamentali dei sistemi operativi.

- **Kernel Monolitico**:
            
      xv6 adotta un'architettura monolitica, dove il kernel è una singola immagine eseguibile che gestisce direttamente tutte le funzioni del sistema operativo. Questo facilita l'avvio e la comprensione, ma potrebbe limitare la modularità in confronto ad altri design.

- **Gestione della Memoria**:
  
      Implementa un sistema di gestione della memoria basato su paging. xv6 include concetti come l'allocazione dinamica della memoria e la gestione degli spazi degli indirizzi per i processi.

- **Gestione dei Processi**:
     
      Utilizza un modello di gestione dei processi basato su fork e exec, seguendo il modello di Unix. La creazione e l'esecuzione dei processi sono concettualmente simili a quanto presente in Unix.

- **File System**:
 
      Fornisce un sistema di file semplificato, ispirato a quello presente in Unix V6. Include concetti come la gestione di directory, la lettura e scrittura di file, e altre operazioni di base di un sistema di file.

- **Interfaccia di Sistema (System Call)**:
 
      xv6 implementa un insieme essenziale di chiamate di sistema (system call) che consentono ai processi di interagire con il kernel. Queste system call includono operazioni di I/O, gestione dei processi e gestione della memoria.

- **Sincronizzazione e Semafori**:
 
      Contiene meccanismi di sincronizzazione, come semafori, che possono essere utilizzati per coordinare l'accesso concorrente alle risorse condivise.

- **Scheduler**:
 
      Implementa un semplice scheduler a round-robin per la gestione dei processi. Anche se di base, fornisce una base per la sperimentazione con diversi algoritmi di scheduling.

- **Portabilità**:

      xv6 è progettato per essere eseguibile su architetture x86, facilitando la sua adozione e utilizzo su diverse piattaforme hardware.
