# Laboratorio  05 

## ALU - Grupo 11 

#### 1. Pedro Javier Puerto    61165
#### 2. Wolfran Esteban Suarez 78259
#### 3. Jorge Sanchez Abella   79344

# lab05 : Unidad de suma, resta, multiplicacion, division y visualizacion BCD

## Introducción

Se realiza la implementacion de las operaciones logicas en una  unidad aritmetica logica ALU en la FPGA para su visualizacion, como desarrollo se debe realizar  todo el procedimiento descrito y se deben seguir las siguientes instrucciones:


## Descripción
La unidad aritmÃƒÂ©tica, es tal que cuenta con componentes para realizar operaciones aritmÃƒÂ©ticas. cada operaciÃƒÂ³n aritmÃƒÂ©tica es ejecutada acuerdo al cÃƒÂ³digo de la operaciÃƒÂ³n. 

## Desarrollo del laboratorio. 
Como ejercicio acadademico, se propone construye una unidad con 4 operaciones aritmeticas: suma, resta, multiplicaciÃƒÂ³n y divisiÃƒÂ³n.  de igual manera, el resultados se visualiza en los display de siete segmentos. El flujo de datos y la selecciÃƒÂ³n de la operaciÃƒÂ³n se realiza acorde a la seÃƒÂ±al opcode, y segun la siguiente tabla:


opcode | operacion enteros positivos
00| suma
01| resta 
10| multiplicacion
11| division 

Por lo tanto, la unidad debe contar con:

1. Los dos puertos de entrada A y B. cada uno de  3 bits.
2. La seÃƒÂ±al `opcode` de dos bits, para configurar la operaciÃƒÂ³n que se realiza con los datos de `portA` y `portB`.
3. La a visualizaciÃƒÂ³n de unidad debe tener las salidas de los 4 ÃƒÂ¡nodos, `An`  y las 7 seÃƒÂ±ales de los cÃƒÂ¡todos, `sseg`.
4. Para las FSM  y las visualizaciÃƒÂ³n dinÃƒÂ¡mica, se debe incluir la seÃƒÂ±al de `clk` de entrada.
5. la seÃƒÂ±al de reset del sistema

## Diagrama de caja negra

SegÃƒÂºn las especificaciones anteriormente descrita, la caja funcional de la unidad aritmÃƒÂ©tica propuesta es:

![caja negra](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/cajanegra.png)


## Diagrama estructural

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/diagraEstructural.png)

El diagrama estructural, se soporta en los componentes desarrollados en los anteriores laboratorios. De esta manera,  tanto el sumador, el multiplicador  y el Display, son tomados de los lab2, lab5 y lab4  respectivamente. Adicional a la estructura de cada operaciÃƒÂ³n se encuentra el decodificador  y el multiplexador.


## Suma
 
Se realiza la  definicion de  una caja negra de entradas y salidas del bloque funcional.

![caja negra](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20SUMADOR.PNG)

luego se instancian los cuatro sumadores de 1 Bit para completar un sumador de 4 bits se instancia el diagrama  de bloques.

![caja negra sumador 4bits](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/SUMADOR%204%20BITS.PNG)

#Instanciamiento suma:

`timescale 1ns / 1ps

module sum4b(init, xi, yi,co,sal);

  input init;
  input [3 :0] xi;
  input [3 :0] yi;
  output co;
  output [3 :0] sal;
  
  
  wire [4:0] st;
  assign sal= st[3:0];
  assign Cout = st[4];

  assign st  = 	xi+yi;

endmodule

## Multiplicacion 
## Funcionamiento (tomado de la presentacion de clase).

El algoritmo de multiplicaciÃ³n que se implementa se basa en productos parciales (PP). Se realiza la multiplicaciÃ³n iniciando con el bit menos significativo del multiplicador, el resultado de la multiplicaciÃ³n se suma al primer producto parcial y se obtiene el segundo producto parcial; si el bit del multiplicador es 0 no se afecta el contenido de PP, por lo que no se realiza la suma. A continuaciÃ³n se realiza la multiplicaciÃ³n del siguiente bit (a la izquierda del LSB) y el resultado se suma al producto parcial pero corrido un bit a la izquierda. Este proceso continua hasta completar todos los bits del multiplicador y el Ãºltimo producto parcial es el resultado final.

La descripciÃ³n de la operaciÃ³n esta en las presentaciones de clase

La caja negra tiene como entradas multiplicando y el multiplicador (A y B), seÃ±ales de m bits cada una. la salida es el resultado de la multiplicaciÃ³n PP (Bus de m Bits). Ademas, la seÃ±al de reloj (CLOCK), entrada. Las seÃ±ales INIT y DONE, entrada y salida, se utilizan para iniciar el proceso de multiplicaciÃ³n e indicar que el resultado esta a disponible respectivamente

![caja negra Multiplicacion](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20MULTIPLICADOR.PNG)

##Instanciar

module multiplicador( input [2:0] MR, 
							 input [2:0] MD, 
							input init, 
							 input clk,  
							 output reg [5:0] pp, 
							 output reg done
    );

reg sh;
reg rst;
reg add;
reg [5:0] A;
reg [2:0] B;
wire z;

reg [2:0] status =0;

// bloque comparador 
assign z=(B==0)?1:0;


//bloques de registros de desplazamiento para A y B
always @(posedge clk) begin
   
	if (rst) begin
		A = {3'b000,MD};
		B = MR;
	end
	else	begin 
		if (sh) begin
			A= A << 1;
			B = B >> 1;
		end
	end

end 

//bloque de add pp
always @(posedge clk) begin
   
	if (rst) begin
		pp =0;
	end
	else	begin 
		if (add) begin
		pp =pp+A;
		end
	end

end

// FSM 
parameter START =0,  CHECK =1, ADD =2, SHIFT =3, END1 =4;

always @(posedge clk) begin
	case (status)
	START: begin
		sh=0;
		add=0;
		if (init) begin
			status=CHECK;
			done =0;
			rst=1;
		end
		end
	CHECK: begin 
		done=0;
		rst=0;
		sh=0;
		add=0;
		if (B[0]==1)
			status=ADD;
		else
			status=SHIFT;
		end
	ADD: begin
		done=0;
		rst=0;
		sh=0;
		add=1;
		status=SHIFT;
		end
	SHIFT: begin
		done=0;
		rst=0;
		sh=1;
		add=0;
		if (z==1)
			status=END1;
		else
			status=CHECK;
		end
	END1: begin
		done =1;
		rst =0;
		sh =0;
		add =0;
		status =START;
	end
	 default:
		status =START;
	endcase 
	
end 


endmodule



 
 
 
 
 
 
 
 
