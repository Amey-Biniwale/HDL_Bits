# HDL_Bits

## Verilog Language

### Basics

1] Simple Wire
Create a module with one input and one output that behaves like a wire.
```
module top_module( input in, output out );
    
    assign out=in;
    
endmodule
```

2] Four Wires
Create a module with 3 inputs and 4 outputs that behaves like wires that makes these connections:

a -> w
b -> x
b -> y
c -> z
```
module top_module( 
    input a,b,c,
    output w,x,y,z );
    
    assign w = a;
    assign x = b;
    assign y = b;
    assign z = c;

endmodule
```

3] Inverter
Create a module that implements a NOT gate.
```
module top_module( input in, output out );
    
    assign out = ~in;

endmodule
```

4] AND Gate
Create a module that implements an AND gate.
```
module top_module( 
    input a, 
    input b, 
    output out );
    assign out = a & b;

endmodule
```

5] NOR Gate
Create a module that implements a NOR gate. A NOR gate is an OR gate with its output inverted. A NOR function needs two operators when written in Verilog.
```
module top_module( 
    input a, 
    input b, 
    output out );
    
    assign out = ~(a | b);

endmodule
```

6] XNOR Gate
Create a module that implements an XNOR gate.
```
module top_module( 
    input a, 
    input b, 
    output out );
    
    assign out = ~(a ^ b);

endmodule
```

7] Declaring Wires

<img width="1304" height="624" alt="image" src="https://github.com/user-attachments/assets/092e3c7b-e6a8-4335-bb00-d3d778c2b447" />

```
`default_nettype none
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out,
    output out_n   ); 
    
    wire w1,w2,w3;
    
    assign w1 = a & b;
    assign w2 = c & d;
    assign w3 = w1 | w2;
    assign out = w3;
    assign out_n = ~w3;

endmodule
```

8] 7458 Chip

<img width="1307" height="749" alt="image" src="https://github.com/user-attachments/assets/b56c8ec9-f24e-485f-a074-cac5f8f3add6" />

```
module top_module ( 
    input p1a, p1b, p1c, p1d, p1e, p1f,
    output p1y,
    input p2a, p2b, p2c, p2d,
    output p2y );
    wire w1,w2,w3,w4;
    
    assign w1 = p2a & p2b;
    assign w2 = p2c & p2d;
    assign w3 = p1a & p1b & p1c;
    assign w4 = p1d & p1e & p1f;
    
    assign p1y = w3 | w4;
    assign p2y = w1 | w2;
    


endmodule
```

### Vectors

1] Vectors

Build a circuit that has one 3-bit input, then outputs the same vector, and also splits it into three separate 1-bit outputs. Connect output o0 to the input vector's position 0, o1 to position 1, etc.

```
module top_module ( 
    input wire [2:0] vec,
    output wire [2:0] outv,
    output wire o2,
    output wire o1,
    output wire o0  ); // Module body starts after module declaration
    
    assign outv = vec;
    assign o2 = vec[2];
    assign o1 = vec[1];
    assign o0 = vec[0];

endmodule
```

2] Vectors in more detail
Build a combinational circuit that splits an input half-word (16 bits, [15:0] ) into lower [7:0] and upper [15:8] bytes.

```
`default_nettype none     // Disable implicit nets. Reduces some types of bugs.
module top_module( 
    input wire [15:0] in,
    output wire [7:0] out_hi,
    output wire [7:0] out_lo );
    
    assign out_hi = in[15:8];
    assign out_lo = in[7:0];

endmodule
```

3] Vector Part Select
A 32-bit vector can be viewed as containing 4 bytes (bits [31:24], [23:16], etc.). Build a circuit that will reverse the byte ordering of the 4-byte word.


AaaaaaaaBbbbbbbbCcccccccDddddddd => DdddddddCcccccccBbbbbbbbAaaaaaaa

```
module top_module( 
    input [31:0] in,
    output [31:0] out );//

    assign out[7:0] = in[31:24];
    assign out[15:8] = in[23:16];
    assign out[23:16] = in[15:8];
    assign out[31:24] = in[7:0];

endmodule
```

4] Bitwise Operators

<img width="1300" height="670" alt="image" src="https://github.com/user-attachments/assets/7293f3ed-6dd8-49ac-8d81-c1cc3cb6197c" />

```
module top_module( 
    input [2:0] a,
    input [2:0] b,
    output [2:0] out_or_bitwise,
    output out_or_logical,
    output [5:0] out_not
);
    assign out_or_bitwise = a | b;
    assign out_or_logical = a || b;
    assign out_not = ~{b , a};

endmodule
```

5] Four Input Gates
Build a combinational circuit with four inputs, in[3:0].

There are 3 outputs:

out_and: output of a 4-input AND gate.

out_or: output of a 4-input OR gate.

out_xor: output of a 4-input XOR gate.

```
module top_module( 
    input [3:0] in,
    output out_and,
    output out_or,
    output out_xor
);
    
    assign out_and = &in;
    assign out_or = |in;
    assign out_xor = ^in;

endmodule
```

6] Vector Concatenation Operator

<img width="1316" height="253" alt="image" src="https://github.com/user-attachments/assets/deded084-266b-42b0-a473-72a6baaa40ac" />

```
module top_module (
    input [4:0] a, b, c, d, e, f,
    output [7:0] w, x, y, z );//

    assign {w, x, y, z} = {a, b, c, d, e, f, 2'b11};

endmodule

```

7] Vector Reversal 1
Given an 8-bit input vector [7:0], reverse its bit ordering.

```
module top_module( 
    input [7:0] in,
    output [7:0] out
);
    assign out = {in[0],in[1],in[2],in[3],in[4],in[5],in[6],in[7]};

endmodule
```

8] Replication Operator
Build a circuit that sign-extends an 8-bit number to 32 bits. This requires a concatenation of 24 copies of the sign bit (i.e., replicate bit[7] 24 times) followed by the 8-bit number itself.

```
module top_module (
    input [7:0] in,
    output [31:0] out );//

    assign out = {{24{in[7]}},in};

endmodule
```

