# Laboratorio  05

## ALU - Grupo 11

#### 1. Pedro Javier Puerto    61165
#### 2. Wolfran Esteban Suarez 78259
#### 3. Jorge Sanchez Abella   79344

# lab05 : Unidad de suma, resta, multiplicación, división y visualización BCD

## Introducción

Se realiza la implementacion de las operaciones logicas en una  unidad aritmetica logica ALU en la FPGA para su visualizacion, como desarrollo se debe realizar  todo el procedimiento descrito y se deben seguir las siguientes instrucciones:


## Descripción 
La unidad aritmética, es tal que cuenta con componentes para realizar operaciones aritméticas. cada operación aritmética es ejecutada acuerdo al código de la operación. 

## Desarrollo del laboratorio. 
Como ejercicio académico, se propone construye una unidad con 4 operaciones aritméticas: suma, resta, multiplicación y división.  de igual manera, el resultados se visualiza en los display de siete segmentos. El flujo de datos y la selección de la operación se realiza acorde a la señal opcode, y segun la siguiente tabla:


opcode | operación  enteros positivos
00| suma
01| resta 
10| multiplicación
11| división 

Por lo tanto, la unidad debe contar con:

1. Los dos puertos de entrada A y B. cada uno de  3 bits.
2. La señal `opcode` de dos bits, para configurar la operación que se realiza con los datos de `portA` y `portB`.
3. La a visualización de unidad debe tener las salidas de los 4 ánodos, `An`  y las 7 señales de los cátodos, `sseg`.
4. Para las FSM  y las visualización dinámica, se debe incluir la señal de `clk` de entrada.
5. la señal de reset del sistema

## Diagrama de caja negra

Según las especificaciones anteriormente descrita, la caja funcional de la unidad aritmética propuesta es:

![caja negra](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/cajanegra.png)


## Diagrama estructural

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/diagraEstructural.png)

El diagrama estructural, se soporta en los componentes desarrollados en los anteriores laboratorios. De esta manera,  tanto el sumador, el multiplicador  y el Display, son tomados de los lab2, lab5 y lab4  respectivamente. Adicional a la estructura de cada operación se encuentra el decodificador  y el multiplexador.


## Suma
 
Se realiza la  definicion de  una caja negra de entradas y salidas del bloque funcional.

![caja negra](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20SUMADOR.PNG)

luego se instancian los cuatro sumadores de 1 Bit para completar un sumador de 4 bits se instancia el diagrama  de bloques.

![caja negra sumador 4bits](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/SUMADOR%204%20BITS.PNG)




 
 
 
 
 
 
 
 
