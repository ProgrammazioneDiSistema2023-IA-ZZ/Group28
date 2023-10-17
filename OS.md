# Progetto PDS 2022/2023
## Parte 1 - Differenze tra sistemi operativi: Os161 e Nachos


Ci sono diversi sistemi operativi open-source più semplici e compatti rispetto a OS161 e Linux che potresti considerare per l'analisi comparativa. Ecco alcuni esempi:
- **FreeRTOS**: FreeRTOS è un sistema operativo in tempo reale open-source progettato per sistemi embedded. È noto per la sua leggerezza e il basso impatto sulle risorse. È ampiamente utilizzato in dispositivi come microcontrollori e microprocessori.
- **TinyOS**: TinyOS è un sistema operativo open-source orientato ai sensori, progettato per reti di sensori wireless e dispositivi a bassa potenza. È ottimizzato per applicazioni IoT e dispone di un'architettura modulare.
µC/OS-II e µC/OS-III: µC/OS-II e µC/OS-III sono sistemi operativi in tempo reale progettati per sistemi embedded. µC/OS-II è una versione più leggera, mentre µC/OS-III offre una maggiore scalabilità e funzionalità avanzate.
- **Contiki**: Contiki è un sistema operativo open-source progettato per reti di sensori e dispositivi IoT. È noto per la sua efficienza in termini di memoria e risorse, ed è adatto per sistemi a basso consumo energetico.
- **RTEMS**: RTEMS è un sistema operativo in tempo reale open-source con un'ampia gamma di funzionalità. È progettato per sistemi embedded e applicazioni critiche e supporta una varietà di architetture hardware.
- **Nuttx**: Nuttx è un sistema operativo in tempo reale e open-source con un design leggero e modulare. È adatto per sistemi embedded e offre supporto per diverse architetture.
- **ChibiOS/RT**: ChibiOS/RT è un sistema operativo in tempo reale open-source progettato per sistemi embedded. È conosciuto per le sue prestazioni e la sua architettura modulare.

### Nachos

- **Vantaggi**:
Nachos è stato sviluppato all'Università di Berkeley, quindi potrebbe avere alcune somiglianze concettuali con OS161, che è spesso utilizzato in contesti accademici.
La struttura modulare di Nachos potrebbe facilitare l'implementazione e l'analisi delle diverse caratteristiche da valutare.
È progettato per supportare sistemi operativi embedded e generali, il che lo rende adatto alle esigenze del tuo progetto.
- **Svantaggi**:
Potrebbe essere più complesso rispetto a OS161, il che richiederebbe un'ulteriore curva di apprendimento.
### JOS (J Operating System):
- **Vantaggi**:
JOS è stato sviluppato presso il MIT e potrebbe quindi condividere alcune somiglianze concettuali con OS161, che ha radici accademiche simili.
Potrebbe essere progettato per la comprensione dei principi dei sistemi operativi, quindi potrebbe essere adattabile alle tue esigenze di analisi.
- **Svantaggi**:
Potrebbe essere più complesso rispetto ad altri sistemi operativi accademici, richiedendo quindi una curva di apprendimento più lunga.

---- Aggiornamento 14-08
Considero solo i SO: Nachos JOS
NACHOS
https://www.irisa.fr/alf/downloads/puaut/Systeme/NachosEnglish.pdf
JOS
https://homes.di.unimi.it/sisop/lucidi1415/Dispensa.pdf

--- Aggiornamento 22/08/2023