9] More Replication

<img width="1310" height="543" alt="image" src="https://github.com/user-attachments/assets/dff748e3-40d7-4e9d-b672-8dd54805a096" />

```
module top_module (
    input a, b, c, d, e,
    output [24:0] out );//

    assign out = ~{{5{a}},{5{b}},{5{c}},{5{d}},{5{e}}} ^ {5{a,b,c,d,e}};

endmodule
```

### Modules : Hierarchy

1] Modules

<img width="1058" height="772" alt="image" src="https://github.com/user-attachments/assets/2b242c8a-41f0-488a-83cc-c6f7ed41bcd0" />

```
module top_module ( input a, input b, output out );
    
    mod_a a1(.in1(a),.in2(b),.out(out));

endmodule

```

2] Connecting ports by position

<img width="1299" height="585" alt="image" src="https://github.com/user-attachments/assets/75ab5078-b955-46a0-97b2-54db76de62dc" />

```
module top_module ( 
    input a, 
    input b, 
    input c,
    input d,
    output out1,
    output out2
);
    
    mod_a(out1,out2,a,b,c,d);

endmodule
```

3] Connecting ports by name

<img width="1314" height="840" alt="image" src="https://github.com/user-attachments/assets/b264c963-68ee-44d7-aa20-d13b794003b1" />

```
module top_module ( 
    input a, 
    input b, 
    input c,
    input d,
    output out1,
    output out2
);
    
    mod_a a1(.out1(out1),.out2(out2),.in1(a),.in2(b),.in3(c),.in4(d));
    

endmodule
```

4] Three modules

<img width="1306" height="544" alt="image" src="https://github.com/user-attachments/assets/0b25bb4e-4556-42f1-aa29-8ae75d0ca145" />

```
module top_module ( input clk, input d, output q );
    wire w1,w2;
    
    my_dff d1(clk,d,w1);
    my_dff d2(clk,w1,w2);
    my_dff d3(clk,w2,q);

endmodule
```

5] Modules and Vectors

<img width="1346" height="742" alt="image" src="https://github.com/user-attachments/assets/22fcda6a-84c7-416c-ba21-f775077f7701" />

```
module top_module ( 
    input clk, 
    input [7:0] d, 
    input [1:0] sel, 
    output [7:0] q 
);
    wire [7:0]w1,w2,w3;
    
    my_dff8 d1(clk,d,w1);
    my_dff8 d2(clk,w1,w2);
    my_dff8 d3(clk,w2,w3);
    
    always @(*) begin
        case(sel)
            0: q<=d;
            1: q<=w1;
            2: q<=w2;
            3: q<=w3;
        endcase
    end

endmodule
```

6] Adder 1

<img width="1319" height="677" alt="image" src="https://github.com/user-attachments/assets/b8fc726d-c139-45e1-8ab9-a8eda7687ad8" />

```
module top_module(
    input [31:0] a,
    input [31:0] b,
    output [31:0] sum
);
    wire cout1, cout2;
    
    add16 a1(a[15:0],b[15:0],0,sum[15:0],cout1);
    add16 a2(a[31:16],b[31:16],cout1,sum[31:16],cout2);

endmodule
```

7] Adder 2

In this exercise, you will create a circuit with two levels of hierarchy. Your top_module will instantiate two copies of add16 (provided), each of which will instantiate 16 copies of add1 (which you must write). Thus, you must write two modules: top_module and add1.

Like module_add, you are given a module add16 that performs a 16-bit addition. You must instantiate two of them to create a 32-bit adder. One add16 module computes the lower 16 bits of the addition result, while the second add16 module computes the upper 16 bits of the result. Your 32-bit adder does not need to handle carry-in (assume 0) or carry-out (ignored).

Connect the add16 modules together as shown in the diagram below. The provided module add16 has the following declaration:

module add16 ( input[15:0] a, input[15:0] b, input cin, output[15:0] sum, output cout );

Within each add16, 16 full adders (module add1, not provided) are instantiated to actually perform the addition. You must write the full adder module that has the following declaration:

module add1 ( input a, input b, input cin, output sum, output cout );

Recall that a full adder computes the sum and carry-out of a+b+cin.

In summary, there are three modules in this design:

top_module — Your top-level module that contains two of add16, provided — A 16-bit adder module that is composed of 16 of add1 — A 1-bit full adder module.

<img width="870" height="869" alt="image" src="https://github.com/user-attachments/assets/d9427307-49d4-4357-98c0-1faf6802f24b" />

```
module top_module (
    input [31:0] a,
    input [31:0] b,
    output [31:0] sum
);//
    wire cout1,cout2;
    
    add16 a1(a[15:0],b[15:0],0,sum[15:0],cout1);
    add16 a2(a[31:16],b[31:16],cout1,sum[31:16],cout2);

endmodule

module add1 ( input a, input b, input cin,   output sum, output cout );
    
    assign sum = a^b^cin;
    assign cout = (a&b)|(b&cin)|(cin&a);

endmodule
```

8] Carry Select Adder

<img width="1304" height="727" alt="image" src="https://github.com/user-attachments/assets/398d31e5-92fb-4d2a-9ed2-8b325f63cc80" />

```
module top_module(
    input [31:0] a,
    input [31:0] b,
    output [31:0] sum
);
    wire w1,w2,cout1,cout2,cout3;
    wire [15:0] sum1,sum0;
    
    add16 a1(a[15:0],b[15:0],0,sum[15:0],cout1);
    add16 a2(a[31:16],b[31:16],0,sum0,cout2);
    add16 a3(a[31:16],b[31:16],1,sum1,cout3);
    
    always @(*) begin
        if(!cout1) begin
            sum[31:16] <= sum0;
        end
        else begin
            sum[31:16] <= sum1;
        end
    end
    

endmodule

```

9] Adder Subtractor

<img width="1308" height="765" alt="image" src="https://github.com/user-attachments/assets/de667fd4-6431-40ff-b285-5e4cbedc44e9" />

