# Proyecto Estaciones de Subte
![Tinkercad](./img/ArduinoTinkercad.jpg)

___
## Materia:
- Sistemas de Procesamiento de Datos
___
## Profesor:
- Darío Cuda
## Tutor:
- Esteban Quiroz
___
## Autor del proyecto: 
- Ángel Fabián Cabello
___

## Proyecto: Estaciones de Subte
![Tinkercad](./img/ProyectoEstacionesSubte.png)
___
## Descripción
El presente modelo de Tinkercad simula el recorrido de una formación de subte a cuatro estaciones consecutivas de la linea C, en el siguiente orden: Moreno, Independiencia, San Juan y Constitución. Al iniciar la simulación, la formación llega a la estación de Moreno, se enciende un led (el rojo) indicando su llegada, el display de 7 segmentos muestra el número de estaciones faltantes para llegar a destino y suena el zumbador a modo de sirena de aviso. Además, a medida que la formación avanza, se modifica el tono del zumbador de manera que al llegar a destino, el tono en este punto sea distingible de los anteriores. Este ciclo se repite indicando el avance de la formación a través de las estaciones. Por último, el modelo también incluye un interruptor para reiniciar el recorrido antes mencionado.
___
## :electric_plug: Elementos de Tinkercad utilizados
- Arduino: 1 unidad
- Display de 7 segmentos: 1 unidad
- LEDs: 4 unidades
- Zumbador (o Buzzer): 1 unidad
- Interruptor: 1 unidad
- Resistencia de 220 Ohm: 11 unidades
- Cables
___
## :computer: Programación del Arduino
El código de control del Arduino está programado en C++.
### 1. Etiquetado de pines
~~~ C++
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
~~~ C++
int DESACTIVAR = 4;
bool flag_presionado = false;
int reinicio;
int tiempo_apagado;
int tiempo_transcurrido;
int inicio = millis();
~~~
### 3. Funciones

### :file_folder: 3.1. __setup__
Los pines correspondientes a los LEDs y al zumbador se declaran como OUTPUT (salida). Solamente el pin del interruptor se declara INPUT_PULLUP (entrada), o sea, que al aplicarle la función digitalRead marcará 0 cuando reciba voltaje y 1, cuando no.
~~~ C++
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

### :file_folder: 3.2. __calcular_digito__
Esta función:
- recibe valores enteros de la función __millis__;
- retorna valores enteros entre 3 y 0 (incluidos);
- funcionamiento:
  - recibe los milisegundos transcurridos en __loop__, los divide por 1000 para pasarlos a segundos y al resultado le aplica módulo 4, ya que de este modo los únicos resultados posibles van a ser 0, 1, 2 y 3;
  - dado que los milisegundos son crecientes, y por lo tanto también el ciclo de restos 0,1, 2 y 3; para hacerlos decrecientes, a 3 se le restan dichos restos;
  - el resultado es el ciclo decreciente 3, 2, 1, 0; y de ahí el retorno antes descripto.
~~~ C++
unsigned long calcular_digito(int milliseg)
{
  int digito = 3 - ((milliseg / 1000) % 4);
  return digito;
}
~~~

### :file_folder: 3.3. __activar_todo__
Esta función:
- recibe valores enteros entre 0 y 4 (incluidos);
- al valor 4 lo recibe mediante la variable global DESACTIVAR;
- no tiene retorno;
- funcionamiento:
  - utiliza la función __switch__ para ejecutar los __case__ recibidos por parámetro;
  - en cada __case__:
    - enciende y apaga los LEDs alternadamente mediante __digitalWrite__;
    - enciende y apaga el display mostrando el dígito correspondiente a través de la función auxiliar __activar_display__;
    - enciende y apaga el zumbador a través de la función auxiliar __activar_buzzer__ (el apagado del zumbador se realiza por defecto dentro de esta última);
    - el __case__ 4 desactiva todos los LEDs y el display.

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
### :file_folder: 3.4. __loop__
Esta función:
- toma "internamente" el valor del pin INTERRUPTOR a través de la función __digitalRead__ y lo guarda en la variable local __entradaINTERRUPTOR__;
- a través de una estructura de control __if/else__ anidada:
  - si el Arduino arranca con el interruptor _encendido_, es decir, __entradaINTERRUPTOR__ igual a 0 y bandera global __flag_presionado__ en __false__, calcula el __tiempo_transcurrido__ como variable local haciendo la diferencia entre __millis__ y la variable global __inicio__, para luego asignar dicha variable como parámetro de la función __calcular_digito__ que a su vez pasa su retorno a la función __activar_todo__;
    - cuando el estado del interruptor cambia a _apagado_ por primera vez (__entradaINTERRUPTOR__ igual a 1), guarda los valores de __millis__ en la variable local __tiempo_apagado__ y el __flag_presionado__ pasa a __true__ no volviendo más a __false__ mientras el Arduino siga funcionando;
    - al volver el interruptor a _encendido_, es decir, __entradaINTERRUPTOR__ igual a 1 y bandera global __flag_presionado__ en __true__, declara la variable local __reinicio__ como diferencia entre __millis__ y el valor de __tiempo_apagado__, o sea, vuelve el conteo del tiempo transcurrdio a 0 (o casi 0);
  - si el Arduino arranca con el interruptor _apagado_, el proceso resulta el mismo que en los dos últimos ítems, o sea que inicia el calculo del tiempo transcurrido directamente con las variables __tiempo_apagado__ y __reinicio__.