Sia OS/161 che NACHOS sono sistemi operativi educativi utilizzati per scopi didattici nel campo dei sistemi operativi. Entrambi offrono un ambiente per esplorare i concetti fondamentali dei sistemi operativi, ma ci sono differenze significative nel modo in cui gestiscono le system call (chiamate di sistema). Di seguito, confronto come funzionano le system call in OS/161 e NACHOS:
#### OS/161:
- **Gestione delle System Call**: In OS/161, le system call vengono gestite in modalità kernel. Quando un'applicazione utente esegue una system call, il controllo passa al kernel.
- **Switch di Modalità**: Quando avviene una system call, OS/161 passa dalla modalità utente alla modalità kernel, nota anche come "modalità privilegiata". Questo è reso possibile grazie alle istruzioni specifiche delle architetture dei processori moderni.
- **Interrupt e Traps**: Per passare dalla modalità utente alla modalità kernel, OS/161 fa uso di interruzioni o trappole. L'applicazione utente genera un'interruzione o trappola attraverso un'istruzione di assembly, e il controllo passa al gestore di interruzioni del kernel.
- **Gestione del Contesto**: Quando il kernel gestisce una system call, salva il contesto dell'applicazione utente (registri, puntatori di stack, ecc.), esegue il codice della system call e poi ripristina il contesto dell'applicazione utente prima di restituire il controllo all'applicazione.
#### NACHOS:
- **Gestione delle System Call**: NACHOS gestisce le system call in modo simile a OS/161. Le chiamate di sistema vengono gestite dal kernel di NACHOS.
- **Switch di Modalità**: Anche NACHOS effettua un passaggio dalla modalità utente alla modalità kernel quando viene eseguita una system call.
- **Interrupt e Traps**: Come OS/161, NACHOS fa uso di interruzioni o trappole per passare dalla modalità utente alla modalità kernel durante l'esecuzione di una system call.
- **Gestione del Contesto**: NACHOS segue un approccio simile a OS/161 nel salvare e ripristinare il contesto durante la transizione tra modalità utente e modalità kernel.




In OS/161, le informazioni sulle chiamate di sistema sono organizzate all'interno di tabelle e strutture dati specifiche. Vediamo come funziona nel dettaglio:
#### Tabella delle Chiamate di Sistema: 
OS/161 utilizza una tabella delle chiamate di sistema (system call table) per associare numeri di chiamate di sistema alle funzioni del kernel corrispondenti. Questa tabella è spesso definita nel codice del kernel e può essere implementata come un array di puntatori a funzione.
Esempio di definizione di una tabella delle chiamate di sistema in OS/161:

```c
// Definizione della tabella delle chiamate di sistema in OS/161
typedef int (*syscall_function_t)(void);
syscall_function_t syscall_table[SYSCALL_COUNT];
```
#### Gestione delle Chiamate di Sistema: 
Ogni numero di chiamata di sistema è associato a una funzione specifica nel kernel. Ecco un esempio semplificato di come potrebbe apparire la definizione e l'inizializzazione della tabella delle chiamate di sistema in OS/161:

```c
// Definizione delle chiamate di sistema in OS/161
#define SYS_HALT 0
#define SYS_READ 2
// ... altre chiamate di sistema ...
// Inizializzazione della tabella delle chiamate di sistema
syscall_table[SYS_HALT] = sys_halt;
syscall_table[SYS_READ] = sys_read;
// ... inizializzazione di altre chiamate di sistema ...
```
Esecuzione delle Chiamate di Sistema: 
volta che una chiamata di sistema è stata richiesta dall'applicazione utente, il kernel esegue la funzione associata all'interno della tabella delle chiamate di sistema.
Esempio di implementazione semplificata di sys_read in OS/161:
```c
int sys_read(int filehandle, char *buf, size_t size) {
    // Implementazione della lettura dal filehandle nel buffer
    // Restituzione del numero di byte letti o di un valore di errore }
```
#### Gestione del Contesto: 
Durante il passaggio tra modalità utente e kernel, il kernel salva e ripristina il contesto del processo corrente. Questo può includere la gestione dei registri, degli stack e altre informazioni cruciali.
Ad esempio, durante il passaggio dalla modalità utente alla modalità kernel, il kernel può salvare il contesto dell'applicazione utente:
```c
void enter_kernel_mode() {
    // Salva lo stato del contesto dell'applicazione utente
    // Esegui il passaggio alla modalità kernel
    // Gestisci la chiamata di sistema
    // Ripristina lo stato del contesto dell'applicazione utente
}
```
Le informazioni sulle chiamate di sistema, come la tabella delle chiamate di sistema e le funzioni associate, sono generalmente definite e organizzate all'interno del codice del kernel. Le chiamate di sistema possono variare in base all'implementazione specifica di OS/161 e alla versione del sistema operativo utilizzata.