```
module top_module(
    input [31:0] a,
    input [31:0] b,
    input sub,
    output [31:0] sum
);
    wire [31:0]sub_out;
    wire cout1,cout2;
    
    assign sub_out = b ^ {32{sub}};
    
    add16 a1(a[15:0],sub_out[15:0],sub,sum[15:0],cout1);
    add16 a2(a[31:16],sub_out[31:16],cout1,sum[31:16],cout2);

endmodule
```

### Procedures

1] Always Block (Combinational)

Build an AND gate using both an assign statement and a combinational always block.

```
// synthesis verilog_input_version verilog_2001
module top_module(
    input a, 
    input b,
    output wire out_assign,
    output reg out_alwaysblock
);
    assign out_assign = a&b;
    
    always @(*) begin
       out_alwaysblock = a&b;
    end

endmodule
```

2] Always block (Clocked)

Build an XOR gate three ways, using an assign statement, a combinational always block, and a clocked always block. Note that the clocked always block produces a different circuit from the other two: There is a flip-flop so the output is delayed.

```
module top_module(
    input clk,
    input a,
    input b,
    output wire out_assign,
    output reg out_always_comb,
    output reg out_always_ff   );
    
    assign out_assign = a^b;
    
    always @(*) begin
       out_always_comb = a^b; 
    end
    
    always @(posedge clk) begin
       out_always_ff = a^b; 
    end

endmodule
```

3] If statement

Build a 2-to-1 mux that chooses between a and b. Choose b if both sel_b1 and sel_b2 are true. Otherwise, choose a. Do the same twice, once using assign statements and once using a procedural if statement.

```
module top_module(
    input a,
    input b,
    input sel_b1,
    input sel_b2,
    output wire out_assign,
    output reg out_always   ); 
    
    assign out_assign = (sel_b1 && sel_b2) ? b:a;
    
    always @(*) begin
        if(sel_b1 && sel_b2) begin
           out_always = b; 
        end
        else begin
           out_always = a; 
        end
    end

endmodule
```

4] If statement Latches

<img width="1295" height="653" alt="image" src="https://github.com/user-attachments/assets/1573aec2-feba-4fa3-9d9b-c3b005dde03c" />

```
module top_module (
    input      cpu_overheated,
    output reg shut_off_computer,
    input      arrived,
    input      gas_tank_empty,
    output reg keep_driving  ); //

    always @(*) begin
        if (cpu_overheated)
           shut_off_computer = 1;
        else
            shut_off_computer = 0;
    end

    always @(*) begin
        if (~arrived)
           keep_driving = ~gas_tank_empty;
        else
            keep_driving = 0;;
    end

endmodule
```

5] Case Statement

Case statements are more convenient than if statements if there are a large number of cases. So, in this exercise, create a 6-to-1 multiplexer. When sel is between 0 and 5, choose the corresponding data input. Otherwise, output 0. The data inputs and outputs are all 4 bits wide.

```
module top_module ( 
    input [2:0] sel, 
    input [3:0] data0,
    input [3:0] data1,
    input [3:0] data2,
    input [3:0] data3,
    input [3:0] data4,
    input [3:0] data5,
    output reg [3:0] out   );//

    always@(*) begin  // This is a combinational circuit
        case(sel)
            0: out<=data0;
            1: out<=data1;
            2: out<=data2;
            3: out<=data3;
            4: out<=data4;
            5: out<=data5;
            default: out<=0;
        endcase
    end

endmodule
```

6] Priority Encoder

Build a 4-bit priority encoder. For this problem, if none of the input bits are high (i.e., input is zero), output zero. Note that a 4-bit number has 16 possible combinations.

```
module top_module (
    input [3:0] in,
    output reg [1:0] pos  );
    
    always @(*) begin
        casex(in)
            4'bxxx1 : pos<=0;
            4'bxx10 : pos<=1;
            4'bx100 : pos<=2;
            4'b1000 : pos<=3;
            default: pos<=0;
        endcase
    end

endmodule
```

7] Priority Encoder with casez

Build a priority encoder for 8-bit inputs. Given an 8-bit vector, the output should report the first (least significant) bit in the vector that is 1. Report zero if the input vector has no bits that are high. For example, the input 8'b10010000 should output 3'd4, because bit[4] is first bit that is high.

```
module top_module (
    input [7:0] in,
    output reg [2:0] pos );
    
    always @(*) begin
        casez(in)
            8'bzzzzzzz1: pos=0;
            8'bzzzzzz1z: pos=1;
            8'bzzzzz1zz: pos=2;
            8'bzzzz1zzz: pos=3;
            8'bzzz1zzzz: pos=4;
            8'bzz1zzzzz: pos=5;
            8'bz1zzzzzz: pos=6;
            8'b1zzzzzzz: pos=7;
            default: pos=0;
        endcase
    end

endmodule
```

8] Avoiding Latches

<img width="1308" height="746" alt="image" src="https://github.com/user-attachments/assets/6afeb448-583e-40d3-a815-a3428bce2c22" />

```
module top_module (
    input [15:0] scancode,
    output reg left,
    output reg down,
    output reg right,
    output reg up  ); 
    
    always @(*) begin
        left=0;right=0;up=0;down=0;
        case(scancode)
            16'he06b: left=1;
            16'he072: down=1;
            16'he074: right=1;
            16'he075: up=1;
            default: begin
               left=0;right=0;up=0;down=0; 
            end
        endcase
    end

endmodule
```


### More Verilog Features

1] Conditional Ternary Operator

Given four unsigned numbers, find the minimum. Unsigned numbers can be compared with standard comparison operators (a < b). Use the conditional operator to make two-way min circuits, then compose a few of them to create a 4-way min circuit. You'll probably want some wire vectors for the intermediate results.

```
module top_module (
    input [7:0] a, b, c, d,
    output [7:0] min);//
    wire [7:0]min1,min2;
    
    assign min1 = (a<b)? a:b;
    assign min2 = (c<d)? c:d;
    
    assign min = (min1<min2)? min1:min2;

endmodule
```

2] Reduction Operators

