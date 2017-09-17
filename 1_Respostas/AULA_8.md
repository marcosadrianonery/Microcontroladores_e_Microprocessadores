# RESPOSTAS AULA_08
## 12/09/2017 

#### Para todas as questões, utilize os LEDs e/ou os botões da placa Launchpad do MSP430.
*****

1. Escreva um código em C que pisca os LEDs ininterruptamente.

```C

/*Piscar leds ininterrptamente.*/
#include <msp430.h> // inclusão da biblioteca, para acessar funções do msp
#include <msp430g2553.h>
#define led1 BIT0
#define led2 BIT6
#define leds (led1|led2)

void delay(volatile unsigned int n)
{
	while(n--);
}

int main(void)
    {
        //PARA PARAR O WHATCHDOG TIMER
        WDTCTL = WDTPW | WDTHOLD;
        //DEFINIR UMA VARIAVEL, TIPO INTEIRO PARA AUXILIO
        P1DIR |= leds;
        P1OUT &= ~leds;
        volatile int i;
        	while(1)
        	{
        		P1OUT ^= leds;
        		// ENQUANTO 'n' DIFERENTE DE '0'.
        		delay (0xFFFF);

        	}
    }
```
*****
2. Escreva um código em C que pisca os LEDs ininterruptamente. No ciclo que pisca os LEDs, o tempo que os LEDs ficam ligados deve ser duas vezes maior do que o tempo que eles ficam desligados.

    ```C
    #include <msp430.h>
    #define led1 BIT0
    #define led2 BIT6
    #define leds (led1|led2)

    // FUNÇÃO PRINCIPAL
    int main (void)
    {
        WDTCTL = WDTPW | WDTHOLD; // PARA O WATCHDOG TIMER
        volatile int i; // VARIAVEL AUXILIAR
        P1DIR |= leds;

        while(1)
        {
            i = 0XFFFF;
            P1OUT ^= leds;
            if (P1OUT == 0X0000)
            {
                while((i = i - 2) != 0x0000);
            }  else while(i-- != 0);
        }
        return 0;
    }
     
```

3. Escreva um código em C que acende os LEDs quando o botão é pressionado.
```
```C
#include <msp430.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1|LED2)
#define BTN BIT3
int main(void)
{
// PARAR WATCHDOG TIMER
	WDTCTL = WDTPW|WDTHOLD;
// DEFINIR COMO SAIDA OS BITS 1 E 6 QUE SÃO OS LEDS
	P1DIR |= LEDS;
	P1DIR &= ~BTN;
// Configurar pull-up e pull-down
	P1REN |= BTN;
// Escolher pull-down
	P1OUT |= BTN;
// Começar desligado, estrutura para reset os bits
	P1OUT &= ~LEDS;

	while (1)
	{
		if ((P1IN & BTN) == 0)
		{
		P1OUT |= LEDS;
		}
		else
		{
		P1OUT &= ~LEDS;
		}
	}
return 0;
}
```
4. Escreva um código em C que pisca os LEDs ininterruptamente somente se o botão for pressionado.

```C
#include <msp430g2553.h>
#include <msp430.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1|LED2)
#define BTN BIT3

void atraso(volatile unsigned int n)
{
	while(n--);
}


int main(void)
{
	WDTCTL = WDTPW|WDTHOLD;  //parar wdtctl--watchdog timer
	P1DIR |= LEDS;			//DEFINE LEDS COMO SAIDAS.
	P1DIR &= ~(BTN);			//DEFINE BTN COMO ENTRADA,
							//P1DIR <= XXXXX0XX.
	P1REN |= BTN;			//DEFINE PULL DOWN OU UP
	P1OUT |= BTN;			// DEFINE PULL UP, RES. ANTES
							// DO BOTÃO
	// Começar desligado, estrutura para reset os bits
	P1OUT &= ~LEDS;


	while(1)
	{
		if ((P1IN & BTN) == 0)
		{

					P1OUT |= LEDS;  //acende
					delay(0xFFFF);
					P1OUT ^= LEDS;	//apaga
					delay(0xFFFF);
		}
	}
	return 0;

}
```

5. Escreva um código em C que acende os LEDs quando o botão é pressionado. Deixe o MSP430 em modo de baixo consumo, e habilite a interrupção do botão.
