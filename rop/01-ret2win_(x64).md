array lungo 32, ma legge 56
-> 0x004006f4      ba20000000     mov edx, 0x20               ; 32 ; size_t n
-> lettura dei 56 char │           0x0040073c      4889c6         mov rsi, rax                ; void *buf
│           0x0040073f      bf00000000     mov edi, 0                  ; int fildes
│           0x00400744      e847feffff     call sym.imp.read           ; ssize_t read(int fildes, void *buf, size_t nbyte)

obiettivo arrivare alla funzione ret2win
Floddare lo stack abusando dei 56byte in modo tale da piazzare l'indirizzo di ret2win nel return address
Utilizzare checksec per controllare le bariere di sicurezze del binario
template create un template basandosi su un binario pwn template ret2win > exploit.py
-
io.interactive svuta il buffer e rende interattivo l'attacco.
RELRO - relocation table, le tabelle della libc sono solo read only. le tabelle posso essere risolte solo quando necessarie oppure pre caricata e messa in readonly alla esecuzione del binario (+sicuro).
Lo stack non è eseguibile, vuol dire che non possiamo usare gli shell code.
PIE - randomizzazione degli indirizzi.
No PIE - 0x400000, tutte le funzioni indiziano per quel numero.
-
per passare la scritta per riempire il buffer ret2win < file
Variabile ebx tra return address e variabili.
1 motivo per cui non funziona l'attacco è non avere lo stack allineato, capita solo nelle sfide a 64, 32 no.
Come mettere apposto lo stack? usare i gadget, usare un gadget che faccia la return (ROPGadget --binary ret2win)
In questo caso ci serve la ret per riordinare lo stack.
chromium system call, cerca rdi, popolare il primo parametro della funzione con il valore che voglio io.
-
disabilitare la randomizzazione degli indirizzi.
echo "0" > /proc/sys/kernel