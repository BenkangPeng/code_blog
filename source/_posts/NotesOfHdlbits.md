---
title: HDLBits Solutions
tags: IC
abbrlink: 4ff0bc03
date: 2021-10-01 20:00:00
---

<h3>Abstract</h3>

<p>The solutions and my notes of <a href="[Problem sets - HDLBits (01xz.net)](https://hdlbits.01xz.net/wiki/Problem_sets)">HDLBits</a> are written in this document .</p>

<!-- more -->

[TOC]



# Getting Started

### Getting Started

```verilog
module top_module( output one );
	
	assign one = 1'b1;
	
endmodule
```

### Output Zero

```verilog
module top_module ( output zero );
	
	assign zero = 1'b0;
	
endmodule
```



# Verilog Language

## Basics

### Simple wire

```verilog
module top_module( input in, output out );
	assign out = in;
endmodule

```

### Four wires

```verilog
module top_module (
	input a,
	input b,
	input c,
	output w,
	output x,
	output y,
	output z  );
	
	assign w = a;
	assign x = b;
	assign y = b;
	assign z = c;

	// If we're certain about the width of each signal, using 
	// the concatenation operator is equivalent and shorter:
	// assign {w,x,y,z} = {a,b,b,c};
	
endmodule

```

### Inverter

```verilog
module top_module(
	input in,
	output out
);
	
	assign out = ~in;
	
endmodule

```

### AND gate

```verilog
module top_module( 
    input a, 
    input b, 
    output out );
	assign out = a & b ;
endmodule

```

### NOR gate

```verilog
module top_module( 
    input a, 
    input b, 
    output out );
    assign out = ! (a | b) ;
endmodule

```



### XNOR gate



### Declaring wires



### 7458 chip



## Vectors

### Vectors



## More Verilog Features

### *Combinational for-loop : Vector reversal 2

> Given a 100-bit input vector [99:0], reverse its bit ordering.

```verilog
// False Vesion
module top_module(
    input [99:0] in ,
    output [99:0] out 
) ;
    for(int i = 0 ; i < 100 ; i++) begin
        assign out[i] = in [99 - i] ;
    end
endmodule
```

> ```shell
> Error (10170): Verilog HDL syntax error at top_module.v(6) near text: "for";  expecting "endmodule". 
> ```

为什么是这样的报错呢？显示在for循环那一行前缺少一个endmodule

> 在module内部，只能**声明输入**、**输出端口、内部信号以及使用连续赋值**（`assign` 语句）来描述硬件电路的连续逻辑

因此，`verilog`编译器认为，`for`循环前应该有一个`endmodule`以结束该`module`,此后才能开始`for-loop`的声明。

**为什么Verilog规定Module内部不能声明for-loop?**

> 当涉及到硬件描述语言（HDL）如Verilog时，有一些关键的设计原则和语法规则是不同于通用编程语言（如C、Python）的。
>
> Verilog 是为硬件设计而设计的，其目的是描述硬件电路的结构和行为，而不是传统意义上的“编程”。在硬件电路中，信号是在不同的时间步长上并行计算的，而不是按顺序执行的像在软件中一样。这导致了一些语法规则的不同。
>
> 具体来说，以下是为什么在 Verilog 中不能直接将 `for` 循环放在模块内部的原因：
>
> 1. **并行性：** Verilog 中的代码被解释为电路，其中不同的信号在相同的时间步长上并行地计算。这与传统编程语言中的迭代循环不同，后者按顺序执行。在硬件电路中，不同的信号和逻辑是同时计算的，因此 `for` 循环的概念与硬件的并行性不一致。
> 2. **延迟和时间问题：** 在 Verilog 中，信号的传播和逻辑运算都需要考虑时间延迟。循环的迭代次数可能需要在编译时已知，但在硬件中，延迟可能会影响电路的行为。如果允许在模块内部使用 `for` 循环，编译器就需要处理如何在时间上正确地展开循环和处理延迟的问题。
>
> 为了符合硬件的特性，Verilog 引入了生成语句（generate statements），允许在编译时生成不同的硬件结构，这包括使用 `for` 循环来生成多个类似的硬件逻辑。生成语句允许在不同的时间步长上并行生成硬件，而不会引入运行时的顺序问题。

