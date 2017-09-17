Para cada questão, escreva funções em C e/ou sub-rotinas na linguagem Assembly do MSP430. Reaproveite funções e sub-rotinas de uma questão em outra, se assim desejar. Leve em consideração que as sub-rotinas são utilizadas em um código maior, portanto utilize adequadamente os registradores R4 a R11. As instruções da linguagem Assembly do MSP430 se encontram ao final deste texto.

1. (a) Escreva uma função em C que calcule a raiz quadrada `x` de uma variável `S` do tipo float, utilizando o seguinte algoritmo: após `n+1` iterações, a raiz quadrada de `S` é dada por

```
x(n+1) = (x(n) + S/x(n))/2
```

```C
unsigned int Raiz_Quadrada(unsigned int S);
{
      unsigned int s0, s1, diferenca = 0;
      s0 = S;
      if (s0>0)
      {
      do 
      {
      s1 = (s0 + s/s0)/2;
      diferenca = (s1 - s0);
      s0 = s1;	
      } while((diferenca)!=0);
      } else s1 = 0;
  return s1;
}
```
O protótipo da função é:

```C
unsigned int Raiz_Quadrada(unsigned int S);
```

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável `S` é fornecida pelo registrador R15, e a raiz quadrada de `S` (ou seja, a variável `x`) é fornecida pelo registrador R15 também.

```
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #25, R15                  ;ENTRA O VALOR 2 EM R15
        CALL    #RAIZ                   ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          DIVISÃO
;=============================================================================== 
      
DIVISAO:                                    ; FUNÇAO
        CLR.W R13
	TST.W R14                          ;R14==0?
        JNZ DIV_LACO
        CLR R15
        RET                               
DIV_LACO:
        SUB.W R14, R15                    ;GUARDA R15 NA PILHA
        INC.W R13
        CMP.W R14,R15                     ; 
        JGE DIV_LACO                      ; SE R15 >= R14 pula
        MOV R13, R15
        RET
;===============================================================================
;            RAIZ
;===============================================================================

RAIZ:
        CLR.W R11
        MOV.W R15, R14
        MOV.W R15, R12                    ;VALOR DE ENTRADA SALVO EM R12 
        CMP.W #1,R15                     ; 
        JGE MAIOR_0                      ; SE R15 >= R14 pula
        CLR.W R15
MAIOR_0:
        CALL #DIVISAO             ; s/s0
        ADD.W R14,R15            ; SOMANDO S0-R14, COM S-R15: (s0 + s/s0)
        RRA.W R15                ; DIVIDINDO POR 2: s1 = (s0 + s/s0)/2;
        SUB.W R14,R15                     ; 
        JZ RESULT_RAIZ                      ; SE R15 - R14 == 0, pula
        ;INC.W R11
        ;RRA.W R11
        ;CMP.W #999,R11                     ; 
        ;JGE RESULT_RAIZ                     ; SE R11 >= X pula
        
        ;SUB.W #999, R11
        ;JZ RESULT_RAIZ
        ADD.W R14, R15
        MOV.W R15, R14
        MOV.W R12, R15
        CALL #MAIOR_0
RESULT_RAIZ:
        CLR.W R15
        ADD.W R14,R15
        RET
        END
```
2. (a) Escreva uma função em C que calcule `x` elevado à `N`-ésima potência, seguindo o seguinte protótipo: 