Parity checking is often used as a simple method of detecting errors when transmitting data through an imperfect channel. Create a circuit that will compute a parity bit for a 8-bit byte (which will add a 9th bit to the byte). We will use "even" parity, where the parity bit is just the XOR of all 8 data bits.

```
module top_module (
    input [7:0] in,
    output parity); 
    
    assign parity = ^in;

endmodule
```

3] Reduction : Even Wider Gates

Build a combinational circuit with 100 inputs, in[99:0].

There are 3 outputs:

out_and: output of a 100-input AND gate.
out_or: output of a 100-input OR gate.
out_xor: output of a 100-input XOR gate.

```
module top_module( 
    input [99:0] in,
    output out_and,
    output out_or,
    output out_xor 
);
    
    assign out_and = &in;
    assign out_or = |in;
    assign out_xor = ^in;

endmodule
```

4] Combinational For Loop

Given a 100-bit input vector [99:0], reverse its bit ordering.

```
module top_module( 
    input [99:0] in,
    output [99:0] out
);
    
    always @(*) begin
        for(int i=0;i<100;i++) begin
            out[i] = in[99-i]; 
        end
    end

endmodule
```

5] 255 bit population count

A "population count" circuit counts the number of '1's in an input vector. Build a population count circuit for a 255-bit input vector.

```
module top_module (
	input [254:0] in,
	output reg [7:0] out
);

	always @(*) begin	// Combinational always block
		out = 0;
		for (int i=0;i<255;i++)
			out = out + in[i];
	end
	
endmodule
```

6] 100 bit binary adder

Create a 100-bit binary ripple-carry adder by instantiating 100 full adders. The adder adds two 100-bit numbers and a carry-in to produce a 100-bit sum and carry out. To encourage you to actually instantiate full adders, also output the carry-out from each full adder in the ripple-carry adder. cout[99] is the final carry-out from the last full adder, and is the carry-out you usually see.

```
module top_module( 
    input [99:0] a, b,
    input cin,
    output [99:0] cout,
    output [99:0] sum );
        
    fa a1(a[0],b[0],cin,sum[0],cout[0]);
        
    genvar i;
    generate
        for(i=1;i<100;i++) begin : block
       fa a_i(a[i],b[i],cout[i-1],sum[i],cout[i]);     
    end
    endgenerate

endmodule

module fa(input a,b,c, output sum, cout);
    
    assign sum = a^b^c;
    assign cout = (a&b)|(b&c)|(c&a);
    
endmodule
```

7] 100 digit BCD adder

You are provided with a BCD one-digit adder named bcd_fadd that adds two BCD digits and carry-in, and produces a sum and carry-out.

module bcd_fadd (
    input [3:0] a,
    input [3:0] b,
    input     cin,
    output   cout,
    output [3:0] sum );
	
Instantiate 100 copies of bcd_fadd to create a 100-digit BCD ripple-carry adder. Your adder should add two 100-digit BCD numbers (packed into 400-bit vectors) and a carry-in to produce a 100-digit sum and carry out.

```
module top_module( 
    input [399:0] a, b,
    input cin,
    output cout,
    output [399:0] sum );
    
    wire [100:0] carry;
    assign carry[0] = cin;
    assign cout = carry[100];
    
    bcd_fadd f1(a[3:0],b[3:0],carry[0],carry[1],sum[3:0]);
    
    genvar i;
    
    generate
        for(i=1;i<100;i++) begin : bcd_add
            bcd_fadd f2_i(a[4*i+3:4*i],b[4*i+3:4*i],carry[i],carry[i+1],sum[4*i+3:4*i]); 
        end
    endgenerate
    

endmodule
```


## Circuits

### Combinational Logic - BAsic Gates

1] Wire

<img width="333" height="111" alt="image" src="https://github.com/user-attachments/assets/2935a184-ce8f-41fa-8b41-0486b6a02850" />

```
module top_module (
    input in,
    output out);
    
    assign out=in;

endmodule
```

2] GND

<img width="305" height="176" alt="image" src="https://github.com/user-attachments/assets/7545b56e-236a-41c0-b689-feb0301f5661" />

```
module top_module (
    output out);
    assign out=0;

endmodule
```

3] NOR

<img width="380" height="194" alt="image" src="https://github.com/user-attachments/assets/b66e54c3-4281-4932-8224-df6f930b515e" />

```
module top_module (
    input in1,
    input in2,
    output out);
    
    assign out = ~(in1|in2);

endmodule
```

4] Another Gate

<img width="390" height="190" alt="image" src="https://github.com/user-attachments/assets/472249b3-db2a-4b3b-9b51-9f0239d3d0be" />

```
module top_module (
    input in1,
    input in2,
    output out);
    
    assign out = in1 & ~in2;

endmodule
```

5] Two Gates

<img width="660" height="253" alt="image" src="https://github.com/user-attachments/assets/ba1cc0ce-79e6-46a2-bfbe-b0f0577176c6" />

```
module top_module (
    input in1,
    input in2,
    input in3,
    output out);
    wire w1;
    
    assign w1 = ~(in1 ^ in2);
    assign out = w1 ^ in3;

endmodule
```

6] More Logic Gates

Ok, let's try building several logic gates at the same time. Build a combinational circuit with two inputs, a and b.

There are 7 outputs, each with a logic gate driving it:

out_and: a and b
out_or: a or b
out_xor: a xor b
out_nand: a nand b
out_nor: a nor b
out_xnor: a xnor b
out_anotb: a and-not b

```
module top_module( 
    input a, b,
    output out_and,
    output out_or,
    output out_xor,
    output out_nand,
    output out_nor,
    output out_xnor,
    output out_anotb
);
    
    assign out_and = a&b;
    assign out_or = a|b;
    assign out_xor = a^b;
    assign out_nand = ~(a&b);
    assign out_nor = ~(a|b);
    assign out_xnor = ~(a^b);
    assign out_anotb = a&~b;

endmodule
```

7] 7420 Chip

<img width="1168" height="636" alt="image" src="https://github.com/user-attachments/assets/9d27d535-d41c-4ed2-b89d-485097b8b9eb" />

