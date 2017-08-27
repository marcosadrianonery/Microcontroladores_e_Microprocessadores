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
	bis R4, R5
	```
	(b) Somente setar dois bits de R6: o menos significativo e o segundo menos significativo.
	```C 
	mov.w #3, R4
	bis R4, R6
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
	bis R4, R9
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
	bis R4, R9
	FIM
	mov.w #000F, R4
	bis R4, R9
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
	add.w #10,R4
	ELSE:
	sub.w R6, R4
	sub.w #10,R4
	FIM:
	```
3. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
while(save[i]!=k) i++;


```

4. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
for(i=0; i<100; i++) A[i] = i*2;
```

5. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

```C
for(i=99; i>=0; i--) A[i] = i*2;
```