```C
int Potencia(int x, int N);
```
```C
int Potencia(int x, int N)
{
	unsigned int saida, i;
	saida = 1;
    for (i=0; i < N; i++)
	{	
		saida = saida * x;
	}	
	return saida;
}
```

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. `x` e `n` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida no registrador R15.
```C
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #21, R15                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #1, R14                  ;ENTRA O VALOR 2 EM R14
        CALL    #POTENCIA                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          MULTIPLICAÇÃO 
;=============================================================================== 
      
MULT:                                    ; FUNÇAO
        TST.W R13                          ;R14==0?
        JNZ MULT_LACO
        CLR R15
        RET                               
MULT_LACO:
        PUSH.W R15                         ;GUARDA R15 NA PILHA
        DEC.W R13                         ;GUARDA R14 NA PILHA
        CALL #MULT
        POP.W R13                          ; JOGA O VALOR DA PILHA EM R14
        ADD.W R13,R15
        RET
;===============================================================================
;            POTENCIA
;===============================================================================

POTENCIA:                               ; FUNÇAO
        MOV.W R15, R12
        TST.W R14                          ;R14==0?
        JZ VALOR_SALVO

        DEC.W R14                       ; O primeiro termo e equivalente a 2
        JZ POT_FIM                      ; Se expoente é 1, a saida é a base
VALOR_SALVO:
        MOV.W R12, R13                      ;Na função de mult, o valor foi
                                            ;mudado agora resgatamos o valor
                                            ;de base
        TST.W R14                          ;R14==0?
        JNZ POT_DIF_0
        CLR R15
        BIS.W #0X0001, R15
        RET 
POT_DIF_0:
        DEC.W R14
        CALL #MULT                           ;VOLTA VALOR DE R14
        TST.W R14
        JNZ VALOR_SALVO
POT_FIM:     
        RET
        END
```

3. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula a divisão de `a` por `b`, onde `a`, `b` e o valor de saída são inteiros de 16 bits. `a` e `b` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida através do registrador R15.


```
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #999, R15               ;ENTRA O VALOR DE a EM R15
        MOV.W   #3, R14                 ;ENTRA O VALOR DE b EM R14
        CALL    #DIVISAO               ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          DIVISÃO
;=============================================================================== 
      
DIVISAO:                                    ; FUNÇAO
        CLR.W R13
	TST.W R14                          ;R14==0?
        JNZ DIV_LACO
        CLR R15
        RET                               
DIV_LACO:
        SUB.W R14, R15                    ;GUARDA R15 NA PILHA
        INC.W R13
        CMP.W R14,R15                     ; 
        JGE DIV_LACO                      ; SE R15 >= R14 pula
        MOV R13, R15
        RET
        END
```
4. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o resto da divisão de `a` por `b`, onde `a`, `b` e o valor de saída são inteiros de 16 bits. `a` e `b` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida através do registrador R15.

```C

#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #100, R15               ;ENTRA O VALOR DE a EM R15
        MOV.W   #3, R14                 ;ENTRA O VALOR DE b EM R14
        CALL    #DIVISAO               ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          RESTO
;=============================================================================== 
      
DIVISAO:                                    ; FUNÇAO
        CLR.W R13
	TST.W R14                          ;R14==0?
        JNZ DIV_LACO
        CLR R15
        RET                               
DIV_LACO:
        SUB.W R14, R15                    ;GUARDA R15 NA PILHA
        INC.W R13
        CMP.W R14,R15                     ; 
        JGE DIV_LACO                      ; SE R15 >= R14 pula
        RET
        END
	
```

5. (a) Escreva uma função em C que indica a primalidade de uma variável inteira sem sinal, retornando o valor 1 se o número for primo, e 0, caso contrário. Siga o seguinte protótipo:

```C
int Primalidade(unsigned int x);
```

```C
int Primalidade(unsigned int x)
{
	unsigned int n, m, mult = 0;
    
	for (n=2; n <= x/2; n++)
	{
		   for (m=2; m <= x/2; m++)
		   {
			   mult = n * m;
			   if(mult == x)
			   {
				   return 0;   
			   }
		   }
	}
	return 1;
}
```

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

```
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #17, R15                  ;ENTRA O VALOR 2 EM R15
        CALL    #PRIMALIDADE_INICIAL                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          RESTO
;=============================================================================== 
      
RESTO:                                    ; FUNÇAO
        CLR.W R13                          ; AUXILIAR
        TST.W R14                          ;R14==0?
        JNZ DIV_LACO
        CLR R15
        RET                               
DIV_LACO:
        SUB.W R14, R12                    ;GUARDA R15 NA PILHA
        INC.W R13
        CMP.W R14,R12                     ; 
        JGE DIV_LACO                      ; SE R15 >= R14 pula
        RET
;===============================================================================
;            PRIMALIDADE
;===============================================================================

PRIMALIDADE_INICIAL:                               ; FUNÇAO
          MOV.W R15, R12
          MOV.W R15, R14

PRIMALIDADE: 
         DEC.W R14
         CMP.W #2, R14             ;SE R14 É MENOR
         JL PRIMO                  ; QUE 2 PULA PARA PRIMO
         CALL #AUXILIAR             ; VARIAVEL AUXILIAR
         TST.W R12                  
         JZ NAO_PRIMO               ;SE RESTO == O, NÃO É PRIMO
         CALL #PRIMALIDADE
AUXILIAR:
          MOV.W R15, R12            ; O VALOR DE R12, TEM DE SER
                                    ; ATUALIZADO
          JMP RESTO                 ; 

PRIMO:    
         MOV #1, R15
         JMP PRIMALIDADE_FIM        ; PRIMO
NAO_PRIMO:    
         MOV #0, R15         
         JMP PRIMALIDADE_FIM        ; NAO_PRIMO
PRIMALIDADE_FIM:
          RET
         END
```


