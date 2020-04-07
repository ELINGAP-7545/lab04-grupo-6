# lab04-grupo 6
BCD2SSeg

EDWIN DAVID GIRALDO CODIGO: 39910

# Introducción

En esta oportunidad analizaremos el código top y testbench correspondiente a la visualización de números Hexadecimales  y binarios en un display de 7 segmentos de Ánodo común, usando como guía los laboratorios anteriores en Quartus.   

## Diseño BCD-7seg

Para poder avanzar primero debemos conocer un display de 7 segmentos de ánodo común, el cual es una forma de representar caracteres en equipos electrónicos. Está compuesto de siete segmentos que se pueden encender o apagar individualmente. Cada segmento tiene la forma de una pequeña línea. Se podría comparar a escribir números con cerillas o fósforos de madera Display 7 Segmentos](https://en.wikipedia.org/wiki/Seven-segment_display) 

En los de tipo de ánodo común, todos los ánodos de los ledes o segmentos están unidos internamente a una patilla común que debe ser conectada a potencial positivo (nivel “1”). El encendido de cada segmento individual se realiza aplicando potencial negativo (nivel “0”) por la patilla correspondiente a través de una resistencia que limite el paso de la corriente.


![gif display](https://upload.wikimedia.org/wikipedia/commons/2/2b/Seven_segment_display-animated.gif)

Imagen tomada de [User:Guam + Various](https://commons.wikimedia.org/wiki/File:Seven_segment_display-animated.gif)

Cada uno de los segmentos que forman la pantalla están marcados con siete primeras letras del alfabeto ('a'-'g'), y se montan de forma que permiten activar cada segmento por separado, consiguiendo formar cualquier dígito numérico. A continuación se muestran algunos ejemplos:

Si se activan o encienden todos los segmentos se forma el número "8".
Si se activan sólo los segmentos: "a, b, c, d, e, f," se forma el número "0".
Si se activan sólo los segmentos: "a, b, g, e, d," se forma el número "2".
Si se activan sólo los segmentos: "b, c, f, g," se forma el número "4".

**Definir la caja funcional del BCD**: 

![bcd_black](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab03-BCD2SSeg/doc/BCD2SSeg.jpg)

Si observa la caja negra/ funcional  ademas  de la salidad de 7 segmentos contiene  una salida `An`. esta salida es para conectar eventualmente el ánodo del display y  poder hacer visualización dinámica, cuando se tiene mas de un display conectado.


**Definir la descripción Funcional**

Para poder generar nuestro codigo adecuadamente lo primero que debemos hacer es definir nuestra unidad funcional que puede hacer uso, bien sea, de las tablas de verdad o de la descripción algorítmica del BCD a  siete segmentos. Recuerde que cada Segmento es una salida  del diseño. Ejemplo, si desea  visualizar el número **1**, la salida seria  de `Sseg es 0110000`. observe la gráfica a continuación, para generar las salidas acorde al número de entrada.

![sseg](https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/7_segment_display_labeled.svg/1024px-7_segment_display_labeled.svg.png)


# Ejercicio - Visualización Dinámica 4 Display

## Diagrama Caja negra 

Como siempre, antes de realizar la descripción del hardware se debe diseñar la caja funcional del modulo, con las entradas y salidas

![diagrama caja negra ](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/display_7segx4.jpg)

En este sentido, se adiciona al HDL de siete segmentos 4 señales de control para el LCD, llamadas An. cada bit de la señal `An` debe ser modificado en el tiempo, con el fin de activar solo un display.  

## Diagrama Estructural 

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/display_7segx4_Diag_Estructural.jpg)

Se evidencia que se deben construir cuatro módulos  básicos, de los cuales uno de ellos esta descrito en el ejercicio anterior, [BCDtoSSeg.v](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/src_ise_basys2/display_7segx4/BCDtoSSeg.v) . Los otros tres bloques son:

* Divisor de frecuencia: Su función es dividir la frecuencia de  `clk` de entrada, en el tiempo requerido para cada camvio de ánodo
* Selector de Ánodo: Sincronizado con la frecuencia  que genera el divisor, cambia en cada instante de tiempo el  ánodo, se puede ver como un registro de desplazamiento del bit 0 `1110 1101 1011 0111`
* Selector de Datos: dependiendo del ánodo activado, activa los datos correspondientes.

# Entregables

Una vez clone el repositorio y lea la anterior guia, realice lo siguiente:

En el paquete de trabajo [WP04](https://classroom.github.com/g/zCBwHHKX)   esta la descripción del hardware que se implementa para visualizar un número hexadecimal de 32 bits en un display  y en 4 display de 7 segmentos.

* Comprenda cada línea del código HDL de los  archivos que se encuentra en la carpera src. Si cree necesario realice los respectivos comentarios en el mismo archivo y comente
* Realice en quartus la simulación para el BCD-7seg, analice los resultados.
* Cree el nuevo proyecto HDL para Visualización Dinámica 4 Display, tomando como base los archivos dados.
* Creer el archivo testbench.v
* Genera la simulación, Revise que el sistema funciona como usted lo esperaba. Realice lo comentarios necesarios en el archivo README.md.
* Modificar o Añadir los bloques necesarios para que la visualización sea en representación Decimal y no Hexadecimal.
* Realice la respectiva publicación del repositorio antes de la fecha dada con todo el código  fuente 

# SIMULACIÓN 7 SEG HEXADECIMAL

![7SEGEMENTOSHEXA](https://github.com/ELINGAP-7545/lab04-grupo-6/blob/master/imagenes/7%20segmentos%20hexa.PNG)