In OS/161, le chiamate di sistema sono una parte fondamentale dell'interazione tra le applicazioni utente e il kernel del sistema operativo. Le chiamate di sistema consentono alle applicazioni utente di richiedere servizi o risorse dal kernel, come la lettura/scrittura su file, la creazione di processi, l'allocazione di memoria, ecc. Ecco come vengono gestite le chiamate di sistema in OS/161:


- **Interruzioni (Trappole)**: Quando un'applicazione utente richiede un servizio del kernel attraverso una chiamata di sistema, il controllo passa dalla modalità utente alla modalità kernel. Questo passaggio avviene attraverso una trappola o un'interruzione. L'applicazione utente emette un'istruzione di assembly che genera un'interruzione o una trappola specifica per una chiamata di sistema.
- **Interrupt Handler del Kernel**: Una volta che l'interruzione o la trappola viene generata, il controllo passa all'interrupt handler del kernel. Questo handler è una porzione di codice del kernel che gestisce gli eventi generati dalle interruzioni e dalle trappole. Nel caso delle chiamate di sistema, l'interrupt handler riconosce il tipo di chiamata richiesta e la smista al gestore appropriato.
- **Gestione delle Chiamate di Sistema**: Il kernel di OS/161 ha una tabella delle chiamate di sistema (system call table) che associa i numeri delle chiamate di sistema alle funzioni del kernel corrispondenti. Ad esempio, il numero 0 potrebbe essere associato a una chiamata di sistema per terminare un processo, il numero 1 per scrivere su un file, il numero 2 per leggere da un file, e così via.
- **Esecuzione delle Chiamate di Sistema**: Una volta individuata la chiamata di sistema richiesta e associata alla sua funzione corrispondente, il kernel esegue il codice della funzione di chiamata di sistema. Questo può comportare la modifica dello stato del processo, l'allocazione di memoria, l'interazione con dispositivi di I/O e altro ancora.
- **Restituzione dei Risultati**: Dopo l'esecuzione della chiamata di sistema, il controllo ritorna all'applicazione utente, e il risultato della chiamata di sistema (ad esempio, il valore restituito da una funzione di lettura/scrittura) può essere restituito all'applicazione.
- **Ripristino del Contesto**: Durante il passaggio dalla modalità utente alla modalità kernel, e viceversa, il kernel salva e ripristina il contesto del processo corrente, compresi i registri, lo stack e altre informazioni importanti. Questo è necessario per garantire che l'esecuzione possa riprendere correttamente dopo una chiamata di sistema.


In sostanza, OS/161 gestisce le chiamate di sistema attraverso il passaggio controllato tra le modalità utente e kernel, utilizzando interrupt handler specifici per instradare le chiamate al codice appropriato nel kernel.

Sono scritte in linguaggio assembly o in linguaggio C, a seconda dell'implementazione del sistema operativo e dell'architettura del processore. Le system call sono spesso implementate nel kernel del sistema operativo e consentono alle applicazioni utente di accedere a funzionalità del sistema che richiedono privilegi o che non sono direttamente accessibili dalla modalità utente.
Ecco alcuni esempi di system call comuni e importanti:
- **fork()**: Crea un nuovo processo identico (clonato) dal processo chiamante. Questa system call è fondamentale per la creazione di processi in sistemi a processo multipli.
- **exec()**: Sostituisce l'immagine del processo corrente con un nuovo programma specificato. Questa system call è utilizzata per avviare nuovi programmi all'interno di un processo.
- **read()** e **write()**: Consentono di leggere dati da un file o di scrivere dati in un file. Queste system call sono utilizzate per operazioni di I/O di base su file.
- **open()** e **close()**: Aprono e chiudono file. L'apertura di un file restituisce un descrittore di file che può essere utilizzato per altre operazioni di I/O.
- **malloc()** e **free()**: Queste system call sono usate per allocare e liberare la memoria dinamica. Sono fondamentali per la gestione della memoria nei programmi.
- **exit()**: Termina il processo corrente e restituisce un valore di uscita al padre. Questa system call è utilizzata per terminare un programma e restituire un risultato al chiamante.
- **wait()**: Sospende il processo chiamante fino a quando uno dei suoi processi figlio termina. Viene utilizzata per sincronizzare l'esecuzione dei processi.
- **ioctl()**: Fornisce un'interfaccia generica per controllare vari aspetti dei dispositivi di I/O, come terminale, stampante, ecc.
- **chdir()** e **getcwd()**: Cambiano la directory corrente o restituiscono il percorso della directory corrente.
- **time()**: Restituisce l'ora di sistema corrente.