6. Escreva uma função em C que calcula o duplo fatorial de n, representado por n!!. Se n for ímpar, n!! = 1*3*5*...*n, e se n for par, n!! = 2*4*6*...*n. Por exemplo, 9!! = 1*3*5*7*9 = 945 e 10!! = 2*4*6*8*10 = 3840. Além disso, 0!! = 1!! = 1.
O protótipo da função é:

```C
unsigned long long DuploFatorial(unsigned long long n);
```

```C
unsigned long long DuploFatorial(unsigned long long n)
{
	unsigned int i, paridade = 0;
	unsigned long long resultado = 1;
	if (n%2 == 0) //TESTAR SE É PAR
	{
		paridade = 2; // PAR
	} else paridade = 1; // IMPAR
	for (i = paridade ; i <= n ; i = i+2)
	{
		resultado = resultado * i;
		
	}		
	return resultado;
}
```

7. (a) Escreva uma função em C que calcula a função exponencial utilizando a série de Taylor da mesma. Considere o cálculo até o termo n = 20. O protótipo da função é `double ExpTaylor(double x);`

```C
double ExpTaylor(double x)
{
	unsigned int i, n;
	double resultado = 0, numerador = 1, denominador = 1;
	if(x==0) return 0;
	for (i=0 ; i < 20; i++)
	{
			denominador = 1;
			numerador = 1;
		    for (n=1; n <= i; n++)
	{	
		numerador = numerador * x;
		denominador = denominador * n;
	}
		resultado = resultado + (numerador/denominador);
	}
	return resultado;
}
```

EM ASSEMBY/// TERMINAR

```

#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #2, R15                  ;ENTRA O VALOR 2 EM R15
        CALL    #TAYLOR                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)

;===============================================================================
;          MULTIPLICAÇÃO 
;=============================================================================== 
      


MULT:                                    ; FUNÇAO
        TST.W R13                          ;R14==0?
        JNZ MULT_LACO
        CLR R15
        RET                               


MULT_LACO:
        PUSH.W R15                         ;GUARDA R15 NA PILHA
        DEC.W R13                         ;GUARDA R14 NA PILHA
        CALL #MULT
        POP.W R13                          ; JOGA O VALOR DA PILHA EM R14
        ADD.W R13,R15
        RET
;===============================================================================
;            POTENCIA
;===============================================================================

POTENCIA:                               ; FUNÇAO
        MOV.W R15, R12
        TST.W R14                          ;R14==0?
        JZ VALOR_SALVO

        DEC.W R14                       ; O primeiro termo e equivalente a 2
        JZ POT_FIM                      ; Se expoente é 1, a saida é a base
VALOR_SALVO:
        MOV.W R12, R13                      ;Na função de mult, o valor foi
                                            ;mudado agora resgatamos o valor
                                            ;de base
        TST.W R14                          ;R14==0?
        JNZ POT_DIF_0
        CLR R15
        BIS.W #0X0001, R15
        RET 
POT_DIF_0:
        DEC.W R14
        CALL #MULT                           ;VOLTA VALOR DE R14
        TST.W R14
        JNZ VALOR_SALVO
POT_FIM:     
        RET
;===============================================================================
;          DIVISÃO
;=============================================================================== 
      
DIVISAO:                                    ; FUNÇAO
        CLR.W R13
	TST.W R14                          ;R14==0?
        JNZ DIV_LACO
        CLR R15
        RET                               
DIV_LACO:
        SUB.W R14, R15                    ;GUARDA R15 NA PILHA
        INC.W R13
        CMP.W R14,R15                     ; 
        JGE DIV_LACO                      ; SE R15 >= R14 pula
        MOV R13, R15
        RET
;===============================================================================
;            TAYLOR
;===============================================================================

TAYLOR:
        MOV.W #1, R14
        MOV.W R15, R13
        ; R13 DEFINE POR QUEM O FATORIA TEM DE MULTIPLICAR
TAYLOR_0:        
        PUSH.W R14
        MOV R13, R15
        CALL #POTENCIA          ;CHAMA POTENCIA E FAZ (R15)^R14
        POP.W  R14
        PUSH.W R14
        PUSH.W 13
        CALL #DENOMINADOR
        POP.W 13
        POP.W  R14
        INC.W R14
        CALL #SOMA
        CMP.W #21, R14
        JGE FIM
        JMP TAYLOR_0   
DENOMINADOR:
        MOV R14, R13
        CALL #MULT
        DEC.W R13
        TST.W R13
        JNZ DENOMINADOR
        RET
SOMA: 
        CALL #DIVISAO
        ADD.W R15, R10
FIM:        
        RET
        END
	
```

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430, mas considere que os valores de entrada e de saída são inteiros de 16 bits. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

