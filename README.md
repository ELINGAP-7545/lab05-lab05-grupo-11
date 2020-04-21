# Laboratorio  05

## ALU - Grupo 11

#### 1. Pedro Javier Puerto    61165
#### 2. Wolfran Esteban Suarez 78259
#### 3. Jorge Sanchez Abella   79344

# lab05 : Unidad de suma, resta, multiplicaci贸n, divisi贸n y visualizaci贸n BCD

## Introducci贸n

Se realiza la implementacion de las operaciones logicas en una  unidad aritmetica logica ALU en la FPGA para su visualizacion, como desarrollo se debe realizar  todo el procedimiento descrito y se deben seguir las siguientes instrucciones:


## Descripci贸n 
La unidad aritm茅tica, es tal que cuenta con componentes para realizar operaciones aritm茅ticas. cada operaci贸n aritm茅tica es ejecutada acuerdo al c贸digo de la operaci贸n. 

## Desarrollo del laboratorio. 
Como ejercicio acad茅mico, se propone construye una unidad con 4 operaciones aritm茅ticas: suma, resta, multiplicaci贸n y divisi贸n.  de igual manera, el resultados se visualiza en los display de siete segmentos. El flujo de datos y la selecci贸n de la operaci贸n se realiza acorde a la se帽al opcode, y segun la siguiente tabla:


opcode | operaci贸n  enteros positivos
00| suma
01| resta 
10| multiplicaci贸n
11| divisi贸n 

Por lo tanto, la unidad debe contar con:

1. Los dos puertos de entrada A y B. cada uno de  3 bits.
2. La se帽al `opcode` de dos bits, para configurar la operaci贸n que se realiza con los datos de `portA` y `portB`.
3. La a visualizaci贸n de unidad debe tener las salidas de los 4 谩nodos, `An`  y las 7 se帽ales de los c谩todos, `sseg`.
4. Para las FSM  y las visualizaci贸n din谩mica, se debe incluir la se帽al de `clk` de entrada.
5. la se帽al de reset del sistema

## Diagrama de caja negra

Seg煤n las especificaciones anteriormente descrita, la caja funcional de la unidad aritm茅tica propuesta es:

![caja negra](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/cajanegra.png)


## Diagrama estructural

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/diagraEstructural.png)

El diagrama estructural, se soporta en los componentes desarrollados en los anteriores laboratorios. De esta manera,  tanto el sumador, el multiplicador  y el Display, son tomados de los lab2, lab5 y lab4  respectivamente. Adicional a la estructura de cada operaci贸n se encuentra el decodificador  y el multiplexador.


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

El algoritmo de multiplicaci髇 que se implementa se basa en productos parciales (PP). Se realiza la multiplicaci髇 iniciando con el bit menos significativo del multiplicador, el resultado de la multiplicaci髇 se suma al primer producto parcial y se obtiene el segundo producto parcial; si el bit del multiplicador es 0 no se afecta el contenido de PP, por lo que no se realiza la suma. A continuaci髇 se realiza la multiplicaci髇 del siguiente bit (a la izquierda del LSB) y el resultado se suma al producto parcial pero corrido un bit a la izquierda. Este proceso continua hasta completar todos los bits del multiplicador y el 鷏timo producto parcial es el resultado final.

La descripci髇 de la operaci髇 esta en las presentaciones de clase

La caja negra tiene como entradas multiplicando y el multiplicador (A y B), se馻les de m bits cada una. la salida es el resultado de la multiplicaci髇 PP (Bus de m Bits). Ademas, la se馻l de reloj (CLOCK), entrada. Las se馻les INIT y DONE, entrada y salida, se utilizan para iniciar el proceso de multiplicaci髇 e indicar que el resultado esta a disponible respectivamente

![caja negra Multiplicacion](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20MULTIPLICADOR.PNG)



 
 
 
 
 
 
 
 