```
module top_module ( 
    input p1a, p1b, p1c, p1d,
    output p1y,
    input p2a, p2b, p2c, p2d,
    output p2y );
    
    assign p1y = ~(p1a&p1b&p1c&p1d);
    assign p2y = ~(p2a&p2b&p2c&p2d);


endmodule
```

8] Truth Tables

<img width="1317" height="627" alt="image" src="https://github.com/user-attachments/assets/4a96277e-a766-4bef-a592-708ac615ec7b" />

```
module top_module( 
    input x3,
    input x2,
    input x1,  // three inputs
    output f   // one output
);
    
    assign f = (~x3&x2&~x1)|(~x3&x2&x1)|(x3&~x2&x1)|(x3&x2&x1);

endmodule
```

9] Two bit equality

Create a circuit that has two 2-bit inputs A[1:0] and B[1:0], and produces an output z. The value of z should be 1 if A = B, otherwise z should be 0.

```
module top_module ( input [1:0] A, input [1:0] B, output z ); 
    
    assign z = (A == B) ? 1:0;

endmodule
```

10] Simple Circuit A

Module A is supposed to implement the function z = (x^y) & x. Implement this module.

```
module top_module (input x, input y, output z);
    
    assign z = (x^y)&x;

endmodule
```

11] Simple Circuit B

<img width="1011" height="226" alt="image" src="https://github.com/user-attachments/assets/4bc4252e-ec75-454e-87eb-000f7b740733" />

```
module top_module ( input x, input y, output z );
    
    assign z = ~(x^y);

endmodule
```

12] Combine Circuits A and B

<img width="1241" height="784" alt="image" src="https://github.com/user-attachments/assets/9bc58de0-15f4-4b5c-b2d0-44e7f4806d1a" />

```
module top_module (input x, input y, output z);
    
    wire w1,w2,w3,w4,w5,w6;
    
    module_A A1(x,y,w1);
    module_B B1(x,y,w2);
    module_A A2(x,y,w3);
    module_B B2(x,y,w4);
    assign w5 = w1|w2;
    assign w6 = w3&w4;
    assign z = w5^w6;
    
    

endmodule

module module_A (input x, input y, output z);
    
    assign z = (x^y)&x;

endmodule

module module_B ( input x, input y, output z );
    
    assign z = ~(x^y);

endmodule
```

13] Ring or Vibrate

Suppose you are designing a circuit to control a cellphone's ringer and vibration motor. Whenever the phone needs to ring from an incoming call (input ring), your circuit must either turn on the ringer (output ringer = 1) or the motor (output motor = 1), but not both. If the phone is in vibrate mode (input vibrate_mode = 1), turn on the motor. Otherwise, turn on the ringer.

```
module top_module (
    input ring,
    input vibrate_mode,
    output ringer,       // Make sound
    output motor         // Vibrate
);
    
    assign ringer = ring&~vibrate_mode;
    assign motor = ring&vibrate_mode;

endmodule
```

14] Thermostat

A heating/cooling thermostat controls both a heater (during winter) and an air conditioner (during summer). Implement a circuit that will turn on and off the heater, air conditioning, and blower fan as appropriate.

The thermostat can be in one of two modes: heating (mode = 1) and cooling (mode = 0). In heating mode, turn the heater on when it is too cold (too_cold = 1) but do not use the air conditioner. In cooling mode, turn the air conditioner on when it is too hot (too_hot = 1), but do not turn on the heater. When the heater or air conditioner are on, also turn on the fan to circulate the air. In addition, the user can also request the fan to turn on (fan_on = 1), even if the heater and air conditioner are off.

```
module top_module (
    input too_cold,
    input too_hot,
    input mode,
    input fan_on,
    output heater,
    output aircon,
    output fan
); 
    assign heater = too_cold&mode;
    assign aircon = too_hot&~mode;
    assign fan = fan_on|(too_hot&~mode)|(too_cold&mode);

endmodule
```

15] 3 bit population count

A "population count" circuit counts the number of '1's in an input vector. Build a population count circuit for a 3-bit input vector.

```
module top_module( 
    input [2:0] in,
    output [1:0] out );
    
    always @(*) begin
       out=0;
        for(int i=0;i<3;i++) begin
            if(in[i]) out++; 
        end
    end

endmodule
```

16] Gates and Vectors

You are given a four-bit input vector in[3:0]. We want to know some relationships between each bit and its neighbour:

out_both: Each bit of this output vector should indicate whether both the corresponding input bit and its neighbour to the left (higher index) are '1'. For example, out_both[2] should indicate if in[2] and in[3] are both 1. Since in[3] has no neighbour to the left, the answer is obvious so we don't need to know out_both[3].

out_any: Each bit of this output vector should indicate whether any of the corresponding input bit and its neighbour to the right are '1'. For example, out_any[2] should indicate if either in[2] or in[1] are 1. Since in[0] has no neighbour to the right, the answer is obvious so we don't need to know out_any[0].

out_different: Each bit of this output vector should indicate whether the corresponding input bit is different from its neighbour to the left. For example, out_different[2] should indicate if in[2] is different from in[3]. For this part, treat the vector as wrapping around, so in[3]'s neighbour to the left is in[0].

```
module top_module( 
    input [3:0] in,
    output [2:0] out_both,
    output [3:1] out_any,
    output [3:0] out_different );
    
    assign out_both = in[2:0]&in[3:1];
    assign out_any = in[3:1] | in[2:0];
    assign out_different = in ^ {in[0], in[3:1]};
    
    

endmodule
```

17] Even Longer Vectors

You are given a 100-bit input vector in[99:0]. We want to know some relationships between each bit and its neighbour:

out_both: Each bit of this output vector should indicate whether both the corresponding input bit and its neighbour to the left are '1'. For example, out_both[98] should indicate if in[98] and in[99] are both 1. Since in[99] has no neighbour to the left, the answer is obvious so we don't need to know out_both[99].

