# Respostas às Perguntas Aula 10
## Data: 18/09/2017
1. Projete o hardware necessário para o MSP430 controlar um motor DC de 12V e 4A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V, beta = 100 e Vce(saturação) = 0,2 V. Além disso, considere que Vcc = 3 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.
```
Dados:

Vbe = 0,7 V
β = 100
VCE = 0,2 V
Vcc = 3 V
Imax,msp = 10 ma

A primeira coisa é obter um transistor que suporte a corrente de coletor dada pela carga,
que nesse caso é o motor DC. A corrente de coletor necessária será de 4 A.
A corrente de base será nesse caso:

Ib = Ic / 100 = 4 / 100 = 40 mA.

Como a corrente de base ultrapassa a corrente que a porta digital pode fornecer,
então deve-se utilizar dois transistores, na forma do par Darlington.
Então a corrente de base será:

Ib = Ic / β2 = 4 /10000 = 0,4 mA

O resistor Rb = (Vcc – 2*Vbe)/Ib = (3 – 2*0,7)/0,4*10^(-3) = 4kΩ 

```
*****

2. Projete o hardware necessário para o MSP430 controlar um motor DC de 10V e 1A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V e beta = 120. Além disso, considere que Vcc = 3,5 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.

```
Vbe = 0,7 V
β = 120
VCE = 0,2 V
Vcc = 3,5 V
Imax,msp = 10 ma

A corrente de coletor Ic = 1A. A corrente de base será então

Ib = Ic/β = 1/120 = 8,33 mA.

Rb = (Vcc-Vbe)/Ib
= (Vcc-Vbe)*β/Ic = ((3,5 - 0,7)*120)/1 = 336 Ω

```
*****

3. Projete o hardware utilizado para controlar 6 LEDs utilizando charlieplexing. Apresente os pinos utilizados no MSP430 e os LEDs, nomeados L1-L6.
```
P1.1----R-------------------------------------------------
                  |       |                       |       |
                L1▼     L2▲                       |       |
                  |       |                       |       |
P1.2----R--------------------------             L5▼     L6▲
                          |       |               |       |
                        L3▼     L4▲               |       |
                          |       |               |       |
P1.3----R-------------------------------------------------
```
*****
4. Defina a função `void main(void){}` para controlar 6 LEDs de uma árvore de natal usando o hardware da questão anterior. Acenda os LEDs de forma que um ser humano veja todos acesos ao mesmo tempo.

```C

#include <msp430g2553.h>
#include <msp430.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1|LED2)
#define LINHA1 BIT1
#define LINHA2 BIT2
#define LINHA3 BIT4
#define BTN BIT3

int main(void)
{
    WDTCTL = WDTPW|WDTHOLD;  //parar wdtctl--watchdog timer
    P1DIR |= LEDS;          //DEFINE LEDS COMO SAIDAS.
    int n;
    while(1)
    {
///////////////////////////////////////////////////////////////////
//                  LED-1
///////////////////////////////////////////////////////////////////
    P1DIR |= (LINHA1 + LINHA2);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
    P1DIR &= ~(LINHA3);            //DEFINE LINHA3 COMO ENTRADA
    P1OUT &= 0;
    P1OUT |= LINHA1;           // SETA LINHA1
    //for ( n = 0; n < 0XFFFF; n++);
    ///////////////////////////////////////////////////////////////////
    //                  LED-2
    ///////////////////////////////////////////////////////////////////
        P1DIR |= (LINHA1 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
        P1DIR &= ~(LINHA2);            //DEFINE LINHA3 COMO ENTRADA
        P1OUT &= 0;
        P1OUT |= LINHA1;           // SETA LINHA1
        //for ( n = 0; n < 0XFFFF; n++);
        ///////////////////////////////////////////////////////////////////
        //                  LED-3
        ///////////////////////////////////////////////////////////////////
            P1DIR |= (LINHA1 + LINHA2);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
            P1DIR &= ~(LINHA3);            //DEFINE LINHA3 COMO ENTRADA
            P1OUT &= 0;
            P1OUT |= LINHA2;           // SETA LINHA1
            //for ( n = 0; n < 0XFFFF; n++);
            ///////////////////////////////////////////////////////////////////
            //                  LED-4
            ///////////////////////////////////////////////////////////////////
                P1DIR |= (LINHA2 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                P1DIR &= ~(LINHA1);            //DEFINE LINHA3 COMO ENTRADA
                P1OUT &= 0;
                P1OUT |= LINHA2;           // SETA LINHA1
                //for ( n = 0; n < 0XFFFF; n++);
///////////////////////////////////////////////////////////////////
//                  LED-5
///////////////////////////////////////////////////////////////////
                            P1DIR |= (LINHA1 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                            P1DIR &= ~(LINHA2);            //DEFINE LINHA3 COMO ENTRADA
                            P1OUT &= 0;
                            P1OUT |= LINHA3;           // SETA LINHA1
                            //for ( n = 0; n < 0XFFFF; n++);
///////////////////////////////////////////////////////////////////
//                  LED-6
///////////////////////////////////////////////////////////////////
                                P1DIR |= (LINHA2 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                                P1DIR &= ~(LINHA1);            //DEFINE LINHA3 COMO ENTRADA
                                P1OUT &= 0;
                                P1OUT |= LINHA3;           // SETA LINHA1
                                //for ( n = 0; n < 0XFFFF; n++);

        }
    return 0;
            }
```
*****