#### Tabella delle Chiamate di Sistema:
 In NACHOS, la tabella delle chiamate di sistema è spesso definita nella classe syscall.h. Questa tabella associa i numeri delle chiamate di sistema alle funzioni del kernel corrispondenti.
 Esempio di definizione di una tabella delle chiamate di sistema in syscall.h:
```c
// Definizione della tabella delle chiamate di sistema in NACHOS
class NachosSyscall {
    public:
        NachosSyscall();
        ~NachosSyscall();
        void Init();
        void Halt();
        void Exit(int status);
        void Exec();
        void Read();
        // ... altre chiamate di sistema ...
};
```
#### Gestione delle Chiamate di Sistema:
 All'interno di NACHOS, il controllo delle chiamate di sistema avviene tramite la classe ExceptionHandler. Quando viene generata un'interruzione o una trappola, l'ExceptionHandler esamina il tipo di chiamata di sistema richiesta e inoltra l'esecuzione alla funzione corrispondente nella tabella delle chiamate di sistema.
 Esempio semplificato di gestione delle chiamate di sistema in ExceptionHandler:
```c
// Classe per la gestione delle chiamate di sistema in NACHOS
class ExceptionHandler {
    public:
        ExceptionHandler();
        ~ExceptionHandler();
        void HandleException(ExceptionType which);
};

void ExceptionHandler::HandleException(ExceptionType which) {
    // Ottieni il tipo di eccezione (interruzione o trappola)
    if (which == SyscallException) {
        // Otteniamo il numero di chiamata di sistema dal registro "2"
        int type = machine->ReadRegister(2);

        // Esegue la funzione corrispondente nella tabella delle chiamate di sistema
        nachosSyscallTable->DoSyscall(type);
    }
}
```
#### Esecuzione delle Chiamate di Sistema:
 Ogni funzione nella classe NachosSyscall corrisponde a una chiamata di sistema specifica. L'implementazione di queste funzioni dipende dalla specifica azione richiesta dalla chiamata di sistema. Ad esempio, Halt() potrebbe arrestare il sistema, Exit(int status) potrebbe terminare un processo, e così via.
 Esempio di implementazione semplificata della funzione Halt():
```c
void NachosSyscall::Halt() {
    DEBUG('a', "Shutdown, initiated by user program.\n");
    interrupt->Halt();
}
```
Restituzione dei Risultati: Alcune chiamate di sistema potrebbero restituire dei valori all'applicazione utente, ad esempio il risultato di una lettura da file. Questi valori vengono spesso restituiti attraverso registri specifici, come ad esempio il registro "2" (2 è un registro specifico del MIPS che spesso contiene il risultato delle chiamate di sistema).

In sintesi, in NACHOS, le chiamate di sistema sono definite all'interno della classe NachosSyscall, la quale contiene funzioni che corrispondono a ciascuna chiamata di sistema. Quando viene generata un'interruzione o una trappola, l'ExceptionHandler determina il tipo di chiamata di sistema e inoltra l'esecuzione alla funzione corrispondente nella tabella delle chiamate di sistema. Le implementazioni delle chiamate di sistema possono variare a seconda delle azioni specifiche richieste. La classe NachosSyscall è una parte fondamentale di NACHOS ed è responsabile della gestione dell'interfaccia tra le applicazioni utente e il kernel del sistema operativo.

Nachos è un sistema operativo didattico progettato per l'apprendimento dei principi dei sistemi operativi. Le **system call** (chiamate di sistema) in Nachos sono implementate attraverso l'interfaccia tra il codice dell'applicazione e il kernel del sistema operativo. Le chiamate di sistema permettono alle applicazioni utente di richiedere servizi dal kernel, come l'accesso ai file, la gestione dei processi, la comunicazione tra processi, e così via.

Le *system call* in Nachos sono scritte usando una combinazione di linguaggio Assembly MIPS e C++. Di seguito ti fornirò alcuni esempi di chiamate di sistema importanti in Nachos:

1. **Halt**: Una chiamata di sistema che ferma il sistema operativo.
```c
// Interfaccia in C++
void Halt();

// Implementazione in Assembly MIPS
Halt:
    # Carica il codice dell'interrupt per la chiamata di sistema Halt
    li $v0, 10
    syscall
    j $ra
```

2. **Exit**: Una chiamata di sistema per terminare un processo.
```c
// Interfaccia in C++
void Exit(int status);

// Implementazione in Assembly MIPS
Exit:
    # Carica il codice dell'interrupt per la chiamata di sistema Exit
    li $v0, 17
    syscall
    j $ra
```

3. **Create**: Crea un nuovo file.
```c
// Interfaccia in C++
OpenFileId Create(char *name);

// Implementazione in Assembly MIPS
Create:
    # Carica il codice dell'interrupt per la chiamata di sistema Create
    li $v0, 8
    syscall
    j $ra
```

4. **Open**: Apre un file esistente.
```c
// Interfaccia in C++
OpenFileId Open(char *name);

// Implementazione in Assembly MIPS
Open:
    # Carica il codice dell'interrupt per la chiamata di sistema Open
    li $v0, 9
    syscall
    j $ra
```

5. **Read**: Legge dati da un file aperto.
```c
// Interfaccia in C++
int Read(char *buffer, int size, OpenFileId id);

// Implementazione in Assembly MIPS
Read:
    # Carica il codice dell'interrupt per la chiamata di sistema Read
    li $v0, 14
    syscall
    j $ra
```

6. **Write**: Scrive dati in un file aperto.
```c
// Interfaccia in C++
void Write(char *buffer, int size, OpenFileId id);

// Implementazione in Assembly MIPS
Write:
    # Carica il codice dell'interrupt per la chiamata di sistema Write
    li $v0, 15
    syscall
    j $ra
```

---
---

##### Nachos
Le **system call** (chiamate di sistema) in Nachos sono implementate attraverso l'interfaccia tra il codice dell'applicazione e il kernel del sistema operativo.
Analizzando il codice di Nachos possiamo notare come le chiamate di sistema vengono gestite e implementate.

In particolar modo poniamo la nostra attenzione su tutto il processo di gestione delle chiamate di sistema partendo da machine.cc (con machine.h) e continuando con exception.cc (con exception.h), start.s (con start.c) e syscall.h.

In *machine.h* vanno definite le strutture dati per simulare l'esecuzione dei programmi utente eseguite sopra Nachos. 
I programmi utente vengono caricati nella "mainMemory". All'interno della memoria è presente anche il kernel, ma viene caricato in una regione di memoria separata da quella dell'utente.
I programmi e gli accessi alla memoria del kernel non vengono tradotti o paginati. I programmi utente vengono eseguiti un'istruzione alla volta, dal simulatore. Ogni riferimento alla memoria viene tradotto e controllato per la verifica degli errori. 
All'interno della classe machine viene definita l'hardware della workstation host simulata, come visto dai programmi utente: i registri della CPU, la memoria principale, ecc.
In particolare si può notare la presenza del metodo ```c void IncreasePC() ``` che va a simulare l'hw andando ad incrementare di 4 il valore del program counter (PC) per poter eseguire la prossima istruzione.
I programmi utente non dovrebbero essere in grado di riconoscere che sono in esecuzione sul simulatore o sull'hardware reale.
Per l'invocazione delle system call o nel caso di eccezioni è necessario passare dalla modalità utente alla modalità kernel. Questo è reso possibile dal metodo Machine::RaiseException, di seguito l'implementazione presente in *machine.cc*.

```c
//	"which" -- the cause of the kernel trap
//	"badVaddr" -- the virtual address causing the trap, if appropriate
void
Machine::RaiseException(ExceptionType which, int badVAddr)
{
    DEBUG('m', "Exception: %s\n", exceptionNames[which]);
    
//  ASSERT(interrupt->getStatus() == UserMode);
    registers[BadVAddrReg] = badVAddr;
    DelayedLoad(0, 0);			// finish anything in progress
    interrupt->setStatus(SystemMode);
    ExceptionHandler(which);		// interrupts are enabled at this point
    interrupt->setStatus(UserMode);
}
```
Purchè questo sia possibile è necessaria una gestione a basso livello dei registri della CPU. Di seguito è riportata la definizione del set completo di registri MIPS e in aggiunta altri registri per poter avviare/arrestare un programma utente tra due istruzioni qualsiasi (tenendo traccia di load, delay solts, ecc.).