简单说就是：`for-loop`内不同层次(i = 1 , 2 ,3 ……)，体现了顺序的先后，是软件编程的思维；而`verilog`中`assign`描述的各硬件是并行运行的，是硬件编程的思维；二者需要区分开来:smile:

我们使用`generate`来实现(generate生成多个并行计算模块)：[generate介绍]([Verilog中generate的使用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/107047600))

```verilog
module top_module(
    input [99:0] in ,
    output [99:0] out 
) ;
    genvar i ;//state a variable i 
    generate
        for(i = 0 ; i < 100 ; i++) begin : generate_name
            assign out[i] = in [99 - i] ; // type of out is wire
        end
    endgenerate
endmodule
```

或者将`for-loop`写在`always`中：

```verilog
module top_module(
    input [99:0] in ,
    output reg [99:0] out 
) ;
    always @(*)begin
        for(int i = 0 ; i < $bits(out) ; i++ ) begin
            //$bits(signal)获取信号位长
            out[i] = in [ $bits(out) - 1 - i ] ; 
            //type of out is reg
        end
    end
endmodule
```

### Combinational for-loop:255-bit population count

> A "population count" circuit counts the number of '1's in an input vector. Build a population count circuit for a 255-bit input vector.

```verilog
module top_module( 
    input [254:0] in,
    output reg [7:0] out );
    always @(*) begin
       out = 0 ;//寄存器型变量在always块中初始化
        for(int i = 0 ; i < 255 ; i++) begin
            out = out + in[i] ; 
        end
    end
endmodule

```



### Generate for-loop:1000-bit binary adder 2