out_any: Each bit of this output vector should indicate whether any of the corresponding input bit and its neighbour to the right are '1'. For example, out_any[2] should indicate if either in[2] or in[1] are 1. Since in[0] has no neighbour to the right, the answer is obvious so we don't need to know out_any[0].

out_different: Each bit of this output vector should indicate whether the corresponding input bit is different from its neighbour to the left. For example, out_different[98] should indicate if in[98] is different from in[99]. For this part, treat the vector as wrapping around, so in[99]'s neighbour to the left is in[0].

```
module top_module( 
    input [99:0] in,
    output [98:0] out_both,
    output [99:1] out_any,
    output [99:0] out_different );
    
    assign out_both = in[98:0]&in[99:1];
    assign out_any = in[99:1] | in[98:0];
    assign out_different = in ^ {in[0], in[99:1]};

endmodule
```

### Combinational Logic - Multiplexers

1] 2 to 1 Multiplexer

Create a one-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

```
module top_module( 
    input a, b, sel,
    output out ); 
    
    assign out = sel?b:a;

endmodule
```

2] 2 to 1 bus multiplexer

Create a 100-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

```
module top_module( 
    input [99:0] a, b,
    input sel,
    output [99:0] out );
    
    assign out = sel? b:a;

endmodule
```

3] 9 to 1 multiplexer

Create a 16-bit wide, 9-to-1 multiplexer. sel=0 chooses a, sel=1 chooses b, etc. For the unused cases (sel=9 to 15), set all output bits to '1'.

```
module top_module( 
    input [15:0] a, b, c, d, e, f, g, h, i,
    input [3:0] sel,
    output [15:0] out );
    
    always @(*) begin
        case(sel)
            0: out=a;
            1: out=b;
            2: out=c;
            3: out=d;
            4: out=e;
            5: out=f;
            6: out=g;
            7: out=h;
            8: out=i;
            default: out=16'hffff;
        endcase
    end

endmodule
```

4] 256 to 1 multiplexer

Create a 1-bit wide, 256-to-1 multiplexer. The 256 inputs are all packed into a single 256-bit input vector. sel=0 should select in[0], sel=1 selects bits in[1], sel=2 selects bits in[2], etc.

```
module top_module( 
    input [255:0] in,
    input [7:0] sel,
    output out );
    
    assign out = in[sel];

endmodule
```

5] 256 to 1 4-bit multiplexer

Create a 4-bit wide, 256-to-1 multiplexer. The 256 4-bit inputs are all packed into a single 1024-bit input vector. sel=0 should select bits in[3:0], sel=1 selects bits in[7:4], sel=2 selects bits in[11:8], etc.

```
module top_module( 
    input [1023:0] in,
    input [7:0] sel,
    output [3:0] out );
    
    assign out = in[sel*4 +: 4];

endmodule
```

### Combinational Circuits - Arithmetic Circuits

1] Half Adder

Create a half adder. A half adder adds two bits (with no carry-in) and produces a sum and carry-out.

```
module top_module( 
    input a, b,
    output cout, sum );
    
    assign sum = a^b;
    assign cout = a&b;

endmodule
```

2] Full Adder

Create a full adder. A full adder adds three bits (including carry-in) and produces a sum and carry-out.

```
module top_module( 
    input a, b, cin,
    output cout, sum );
    
    assign sum = a^b^cin;
    assign cout = (a&b)|(b&cin)|(cin&a);

endmodule
```

3] 3 bit binary adder

Now that you know how to build a full adder, make 3 instances of it to create a 3-bit binary ripple-carry adder. The adder adds two 3-bit numbers and a carry-in to produce a 3-bit sum and carry out. To encourage you to actually instantiate full adders, also output the carry-out from each full adder in the ripple-carry adder. cout[2] is the final carry-out from the last full adder, and is the carry-out you usually see.

```
module top_module( 
    input [2:0] a, b,
    input cin,
    output [2:0] cout,
    output [2:0] sum );
    
    fa a1(a[0],b[0],cin,sum[0],cout[0]);
    fa a2(a[1],b[1],cout[0],sum[1],cout[1]);
    fa a3(a[2],b[2],cout[1],sum[2],cout[2]);

endmodule

module fa(input a,b,cin, output sum,cout);
    
    assign sum = a^b^cin;
    assign cout = (a&b)|(b&cin)|(cin&a);
    
endmodule
```

4] Adder

<img width="702" height="345" alt="image" src="https://github.com/user-attachments/assets/6be2f430-2ea1-4103-8621-1a7cc19ae07d" />

```
module top_module (
    input [3:0] x,
    input [3:0] y, 
    output [4:0] sum);
    wire [2:0] cout;
    
    fa a1(x[0],y[0],0,sum[0],cout[0]);
    fa a2(x[1],y[1],cout[0],sum[1],cout[1]);
    fa a3(x[2],y[2],cout[1],sum[2],cout[2]);
    fa a4(x[3],y[3],cout[2],sum[3],sum[4]);

endmodule

module fa(input a,b,cin, output sum,cout);
    
    assign sum = a^b^cin;
    assign cout = (a&b)|(b&cin)|(cin&a);
    
endmodule
```

5] Signed addition overflow

Assume that you have two 8-bit 2's complement numbers, a[7:0] and b[7:0]. These numbers are added to produce s[7:0]. Also compute whether a (signed) overflow has occurred.

```
module top_module (
    input [7:0] a,
    input [7:0] b,
    output [7:0] s,
    output overflow
); //
 
    assign s = a+b;
    assign overflow = (a[7]&b[7]&~s[7])|(~a[7]&~b[7]&s[7]);

endmodule
```

6] 100 bit binary adder

Create a 100-bit binary adder. The adder adds two 100-bit numbers and a carry-in to produce a 100-bit sum and carry out.

```
module top_module( 
    input [99:0] a, b,
    input cin,
    output cout,
    output [99:0] sum );
    
    assign {cout,sum} = a+b+cin;

endmodule
```

7] 4 digit BCD adder

You are provided with a BCD (binary-coded decimal) one-digit adder named bcd_fadd that adds two BCD digits and carry-in, and produces a sum and carry-out.

