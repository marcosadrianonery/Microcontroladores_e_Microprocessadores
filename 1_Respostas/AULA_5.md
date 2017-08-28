Para as questões 2 a 5, considere que as variáveis `f`, `g`, `h`, `i` e `j` são do tipo inteiro (16 bits na arquitetura do MSP430), e que o vetor `A[]` é do tipo inteiro. Estas variáveis estão armazenadas nos seguintes registradores:
	f: R4
	g: R5
	h: R6
	i: R7
	j: R8
	A: R9
Utilize os registradores R11, R12, R13, R14 e R15 para armazenar valores temporários.

1. Escreva os trechos de código assembly do MSP430 para:
	(a) Somente setar o bit menos significativo de R5.
	```C 
	mov.w #1, R4
	bis.w R4, R5
	```
	(b) Somente setar dois bits de R6: o menos significativo e o segundo menos significativo.
	```C 
	mov.w #3, R4
	bis.w R4, R6
	```
	(c) Somente zerar o terceiro bit menos significativo de R7.
	```C 
	mov.w #4, R4
	inv.w R4
	and.w R4, R7
	```	
	(d) Somente zerar o terceiro e o quarto bits menos significativo de R8.
	```C 
	mov.w #C, R4
	inv.w R4
	and.w R4, R8
	```	
	(e) Somente inverter o bit mais significativo de R9.
	```C 
	mov.w #7FFF, R4
	mov.w R9, R5
	and.w R4, R5
	cmp R5, R9
	jeq FIM
	mov.w #8000, R4
	bis.w R4, R9
	FIM
	```	
	(f) Inverter o nibble mais significativo de R10, e setar o nibble menos significativo de R10. 
	```C 
	mov.w #0FFF, R4
	mov.w R, R5
	and.w R4, R5
	cmp R5, R9
	jeq FIM
	mov.w #F000, R4
	bis.w R4, R9
	FIM
	mov.w #000F, R4
	bis.w R4, R9
	```
2. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
if(i>j) f = g+h+10;
else f = g-h-10;
```
```C
	IF:
	mov.w R5, R4
	cmp R8, R7
	jge ELSE ;SE R7 >= R8, VAI PARA ELSE
	add.w R6, R4
	add.w #A,R4
	ELSE:
	sub.w R6, R4
	sub.w #A,R4
	FIM:
```
3. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
while(save[i]!=k) i++;
```

```
; i = R7
;k = R11
;save = R13

loop:
mov.w R7, R12	
rla R12		
add.w R13, R12	
cmp 0(R12), R11
jne end
inc.w R7	
jmp loop
end:

```
4. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
for(i=0; i<100; i++) A[i] = i*2;
```

```
; i = R7, A = R9
mov.w #100, R11	
mov.w #0, R7	
loop:
mov.w R7, R12	
rla R12		
mov.w R12, R13	
add.w R9, R12	
cmp R7, R11
jge end
mov.w R13, 0(R12)
inc.w R7
jmp loop
end:
```

5. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
for(i=99; i>=0; i--) A[i] = i*2;
```


```
; i = R7, A = R9
mov.w #99, R7	
loop:
mov.w R7, R12	
rla R12		
mov.w R12, R13	
add.w R9, R12	
cmp R7, #0
jl end
mov.w R13, 0(R12)
dec.w R7
jmp loop
end:
```
