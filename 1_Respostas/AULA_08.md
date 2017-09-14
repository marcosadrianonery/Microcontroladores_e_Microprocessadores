Para todas as questões, utilize os LEDs e/ou os botões da placa Launchpad do MSP430.

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

2. Escreva um código em C que pisca os LEDs ininterruptamente. No ciclo que pisca os LEDs, o tempo que os LEDs ficam ligados deve ser duas vezes maior do que o tempo que eles ficam desligados.

3. Escreva um código em C que acende os LEDs quando o botão é pressionado.

4. Escreva um código em C que pisca os LEDs ininterruptamente somente se o botão for pressionado.

5. Escreva um código em C que acende os LEDs quando o botão é pressionado. Deixe o MSP430 em modo de baixo consumo, e habilite a interrupção do botão.
