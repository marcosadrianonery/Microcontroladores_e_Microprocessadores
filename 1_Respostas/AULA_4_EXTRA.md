Para todas as questões, considere que as variáveis `f`, `g`, `h`, `i` e `j` são do tipo inteiro (16 bits na arquitetura do MSP430), e que o vetor `A[]` é do tipo inteiro. Estas variáveis estão armazenadas nos seguintes registradores:

- f: R4
- g: R5
- h: R6
- i: R7
- j: R8
- A: R9

Utilize os registradores R11, R12, R13, R14 e R15 para armazenar valores temporários.

1. Traduza as seguintes linhas em C para a linguagem assembly do MSP430. Utilize somente as seguintes instruções: mov.w, add.w, sub.w, clr.w, dec.w, decd.w, inc.w e incd.w.

(a) `f *= 5;`
 ```C
    mov.w R4, R11
    add.w R4, R11
    add.w R4, R11
    add.w R4, R11
    add.w R11, R4
 ```
(b) `g *= 6;`
 ```C
    mov.w R5, R11
    add.w R5, R11
    add.w R5, R11
    add.w R5, R11
    add.w R5, R11
    add.w R11, R4
 ```
(d) `A[2] = 6*A[1] + 5*A[0];`
 ```C
 
    mov.w 2(R9), R11 ; Coloca A[1] em R11;
    
    mov.w R11, R12
    add.w R12, R11
    add.w R12, R11
    add.w R12, R11
    add.w R12, R11
    add.w R12, R11 ; A[1] vezes 6
    mov.w R11, 2(R9)
    
    mov.w 0(R9), R11 ; Coloca A[0] em R11;
    mov.w R11, R12
    add.w R12, R11
    add.w R12, R11
    add.w R12, R11
    add.w R12, R11 ; A[1] vezes 6
    mov.w R11, 0(R9)
    
    mov.w 0(R9), 4(R9) ; move A[0] para A[2]
    add.w 2(R9), 4(R9) ; soma A[2] com A[1] e coloca em A[2]
 ```
(e) `A[3] = 3*f - 5*h;`
 ```C
    mov.w R4, R12
    add.w R12, R4
    add.w R12, R4
    
    mov.w R6, R12
    add.w R12, R6
    add.w R12, R6
    add.w R12, R6
    add.w R12, R6
    
    mov.w R4, 6(R9) ; move f para A[3]
    sub.w R6, 6(R9) ; subtrai A[3] com 5*h  e coloca em A[2]
 ```
(f) `A[5] = 6*(f - 2*h);`
```C
    mov.w R6, R12 ; move h para outro registrador
    add.w R12, R6 ; h*2
    
    sub.w R6, R4
    
    add.w R4, R4
    add.w R4, R4
    add.w R4, R4
    add.w R4, R4
    add.w R4, R4
    
    mov.w R4, 10(R9) ; move para A[5]
 ```




