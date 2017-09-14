# RESPOSTAS AULA_08
## 12/09/2017 

#### Para todas as questões, utilize os LEDs e/ou os botões da placa Launchpad do MSP430.
*****

1. Escreva um código em C que pisca os LEDs ininterruptamente.

    ```C

    /*Piscar leds ininterrptamente.*/
    #include <msp430.h> // inclusão da biblioteca, para acessar funções do msp
    #define led1 bit0
    #define led2 bit6
    #define leds(led1|led2)
    
    int main(void)
    {
        //PARA PARAR O WHATCHDOG TIMER
        WDTCTL = WDTPW | WDTHOLD;
        //DEFINIR UMA VARIAVEL, TIPO INTEIRO PARA AUXILIO
        P1DIR = leds;
        volatile int i;    
        while(1)
        {
          //UM VALOR PARA DECREMENTAR E DEFINIR QUANTO DEVE ESTAR ACESSO/APAGADO
            i = 0xFFFF;
            P1OUT ^= LEDS;
         // ENQUANTO 'n' DIFERENTE DE '0'.     
            while (n!=0)
            {
            n--;
            }
        }
    }
    
    ```
*****
2. Escreva um código em C que pisca os LEDs ininterruptamente. No ciclo que pisca os LEDs, o tempo que os LEDs ficam ligados deve ser duas vezes maior do que o tempo que eles ficam desligados.

    ```C
    #include <msp430.h>
    #define led1 BIT0
    #define led2 BIT6
    #define leds(led1|led2)
  
    // FUNÇÃO PRINCIPAL
    int main (void)
    {
        WDTCTL = WDTPW | WDTHOLD; // PARA O WATCHDOG TIMER
        volatile int i; // VARIAVEL AUXILIAR
        P1DIR |= leds;
        
        while(1)
        {
            i = 0XFFFF;
            PIOUT ^= leds;       
            if (P1OUT == 0X0000)
            {
                while((i = i - 2) != 0x0000)
            }  else while(i-- != 0);
        }
        return 0;
    }


        
```
*****
3. Escreva um código em C que acende os LEDs quando o botão é pressionado.


```C
   
    #include <msp430.h>
    #define LED1 BIT0
    #define LED2 BIT6
    #define LEDS (LED1|LED2)
    #define BTN BIT3

        int main (void)
        {
            // PARAR WATCHDOG TIMER
            WDTCTL = WDTPW|WDTHOLD;
            // DEFINIR COMO SAIDA OS BITS 1 E 6, QUE SÃO OS LEDS
            P1DIR |= LEDS;
            /* DEFINIR COMO ENTRADA OS BIT3, DO BOTÃO
            AQUI PRIMEIRO NEGA O NUMERO, SE TINHAMOS (00001000), PASSAMOS A TER
            (11110111), ASSIM PELA AND O UNICO NUMERO RESETADO SERA O QUE QUEREMOS
            COMO SAIDA, O CORRESPENDENTE AO BOTÃO;*/
            P1DIR &= ~BTN;
            // Configurar pull-up e pull-down
	        P1REN |= BTN;
            // Escolher pull-down
        	P1OUT |= BTN;
            // Começar desligado, estrutura para reset os bits: P1OUT &= ~LEDS;
	        P1OUT &= ~LEDS;
            // ESTRUTURA DE REPETIÇÃO, PARA TER O LAÇO        
            while (1)
            {
                // QUANDO O BOTÃO É PRESSIONADO A ENTRADA É ZERO, ASSIM SE FOR PRESSIONADO SERÁ 
                //DIFERENTE
                if ((P1IN & BTN) == 0)
                {               
                P1OUT |= LEDS;
                } else P1OUT &= ~LEDS; //SENÃO, APAGA OS LEDS
            return 0;
            }
```

*****

4. Escreva um código em C que pisca os LEDs ininterruptamente somente se o botão for pressionado.
*****
5. Escreva um código em C que acende os LEDs quando o botão é pressionado. Deixe o MSP430 em modo de baixo consumo, e habilite a interrupção do botão.
*****