```c
#define StackReg	29	// User's stack pointer
#define RetAddrReg	31	// Holds return address for procedure calls
#define NumGPRegs	32	// 32 general purpose registers on MIPS
#define HiReg		32	// Double register to hold multiply result
#define LoReg		33
#define PCReg		34	// Current program counter
#define NextPCReg	35	// Next program counter (for branch delay) 
#define PrevPCReg	36	// Previous program counter (for debugging)
#define LoadReg		37	// The register target of a delayed load.
#define LoadValueReg 	38	// The value to be loaded by a delayed load.
#define BadVAddrReg	39	// The failing virtual address on an exception

#define NumTotalRegs 	40
```
Nel contesto del sistema operativo NachOs e della sua architettura basata su MIPS, l'utilizzo dell'assembly per alcune parti delle system call è cruciale per ragioni di prestazioni e accesso diretto all'hardware. L'assembly è in grado di gestire in modo più efficiente operazioni a basso livello, come la gestione dei registri e l'accesso ai registri di memoria.

Prima dell'effettiva esecuzione delle system call è necessaria una gestione dei registri affinchè vi siano presenti all'interno tutte le informazioni per fare eseguire correttamente il codice. É presente uno stub per chiamata di sistema, che inserisce il codice della system call da eseguire nel registro r2, e mantiene i valori degli argomenti (in altre parole, arg1 è in r4, arg2 è in r5, arg3 è in r6, arg4 è in r7). Il valore restituito è in r2. Ciò segue la convenzione di chiamata C standard su MIPS.

Di seguito parte del codice in assembly MIPS di *start.s* e definito in *start.c* per poter essere utilizzato nella modadlità utente. 

```s
       .text   
       .align  2

	.globl __start
	.ent	__start
__start:
	jal	main
	move	$4,$0		
	jal	Exit	 /* if we return from main, exit(0) */
	.end __start

	.globl Halt
	.ent	Halt
Halt:
	addiu $2,$0,SC_Halt
	syscall
	j	$31
	.end Halt

	.globl Exit
	.ent	Exit
Exit:
	addiu $2,$0,SC_Exit
	syscall
	j	$31
	.end Exit

	.globl Exec
	.ent	Exec
Exec:
	addiu $2,$0,SC_Exec
	syscall
	j	$31
	.end Exec

	.globl Join
	.ent	Join
Join:
	addiu $2,$0,SC_Join
	syscall
	j	$31
	.end Join

	.globl CreateFile
	.ent	CreateFile
```
Una volta gestito il contenuto dei registri e chiamando la Machine::RaiseException, si passa alla gestione delle eccezioni.

Il file *exception.cc* rappresenta l'entry point nel kernel di NachOS per i programmi utente. Qui, le eccezioni, gli interrupt e le system call vengono gestite nel loro complesso. Questo file contiene la gestione delle eccezioni, l'aggiornamento del program counter (PC) e le funzioni per il passaggio da modalità kernel a modalità utente e viceversa.

La gestione di eccezioni, interrupt e system call viene effettuata tramite l'utilizzo di uno switch case. Come parametro per la selezione del tipo di chiamata si utilizzano i valori definiti dalla tabella delle system call (syscall.h) che va ad associare un numero identificativo univoco (SYSCALL-ID) a ciascuna system call.

```c
//Define System call

#define SC_Halt			0
#define SC_Exit			1
#define SC_Exec			2
#define SC_Join			3
#define SC_CreateFile   4
#define SC_Open			5
#define SC_Read			6
#define SC_Write		7
#define SC_Close		8

```

Un'implementazione dello switch case è la seguente:
    