5. Defina a função `void main(void){}` para controlar 6 LEDs de uma árvore de natal usando o hardware da questão 3. Acenda os LEDs de forma que um ser humano veja os LEDs L1 e L2 acesos juntos por um tempo, depois os LEDs L3 e L4 juntos, e depois os LEDs L5 e L6 juntos.

```C
#include "io430.h"
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1|LED2)
#define LINHA1 BIT1
#define LINHA2 BIT2
#define LINHA3 BIT4
#define BTN BIT3

int main(void)
{
    WDTCTL = WDTPW|WDTHOLD;  //parar wdtctl--watchdog timer
    P1DIR |= LEDS;          //DEFINE LEDS COMO SAIDAS.
    int n;
    while(1)
    {
      
///////////////////////////////////////////////////////////////////
//                  LED-1 E LED-2
///////////////////////////////////////////////////////////////////
    P1DIR |= (LINHA1 + LINHA2);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
    P1DIR &= ~(LINHA3);            //DEFINE LINHA3 COMO ENTRADA
    P1OUT &= 0;
    P1OUT |= LINHA1;           // SETA LINHA1
    //for ( n = 0; n < 0XFFFF; n++);
    ///////////////////////////////////////////////////////////////////
        P1DIR |= (LINHA1 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
        P1DIR &= ~(LINHA2);            //DEFINE LINHA3 COMO ENTRADA
        P1OUT &= 0;
        P1OUT |= LINHA1;           // SETA LINHA1
        for ( n = 0; n < 0XFFFF; n++);
        ///////////////////////////////////////////////////////////////////
        //                  LED-3 E LED-4
        ///////////////////////////////////////////////////////////////////
            P1DIR |= (LINHA1 + LINHA2);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
            P1DIR &= ~(LINHA3);            //DEFINE LINHA3 COMO ENTRADA
            P1OUT &= 0;
            P1OUT |= LINHA2;           // SETA LINHA1
            //for ( n = 0; n < 0XFFFF; n++);
            ///////////////////////////////////////////////////////////////////
                P1DIR |= (LINHA2 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                P1DIR &= ~(LINHA1);            //DEFINE LINHA3 COMO ENTRADA
                P1OUT &= 0;
                P1OUT |= LINHA2;           // SETA LINHA1
                for ( n = 0; n < 0XFFFF; n++);
///////////////////////////////////////////////////////////////////
//                  LED-5 E LED-6
///////////////////////////////////////////////////////////////////
                            P1DIR |= (LINHA1 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                            P1DIR &= ~(LINHA2);            //DEFINE LINHA3 COMO ENTRADA
                            P1OUT &= 0;
                            P1OUT |= LINHA3;           // SETA LINHA1
                            //for ( n = 0; n < 0XFFFF; n++);
///////////////////////////////////////////////////////////////////
                                P1DIR |= (LINHA2 + LINHA3);            //DEFINE LINHA1 E LINHA2 COMO SAIDA
                                P1DIR &= ~(LINHA1);            //DEFINE LINHA3 COMO ENTRADA
                                P1OUT &= 0;
                                P1OUT |= LINHA3;           // SETA LINHA1
                                for ( n = 0; n < 0XFFFF; n++);

        }
    return 0;
            }
	    
```

