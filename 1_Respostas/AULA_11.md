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

7. Multiplexe 2 displays de 7 segmentos para apresentar a seguinte sequência em loop:
	00 - 11 - 22 - 33 - 44 - 55 - 66 - 77 - 88 - 99 - AA - BB - CC - DD - EE - FF
