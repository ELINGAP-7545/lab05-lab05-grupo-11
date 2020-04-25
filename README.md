# Laboratorio  05

## ALU - Grupo 11

#### 1. Pedro Javier Puerto    61165
#### 2. Wolfran Esteban Suarez 78259
#### 3. Jorge Sanchez Abella   79344

## lab05 : Unidad de suma, resta, multiplicación, división y visualización BCD

## Introduccion

Se realiza la implementacion de ALU "unidad logico aritmética" con sus respectivas operaciones basicas, como son:
#### a) suma.
#### B) resta.
#### c) multiplicación.
#### d) división.
#### con visualización en display 7 segmentos mediante uso de FPGA intel en modo remoto, utilizando la página labs land.


## Descripción
El proceso de implementación de la unidad lógica aritmética está condicionada por un opcode, el cuál cumple la función de ser un menú de selección que permite elegir las operaciones a realizar, dadas de la siguiente manera.
#### a) -00- Para Suma.
#### b) -01- Para Resta.
#### c) -10- Para Multiplicación.
#### d) -11- Para División
#### Posteriormente cada operación realizada tendra verificación visual en 2 displays de 7 segmentos a través de una FPGA remota y a su vez constancia en el diagrama de estados lógicos obtenidos en la simulación del software de implemenatción.  

## Desarrollo . 
De acuerdo con las prácticas propuestas a lo largo del semestre, se realiza instanciamiento de los desarrollos obtenidos.
como primer paso se vincula a la unidad el sumador, tanto de 1 como de 4 bits, en segunda instancia se adjunta diseño del operador resta, en base a sumas con complemento A2, en tercer lugar se vincula el código perteneciente al miltiplicador y finalmente diseño anterior del divisor, obteniendo así las operacines básicas de una ALU, con programación de su posterior visualización mediante verilog y ayuda remota de una FPGA.

## Diagrama de caja negra

Según las especificaciones anteriormente descritas, la caja funcional de la unidad aritmética propuesta es:

![caja negra](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/cajanegra.png)


## Diagrama estructural

![estructural](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab06_Unidad_aritmetica/doc/diagraEstructural.png)

El diagrama estructural, soporta en los componentes desarrollados en los anteriores laboratorios. De esta manera,  tanto el sumador, el multiplicador  y el Display, son tomados de los lab2, lab5 y lab4  respectivamente. Adicional a la estructura de cada operación se encuentra el decodificador  y el multiplexador.


## Suma
 
Caja negra de entradas y salidas del bloque funcional.

![caja negra](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20SUMADOR.PNG)

nstanciamiento de sumador de un bit a 4bits

![caja negra sumador 4bits](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/SUMADOR%204%20BITS.PNG)

## Suma de 1 bit
#### module sum1b(init, xi, yi,co,sal);

    input init;
    input [3 :0] xi;
    input [3 :0] yi;
    output co;
    output [3 :0] sal;
  
    wire [4:0] st;
    assign sal= st[3:0];
    assign Cout = st[4];

    assign st = xi+yi;

#### endmodule


## Instanciamiento sumador de 4 bits
module sumador4bits(
	
	input [3:0] xi,
        input [3:0] yi,
        output [3:0] zi,
        output co
	 );
	
	wire c1, c2, c3;
	
	Sumador1bit s0 (.A(xi[0]), .B(yi[0]), .Ci(0), .Cout(c1), .S(zi[0]));
	Sumador1bit s1 (.A(xi[1]), .B(yi[1]), .Ci(c1), .Cout(c2), .S(zi[1]));
	Sumador1bit s2 (.A(xi[2]), .B(yi[2]), .Ci(c2), .Cout(c3), .S(zi[2]));
	Sumador1bit s3 (.A(xi[3]), .B(yi[3]), .Ci(c3), .Cout(co), .S(zi[3]));
 

endmodule
## Multiplicación 
## Funcionamiento.

El algoritmo de multiplicación que se implementa se basa en productos parciales (PP). Se realiza la multiplicación iniciando con el bit menos significativo del multiplicador, el resultado de la multiplicación se suma al primer producto parcial y se obtiene el segundo producto parcial; si el bit del multiplicador es 0 no se afecta el contenido de PP, por lo que no se realiza la suma. A continuación se realiza la multiplicación del siguiente bit (a la izquierda del LSB) y el resultado se suma al producto parcial pero corrido un bit a la izquierda. Este proceso continua hasta completar todos los bits del multiplicador y el último producto parcial es el resultado final.