*****

6. Defina a função `void EscreveDigito(volatile char dig);` que escreve um dos dígitos 0x0-0xF em um único display de 7 segmentos via porta P1, baseado na figura abaixo. Considere que em outra parte do código os pinos P1.0-P1.6 já foram configurados para corresponderem aos LEDs A-G, e que estes LEDs possuem resistores externos para limitar a corrente.

```
        ---  ==> A
       |   |
 F <== |   | ==> B
       |   |
        ---  ==> G
       |   |
 E <== |   | ==> C
       |   |
        ---  ==> D
```

```C
#include <msp430g2553.h>
#include <msp430.h>

/*
 *      ---  ==> A
       |   |
 F <== |   | ==> B
       |   |
        ---  ==> G
       |   |
 E <== |   | ==> C
       |   |
        ---  ==> D
*/

void EscreveDigito(volatile char dig)
{
    char entrada;
    int ZERO = 0x7E;
    int UM   = 0x60;
    int DOIS = 0x6D;
    int TRES = 0x79;
    int QUATRO = 0x33;
    int CINCO = 0x5B;
    int SEIS = 0x1F;
    int SETE = 0x70;
    int OITO = 0x7F;
    int NOVE = 0x73;
    int A = 0x77;
    int B = 0x1F;
    int C12 = 0x4D;
    int D = 0x3D;
    int E = 0x4F;
    int F = 0x47;
    entrada = dig;
	unsigned int P1OUT;
    if (entrada == 0)
    {
        P1OUT &= 0;         // desliga o que estiver acesso, zera saida
        P1OUT |= ZERO;      //seta a saida, com a entrada
    }
    else if (entrada == 1)
    {
        P1OUT &= 0;
        P1OUT |= UM;
    }
    else if (entrada == 2)
    {
        P1OUT &= 0;
        P1OUT |= DOIS;
    }
    else if (entrada == 3)
    {
        P1OUT &= 0;
        P1OUT |= TRES;
    }
    else if (entrada == 4)
    {
        P1OUT &= 0;
        P1OUT |= QUATRO;
    }
    else if (entrada == 5)
    {
        P1OUT &= 0;
        P1OUT |= CINCO;
    }
    else if (entrada == 6)
    {
        P1OUT &= 0;
        P1OUT |= SEIS;
    }
    else if (entrada == 7)
    {
        P1OUT &= 0;
        P1OUT |= SETE;
    }
    else if (entrada == 8)
    {
        P1OUT &= 0;
        P1OUT |= OITO;
    }
    else if (entrada == 9)
    {
        P1OUT &= 0;
        P1OUT |= NOVE;
    }
    else if (entrada == 'A')
    {
        P1OUT &= 0;
        P1OUT |= A;
    }
    else if (entrada == 'B')
    {
        P1OUT &= 0;
        P1OUT |= B;
    }
    else if (entrada == 'C')
    {
        P1OUT &= 0;
        P1OUT |= C12;
    }
    else if (entrada == 'D')
    {
        P1OUT &= 0;
        P1OUT |= D;
    }else if (entrada == 'E')
    {
        P1OUT &= 0;
        P1OUT |= E;
    }else if (entrada == 'F')
    {
        P1OUT &= 0;
        P1OUT |= F;
    }
}

```

