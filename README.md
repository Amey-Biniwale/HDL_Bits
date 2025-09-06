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