La descripción de la operación esta en las presentaciones de clase

La caja negra tiene como entradas multiplicando y el multiplicador (A y B), señales de m bits cada una. la salida es el resultado de la multiplicación PP (Bus de m Bits). Ademas, la señal de reloj (CLOCK), entrada. Las señales INIT y DONE, entrada y salida, se utilizan para iniciar el proceso de multiplicación e indicar que el resultado esta a disponible respectivamente

![caja negra Multiplicacion](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/CAJA%20NEGRA%20MULTIPLICADOR.PNG)

## Diagrama funcional

![Diagrama Funcional](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/DESCRIPCION%20FUNCIONAL%20MULTIPLICADOR.PNG)

## Unidad de Control.

![Datapath](https://github.com/ELINGAP-7545/lab05-lab05-grupo-11/blob/master/unidad%20de%20control%20datapath%201.PNG)


## Codigo multiplicador

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

#### bloque comparador 
    assign z=(B==0)?1:0;

#### bloques de registros de desplazamiento para A y B
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

#### bloque de add pp
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

#### FSM 
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

## Division

####  Entradas y Salidas Caja negra Bloque Divisor
    module divisor( A, B, init, clk, z, done);
	
		input [15:0] A;  
		input [15:0] B;
		input init; 
		input clk; 
		output reg[15:0] z; 
		output reg done;
		
#### Registros Utilizados
		reg [15:0] Q0;
		reg [15:0] Q1;
		reg [15:0] Q2;
		reg [15:0] A2;
		reg [15:0] n = 0;
 
#### Bloque de desplazamiento, adición y sustracción para la divición.
		always @(posedge clk) begin
			 
		//complemento a 2
		A2 = ~A +1;
	        Q0 = B;
	end
		always @(posedge clk) begin
		Q1 = A2+Q0;
	end
		always @(posedge clk) begin
		if (Q1 < 0)
		z = n;
	 else
		n = n+1;
	end
		always @(posedge clk) begin
		if (Q1 == 0)
		z = n;
	else
	        Q2 = A2+Q1;
	
	end
    endmodule


## Resta

#### Entradas y salidas caja negra Restador:
	module res3b(
		input [3:0] A,
		input [3:0] B,
		input init,
		output [3:0] S
	);
		
#### Registros utilizados:
		reg [4:0] resta; 
		reg [4:0] salida;
		reg [4:0] CA2;

#### Configuración de Init:
		assign S = init?salida:0;

#### Bloque de Complemento A2 y Sustracción:
	always @ ( * ) begin// siempre que haya algun cambio dentro de ( * ) se ejecuta
		
		CA2 = ~B+1;		            // Complemento A2 entrada
		resta = A + CA2;
		if (resta[4] == 1) begin            // Cuando el MSB es más es 1, el resultado es negativo
			salida = ~resta+1;          // Complemento A2 Salida
			salida = salida + 10000 ;   // Bit de signo
		end 
		else begin
			salida = resta;
	   end
		
	end
 
 
 ## Visualización en Displays 7 Segmentos
 
 #### Entradas, Salidas y registros Caja negra de visualización:
	module BCDtoSSeg (v_sw, g_hex);

		input  wire [3:0] v_sw;
		output wire [0:6] g_hex;
		 
		reg [0:6] SSeg;
		assign g_hex = SSeg;
 
 #### Codificación BCD a 7 Segmentos:
 	always @ ( * ) begin
	  case (v_sw)
		4'b0000: SSeg = 7'b0000001; // "0"  
		4'b0001: SSeg = 7'b1001111; // "1" 
		4'b0010: SSeg = 7'b0010010; // "2" 
		4'b0011: SSeg = 7'b0000110; // "3" 
		4'b0100: SSeg = 7'b1001100; // "4" 
		4'b0101: SSeg = 7'b0100100; // "5" 
		4'b0110: SSeg = 7'b0100000; // "6" 
		4'b0111: SSeg = 7'b0001111; // "7" 
		4'b1000: SSeg = 7'b0000000; // "8"  
		4'b1001: SSeg = 7'b0000100; // "9" 
		4'ha: SSeg = 7'b0001000;  
		4'hb: SSeg = 7'b1100000;
		4'hc: SSeg = 7'b0110001;
		4'hd: SSeg = 7'b1000010;
		4'he: SSeg = 7'b0110000;
		4'hf: SSeg = 7'b0111000;
		 default:
		 SSeg = 0;
	  endcase
	end


### Modulo de Implementación para uso de 2 Displays 7Seg

#### Entradas y salidas :
	module dos_displays(

		 input [7:0] V_SW,
		 output [0:6] G_HEX0,
		 output [0:6] G_HEX1
		 );
 
#### Separación de la salida en Unidades y Decenas:
		wire [7:0] unidades;
		wire [7:0] decenas;

		assign decenas  =  V_SW / 10;
		assign unidades =  V_SW - (decenas * 10);

#### Instanciación de los displays:
		BCDtoSSeg display1( .v_sw(unidades), .g_hex(G_HEX0));
		BCDtoSSeg display2( .v_sw(decenas), .g_hex(G_HEX1));

	endmodule


## Codigo principal ALU 
 
####  Entradas y salidas Bloque Alu:
 	module alu(
		input    [9:0] V_SW,   
		output   [0:6] G_HEX0,
		output   [0:6] G_HEX1,
		input G_CLK_50
	);
   Para la entrada "V_SW" se divide de la siquiente manera

		assign portA  = V_SW [3:0]; // Entrada A
		assign portB  = V_SW [7:4]; // Entrada B
		assign opcode = V_SW [9:8]; // Bits de selección de operación
		
#### Declaración de salidas de cada Bloque de Operaciónes:		
		// Declaración de salidas de cada bloque 
		wire [3:0] sal_suma;
		wire [3:0] sal_resta;
		wire [3:0] sal_div;
		wire [5:0] sal_mult;
		wire [3:0] portA;
		wire [3:0] portB;
		wire [1:0] opcode;
		
#### Declaración de las entradas init de cada bloque de Operaciones:
		// Declaración de las entradas init 
		reg  [3:0] init; 
		wire init_suma;
		wire init_resta;
		wire init_mult;
		wire init_div;
		
		reg [15:0]int_bcd;  // Salida total del sistema

#### Declaración del clock y registro selector de operación:
		// Declaraciónes adicionales
		wire clk;
		wire [3:0] operacion;

#### Asignaciónes adicionales:
		assign clk = G_CLK_50;

		assign portB  = V_SW [3:0]; // Entrada B
		assign portA  = V_SW [7:4]; // Entrada A
		assign opcode = V_SW [9:8]; // Bits de selección de operación

#### Bloque de Codificación de las operacioes que realizará la ALU:
	always @(*) begin           // Codificación de operaciones
		case(opcode) 
			2'b00: init<=1;
			2'b01: init<=2;
			2'b10: init<=4;
			2'b11: init<=8;
		default:
			init <= 0;
		endcase
	end
	
#### Descripción del Multiplexor y configuración del selector:
	always @(*) begin
		case(opcode) 
			2'b00: int_bcd <={8'b00,sal_suma};
			2'b01: int_bcd <={8'b00,sal_resta};
			2'b10: int_bcd <={8'b00,sal_mult};
			2'b11: int_bcd <={8'b00,sal_div};
			default:
			int_bcd <= 0;
		endcase
	end
	
#### instanciación de todos los componnetes:

	sumador4bits sumador( .xi({1'b0,portA}), .yi({1'b0,portB}),.init(init_suma),.sal(sal_suma));              // Sumador
	mult3b multiplicador( .MR(portA), .MD(portB), .init(init_mult),.clk(clk), .pp(sal_mult));		  // Multiplicador
	res3b  restador     ( .A(portA), .B(portB), .init(init_resta),.S(sal_resta));				  // Restador
	div3b  divisor      ( .A(portA), .B(portB), .init(init_div), .clk(clk), .reset(rst), .Result(sal_div));   // Divisor
	dos_displays visualizacion ( .V_SW (int_bcd[7:0]), .G_HEX0 (G_HEX0), .G_HEX1(G_HEX1));			  // Visualización

