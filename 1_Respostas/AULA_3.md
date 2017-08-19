# Aula 03
## Data: 16/08/2017

``1.`` Dada uma variável `a` do tipo `char` (um byte), escreva os trechos de código em C para:

(a) Somente setar o bit menos significativo de `a`.
	
  ```C
  	a = a | 0x01;
  ```
(b) Somente setar dois bits de `a`: o menos significativo e o segundo menos significativo.
	
```C 
	a = a | 0x03;
```  
  
(c) Somente zerar o terceiro bit menos significativo de `a`.

```C
        a = a & (~0x04);
```
  
(d) Somente zerar o terceiro e o quarto bits menos significativo de `a`.

```C
    a = a & (~0x0C);
```	
  
(e) Somente inverter o bit mais significativo de `a`.

```C
    a = a ^ (0x80);
```	

(f) Inverter o nibble mais significativo de `a`, e setar o nibble menos significativo de `a`. 

```C
    a = a ^ (0xF0);
    a = a | (0x0F);
```	

2. Considerando a placa Launchpad do MSP430, escreva o código em C para piscar os dois LEDs ininterruptamente

```C
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1 | LED2)

int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;
	P1DIR |= LEDS; // LEDS como saída
	P1OUT = 0x00; //  Saída em nível baixo
	while(1)
		P1OUT ^= LEDS; // Piscar os LEDS (toggle)
	return 0;
}
```

3. Considerando a placa Launchpad do MSP430, escreva o código em C para piscar duas vezes os dois LEDs sempre que o usuário pressionar o botão.

```C
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1 | LED2)
#define BTN BIT3

int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;
	P1DIR |= LEDS; // LEDS como saída
	P1OUT = 0x00; //  Saída em nível baixo
	int auxiliar = 0;
	
	while(1)
	{	while((P1IN & BTN) != 0) {};
		P1OUT ^= LEDS; // Liga os LEDS
		P1OUT ^= LEDS; // desliga os LEDS
		P1OUT ^= LEDS; // liga os LEDS
		P1OUT ^= LEDS; // desliga os LEDS
		while((P1IN & BTN) == 0) {};
	};
	return 0;
}
```

4. Considerando a placa Launchpad do MSP430, faça uma função em C que pisca os dois LEDs uma vez.

```C
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1 | LED2)

void pisca_leds (void)
{
	P1OUT ^= LEDS;
	P1OUT ^= LEDS;

}
```

5. Reescreva o código da questão 2 usando a função da questão 4.

```C
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1 | LED2)

void pisca_leds (void)
{
	P1OUT ^= LEDS;
	P1OUT ^= LEDS;

}

int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;
	P1DIR |= LEDS; // LEDS como saída
	P1OUT = 0x00; //  Saída em nível baixo
	while(1)
		pisca_leds(); // Chama a função e pisca os LEDS (toggle)
	return 0;
}
```


6. Reescreva o código da questão 3 usando a função da questão 4.

```C
#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6
#define LEDS (LED1 | LED2)
#define BTN BIT3

void pisca_leds (void)
{
	P1OUT ^= LEDS;
	P1OUT ^= LEDS;

}

int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;
	P1DIR |= LEDS; // LEDS como saída
	P1OUT = 0x00; //  Saída em nível baixo
	int auxiliar = 0;
	
	while(1)
	{	while((P1IN & BTN) != 0) {};
		pisca_leds(); // Chama a função e pisca os LEDS (toggle)
		pisca_leds(); // Chama a função e pisca os LEDS (toggle)
		while((P1IN & BTN) == 0) {};
	};
	return 0;
}
```