module bcd_fadd (
    input [3:0] a,
    input [3:0] b,
    input     cin,
    output   cout,
    output [3:0] sum );
	
Instantiate 4 copies of bcd_fadd to create a 4-digit BCD ripple-carry adder. Your adder should add two 4-digit BCD numbers (packed into 16-bit vectors) and a carry-in to produce a 4-digit sum and carry out.

```
module top_module ( 
    input [15:0] a, b,
    input cin,
    output cout,
    output [15:0] sum );
    wire [4:0] car;
    assign car[0]=cin;
    assign cout=car[4];
    
    bcd_fadd f1(a[3:0],b[3:0],car[0],car[1],sum[3:0]);
    bcd_fadd f2(a[7:4],b[7:4],car[1],car[2],sum[7:4]);
    bcd_fadd f3(a[11:8],b[11:8],car[2],car[3],sum[11:8]);
    bcd_fadd f4(a[15:12],b[15:12],car[3],car[4],sum[15:12]);
    

endmodule
```

### Combinational Logic - Karnaugh Map to Circuit

1] 3 variable

<img width="558" height="478" alt="image" src="https://github.com/user-attachments/assets/a570ee7e-8c31-4bea-9803-26f6cc9bb1e4" />

```
module top_module(
    input a,
    input b,
    input c,
    output out  ); 
    
    assign out = (a)|(b)|(c);

endmodule
```

2] 4 variable

<img width="591" height="437" alt="image" src="https://github.com/user-attachments/assets/06212eda-dca2-4253-bfa7-a9911102dd14" />

```
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    
    assign out = (~a&~d)|(~b&~c)|(c&d&(a|b));

endmodule
```

3] 4 variable

<img width="560" height="423" alt="image" src="https://github.com/user-attachments/assets/54459e77-6750-4fc9-bafa-66205268792a" />


```
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    
    assign out = a|(~b&c);

endmodule
```

4] 4 variable

<img width="554" height="410" alt="image" src="https://github.com/user-attachments/assets/b67bf45d-f8fe-40d9-89c9-e9a4d0088615" />

```
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    
    assign out = (~a&~b&~c&d)|(~a&~b&c&~d)|(~a&b&~c&~d)|(~a&b&c&d)|(a&~b&~c&~d)|(a&~b&c&d)|(a&b&~c&d)|(a&b&c&~d);

endmodule
```

5] Minimum POS and SOP

A single-output digital system with four inputs (a,b,c,d) generates a logic-1 when 2, 7, or 15 appears on the inputs, and a logic-0 when 0, 1, 4, 5, 6, 9, 10, 13, or 14 appears. The input conditions for the numbers 3, 8, 11, and 12 never occur in this system. For example, 7 corresponds to a,b,c,d being set to 0,1,1,1, respectively.

Determine the output out_sop in minimum SOP form, and the output out_pos in minimum POS form.

```
module top_module (
    input a,
    input b,
    input c,
    input d,
    output out_sop,
    output out_pos
); 
    
    assign out_sop = (c&d)|(~a&~b&c);
    assign out_pos = c&(d|~a)&(d|~b);

endmodule
```

6] Karnaugh Map

<img width="964" height="380" alt="image" src="https://github.com/user-attachments/assets/d297c702-ea33-4b42-aa64-1d78b696f623" />

```
module top_module (
    input [4:1] x, 
    output f );
    
    assign f = (x[3]&~x[1])|(x[1]&x[2]&x[4]);

endmodule
```

7] Karnaugh Map

<img width="755" height="312" alt="image" src="https://github.com/user-attachments/assets/7af5e5a3-4a71-4b8e-bfbd-556948c4e8b8" />

```
module top_module (
    input [4:1] x,
    output f
); 
    
    assign f = (x[3]&~x[1])|(~x[2]&~x[4])|(x[2]&x[3]&x[4]);

endmodule
```

8] K-map implemented with multiplexer

<img width="1325" height="661" alt="image" src="https://github.com/user-attachments/assets/963ea0e3-a53b-4ca1-8073-b225eb743ba1" />

```
module top_module (
    input c,
    input d,
    output [3:0] mux_in
); 
    
    assign mux_in[0] = (~c&d)|(c&d)|(c&~d);
    assign mux_in[1] = 0;
    assign mux_in[2] = (~c&~d)|(c&~d);
    assign mux_in[3] = c&d;

endmodule
```

### Sequential Logic - Latches and Flip Flops

1] D Flip Flop

<img width="1319" height="314" alt="image" src="https://github.com/user-attachments/assets/67c740a9-1b07-4d7a-ad5b-7a0217ebf00b" />

```
module top_module (
    input clk,    // Clocks are used in sequential circuits
    input d,
    output reg q );//

    always @(posedge clk) begin
       q = d; 
    end

endmodule
```

2] D flip flops

Create 8 D flip-flops. All DFFs should be triggered by the positive edge of clk.

```
module top_module (
    input clk,
    input [7:0] d,
    output [7:0] q
);
    
    always @(posedge clk) begin
       q <= d; 
    end

endmodule
```

3] DFF with reset

Create 8 D flip-flops with active high synchronous reset. All DFFs should be triggered by the positive edge of clk.

```
module top_module (
    input clk,
    input reset,            // Synchronous reset
    input [7:0] d,
    output [7:0] q
);
    
    always @(posedge clk) begin
        if(reset) q <= 0;
        else q <=d;
    end

endmodule
```

4] DFF with reset value

Create 8 D flip-flops with active high synchronous reset. The flip-flops must be reset to 0x34 rather than zero. All DFFs should be triggered by the negative edge of clk.

```
module top_module (
    input clk,
    input reset,
    input [7:0] d,
    output [7:0] q
);
    
    always @(negedge clk) begin
        if(reset) q <= 8'h34;
        else q <= d;
    end

endmodule
```

5] DFF with asynchronous reset

Create 8 D flip-flops with active high asynchronous reset. All DFFs should be triggered by the positive edge of clk.

