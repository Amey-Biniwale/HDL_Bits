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