~~~ C++
void loop()
{ 
  int entradaINTERRUPTOR = digitalRead(INTERRUPTOR);
  
  if(entradaINTERRUPTOR == 0){
    if(flag_presionado == false){
      tiempo_transcurrido = millis() - inicio;
      activar_todo(calcular_digito(tiempo_transcurrido));
    }
    else{
      reinicio = millis() - tiempo_apagado;
      activar_todo(calcular_digito(reinicio));
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

## :file_folder: 3.5. __activar_display__
Esta función:
- recibe valores enteros entre 0 y 4 (incluidos);
- al valor 4 lo recibe mediante la variable global DESACTIVAR;
- no tiene retorno;
- funcionamiento:
  - utiliza la función __switch__ para ejecutar los __case__ recibidos por parámetro;
  - en cada __case__, enciende y apaga los segmentos del display mostrando el dígito correspondiente a través de la función __digitalWrite__;
  - el __case__ 4 desactiva todos los segmentos del display.

~~~ C++
void activar_display(int digito)
{
  switch(digito)
  {
    case 0:
      digitalWrite(A,1);
      digitalWrite(B,1);
      digitalWrite(C,1);
      digitalWrite(D,1);
      digitalWrite(E,1);
      digitalWrite(F,1);
      break;
    case 1:
      digitalWrite(B,1);
      digitalWrite(C,1);
      break;
    case 2:
      digitalWrite(A,1);
      digitalWrite(B,1);
      digitalWrite(D,1);
      digitalWrite(E,1);
      digitalWrite(G,1);
      break;
    case 3:
      digitalWrite(A,1);
      digitalWrite(B,1);
      digitalWrite(C,1);
      digitalWrite(D,1);
      digitalWrite(G,1);
      break;
    case 4:
      digitalWrite(A,0);
      digitalWrite(B,0);
      digitalWrite(C,0);
      digitalWrite(D,0);
      digitalWrite(E,0);
      digitalWrite(F,0);
      digitalWrite(G,0);
 	  break;
  }
}
~~~

## :file_folder: 3.6. __activar_buzzer__
Esta función:
- recibe valores enteros entre 0 y 3 (incluidos);
- no tiene retorno;
- funcionamiento:
  - enciende el zumbador con __digitalWrite__ y setea la frecuencia con __switch__;
  - en cada __case__, a través de la función __tone__, aplica un valor de frecuencia distinto del anterior;
  - aplica un __delay__ de medio segundo y detiene la frecuencia con __noTone__;
  - aplica otro __delay__ de medio segundo y nuevamente con __digitalWrite__ apaga el zumbador.

~~~ C++
void activar_buzzer(int digito)
{
  digitalWrite(BUZZER,1);
  switch(digito)
  {
  	case 3:
      tone(BUZZER,600);
      break;
  	case 2:
      tone(BUZZER,400);
      break;
  	case 1:
      tone(BUZZER,250);
      break;
    case 0:
      tone(BUZZER,185);
      break;
  }
  delay(500);
  noTone(BUZZER);
  digitalWrite(BUZZER,0);
  delay(500);
}
~~~
___
## :robot: Link al proyecto
- [proyecto](https://www.tinkercad.com/things/hq86m6GMRpG)
___
## :tv: Link al video del proceso
- N/A
___
### Fuentes
- [Consejos para documentar](https://www.sohamkamani.com/how-to-write-good-documentation/#architecture-documentation).

- [Lenguaje Markdown](https://markdown.es/sintaxis-markdown/#linkauto).

- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

- [Tutorial GitHub](https://www.youtube.com/watch?v=QaM7zoaEmjo).

- [Emojis](https://gist.github.com/rxaviers/7360908).

___






