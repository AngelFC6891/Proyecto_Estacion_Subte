# Proyecto Estaciones de Subte
![Tinkercad](./img/ArduinoTinkercad.jpg)

## Materia:
- Sistemas de Procesamiento de Datos

## Profesor:
- Darío Cuda
## Tutor:
- Estaben Quiroz

## Integrante 
- Ángel Fabián Cabello

## Proyecto: Estaciones de Subte
![Tinkercad](./img/ProyectoEstacionesSubte.png)


## Descripción
El presente modelo de Arduino simula la llegada de una formación de subte a cuatro estaciones consecutivas de la linea C, en el siguiente orden: Moreno, Independiencia, San Juan y Constitución. Entonces, al iniciar la simulación, el subte llega a la estación de Moreno, se enciende un led (el rojo) indicando su llegada, el display de 7 segmentos muestra el número de estaciones faltantes para llegar a destino y suena el zumbador a modo de sirena de aviso. Luego este ciclo se repite indicando el avance el subte a través de las estaciones. Nótese que a medida que el subte avanza se modifica el tono del zumbador de manera que al llegar a destino, el tono en este punto sea distingible de los anteriores

## Funciones utilizadas:
:page_facing_up: activar_todo

Esta función se encarga de:
- encender y apagar los leds
- encender y apagar los segmentos del display mostrando el dígito correspondiente
- encender y apagar el buzzer

código:
~~~ C++
void activar_todo(int digito_actual)
{
  switch(digito_actual)
  {
    case 4:
      activar_display(DESACTIVAR);
      digitalWrite(LED_ROJO,0);
      digitalWrite(LED_AZUL,0);
      digitalWrite(LED_VERDE,0);
      digitalWrite(LED_AMARILLO,0);
      break;
    case 3:
      activar_display(DESACTIVAR);
      digitalWrite(LED_AMARILLO,0);
      delay(20);
      digitalWrite(LED_ROJO,1);
      delay(10);
      activar_display(digito_actual);
      activar_buzzer(digito_actual);
      break;
    case 2:
      activar_display(DESACTIVAR);
      digitalWrite(LED_ROJO,0);
      delay(20);
      digitalWrite(LED_AZUL,1);
      delay(10);
      activar_display(digito_actual);
      activar_buzzer(digito_actual);
      break;
    case 1:
      activar_display(DESACTIVAR);
      digitalWrite(LED_AZUL,0);
      delay(20);
      digitalWrite(LED_VERDE,1);
      delay(10);
      activar_display(digito_actual);
      activar_buzzer(digito_actual);
	  break;
  	case 0:
      activar_display(DESACTIVAR);
      digitalWrite(LED_VERDE,0);
      delay(20);
      digitalWrite(LED_AMARILLO,1);
      delay(10);
      activar_display(digito_actual);    
      activar_buzzer(digito_actual);
      break;
  }
}
~~~

:page_facing_up: 

:page_facing_up:

:page_facing_up:


:page_facing_up: loop (bucle)


B0, B1, B2, B3 son #define que utilizamos para agregar los leds, asociandolo a pines de la placa arduino.

(Breve explicación de la función)

~~~ C (lenguaje en el que esta escrito)
void EncenderBinario(int estado3, int estado2,int estado1,int estado0)
{
  digitalWrite(B3,estado3);
  digitalWrite(B2,estado2);
  digitalWrite(B1,estado1);
  digitalWrite(B0,estado0);
}
~~~

## :robot: Link al proyecto
- [proyecto](https://www.tinkercad.com/things/hq86m6GMRpG)
## :tv: Link al video del proceso
- [video](N/A)

---
### Fuentes
- [Consejos para documentar](https://www.sohamkamani.com/how-to-write-good-documentation/#architecture-documentation).

- [Lenguaje Markdown](https://markdown.es/sintaxis-markdown/#linkauto).

- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

- [Tutorial](https://www.youtube.com/watch?v=oxaH9CFpeEE).

- [Emojis](https://gist.github.com/rxaviers/7360908).

---