```c
void ExceptionHandler(ExceptionType which)
{
    int type = machine->ReadRegister(2);

	
	switch (which) {
	case NoException:
		return;

	case PageFaultException:
		DEBUG('a', "\n No valid translation found");
		printf("\n\n No valid translation found");
		interrupt->Halt();
		break;

	case ReadOnlyException:
		DEBUG('a', "\n Write attempted to page marked read-only");
		printf("\n\n Write attempted to page marked read-only");
		interrupt->Halt();
		break;

	case BusErrorException:
		DEBUG('a', "\n Translation resulted invalid physical address");
		printf("\n\n Translation resulted invalid physical address");
		interrupt->Halt();
		break;

	case AddressErrorException:
		DEBUG('a', "\n Unaligned reference or one that was beyond the end of the address space");
		printf("\n\n Unaligned reference or one that was beyond the end of the address space");
		interrupt->Halt();
		break;

	case OverflowException:
		DEBUG('a', "\nInteger overflow in add or sub.");
		printf("\n\n Integer overflow in add or sub.");
		interrupt->Halt();
		break;

	case IllegalInstrException:
		DEBUG('a', "\n Unimplemented or reserved instr.");
		printf("\n\n Unimplemented or reserved instr.");
		interrupt->Halt();
		break;

	case NumExceptionTypes:
		DEBUG('a', "\n Number exception types");
		printf("\n\n Number exception types");
		interrupt->Halt();
		break;

	case SyscallException:
		switch (type){

		case SC_Halt:
			DEBUG('a', "\nShutdown, initiated by user program. ");
			printf("\nShutdown, initiated by user program. ");
			interrupt->Halt();
			return;

        default:
			break;
		}
		IncreasePC();
	}
}
 ```


---

PARTE VECCHIA

Andando ad approfondire la gestione delle system call in NachOS possiamo notare come vengono suddivise ed eseguite le varie parti di codice che ci garantiscono il funzionamento voluto.

In particolar modo i file che andremo ad analizzare sono: machine.cc (con machine.h), exception.cc (con exception.h), start.c (simile a start.s) e syscall.h.

In machine.h vanno definite le strutture dati per simulare l'esecuzione dei programmi utente eseguite sopra Nachos. 
I programmi utente vengono caricati nella "mainMemory". All'interno della memoria è presente anche il kernel, ma viene caricato in una regione di memoria separata da quella dell'utente.
I programmi e gli accessi alla memoria del kernel non vengono tradotti o paginati. I programmi utente vengono eseguiti un'istruzione alla volta, dal simulatore. Ogni riferimento alla memoria viene tradotto e controllato per la verifica degli errori.
All'interno della classe machine viene definita l'hardware della workstation host simulata, come visto dai programmi utente: i registri della CPU, la memoria principale, ecc. I programmi utente non dovrebbero essere in grado di riconoscere che sono in esecuzione sul simulatore o sull'hardware reale.
Per l'invocazione delle system call o nel caso di eccezioni è necessario passare dalla modalità utente alla modalità kernel. Questo è reso possibile dal metodo Machine::RaiseException 

```c
//	"which" -- the cause of the kernel trap
//	"badVaddr" -- the virtual address causing the trap, if appropriate
void
Machine::RaiseException(ExceptionType which, int badVAddr)
{
    DEBUG('m', "Exception: %s\n", exceptionNames[which]);
    
//  ASSERT(interrupt->getStatus() == UserMode);
    registers[BadVAddrReg] = badVAddr;
    DelayedLoad(0, 0);			// finish anything in progress
    interrupt->setStatus(SystemMode);
    ExceptionHandler(which);		// interrupts are enabled at this point
    interrupt->setStatus(UserMode);
}
```

Purchè questo sia possibile è necessaria una gestione a basso livello dei registri della CPU. Di seguito è riportata la definizione del set completo di registri MIPS e in aggiunta altri registri per poter avviare/arrestare un programma utente tra due istruzioni qualsiasi (tenendo traccia di load, delay solts, ecc.)

```c
#define StackReg	29	// User's stack pointer
#define RetAddrReg	31	// Holds return address for procedure calls
#define NumGPRegs	32	// 32 general purpose registers on MIPS
#define HiReg		32	// Double register to hold multiply result
#define LoReg		33
#define PCReg		34	// Current program counter
#define NextPCReg	35	// Next program counter (for branch delay) 
#define PrevPCReg	36	// Previous program counter (for debugging)
#define LoadReg		37	// The register target of a delayed load.
#define LoadValueReg 	38	// The value to be loaded by a delayed load.
#define BadVAddrReg	39	// The failing virtual address on an exception

#define NumTotalRegs 	40
```