*****

7. Multiplexe 2 displays de 7 segmentos para apresentar a seguinte sequência em loop:
	00 - 11 - 22 - 33 - 44 - 55 - 66 - 77 - 88 - 99 - AA - BB - CC - DD - EE - FF
	
```C
#include <msp430g2553.h>
#include <msp430.h>

/*
 *      ---  ==> A
       |   |
 F <== |   | ==> B
       |   |
        ---  ==> G
       |   |
 E <== |   | ==> C
       |   |
        ---  ==> D
    CATODO COMUM ==> LIGA COM SAIDA IGUAL A 1


*/

void EscreveDigito(volatile char dig)
{
    char entrada;
    int ZERO = 0x7E;
    int UM   = 0x60;
    int DOIS = 0x6D;
    int TRES = 0x79;
    int QUATRO = 0x33;
    int CINCO = 0x5B;
    int SEIS = 0x1F;
    int SETE = 0x70;
    int OITO = 0x7F;
    int NOVE = 0x73;
    int A = 0x77;
    int B = 0x1F;
    int C12 = 0x4D;
    int D = 0x3D;
    int E = 0x4F;
    int F = 0x47;
    entrada = dig;
    if (entrada == 0)
    {
        P1OUT &= 0;         // desliga o que estiver acesso, zera saida
        P1OUT |= ZERO;      //seta a saida, com a entrada
    }
    else if (entrada == 1)
    {
        P1OUT &= 0;
        P1OUT |= UM;
    }
    else if (entrada == 2)
    {
        P1OUT &= 0;
        P1OUT |= DOIS;
    }
    else if (entrada == 3)
    {
        P1OUT &= 0;
        P1OUT |= TRES;
    }
    else if (entrada == 4)
    {
        P1OUT &= 0;
        P1OUT |= QUATRO;
    }
    else if (entrada == 5)
    {
        P1OUT &= 0;
        P1OUT |= CINCO;
    }
    else if (entrada == 6)
    {
        P1OUT &= 0;
        P1OUT |= SEIS;
    }
    else if (entrada == 7)
    {
        P1OUT &= 0;
        P1OUT |= SETE;
    }
    else if (entrada == 8)
    {
        P1OUT &= 0;
        P1OUT |= OITO;
    }
    else if (entrada == 9)
    {
        P1OUT &= 0;
        P1OUT |= NOVE;
    }
    else if (entrada == 'A')
    {
        P1OUT &= 0;
        P1OUT |= A;
    }
    else if (entrada == 'B')
    {
        P1OUT &= 0;
        P1OUT |= B;
    }
    else if (entrada == 'C')
    {
        P1OUT &= 0;
        P1OUT |= C12;
    }
    else if (entrada == 'D')
    {
        P1OUT &= 0;
        P1OUT |= D;
    }else if (entrada == 'E')
    {
        P1OUT &= 0;
        P1OUT |= E;
    }else if (entrada == 'F')
    {
        P1OUT &= 0;
        P1OUT |= F;
    }
}

int main (void)
{
    char sequencia [] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 'A', 'B', 'C', 'D', 'E', 'F'};
    int n=0;
    while(1)
    {
    for(n=0; n<= 15; n++)
    {
        P2OUT |= 0x03;                 // ZERAR SAIDAS P2
        P2DIR |= BIT0;              // DEFINIR SAIDA PARA ACIONAR DISPLAY 1
        P2OUT |= ~BIT0;              // COMUM DO DISPLAY CATHODO
        EscreveDigito(sequencia [n]);
        P2OUT &= 0x03;                 // ZERAR SAIDAS P2
        P2DIR |= BIT1;              // DEFINIR SAIDA PARA ACIONAR DISPLAY 1
        P2OUT |= ~BIT1;              // COMUM DO DISPLAY CATHODO
        EscreveDigito(sequencia [n]);
    }
    }
    return 0;
}

```
