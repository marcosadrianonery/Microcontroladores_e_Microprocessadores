# Respostas às Perguntas Aula 9
## Data: 18/09/2017

1. Escreva uma função em C que faz o debounce de botões ligados à porta P1.

```C
#include <msp430g2553.h>

int Deboucing(int porta)
{
  /*A função recebe qual porta deve realizar o deboucing e retorna o valor lido da porta P1.X 
  onde X refere-se a porta escolhida*/
  /*Supondo que a porta P1 já está definida.*/
  
  x = P1IN;
  x &= porta;
  for(i=0;i<5000;i++)
  {
    if((P1IN&porta) != x))
      {
        i = 0; //Houve variação na entrada, deve-se reiniciar a contagem;
      }
  }
  return x;
}
int main( void )
{
  // Stop watchdog timer to prevent time out reset
  WDTCTL = WDTPW + WDTHOLD;

  return 0;
}
```
*****
2. Escreva um código em C que lê 9 botões multiplexados por 6 pinos, e pisca os LEDs da placa Launchpad de acordo com os botões. Por exemplo, se o primeiro botão é pressionado, os LEDs piscam uma vez; se o segundo botão é pressionado, os LEDs piscam duas vezes; e assim por diante. Se mais de um botão pressionado, os LEDs não piscam.
```C
#include <msp430.h>
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1|LED2)
#define BARRA_Y (BIT1+BIT2+BIT3)
#define BARRA_X (BIT4+BIT5+BIT6)

int main(void)
{
// Parar o watchdog timer
	WDTCTL = WDTPW | WDTHOLD;

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

	int numero = 0;

	while(1)
	    {
	        numero = multiplexa();
	        pisca_led(numero);
	        numero = 0;
	    }
	    return 0;
}

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

int multiplexa(void)
	{
    P1DIR |= LEDS;            // Configurar os LEDs E Y_BARRA COMO SAÍDA
    P1DIR &= ~(BARRA_X);                // TODOS COMO ENTRADA
    P1REN |= BARRA_X ;                          // Configurar pull-up e pull-down
    P1OUT |= BARRA_X;                       // Escolher pull-UPP PARA X
    P1OUT &= ~LEDS;

    ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

    int linha = 1, coluna = 1, i, j, BTN = 0, aux=0;

//////////////////////////////////////////////////////////////////////////////////////////////
//                         LINHAS --- [PULL-UP] ----- BARRA_X
//////////////////////////////////////////////////////////////////////////////////////////////

	if (((P1IN & BIT4) == 0)&&((P1IN & BIT5) == BIT5)&&((P1IN & BIT7) == BIT7))
	        {
	            linha = 1;
	        } else if (((P1IN & BIT4) == BIT4)&&((P1IN & BIT5) == 0)&&((P1IN & BIT7) == BIT7))
	        {
	            linha = 2;
	        } else if (((P1IN & BIT4) == BIT4)&&((P1IN & BIT5) == BIT5)&&((P1IN & BIT7) == 0))
	        {
	            linha = 3;
	        } else { linha = 1;}
/////////////////////////////////////////////////////////////////////////////////////////////////////////////

	P1DIR &= 0;
    P1DIR |= LEDS;            // Configurar os LEDs E Y_BARRA COMO SAÍDA
    P1DIR &= ~(BARRA_Y);                // TODOS COMO ENTRADA
    P1REN |= BARRA_Y ;                          // Configurar pull-up e pull-down
    P1OUT |= BARRA_Y;                       // Escolher pull-UPP PARA X

//////////////////////////////////////////////////////////////////////////////////////////////
//                         COLUNAS --- [PULL-DOWN] ----- BARRA_Y
//////////////////////////////////////////////////////////////////////////////////////////////

	    if (((P1IN & BIT1) == 0)&&((P1IN & BIT2) == BIT2)&&((P1IN & BIT3)) == BIT3)
	    {
	        coluna = 3;
	    } else if (((P1IN & BIT1) == BIT1)&&((P1IN & BIT2) == 0)&&((P1IN & BIT3) == BIT3))
	    {
	        coluna = 2;
	    } else if (((P1IN & BIT1) == BIT1)&&((P1IN & BIT2) == BIT2)&&((P1IN & BIT3) == 0))
	    {
	        coluna = 3;
	    } else { coluna = 0;}

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

	    ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	    if(linha==0 || coluna==0)
	    {
	        return 0; /*Nesse caso, nenhum botao foi pressionado, ou dois foram pressionados de uma vez*/
	    }
	    else
	    {
	    for (i = 1; i <= 4; i++)
	    {
	        for (j = 1; j <= 4; j++)
	                {
	                    aux++;
	                    if ((linha == i)&&(coluna == j))
	                    {
	                        BTN = aux;
	                    }

	                }
	    }
	    }
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	    return BTN;
}

void delay(volatile unsigned long int t)
{
    while(t--);
}

void pisca_led(int num_blink)
{
    while(num_blink>0)
    {
        num_blink--;
        P1OUT ^= LED1;
        delay(30000);
        P1OUT ^= LED1;
        delay(30000);
    }
}
```
*****




