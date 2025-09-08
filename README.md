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

top_module — Your top-level module that contains two of...
add16, provided — A 16-bit adder module that is composed of 16 of...
add1 — A 1-bit full adder module.

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