```
module top_module (
    input clk,
    input areset,   // active high asynchronous reset
    input [7:0] d,
    output [7:0] q
);
    
    always @(posedge clk or posedge areset) begin
        if(areset) q <= 0;
        else q <= d;
    end

endmodule
```

6] DFF with Byte enable

Create 16 D flip-flops. It's sometimes useful to only modify parts of a group of flip-flops. The byte-enable inputs control whether each byte of the 16 registers should be written to on that cycle. byteena[1] controls the upper byte d[15:8], while byteena[0] controls the lower byte d[7:0].

resetn is a synchronous, active-low reset.

All DFFs should be triggered by the positive edge of clk.

```
module top_module (
    input clk,
    input resetn,
    input [1:0] byteena,
    input [15:0] d,
    output [15:0] q
);
    always @(posedge clk) begin
        if(!resetn) begin
           q <= 0; 
        end
        else begin
            if(byteena[1]) begin
                q[15:8] <= d[15:8]; 
            end
            if(byteena[0]) begin
                q[7:0] <= d[7:0]; 
            end
        end
    end

endmodule
```

7] D Latch

<img width="773" height="199" alt="image" src="https://github.com/user-attachments/assets/4d1ed4a3-bf43-4362-8259-e9f2fe83c7d2" />

```
module top_module (
    input d, 
    input ena,
    output q);
    
    always @(*) begin
        if(ena) q <= d; 
    end

endmodule
```

8] DFF

<img width="346" height="202" alt="image" src="https://github.com/user-attachments/assets/78743364-f5e2-4cbb-8614-d124cb5bbe7c" />

```
module top_module (
    input clk,
    input d, 
    input ar,   // asynchronous reset
    output q);
    
    always @(posedge clk or posedge ar) begin
        if(ar) q <= 0;
        else q <= d;
    end

endmodule
```

9] DFF

<img width="306" height="182" alt="image" src="https://github.com/user-attachments/assets/a0e9f7e0-e5d7-429d-a693-1d0eafc2e623" />

```
module top_module (
    input clk,
    input d, 
    input r,   // synchronous reset
    output q);
    
    always @(posedge clk) begin
        if(r) q <= 0;
        else q <= d;
    end

endmodule
```

10] DFF + gate

<img width="575" height="273" alt="image" src="https://github.com/user-attachments/assets/a56435d1-8726-44a6-a0d3-928d04eeb469" />

```
module top_module (
    input clk,
    input in, 
    output out);
    
    always @(posedge clk) begin
       out <= in ^ out; 
    end

endmodule
```

11] Mux and DFF

<img width="1261" height="581" alt="image" src="https://github.com/user-attachments/assets/8e3d2538-9c32-4717-b8c2-bead53547c8b" />

```
module top_module (
	input clk,
	input L,
	input r_in,
	input q_in,
	output reg Q);
    
    always @(posedge clk) begin
       Q <= L ? r_in:q_in; 
    end

endmodule
```

12] Mux and DFF

<img width="1192" height="541" alt="image" src="https://github.com/user-attachments/assets/ce9c4be2-fd23-49e6-9ceb-7b3406f0be64" />

```
module top_module (
    input clk,
    input w, R, E, L,
    output Q
);
    
    always @(posedge clk) begin
        Q <= L? R : (E? w: Q) ; 
    end

endmodule
```

13] DFFs and Gates

<img width="1136" height="571" alt="image" src="https://github.com/user-attachments/assets/88b6f391-06ba-4fde-a61b-91dbd776e5da" />

```
module top_module (
    input clk,
    input x,
    output z
); 
    wire w1,w2,w3;
    
     
    
    always @(posedge clk) begin
        w1 <= x^w1;
        w2 <= x & ~w2;
        w3 <= x | ~w3;
        
    end
    assign  z =~(w1|w2|w3);
    
endmodule
```

14] Create circuit from truth table

<img width="1319" height="245" alt="image" src="https://github.com/user-attachments/assets/d1793dad-4584-4cc7-af4f-c73b20405f0f" />

```
module top_module (
    input clk,
    input j,
    input k,
    output Q); 
    
    always @(posedge clk) begin
        Q <= j ? (k ? ~Q:1) : (k ? 0:Q) ;
    end

endmodule
```

15] Setect and edge


<img width="1276" height="309" alt="image" src="https://github.com/user-attachments/assets/12479c94-c81e-4e2b-8095-c585f7f30ca3" />

```
module top_module (
    input clk,
    input [7:0] in,
    output [7:0] pedge
);
    reg [7:0] last;
    always @(posedge clk) begin
        last <= in;
        pedge <= ~last&in;
    end

endmodule
```

16] Detect both edges


<img width="1296" height="315" alt="image" src="https://github.com/user-attachments/assets/1f15a0a4-6a07-4144-b838-f7dbb067ea77" />

```
module top_module (
    input clk,
    input [7:0] in,
    output [7:0] anyedge
);
    reg [7:0] last;
    always @(posedge clk) begin
        last <= in;
        
        anyedge <= (~last & in)|(last & ~in);
    end

endmodule
```

17] Edge Capture Register


<img width="1296" height="549" alt="image" src="https://github.com/user-attachments/assets/ff7f8146-12f6-4bc8-a39f-9154e27a4995" />

```
module top_module (
    input clk,
    input reset,
    input [31:0] in,
    output [31:0] out
);
    
    reg [31:0] last;
    
    always @(posedge clk) begin
        
        if(reset) out <=0;
        else begin
           
           out <= out | (~in&last); 
        end
        last <= in;
    end

endmodule
```

18] Dual edge-triggered Flip Flop

<img width="1304" height="358" alt="image" src="https://github.com/user-attachments/assets/642b5cfb-cb79-42dd-8fc3-5196a3b1c481" />

```
module top_module (
    input clk,
    input d,
    output q
);
    reg q1,q2;
    
    always @(posedge clk) begin
       q1 <= d; 
    end
    
    always @(negedge clk) begin
       q2 <= d; 
    end
    
    assign q = clk ? q1:q2;
 
endmodule
```

### Sequential Logic - Counters

1] 
