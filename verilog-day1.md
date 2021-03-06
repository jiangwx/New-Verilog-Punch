---
title: Verilog没有葵花宝典——day1（进制与编码）
date: 2019-04-22 20:57:17
tags: [Verilog HDL,学习笔记]
published: true
hideInList: false
feature: https://i.loli.net/2019/04/25/5cc1d354629e4.jpg
---
## 题目
>1. bit, byte, word, dword, qword的区别。
>2. 什么是原码，反码，补码，符号-数值码。以8bit为例，给出各自表示的数值范围。
>3. 十进制转换为二进制编码： 127， （-127），127.375，（-127.375）
>4. 设计BCD译码器，输入0~9。采用verilog描述并画出门级电路图。
>5. 异步FIFO深度为17，如何设计地址格雷码？

<!-- more -->

- [题目](#题目)
	- [第一题](#第一题)
	- [第二题](#第二题)
	- [第三题](#第三题)
	- [第四题](#第四题)
		- [verilog描述](#verilog描述)
		- [思路及电路图](#思路及电路图)
	- [第五题（待后续研究）](#第五题待后续研究)
		- [只能偶数，深度为18（5bit）](#只能偶数深度为185bit)
		- [也有文档说17+17的（6bit）](#也有文档说1717的6bit)

### 第一题
bit：位，二进制中的一位，是计算机存储信息的最小单位；

byte：字节，1byte=8bit；

word：字，1word=2byte=16bit；

dword：双字，double word，1dword=2word=4byte=32bit;

qword：四字，quad-word，1qword=4word=8byte=64bit。

### 第二题

这个题目以前做过，这里再复习一遍。

**原码**：带符号数的符号数值码表示，又称作原码，用二进制数位串的最高有效位（MSB）作为符号位，0表示正号（Plus），1表示负号（Minus），其余较低位表示数的绝对值（数值）（Magnitude）。以8bit为例，最大——0111_1111；最小——1111_1111，范围-127~+127。

**反码**：正数的反码是它本身，负数的反码是保留符号位，其它位按位取反。范围与原码相同，以8bit为例是-127~+127。

**补码**：正数的补码是它本身，负数的补码是保留符号位，其它位按位取反再加1。以4bit为例，表示范围为-128~+127。

**符号-数值码**：sign magnitude，有的教材认为与原码一样；有的教材认为符号-数值码相较于原码没有负0。范围-127~+127。

### 第三题

仍然是复习。

整数部分最基础的就是“除2取余法”，小数部分是"乘2取整法"。

整数部分比较快捷的方法：127=64+32+16+8+4+2+1=$2^6+2^5+2^4+2^3+2^2+2^1+2^0$=$(1111111)_b$。

小数部分没有什么比较快的，就是基础法：$0.375\times2=0.75$取0；$0.75\times2=1.5$取1；$0.5\times2=1$取1。

所以可得

127 $\to$ 0111_1111

-127 $\to$ 1111_1111

127.375 $\to$ 0111_1111.011

-127.375 $\to$ 1111_1111.011

### 第四题

#### verilog描述
```v
module BCD_Decoder(
	input		[3:0]	A,
	//output	reg	[9:0]	Y_L
	output		[9:0]	Y_L
);

always @ (*)
	case(A)
		4'd0:Y_L=10'b11_1111_1110;
		4'd1:Y_L=10'b11_1111_1101;
		4'd2:Y_L=10'b11_1111_1011;
		4'd3:Y_L=10'b11_1111_0111;
		4'd4:Y_L=10'b11_1110_1111;
		4'd5:Y_L=10'b11_1101_1111;
		4'd6:Y_L=10'b11_1011_1111;
		4'd7:Y_L=10'b11_0111_1111;
		4'd8:Y_L=10'b10_1111_1111;
		4'd9:Y_L=10'b01_1111_1111;
		default:Y_L=10'b11_1111_1111;
	endcase
/*
assign Y_L[0] = ~(~A[3] & ~A[2] & ~A[1] & ~A[0]);
assign Y_L[1] = ~(~A[3] & ~A[2] & ~A[1] & A[0]);
assign Y_L[2] = ~(~A[3] & ~A[2] & A[1]  & ~A[0]);
assign Y_L[3] = ~(~A[3] & ~A[2] & A[1]  & A[0]);
assign Y_L[4] = ~(~A[3] & A[2]  & ~A[1] & ~A[0]);
assign Y_L[5] = ~(~A[3] & A[2]  & ~A[1] & A[0]);
assign Y_L[6] = ~(~A[3] & A[2]  & A[1]  & ~A[0]);
assign Y_L[7] = ~(~A[3] & A[2]  & A[1]  & A[0]);
assign Y_L[8] = ~(A[3]  & ~A[2] & ~A[1] & ~A[0]);
assign Y_L[9] = ~(A[3]  & ~A[2] & ~A[1] & A[0]);
*/
endmodule
```

#### 思路及电路图
<figure class="third">
    <img src="https://i.loli.net/2019/04/25/5cc1cf28606f4.png" alt="BCD译码器功能表" title="BCD译码器功能表">
    <img src="https://i.loli.net/2019/04/25/5cc1cf9069464.jpg" alt="门级电路图" title="门级电路图">
    <img src="https://i.loli.net/2019/04/25/5cc1cf4c6a932.png" alt="Vivado综合的门级电路图" title="Vivado综合的门级电路图">
</figure>

### 第五题（待后续研究）

#### 只能偶数，深度为18（5bit）

00100 $\to$ 
01100 $\to$ 
01101 $\to$ 
01111 $\to$ 
01110 $\to$ 
01010 $\to$ 
01011 $\to$ 
01001 $\to$ 
01000 $\to$ 
11000 $\to$ 
11001 $\to$ 
11011 $\to$ 
11010 $\to$ 
11110 $\to$ 
11111 $\to$ 
11101 $\to$ 
11100 $\to$ 
10100 $\to$ 

#### 也有文档说17+17的（6bit）

001000 $\to$ 
011000 $\to$ 
011001 $\to$ 
011011 $\to$ 
011010 $\to$ 
011110 $\to$ 
011111 $\to$ 
011101 $\to$ 
011100 $\to$ 
010100 $\to$ 
010101 $\to$ 
010111 $\to$ 
010110 $\to$ 
010010 $\to$ 
010011 $\to$ 
010001 $\to$ 
010000 $\to$ 
110000 $\to$ 
110001 $\to$ 
110011 $\to$ 
110010 $\to$ 
110110 $\to$ 
110111 $\to$ 
110101 $\to$ 
110100 $\to$ 
111100 $\to$ 
111101 $\to$ 
111111 $\to$ 
111110 $\to$ 
111010 $\to$ 
111011 $\to$ 
111001 $\to$ 
111000 $\to$ 
101000 $\to$ 