8. Escreva uma sub-rotina na linguagem Assembly do MSP430 que indica se um vetor esta ordenado de forma decrescente. Por exemplo:
[5 4 3 2 1] e [90 23 20 10] estão ordenados de forma decrescente.
[1 2 3 4 5] e [1 2 3 2] não estão.
O primeiro endereço do vetor é fornecido pelo registrador R15, e o tamanho do vetor é fornecido pelo registrador R14. A saída deverá ser fornecida no registrador R15, valendo 1 quando o vetor estiver ordenado de forma decrescente, e valendo 0 em caso contrário.
```
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #0x0A30, R15
        MOV.W   #99, 0(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #20, 2(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #14, 4(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #9, 6(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #0, 8(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #10, R14                 ;TAMANHO IGUAL A QUANT. VEZES 2
        CALL    #ORDENADO                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)
;===============================================================================
;            ORDENADO
;===============================================================================

ORDENADO:
        MOV.W #0, R13
ORDENADO_0:
        MOV.W 0(R15), R12
        INCD.W R15 
        INCD.W R13
        MOV.W 0(R15), R11
        CMP.W R14, R13
        JGE DECRESCENTE
        CMP.W R12, 0(R15)
        JGE NAO_ESTA
        JMP ORDENADO_0
        
DECRESCENTE: MOV.W #1, R15
             JMP FIM
NAO_ESTA:    MOV.W #0, R15
             JMP FIM
FIM:        
        RET
        END
	
```


9. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o produto escalar de dois vetores, `a` e `b`. O primeiro endereço do vetor `a` deverá ser passado através do registrador R15, o primeiro endereço do vetor `b` deverá ser passado através do registrador R14, e o tamanho do vetor deverá ser passado pelo registrador R13. A saída deverá ser fornecida no registrador R15.

```

#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #0x0A30, R15
        MOV.W   #2, 0(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 2(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 4(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #3, 6(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 8(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #0x0B30, R14
        MOV.W   #2, 0(R14)                  ;ENTRA O VALOR 2 EM R14
        MOV.W   #0, 2(R14)                  ;ENTRA O VALOR 2 EM R14
        MOV.W   #0, 4(R14)                  ;ENTRA O VALOR 2 EM R14
        MOV.W   #0, 6(R14)                  ;ENTRA O VALOR 2 EM R14
        MOV.W   #1, 8(R14)                  ;ENTRA O VALOR 2 EM R14
        MOV.W   #10, R13                 ;TAMANHO IGUAL A QUANT. VEZES 2
        CALL    #PRODUTO                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)
                                        
                                        
;===============================================================================
;          MULTIPLICAÇÃO 
;=============================================================================== 
MULT:                                    ; FUNÇAO
        TST.W R10                          ;R14==0?
        JNZ MULT_LACO
        CLR R11
        RET                               


MULT_LACO:
        PUSH.W R11                         ;GUARDA R15 NA PILHA
        DEC.W R10                         ;GUARDA R14 NA PILHA
        CALL #MULT
        POP.W R10                          ; JOGA O VALOR DA PILHA EM R14
        ADD.W R10,R11
        RET
;===============================================================================
;            PRODUTO ESCALAR
;===============================================================================

PRODUTO:
        MOV.W #0, R12             ;AUXILIAR
        MOV.W #0, R9
PRODUTO_0:
        MOV.W 0(R15), R11
        MOV.W 0(R14), R10
        CALL #MULT
        ADD.W R11, R9
        INCD.W R15
        INCD.W R14
        INCD.W R12
        CMP.W R13, R12
        JGE FIM
        JMP PRODUTO_0
        
FIM:
        MOV.W R9, R15
        RET
        END
```	

