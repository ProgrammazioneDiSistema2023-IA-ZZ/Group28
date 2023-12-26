# Analisi comparativa tra OS161 e xv6

#### Sources:
https://clownote.github.io/2021/02/27/xv6/Xv6-operating-system-organization/
https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf

## Introduzione
Attraverso questa analisi comparativa, si mira a fornire una panoramica delle capacità di OS161 e xv6, ponendo una particolare attenzione sulle differenze e sulle similarità di questi due sistemi operativi open source.

## xv6

Così come OS161, xv6 è un sistema operativo nato con scopi didattici, sviluppato presso il MIT, che si propone come un sistema operativo semplice e leggero, ma allo stesso tempo completo e funzionale. É basato su UNIX V6, da cui prende il nome, realizzato in C e assembly. La prima versione del 2006 è basata su un'architettura x86, mentre la versione del 2020 è statarealizzata una conversione per RISC-V.
La versione che abbiamo scelto di analizzare è quella del 2006 per x86.

Le principali caratteristiche di xv6 sono:
- Un kernel monolitico
- Un sistema di gestione dei processi basato su scheduler a priorità fissa
- Un sistema di gestione della memoria basato su paginazione
- Un sistema di gestione dei file basato su UNIX V6
- Un sistema di gestione dei dispositivi basato su UNIX V6
- Un sistema di gestione dei segnali basato su UNIX V6
- Una gestione della concorrenza livello processi basato su UNIX V6