Il linguaggio assembly aiuta a effettuare chiamate di sistema al kernel Nachos. C'è uno stub per chiamata di sistema, che inserisce il codice per la chiamata di sistema nel registro r2, e lascia da soli gli argomenti della chiamata di sistema (in altre parole, arg1 è in r4, arg2 è in r5, arg3 è in r6, arg4 è in r7) Il valore restituito è in r2. Ciò segue la convenzione di chiamata C standard su MIPS.

Di seguito parte del codice in assembly MIPS di start.s e definito in start.c per poter essere utilizzato nella modadlità utente. 

```s
       .text   
       .align  2

	.globl __start
	.ent	__start
__start:
	jal	main
	move	$4,$0		
	jal	Exit	 /* if we return from main, exit(0) */
	.end __start

	.globl Halt
	.ent	Halt
Halt:
	addiu $2,$0,SC_Halt
	syscall
	j	$31
	.end Halt

	.globl Exit
	.ent	Exit
Exit:
	addiu $2,$0,SC_Exit
	syscall
	j	$31
	.end Exit

	.globl Exec
	.ent	Exec
Exec:
	addiu $2,$0,SC_Exec
	syscall
	j	$31
	.end Exec

	.globl Join
	.ent	Join
Join:
	addiu $2,$0,SC_Join
	syscall
	j	$31
	.end Join

	.globl CreateFile
	.ent	CreateFile
```

Una volta gestito il contenuto dei registri e chiamando la Machine::RaiseException, si passa alla gestione delle eccezioni.

Il file *exception.cc* rappresenta l'entry point nel kernel di NachOS per i programmi utente. Qui, le eccezioni, gli interrupt e le system call vengono gestite nel loro complesso. Questo file contiene la gestione delle eccezioni, l'aggiornamento del program counter (PC) e le funzioni per il passaggio da modalità kernel a modalità utente e viceversa.

La gestione di eccezioni, interrupt e system call viene effettuata tramite l'utilizzo di uno switch case. Come parametro per la selezione del tipo di chiamata si utilizzano i valori definiti dalla tabella delle system call (syscall.h) che va ad associare un numero identificativo univoco (SYSCALL-ID) a ciascuna system call.

```c
//Define System call

#define SC_Halt			0
#define SC_Exit			1
#define SC_Exec			2
#define SC_Join			3
#define SC_CreateFile   4
#define SC_Open			5
#define SC_Read			6
#define SC_Write		7
#define SC_Close		8

```

Un'implementazione dello switch case è la seguente:
    
```c
void ExceptionHandler(ExceptionType which)
{
    int type = machine->ReadRegister(2);

	
	switch (which) {
	case NoException:
		return;

	case PageFaultException:
		DEBUG('a', "\n No valid translation found");
		printf("\n\n No valid translation found");
		interrupt->Halt();
		break;

	case ReadOnlyException:
		DEBUG('a', "\n Write attempted to page marked read-only");
		printf("\n\n Write attempted to page marked read-only");
		interrupt->Halt();
		break;

	case BusErrorException:
		DEBUG('a', "\n Translation resulted invalid physical address");
		printf("\n\n Translation resulted invalid physical address");
		interrupt->Halt();
		break;

	case AddressErrorException:
		DEBUG('a', "\n Unaligned reference or one that was beyond the end of the address space");
		printf("\n\n Unaligned reference or one that was beyond the end of the address space");
		interrupt->Halt();
		break;

	case OverflowException:
		DEBUG('a', "\nInteger overflow in add or sub.");
		printf("\n\n Integer overflow in add or sub.");
		interrupt->Halt();
		break;

	case IllegalInstrException:
		DEBUG('a', "\n Unimplemented or reserved instr.");
		printf("\n\n Unimplemented or reserved instr.");
		interrupt->Halt();
		break;

	case NumExceptionTypes:
		DEBUG('a', "\n Number exception types");
		printf("\n\n Number exception types");
		interrupt->Halt();
		break;

	case SyscallException:
		switch (type){

		case SC_Halt:
			DEBUG('a', "\nShutdown, initiated by user program. ");
			printf("\nShutdown, initiated by user program. ");
			interrupt->Halt();
			return;

        default:
			break;
		}
		IncreasePC();
	}
}
 ```