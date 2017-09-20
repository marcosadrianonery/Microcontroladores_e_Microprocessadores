# Respostas às Perguntas Aula 10
## Data: 18/09/2017
1. Projete o hardware necessário para o MSP430 controlar um motor DC de 12V e 4A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V, beta = 100 e Vce(saturação) = 0,2 V. Além disso, considere que Vcc = 3 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.

2. Projete o hardware necessário para o MSP430 controlar um motor DC de 10V e 1A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V e beta = 120. Além disso, considere que Vcc = 3,5 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.

3. Projete o hardware utilizado para controlar 6 LEDs utilizando charlieplexing. Apresente os pinos utilizados no MSP430 e os LEDs, nomeados L1-L6.

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
/*
 * Defina a função `void EscreveDigito(volatile char dig);` que escreve 
 * um dos dígitos 0x0-0xF em um único display de 7 segmentos via porta
 * P1, baseado na figura abaixo. Considere que em outra parte do código
 * os pinos P1.0-P1.6 já foram configurados para corresponderem aos LEDs
 * A-G, e que estes LEDs possuem resistores externos para limitar a
 * corrente.

        ---  ==> A
       |   |
 F <== |   | ==> B
       |   |
        ---  ==> G
       |   |
 E <== |   | ==> C
       |   |
        ---  ==> D
 */

#include <msp430.h>

void EscreveDigito(volatile char dig)
{
	/* Considerar a ordem A-G = P1.0-P1.6 */
	/* Catodo comum, acende em 1, se for anodo é só inverter com ~ */
	const char zero = 	0x7E;
	const char one = 	0x30;
	const char two =	0x6D;
	const char three =	0x79;
	const char four =	0x33;
	const char five =	0x5B;
	const char six =	0x5F;
	const char seven = 	0x70;
	const char eight = 	0x7F;
	const char nine = 	0x7B;
	const char A10 =	0x77;
	const char B11 = 	0x1F;
	const char C12 =	0x4E;
	const char D13 =	0x3D;
	const char E14 =	0x4F;
	const char F15 =	0x47;
	
	switch(dig){
		case '0':
		P1OUT |= zero;
		break;
		
		case '1':
		P1OUT |= one;
		break;
		
		case '2':
		P1OUT |= two;
		break;
		
		case '3':
		P1OUT |= three;
		break;
		
		case '4':
		P1OUT |= four;
		break;
		
		case '5':
		P1OUT |= five;
		break;
		
		case '6':
		P1OUT |= six;
		break;
		
		case '7':
		P1OUT |= seven;
		break;
		
		case '8':
		P1OUT |= eight;
		break;
		
		case '9':
		P1OUT |= nine;
		break;
		
		case 'A':
		P1OUT |= A10;
		break;
		
		case 'B':
		P1OUT |= B11;
		break;
		
		case 'C':
		P1OUT |= C12;
		break;
		
		case 'D':
		P1OUT |= D13;
		break;
		
		case 'E':
		P1OUT |= E14;
		break;
		
		case 'F':
		P1OUT |= F15;
		break;
	}
}
```


7. Multiplexe 2 displays de 7 segmentos para apresentar a seguinte sequência em loop:
	00 - 11 - 22 - 33 - 44 - 55 - 66 - 77 - 88 - 99 - AA - BB - CC - DD - EE - FF