10. (a) Escreva uma função em C que indica se um vetor é palíndromo. Por exemplo:
	[1 2 3 2 1] e [0 10 20 20 10 0] são palíndromos.
	[5 4 3 2 1] e [1 2 3 2] não são.
Se o vetor for palíndromo, retorne o valor 1. Caso contrário, retorne o valor 0. O protótipo da função é:

```C
int Palindromo(int vetor[ ], int tamanho);
```

```C
int Palindromo(int vetor[ ], int tamanho)
{
	unsigned int i, n;

	for (i=0 ; i <= (tamanho/2); i++)
	{
			if (vetor[i] != vetor[tamanho - i -1])
			{
				return 0;			
			}	
	}
	return 1;
}
```

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. O endereço do vetor de entrada é dado pelo registrador R15, o tamanho do vetor é dado pelo registrador R14, e o resultado é dado pelo registrador R15.
```
#include "msp430.h"                     ; #define controlled include file

        NAME    main                    ; module name

        PUBLIC  main                    ; make the main label vissible
                                        ; outside this module
        ORG     0FFFEh
        DC16    init                    ; set reset vector to 'init' label

        RSEG    CSTACK                  ; pre-declaration of segment
        RSEG    CODE                    ; place program in 'CODE' segment

init:   MOV     #SFE(CSTACK), SP        ; set up stack

main:   NOP                             ; main program
        MOV.W   #WDTPW+WDTHOLD,&WDTCTL  ; Stop watchdog timer
        MOV.W   #0x0A30, R15
        MOV.W   #2, 0(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 2(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 4(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #99, 6(R15)                  ;ENTRA O VALOR 2 EM R15        
        MOV.W   #99, 8(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 10(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 12(R15)                  ;ENTRA O VALOR 2 EM R15
        MOV.W   #2, 14(R15)                  ;ENTRA O VALOR 2 EM R15
        
        MOV.W   #16, R14
        CALL    #PALINDROMO                ;CHAMA A FUNCAO
   
        JMP $                           ; jump to current location '$'
                                        ; (endless loop)
                                        
                                        
;===============================================================================
;          MULTIPLICAÇÃO 
;=============================================================================== 
MULT:                                    ; FUNÇAO
        TST.W R10                          ;R14==0?
        JNZ MULT_LACO
        CLR R11
        RET                               
MULT_LACO:
        PUSH.W R11                         ;GUARDA R15 NA PILHA
        DEC.W R10                         ;GUARDA R14 NA PILHA
        CALL #MULT
        POP.W R10                          ; JOGA O VALOR DA PILHA EM R14
        ADD.W R10,R11
        RET
;===============================================================================
;            PALINDROMO
;===============================================================================
PALINDROMO:
          MOV.W R14, R13
          ADD.W R15, R14
          DECD.W R14
          CMP.W 0(R14),0(R15)
          JNZ NAO_PALINDROMO
PALINDROMO_0:
          CMP.W R14, R15
          JGE E_PALINDROMO
          INCD.W R15
          DECD.W R14
          CMP.W 0(R15),0(R14)
          JZ PALINDROMO_0
          JNZ NAO_PALINDROMO
          
NAO_PALINDROMO:
          MOV.W #0, R15
          JMP FIM
E_PALINDROMO          
          MOV.W #1, R15
          JMP FIM
FIM:
          RET
          END
```            