> Create a 100-bit binary ripple-carry adder by instantiating 100 [full adders](https://hdlbits.01xz.net/wiki/Fadd). The adder adds two 100-bit numbers and a carry-in to produce a 100-bit sum and carry out. To encourage you to actually instantiate full adders, also output the carry-out from *each* full adder in the ripple-carry adder. cout[99] is the final carry-out from the last full adder, and is the carry-out you usually see.

```verilog
module top_module( 
    input [99:0] a, b,
    input cin,
    output [99:0] cout,
    output [99:0] sum );
	generate
        genvar i ;//好像在for循环内声明genvar i 会报错
        assign sum[0] = a[0] ^ b[0] ^ cin ;
        assign cout[0] = a[0] & b[0] | a[0] & cin | b[0] & cin ;
        for (i = 1 ; i < 100 ; i++) begin : generate_name 
            assign sum[i] = a[i] ^ b[i] ^ cout[i-1] ;
            assign cout[i] = a[i] & b[i] | a[i] & cout[i-1] | b[i] & cout[i-1] ;
        end
    endgenerate
endmodule

```



### *Generate for-loop:100-digit BCD adder

> You are provided with a BCD one-digit adder named `bcd_fadd` that adds two BCD digits and carry-in, and produces a sum and carry-out.
>
> ```
> module bcd_fadd (
>     input [3:0] a,
>     input [3:0] b,
>     input     cin,
>     output   cout,
>     output [3:0] sum );
> ```
>
> Instantiate 100 copies of `bcd_fadd` to create a 100-digit BCD ripple-carry adder. Your adder should add two 100-digit BCD numbers (packed into 400-bit vectors) and a carry-in to produce a 100-digit sum and carry out.

```verilog
//False Verion 
module top_module( 
    input [399:0] a, b,
    input cin,
    output reg cout,
    output reg [399:0] sum );
    genvar i , j ;
    reg [99:0] temp_cout ;
    assign cout = temp_cout[99] ;
    generate
        bcd_fadd U0( a[3:0] , b[3:0] , cin , temp_cout[0] , sum[3:0]) ;
        for (i = 7 , j = 0; i < 400 ;  i = i + 4 , j++) begin : name 
            bcd_fadd U1( a[i:i-3] , b[i:i-3] , temp_cout[j] , sum[i:i-3]) ;
        end
    endgenerate
endmodule

```

> 报错信息：Error (10170): Verilog HDL syntax error at top_module.v(12) near text: ",";  expecting ";"

没有查到相关信息(先留一个坑)，猜测：`for-loop`中不能有两个`genvar`,于是不能出现`,` ,提示你要用`;`

```verilog
//Right Verion 
module top_module( 
    input [399:0] a, b,
    input cin,
    output reg cout,
    output reg [399:0] sum );
    genvar i ;
    reg [99:0] temp_cout ;
    assign cout = temp_cout[99] ;
    generate
        bcd_fadd U0( a[3:0] , b[3:0] , cin , temp_cout[0] , sum[3:0]) ;
        for (i = 7 ; i < 400 ;  i = i + 4 ) begin : name 
            bcd_fadd U1( a[i:i-3] , b[i:i-3] , temp_cout[(i + 1) / 4 - 2] , temp_cout[(i + 1) / 4 - 1] , sum[i:i-3]) ;
        end
    endgenerate
endmodule
```

**此处实现一下`bcd_fadd`**

```verilog
module bcd_fadd(
    input [3 : 0] a , b ,
    input cin , 
    output cout ,
    output [3:0] sum
) ;
    wire [4:0] temp ;
    assign temp = a + b + cin ;
    assign {cout , sum} = (temp>9) ? (temp+6) : temp ; 
endmodule
```



# Circuits

## Combinational Logic

### Basic Gates

#### Even longer vectors

> See also the shorter version: [Gates and vectors](https://hdlbits.01xz.net/wiki/gatesv).
>
> You are given a 100-bit input vector in[99:0]. We want to know some relationships between each bit and its neighbour:
>
> - **out_both**: Each bit of this output vector should indicate whether *both* the corresponding input bit and its neighbour to the **left** are '1'. For example, `out_both[98]` should indicate if `in[98]` and `in[99]` are both 1. Since `in[99]` has no neighbour to the left, the answer is obvious so we don't need to know `out_both[99]`.
> - **out_any**: Each bit of this output vector should indicate whether *any* of the corresponding input bit and its neighbour to the **right** are '1'. For example, `out_any[2]` should indicate if either `in[2]` or `in[1]` are 1. Since `in[0]` has no neighbour to the right, the answer is obvious so we don't need to know `out_any[0]`.
> - **out_different**: Each bit of this output vector should indicate whether the corresponding input bit is different from its neighbour to the **left**. For example, `out_different[98]` should indicate if `in[98]` is different from `in[99]`. For this part, treat the vector as wrapping around, so `in[99]`'s neighbour to the left is `in[0]`.

```verilog
//My vesion
module top_module( 
    input [99:0] in,
    output [98:0] out_both,
    output [99:1] out_any,
    output [99:0] out_different );
    genvar i ;
	generate
        for( i = 0 ; i < 99 ; i++ ) begin : generate_name
            assign out_both[i] = in[i] & in[i+1] ;
            assign out_any[i+1]= in[i+1] | in[i] ;
            assign out_different[i] = in[i] ^ in[i+1] ;
        end
        assign out_different[99] = in[99] ^ in[0] ;
    endgenerate
endmodule
```

```verilog
//Offical vesion
module top_module( 
    input [99:0] in,
    output [98:0] out_both,
    output [99:1] out_any,
    output [99:0] out_different );
    assign out_both = in[98:0] & in[99:1] ;
    assign out_any  = in[99:1] | in[98:0] ;
    assign out_different = in ^ { in[0] , in[99:1] } ;
endmodule
```

### Multiplexers

#### 9-to-1 multiplexer

> Create a 16-bit wide, 9-to-1 multiplexer. sel=0 chooses a, sel=1 chooses b, etc. For the unused cases (sel=9 to 15), set all output bits to '1'.

```verilog
//Flase vesion
module top_module( 
    input [15:0] a, b, c, d, e, f, g, h, i,
    input [3:0] sel,
    output reg [15:0] out );
    always @(*)begin 
    case(sel)
        4'b0000 : out = a ;
        4'b0001 : out = b ;
        4'b0010 : out = c ;
        4'b0011 : out = d ;
        4'b0100 : out = e ;
        4'b0101 : out = f ;
        4'b0110 : out = g ;
        4'b0111 : out = h ;
        4'b1000 : out = i ;
     	default : out = 16'hFFFF ;//十六进制应该为4'hFFFF
        //踩过的坑 ： out = 1 ; out = 0xFFFF ; out = 4'hFFFF
        endcase
    end      
endmodule
```



#### 256-to-1 4-bit multiplexer

> Create a 4-bit wide, 256-to-1 multiplexer. The 256 4-bit inputs are all packed into a single 1024-bit input vector. sel=0 should select bits `in[3:0]`, sel=1 selects bits `in[7:4]`, sel=2 selects bits `in[11:8]`, etc.

> Hint
>
> - With this many options, a case statement isn't so useful.
> - Vector indices can be variable, as long as the synthesizer can figure out that the width of the bits being selected is constant. It's not always good at this. An error saying "... is not a constant" means it couldn't prove that the select width is constant. In particular, `in[ sel*4+3 : sel*4 ]` does not work. // verilog 索引不支持全为变量
> - Bit slicing ("Indexed vector part select", since Verilog-2001) has an even more compact syntax.



```verilog
module top_module( 
    input [1023:0] in,
    input [7:0] sel,
    output [3:0] out );
  
    assign out = in[sel * 4 + 3 : sel * 4] ;

endmodule

```

> error : sel is not a constant File ;

```verilog
//True
module top_module( 
    input [1023:0] in,
    input [7:0] sel,
    output [3:0] out );
    
    assign out = in[sel * 4  +: 4] ;//从sel*4向上四位
    
endmodule

```

### Arithmetic Circuits

#### 3-bit binary adder

> Now that you know how to build a [full adder](https://hdlbits.01xz.net/wiki/Fadd), make 3 instances of it to create a 3-bit binary ripple-carry adder. The adder adds two 3-bit numbers and a carry-in to produce a 3-bit sum and carry out. To encourage you to actually instantiate full adders, also output the carry-out from *each* full adder in the ripple-carry adder. cout[2] is the final carry-out from the last full adder, and is the carry-out you usually see.

```verilog
module top_module( 
    input [2:0] a, b,
    input cin,
    output [2:0] cout,
    output [2:0] sum );
    full_adder ripple_adder_1( a[0] , b[0] , cin , cout[0] , sum[0] ) ;
    full_adder ripple_adder_2( a[1] , b[1] , cout[0] , cout[1] , sum[1] ) ;
    full_adder ripple_adder_3( a[2] , b[2] , cout[1] , cout[2] , sum[2] ) ;
endmodule

module full_adder(
    input a , b , cin , 
    output cout , sum
);
    assign cout= a & b | a & cin | b & cin ;
    assign sum = a ^ b ^ cin ;
endmodule
```

#### Signed addition overflow

> Assume that you have two 8-bit 2's complement numbers, a[7:0] and b[7:0]. These numbers are added to produce s[7:0]. Also compute whether a (signed) overflow has occurred.

```verilog
//Flase vesion
module top_module (
    input [7:0] a,
    input [7:0] b,
    output [7:0] s,
    output overflow
); 
    assign { overflow , s } = a + b ;
    //注意题中的signed overflow , 并不等于进位cout
endmodule

```

如何检测符号溢出(**signed overflow**) : 符号不同，相加不会溢出  ； 符号相同，可能溢出，需要设置检测。

```verilog
//True vesion
module top_module(
    input [7:0] a , b ,
    output [7 : 0] s ,
    output overflow
);
    assign s = a + b ;
    assign overflow = ( a[7] == b[7] ) ? (a[7] ^ s[7]) : 0 ;
endmodule
```

#### 100-bit binary adder

```verilog
module top_module (
	input [99:0] a,
	input [99:0] b,
	input cin,
	output cout,
	output [99:0] sum
);

	// The concatenation {cout, sum} is a 101-bit vector.
	assign {cout, sum} = a+b+cin;

endmodule

```

#### 4-digit BCD adder

```verilog
module top_module ( 
    input [15:0] a, b,
    input cin,
    output cout,
    output [15:0] sum );
    wire [2:0] temp_cout ;
    bcd_fadd U0 ( a[3:0] , b[3:0] , cin ,  temp_cout[0] , sum[3:0] ) ;
    bcd_fadd U1 ( a[7:4] , b[7:4] , temp_cout[0] ,  temp_cout[1] , sum[7:4] ) ;
    bcd_fadd U2 ( a[11:8] , b[11:8] , temp_cout[1] ,  temp_cout[2] , sum[11:8] ) ;
    bcd_fadd U3 ( a[15:12] , b[15:12] , temp_cout[2] ,  cout , sum[15:12] ) ;
endmodule

```

### Karnaugh Map to Citcuit

#### 3-variable

```verilog
module top_module(
	input a, 
	input b,
	input c,
	output out
);

	// SOP form: Three prime implicants (1 term each), summed.
	// POS form: One prime implicant (of 3 terms)
	// In this particular case, the result is the same for both SOP and POS.
    // it's easier by using SOP
	assign out = (a | b | c);
	
endmodule

```

#### 4-variable

```verilog
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    assign out = (!b & !c) | (!a & !d) | (!a & b & c) | (a & c & d)  ;
endmodule

```

#### 4-variable

```verilog
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    assign out = a | (!b & c) ;
    //用0去算！f更简单
endmodule

```

#### 4-variable

```verilog
//Stupid vesion
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
    assign out = (!a & !b) & (c ^ d) | (!a & b) & !(c ^ d) | a & b & (c ^ d) | (a & !b) & !(c ^ d);
endmodule

```

```verilog
// Wise vesion
module top_module(
    input a,
    input b,
    input c,
    input d,
    output out  ); 
	assign out = a ^ b ^ c ^ d ;
endmodule
```

**如果会奇偶校验便能一眼看出：`abcd`中有奇数个`1`，卡诺图为`1` .反之为`0`.**

#### Minimum SOP and POS

```verilog
module top_module (
    input a,
    input b,
    input c,
    input d,
    output out_sop,
    output out_pos
); 
	assign out_sop = c & d | !a & !b & c ;
    assign out_pos = c & (!a | b) & (!b | d) ;
endmodule
```

#### Karnaugh map

```verilog
module top_module (
    input [4:1] x, 
    output f );
    assign f = !x[1] & x[3] | x[1] & x[2] & !x[3] ;
endmodule

```

#### Karnaugh map

```verilog
module top_module (
    input [4:1] x,
    output f
); 
    assign f = (x[3] | !x[4]) & (!x[2] | x[3] | x[4]) & (!x[1] | x[2] | !x[4]) & (!x[1] | !x[2] | x[4]) ;
endmodule

```

#### K-map implemented with a multiplexer

```verilog
module top_module (
    input c,
    input d,
    output reg [3:0] mux_in
); 
    always @(*)begin
        case( { c , d } )
            2'b00 : mux_in = 4'b0100 ;
            2'b01 : mux_in = 4'b0001 ;
            2'b11 : mux_in = 4'b1001 ;
            2'b10 : mux_in = 4'b0101 ;
            default : mux_in = 4'b0000 ;
        endcase
    end
endmodule

```

## Sequential Logic

### Latches and Flip-Flops

#### *DFFs and gates

concise solution :

```verilog
module top_module (
    input clk,
    input x,
    output z
); 
    reg [2:0] Q ;
    always @(posedge clk) begin
        Q[0] <= x ^ Q[0] ;
        Q[1] <= x & ~Q[1] ;
        Q[2] <= x | ~Q[2] ;
    end
    assign z = ~ (|Q) ;
endmodule

```

my solution

```verilog
module top_module (
    input clk,
    input x,
    output z
); 
    wire [2:0] Q ;
    DFF_ U0( .clk(clk) , .d(x ^ Q[0]) , .q(Q[0]) ) ;
    DFF_ U1( .clk(clk) , .d(x & ~Q[1]) , .q(Q[1]) ) ;
    DFF_ U2( .clk(clk) , .d(x | ~Q[2]) , .q(Q[2]) ) ;
    assign z = ~ (| Q ) ;
endmodule

module DFF_(
    input clk , d ,
    output reg q
);
    always @(posedge clk) begin
       q <= d ; 
    end
endmodule
```

#### *Detect an edge 

```verilog
module top_module (
    input clk,
    input [7:0] in,
    output reg [7:0] pedge
);
    reg [7:0] last_in ;
    always @(posedge clk) begin
       last_in <= in ;
       pedge <= ~last_in & in ;
    end
endmodule
```

#### *Detect both edges

```verilog
module top_module (
    input clk,
    input [7:0] in,
    output [7:0] anyedge
);
    reg [7:0] last_in ;
    always @(posedge clk) begin
       last_in <= in ;
       anyedge <= last_in ^ in ;
    end
    
endmodule
```

#### *Edge capture register

==Tips== : After clk's change from 1 to 0 , register q = 1 . And then , q is still `1`  until `reset = 1` ; 

```verilog
module top_module (
    input clk,
    input reset,
    input [31:0] in,
    output reg [31:0] out
);
    integer i ;
    reg [31:0] last_in ;
    always @(posedge clk) begin
       last_in <= in ;
        if(reset) out <= 0 ;
        else begin
            for(i = 0 ; i < 32 ; i++)
                out[i] <= last_in[i] & ~in[i] ? 1 : out[i] ;
        end
    end
endmodule
```

==My error solution==

```verilog
module top_module (
    input clk,
    input reset,
    input [31:0] in,
    output reg [31:0] out
);
    integer i ;
    reg [31:0] last_in ;
    always @(posedge clk) begin
       last_in <= in ;
        if(reset) out <= 0 ;
        else begin
            ////////////////////////
            out <= last_in & ~in ;
            ///////////////////////
        end
    end
endmodule
```

#### *Dual-edge triggered filp-flop

```verilog
module top_module (
    input clk,
    input d,
    output q
);
    reg q0 , q1 ;
    always @(posedge clk) begin
       q0 <= d ; 
    end
    always @(negedge clk) begin
       q1 <= d ;
    end
    assign q = clk ? q0 : q1 ;
endmodule
```

### 

### Counters

#### *Counter 1-12

```verilog
module top_module (
    input clk,
    input reset,
    input enable,
    output [3:0] Q,
    output c_enable,
    output c_load,
    output [3:0] c_d
); //

    count4 the_counter (clk, c_enable, c_load, c_d , Q);
    assign c_enable = enable ;
    assign c_load = (Q == 12 && enable) || reset ? 1 : 0 ;
    assign c_d = 4'b0001 ;

endmodule
```

**Count4.v **

```verilog
module count4(
    input clk , 
    input enable ,
    input load ,
    input [3:0] d ,
    output reg [3:0] Q
);
    always @(posedge clk) begin
        if(load)    Q <= d ;
        else if(enable) Q <= Q + 1 ;
    end
endmodule
```

#### Counter 1000

```verilog
//make sure OneHertz turn into 1 once time every 1000 clk.即分频器，1000Hz转1Hz
module top_module (
    input clk,
    input reset,
    output OneHertz,
    output [2:0] c_enable
); //
    wire [11:0] Q ;
    assign {OneHertz , c_enable} = { (Q[11:0] == 12'h999) , (Q[7:0] == 8'h99) ,(Q[3:0] == 4'h9) , 1'b1 } ; 
    bcdcount counter0 (clk, reset, c_enable[0] , Q[3:0]);
    bcdcount counter1 (clk, reset, c_enable[1] , Q[7:4]);
    bcdcount counter2 (clk, reset, c_enable[2] , Q[11:8]);
endmodule

```

#### 4-digit decimal counter

```verilog
module top_module (
    input clk,
    input reset,   // Synchronous active-high reset
    output [3:1] ena,
    output reg [15:0] q);
	
    assign ena = {  (q[11:0] == 12'h999) , (q[7:0] == 8'h99) , (q[3:0] == 4'h9) } ;
    always @(posedge clk) begin
        if(reset) q <= 0 ;
        else begin
            q[3:0] <= (q[3:0] == 4'h9) ? 0 : (q[3:0] + 1) ;
            if(ena[1])	q[7:4] <= (q[7:4] == 4'h9) ? 0 : (q[7:4] + 1 ) ;
            if(ena[2])	q[11:8] <= (q[11:8] == 4'h9) ? 0 : (q[11:8] + 1 ) ;
            if(ena[3])	q[15:12] <= (q[15:12] == 4'h9) ? 0 : (q[15:12] + 1 ) ;
        end
    end
    
endmodule 
```

```verilog
//another solution , Init module
module top_module (
    input clk,
    input reset,   // Synchronous active-high reset
    output [3:1] ena,
    output reg [15:0] q);

wire en0 ;
    assign {ena , en0} = { q[11:0] == 12'h999 , q[7:0] == 8'h99 , q[3:0] == 4'h9, 1'b1} ;
    counter10 U0(clk , reset , en0    , q[3:0]) ;
    counter10 U1(clk , reset , ena[1] , q[7:4]) ;
    counter10 U2(clk , reset , ena[2] , q[11:8]) ;
    counter10 U3(clk , reset , ena[3] , q[15:12]) ;
endmodule

module counter10(
    input clk ,
    input reset,
    input ena ,
    output [3:0] out 
);
    //assign reset = (out == 9) ;
    always@(posedge clk) begin
        if(reset)	out <= 0 ;
        else if(ena) out <= (out == 9) ? 0 : out + 1 ;
    end
endmodule
```

#### 12-hour clock

```verilog
module top_module(
    input clk ,
    input reset ,
    input ena ,
    output reg pm ,
    output reg [7:0] hh,
    output reg [7:0] mm,
    output reg [7:0] ss
);
    wire [1:0] carry ;
    counter60 ss0(clk , reset , ena , carry[0] , ss[7:0]) ;
    counter60 mm0(clk , reset , carry[0] , carry[1] , mm[7:0]) ;
    counter12 hh0(clk , reset , carry[1] && carry[0] , pm       , hh[7:0]) ;
    
endmodule

module counter60(
    input clk ,
    input reset ,
    input en ,
    output carry ,//carry = 1 when Q = 8'h59 , which means carry out .
    output reg [7:0] Q
);
    assign carry = (Q == 8'h59) ;
    always @(posedge clk)   begin
        if(reset)   Q <= 0 ;
        else if(en) begin
            Q[3:0] <= (Q[3:0] == 4'h9) ? 0 : Q[3:0] + 1 ;
            if(Q[3:0] == 4'h9)  Q[7:4] <= (Q[7:4] == 4'h5) ? 0 : Q[7:4] + 1 ;
        end
    end
endmodule

module counter12(
    input clk ,
    input reset ,
    input en ,
    output reg pm ,
    output reg [7:0] Q  
);
    always @(posedge clk)   begin
        if(reset)   Q <= 8'b0001_0010 ;
        else if(en) begin
            case(Q[3:0]) 
                4'b1001 : Q[7:0] <= 8'b0001_0000 ;
                4'b0010 : begin
                    if(Q[7:4] == 4'h1)  Q <= 8'h01 ;
                    else    Q[3:0] <= Q[3:0] + 1 ;
                end
                default : Q[3:0] <= Q[3:0] + 1 ;
            endcase
        end
    end

    always @(posedge clk)   begin
        if(reset)   pm <= 0 ;
        else if(en && Q[7:0] == 8'h11)  pm <= ~pm ;
        else pm <= pm ;
    end

endmodule
```







## Some worthwhile retry practice ;

* [Detect an edge](https://hdlbits.01xz.net/wiki/edgedetect)
* [Detect both edges](https://hdlbits.01xz.net/wiki/edgedetect2)
* [Edge capture register](https://hdlbits.01xz.net/wiki/edgecapture)
* [Dual-edge triggered flip-flop](https://hdlbits.01xz.net/wiki/dualedge)
* [Slow decade counter](https://hdlbits.01xz.net/wiki/Countslow)
* [Counter 1-12](https://hdlbits.01xz.net/wiki/Exams/ece241_2014_q7a)
* [Counter 1000](https://hdlbits.01xz.net/wiki/Exams/ece241_2014_q7b)
* [4-digit decimal counter](https://hdlbits.01xz.net/wiki/Countbcd)
* [12-hour clock](https://hdlbits.01xz.net/wiki/Count_clock)
* [3-input LUT](https://hdlbits.01xz.net/wiki/Exams/ece241_2013_q12)