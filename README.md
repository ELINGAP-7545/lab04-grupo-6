# lab04-grupo 6
BCD2SSeg

EDWIN DAVID GIRALDO CODIGO: 39910

# Introducción

En esta oportunidad analizaremos el código top y testbench correspondiente a la visualización de números Hexadecimales  y binarios en un display de 7 segmentos de Ánodo común, usando como guía los laboratorios anteriores en Quartus.   

## Diseño BCD-7seg

Para poder avanzar primero debemos conocer un display de 7 segmentos de ánodo común, el cual es una forma de representar caracteres en equipos electrónicos. Está compuesto de siete segmentos que se pueden encender o apagar individualmente. Cada segmento tiene la forma de una pequeña línea. Se podría comparar a escribir números con cerillas o fósforos de madera. 
![Display 7 Segmentos](https://en.wikipedia.org/wiki/Seven-segment_display) 

En los de tipo de ánodo común, todos los ánodos de los ledes o segmentos están unidos internamente a una patilla común que debe ser conectada a potencial positivo (nivel “1”). El encendido de cada segmento individual se realiza aplicando potencial negativo (nivel “0”) por la patilla correspondiente a través de una resistencia que limite el paso de la corriente.


![gif display](https://upload.wikimedia.org/wikipedia/commons/2/2b/Seven_segment_display-animated.gif)

Imagen tomada de [User:Guam + Various](https://commons.wikimedia.org/wiki/File:Seven_segment_display-animated.gif)

Cada uno de los segmentos que forman la pantalla están marcados con siete primeras letras del alfabeto ('a'-'g'), y se montan de forma que permiten activar cada segmento por separado, consiguiendo formar cualquier dígito numérico. A continuación se muestran algunos ejemplos:

Si se activan o encienden todos los segmentos se forma el número "8".
Si se activan sólo los segmentos: "a, b, c, d, e, f," se forma el número "0".
Si se activan sólo los segmentos: "a, b, g, e, d," se forma el número "2".
Si se activan sólo los segmentos: "b, c, f, g," se forma el número "4".

**Diagrama Estructural 1 Display**: 

![bcd_black](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab03-BCD2SSeg/doc/BCD2SSeg.jpg)

Si observa la caja negra/ funcional  ademas  de la salidad de 7 segmentos contiene  una salida `An`. esta salida es para conectar eventualmente el ánodo del display y  poder hacer visualización dinámica, cuando se tiene mas de un display conectado.


**Definir la descripción Funcional**

Para poder generar nuestro codigo adecuadamente lo primero que debemos hacer es definir nuestra unidad funcional que puede hacer uso, bien sea, de las tablas de verdad o de la descripción algorítmica del BCD a  siete segmentos. Recuerde que cada Segmento es una salida  del diseño. Ejemplo, si desea  visualizar el número **1**, la salida seria  de `Sseg es 0110000`. observe la gráfica a continuación, para generar las salidas acorde al número de entrada.

![sseg](https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/7_segment_display_labeled.svg/1024px-7_segment_display_labeled.svg.png)


# Ejercicio - Visualización Dinámica 1 Display

## Diagrama Caja negra 

Como siempre, antes de realizar la descripción del hardware se debe diseñar la caja funcional del modulo, con las entradas y salidas

![diagrama caja negra ](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/display_7segx4.jpg)

En este sentido, se adiciona al HDL de siete segmentos 4 señales de control para el LCD, llamadas An. cada bit de la señal `An` debe ser modificado en el tiempo, con el fin de activar solo un display.  

# Verilog Visualización 1 Display

```verilog
module BCDtoSSeg (BCD, SSeg, an); //asignamos el modúlo al igual que las variables que utilizaremos 

  input [3:0] BCD; // entrada BCD de 4 bits 
  output reg [0:6] SSeg; // registro de salida de los 7 bits de cada segmento
  output [3:0] an; // salida de anodo para activacion de display

assign an=4'b1110;// asignamos el display que activaremos el cual en este caso es el 1 por medio de un "0"


always @ ( * ) begin // definimos el inicio del programa al igual que un continuo funcionamiento
  case (BCD) // casos que sucederan en la entrada BCD
   4'b0000: SSeg = 7'b0000001; // "0"  conversión para activacion de los segmentos del display por medio de 0 
	4'b0001: SSeg = 7'b1001111; // "1"  conversión para activacion de los segmentos del display por medio de 0
	4'b0010: SSeg = 7'b0010010; // "2" conversión para activacion de los segmentos del display por medio de 0
	4'b0011: SSeg = 7'b0000110; // "3" conversión para activacion de los segmentos del display por medio de 0
	4'b0100: SSeg = 7'b1001100; // "4" conversión para activacion de los segmentos del display por medio de 0
	4'b0101: SSeg = 7'b0100100; // "5" conversión para activacion de los segmentos del display por medio de 0
	4'b0110: SSeg = 7'b0100000; // "6" conversión para activacion de los segmentos del display por medio de 0
	4'b0111: SSeg = 7'b0001111; // "7" conversión para activacion de los segmentos del display por medio de 0
	4'b1000: SSeg = 7'b0000000; // "8"  conversión para activacion de los segmentos del display por medio de 0
	4'b1001: SSeg = 7'b0000100; // "9" conversión para activacion de los segmentos del display por medio de 0
   4'ha: SSeg = 7'b0001000;  // "a" conversión para activacion de los segmentos del display por medio de 0
   4'hb: SSeg = 7'b1100000; // "b" conversión para activacion de los segmentos del display por medio de 0
   4'hc: SSeg = 7'b0110001; // "c" conversión para activacion de los segmentos del display por medio de 0
   4'hd: SSeg = 7'b1000010; // "d" conversión para activacion de los segmentos del display por medio de 0
   4'he: SSeg = 7'b0110000; // "f" conversión para activacion de los segmentos del display por medio de 0
   4'hf: SSeg = 7'b0111000; // "g" conversión para activacion de los segmentos del display por medio de 0
    default:
    SSeg = 0;     // ponemos Sseg en 0
  endcase   // terminamos los casos
end 

endmodule // finalizamos el modulo
```


# TESTBENCH
```verilog

`timescale 1ns / 1ps // Escala de tiempo en la cual se visualizara la simulación 


module BCDtoSSeg_TB; // llamamos el modulo de Testbench

	// Inputs
	reg [3:0] BCD; // asignamos el registro de entrada BCD 

	// Outputs
	wire [6:0] SSeg; // asignamos la salida de los 7 segmentos

	// Instantiate the Unit Under Test (UUT)
	BCDtoSSeg uut (
		.BCD(BCD), 
		.SSeg(SSeg)
	); // instanciamos el modulo principal describiendo las variables que necesitamos y el puerto donde se ubican

	initial begin  // damos la condicion inicial y planteamos los bloques y casos que usaremos del BCD del modulo Top

		BCD = 0; #10; // número que se visualizara y espera de 10ns por medio del #10 
		BCD = 1; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 2; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 3; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 4; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 5; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 6; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 7; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 8; #10; // número que se visualizara y espera de 10ns por medio del #10
		BCD = 9; #10; // número que se visualizara y espera de 10ns por medio del #10

	end

   initial begin: TEST_CASE                                     
     $dumpfile("BCDtoSSeg_TB.vcd");        // indicación donde quedara guardado el archivo
     #(200) $finish;
   end

endmodule

```
# SIMULACIÓN

![gif display](https://github.com/ELINGAP-7545/lab04-grupo-6/blob/master/imagenes/SIMULACION%20ARQUI.jpg)

# Ejercicio - Visualización Dinámica 4 Display´s

A continuación procederemos a avanzar en nuestro proyecto y vamos a visualizar en 4 Display´s , es necesario generar una visualización tal que sea necesario el menor número de pines para conectar todos los display en con la FPGA.

Por ahora, se ha visualizado el números en un solo display de 7 segmentos. Pero en la mayoría de los casos, los 7 pines de los cátodos están inter-conectados entre cada display, como se observa en la figura: 

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/conex.png)

# Diagrama Caja negra
Como siempre, antes de realizar la descripción del hardware se debe diseñar la caja funcional del modulo, con las entradas y salidas

![cajan_negr](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/display_7segx4.jpg)

En este sentido, se adiciona al HDL de siete segmentos 4 señales de control para el LCD, llamadas An. cada bit de la señal An debe ser modificado en el tiempo, con el fin de activar solo un display.

## Diagrama Estructural 4 Display´s

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/doc/display_7segx4_Diag_Estructural.jpg)

Se evidencia que se deben construir cuatro módulos  básicos, de los cuales uno de ellos esta descrito en el ejercicio anterior, [BCDtoSSeg.v](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab04_display_7segx4/src_ise_basys2/display_7segx4/BCDtoSSeg.v) . Los otros tres bloques son:

* Divisor de frecuencia: Su función es dividir la frecuencia de  `clk` de entrada, en el tiempo requerido para cada camvio de ánodo
* Selector de Ánodo: Sincronizado con la frecuencia  que genera el divisor, cambia en cada instante de tiempo el  ánodo, se puede ver como un registro de desplazamiento del bit 0 `1110 1101 1011 0111`
* Selector de Datos: dependiendo del ánodo activado, activa los datos correspondientes.

# Verilog Visualización 4 Display´s

```verilog
`timescale 1ns / 1ps // Escala de tiempo en la cual se visualizara la simulación
module display(  // nuevo modulo de display 
    input [15:0] num, // nueva variable de entrada de los números hexadecimal
    input clk,        // entrada de reloj
    output [0:6] sseg,//salida de los 7 segmentos correspondiente al display
    output reg [3:0] an, // registro de salida de ánodo para la activación del display debe ser un registro ya que ahora se activaran 4
	 input rst,  // entrada de reset
	 output led // salida de led
    );



reg [3:0]bcd=0;               // registo de 4 bits para el número a visualizar
//wire [15:0] num=16'h4321;   // conector de hexadecimal y asignación del número a visualizar con num=
 
BCDtoSSeg bcdtosseg(.BCD(bcd), .SSeg(sseg)); // instanciamiento del codigo de BCD hecho anteriormente 

reg [26:0] cfreq=0;  // variable de registro para el divisor de frecuencia para sincronizar con la activación de cada display
wire enable;         // conector enable

// Divisor de frecuecia

assign enable = cfreq[16];  // asiganmos la activacion del enable con un divisor de frecuencia de 16 para el cambio entre display´s
assign led =enable;         // activamos la salida de led
always @(posedge clk) begin // definimos el inicio del programa y su continuo funcionamiento siempre que el reloj  tenga un posedge
  if(rst==1) begin          // si reset es = 1 
		cfreq <= 0; // bloqueamos la frecuencia en 0
	end else begin     // si no iniciamos
		cfreq <=cfreq+1; // y aumentamos la frecuencia en 1
	end
end
// desplazamiento de anodo para activación de lso  display´s
reg [1:0] count =0;           // registro de conteo 
always @(posedge enable) begin // siempre que el posedge se encuentre habilitado iniciamos 
		if(rst==1) begin // si reset es = a 1 iniciamos
			count<= 0; // bloqueamos el contador en 0
			an<=4'b1111; // y apagamos todos los display´s con 1
		end else begin // si no
			count<= count+1; // aumentamos el contador en 1
			an<=4'b1101; // y activamos el display
			case (count) // especificamos los casos en el conteo
				2'h0: begin bcd <= num[3:0];   an<=4'b1110; end // encendemos el primer display con los bits de [3:0] 
				2'h1: begin bcd <= num[7:4];   an<=4'b1101; end // encendemos el segundo display con los bits de [7:4]
				2'h2: begin bcd <= num[11:8];  an<=4'b1011; end // encendemos el tercer display con los bits de [11:8]
				2'h3: begin bcd <= num[15:12]; an<=4'b0111; end // encendemos el cuarto display con los bits de [15:12]
			endcase
		end
end

endmodule
```
# TESTBENCH VISUALIZACIÓN DINAMICA 4 DISPLAY´S

```verilog
module testbench; // nombre del modulo testbench

	// Inputs
	reg [15:0] num; // declaramos registro de entrada de número hexadecimal
	reg clk2;       // declaramos registro de reloj 2 para sincronización de los anodos con la visualización
	reg rst;        // declaramos registro de reset

	// Outputs
	wire [0:6] sseg;  //declaramos el conector de salida de los ánodos de los 7 segmentos
	wire [3:0] an;    //declaramos el conector de salida del ánodo para la activación de los 4 display´s 

	// Instantiate the Unit Under Test (UUT)
	display uut (
	//	.num(num), //instanciamos el modulo display y declaramos la variable num y el puerto que traeremos
		.clk(clk2), // declaramos la variable de reloj y el puerto que traeremos
		.sseg(sseg), //declaramos la variable los siete segmentos y el puerto que traeremos
		.an(an), // declaramos la variable del ánodo para los display y el puerto que traeremos
		.rst(rst) //declaramos la variable de reset y el puerto que traeremos
	);

	initial begin
		// Initialize Inputs // declaramos las entradas de inicio 
		clk2= 0;             // reloj en "0"
		rst = 1;             // reset en "1"
		#10 rst =0;          // esperamos 10ns y ponemos reset en "0"
		
		num = 16'h4321; #10;  // cargamos el número que queremos visualizar
        

	end
      

	always #1 clk2 = ~clk2;  // nos aseguramos que el reloj solo se active en pulsos de subida posedge
	
endmodule

```


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
