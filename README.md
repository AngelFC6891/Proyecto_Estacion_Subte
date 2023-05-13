# Proyecto Estaciones de Subte
![Tinkercad](./img/ArduinoTinkercad.jpg)

___
## Materia:
- Sistemas de Procesamiento de Datos
___
## Profesor:
- Darío Cuda
## Tutor:
- Estaben Quiroz
___
## Integrante 
- Ángel Fabián Cabello
___

## Proyecto: Estaciones de Subte
![Tinkercad](./img/ProyectoEstacionesSubte.png)
___
## Descripción
El presente modelo de Tinkercad simula la llegada de una formación de subte a cuatro estaciones consecutivas de la linea C, en el siguiente orden: Moreno, Independiencia, San Juan y Constitución. Entonces, al iniciar la simulación, la formación llega a la estación de Moreno, se enciende un led (el rojo) indicando su llegada, el display de 7 segmentos muestra el número de estaciones faltantes para llegar a destino y suena el zumbador a modo de sirena de aviso. Además, a medida que la formación avanza, se modifica el tono del zumbador de manera que al llegar a destino, el tono en este punto sea distingible de los anteriores. Este ciclo se repite indicando el avance de la formación a través de las estaciones.  
Por último, el modelo también incluye un interruptor para reiniciar el ciclo antes mencionado.
___
## Elementos de Tinkercad utilizados
- Arduino: 1 unidad
- Display de 7 segmentos: 1 unidad
- LEDs: 4 unidades
- Zumbador (o Buzzer): 1 unidad
- Interruptor: 1 unidad
- Resistencia de 220 Ohm: 11 unidades
- Cables
___
## Programación del Arduino
El código de control del Arduino está programado en C++.
### 1. Etiquetado de pines
~~~
#define A 11
#define B 12
#define C A3
#define D A4
#define E A5
#define F 10
#define G 9
#define BUZZER A0
#define INTERRUPTOR 8
#define LED_ROJO 7
#define LED_AZUL 6
#define LED_VERDE 5
#define LED_AMARILLO 4
~~~

### 2. Variables globales
Se declaran las siguientes variables globales, cuya utilidad se verá más adelante:
~~~
int DESACTIVAR = 4;
bool flag_presionado = false;
int reinicio;
int tiempo_apagado;
int tiempo_transcurrido;
int inicio = millis();
~~~
### 3. Funciones

### :file_folder: 3.1. __setup__
Los pines correspondientes a los LEDs y al zumbador se declaran como OUTPUT (salida). Solamente el pin del interruptor se declara INPUT_PULLUP (entrada), por cual al aplicarle la función digitalRead marcará 0 cuando reciba voltaje y 1, cuando no.
~~~
void setup()
{
  pinMode(A,OUTPUT);
  pinMode(B,OUTPUT);
  pinMode(C,OUTPUT);
  pinMode(D,OUTPUT);
  pinMode(E,OUTPUT);
  pinMode(F,OUTPUT);
  pinMode(G,OUTPUT);
  pinMode(BUZZER,OUTPUT);
  pinMode(INTERRUPTOR,INPUT_PULLUP);
  Serial.begin(9600);
}
~~~

### :file_folder: 3.2.  __loop__

~~~
void loop()
{ 
  int entradaINTERRUPTOR = digitalRead(INTERRUPTOR);
  
  if(entradaINTERRUPTOR == 0){
    if(flag_presionado == false){
      tiempo_transcurrido = millis() - inicio;
      activar_todo(temporizador(tiempo_transcurrido));
    }
    else{
      reinicio = millis() - tiempo_apagado;
      activar_todo(temporizador(reinicio));
    }
  }
  else{
    activar_todo(DESACTIVAR);
    delay(10);
    tiempo_apagado = millis();
    if(flag_presionado == false){
      flag_presionado = true;
    }
  }
}
~~~


### :file_folder: 3.3.  __activar_todo__
Esta función:
- recibe un entero entre 0 y 4 (incluidos);
- no tiene retorno;
- se encarga de:
  - encender y apagar los LEDs mediante digitalWrite;
  - encender y apagar los segmentos del display mostrando el dígito correspondiente a través de la función auxiliar __activar_display__;
  - encender y apagar el buzzer a través de la función auxiliar __activar_buzzer__ (el apagado del buzzer se realiza por defecto dentro de esta última)

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

:file_folder: activar_display

Esta función 

:file_folder:

:file_folder:


:file_folder: loop (bucle)



## :robot: Link al proyecto
- [proyecto](https://www.tinkercad.com/things/hq86m6GMRpG)
## :tv: Link al video del proceso
- [N/A]

---
### Fuentes
- [Consejos para documentar](https://www.sohamkamani.com/how-to-write-good-documentation/#architecture-documentation).

- [Lenguaje Markdown](https://markdown.es/sintaxis-markdown/#linkauto).

- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

- [Tutorial GitHub](https://www.youtube.com/watch?v=QaM7zoaEmjo).

- [Emojis](https://gist.github.com/rxaviers/7360908).

---






