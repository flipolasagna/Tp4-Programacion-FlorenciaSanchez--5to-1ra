# ˗ˏˋTrabajo Practico N°4´ˎ˗
## ⿻Consignas:
#### ▸ Utilice un display de 7 segmentos para imlplementar lo siguiente:
##### 1. Realizar un contador de 0 a 9 en binario que se uncremente al presionar un pulsador. Mostrar el resultado de la ceunta en un display.
##### 2. Agregar al problema anterior un pulsador que decremente el numero
###### NOTA: El incremento o decremento debe de producirse en una unidad por pulsacion.

## ⿻Se forma por:
###### • Arduino nano.

###### • 1 Display 7 segmentos.

###### • 2 pulsadores.

###### •7 resistencias de 100Ω

# ˗ˏˋInforme´ˎ˗
 
 ##  ⿻Codigo 
#### » lo primero a realizar fue declarar y definir las macros.
###### ↳ Defino los pines(macros.h)
```c
#define bot1 ((PINC >> 5) & 0x01) // Defino los botones
#define bot2 ((PINC >> 4) & 0x01) // Defino los botones

#define g (PORTB |= (1 << PB0)) // Defino g
#define f (PORTD |= (1 << PD2)) // Defino f
#define e (PORTD |= (1 << PD3)) // Defino e
#define d (PORTD |= (1 << PD4)) // Defino d
#define c (PORTD |= (1 << PD5)) // Defino c
#define b (PORTD |= (1 << PD6)) // Defino b
#define a (PORTD |= (1 << PD7)) // Defino a
```
###### ↳ inicio
```c
#include <Arduino.h>
#include "macros.h"
#include "util/delay.h"

int main() {
```
###### ↳ Luego Declaro los botones como entradas y los leds como salida
```c
  DDRC &= ~(1 << PC5); // aca configuro como entrada
  PORTC |= (1 << PC5); // aca prendo el pull up
  DDRC &= ~(1 << PC4); // aca configuro como entrada
  PORTC |= (1 << PC4); // aca prendo el pull up

  DDRB |= (1 << PB0); // En esta linea configuro como salida
  DDRD |= (1 << PD2); // En esta linea configuro como salida
  DDRD |= (1 << PD3); // En esta linea configuro como salida
  DDRD |= (1 << PD4); // En esta linea configuro como salida
  DDRD |= (1 << PD5); // En esta linea configuro como salida
  DDRD |= (1 << PD6); // En esta linea configuro como salida
  DDRD |= (1 << PD7); // En esta linea configuro como salida
```
###### ↳ creo el vector y la variable contador
```c
//                         0             1           2           3           4           5           6           7           8          9
  int8_t numbers[10] = {0b0000000001, 0b11100101, 0b10010000, 0b11000000, 0b01100100, 0b01001000, 0b00001000, 0b11100001, 0b0000000, 0b01100000};// Defino el vector
  //                                                                                                            fedcba g
  uint8_t cont = 0;
```
#### » Lo segundo a realizar fue el codigo principal
###### ↳ Comienzo preguntando por el boton1 para que realice el incremento (punto 1).
```c
while (1)
  {
    if (bot1 == 0) // pregunto si el boton1 se apreto
    {
      _delay_ms(100); // Utilizo un antirrebote(esperando a que las minimas vibraciones que ocacionan las chapitas del boton se detengan y ahi realmente evaluar el estado del boton)
      while (bot1 == 0){} // Se queda en un bucle preguntando si se apreto el boton1 
      _delay_ms(100); // vuelvo a utilizar un antirebote
      cont++;         // incremento el contador el cual se encargaria de ir incrementando los numeros
      if (cont > 9) //  Condiciono al contador no sobrepasar a la posicion 9 (num. 9)
      {
        cont = 0; // si llega a la posicion 9 vuelve a la posicion 0 (num. 0)
      }
    }
    PORTD = (PORTD & 0b00000011) | ~(numbers[cont] & 0b11111100); // |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTD)
    PORTB = (PORTB & 0b11111110) | ~(numbers[cont] & 0b00000001); // |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTB)
```
###### ↳ Pregunto por el boton2 correspondientes para que realice el decremento (punto 2)
 ```c
    if (bot2 == 0) // pregunto si el boton2 se apreto
    {
      _delay_ms(100); // Utilizo un antirrebote(esperando a que las minimas vibraciones que ocacionan las chapitas del boton se detengan y ahi realmente evaluar el estado del boton)
      while (bot2 == 0){} // Se queda en un bucle preguntando si se apreto el boton1 +++
      _delay_ms(100); // vuelvo a utilizar un antirebote
      
      if (cont == 0) //  Condiciono al contador a no sobrepasar a la posicion 0 (num. 0) y asi hacer que no se muestre, en el display, cualquier otro numero el cualno haya configuradop
      {
        cont = 9; // si llega a la posicion 0 vuelve a la posicion 9 (num. 9)
      }
      else // si la condicion es falsa el contador se decrementa
      {
        cont--;
      }

      PORTD = (PORTD & 0b00000011) | (numbers[cont] & 0b11111100); // |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTD)
      PORTB = (PORTB & 0b11111110) | (numbers[cont] & 0b00000001); // |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTB)
    }
  }
}
```


