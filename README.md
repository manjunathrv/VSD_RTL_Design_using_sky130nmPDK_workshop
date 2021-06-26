# VSD_RTL_workshop


# Day 3 

## Contents 
1. Introduction to optimisation 
2. Lab Session - Combinational logic optimisation
3. Lab Session - Sequential logic optimisation

## 1. Introduction to optimisation : 
   Combinational logic optimisation is needed in a digital circuit design for the following main reasons, 
   - To minimise the area and power consumption.
   - Direct 
   - Reducing the number of gates by optimising the boolean logic using K-Map or 

### Constant propogation : 
Consider the following example of a combinational logic network in Figure 3.1a. 


The output is given by,

![\begin{align*}
Y &= \overline{{((AB)+C)}}
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AY+%26%3D+%5Coverline%7B%7B%28%28AB%29%2BC%29%7D%7D%0A%5Cend%7Balign%2A%7D%0A)

![\begin{align*}
If,     A &= 0 \\
         Y &=\overline{(0) + C}\\
         Y &=\overline{C}
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AIf%2C+++++A+%26%3D+0+%5C%5C%0A+++++++++Y+%26%3D%5Coverline%7B%280%29+%2B+C%7D%5C%5C%0A+++++++++Y+%26%3D%5Coverline%7BC%7D%0A%5Cend%7Balign%2A%7D%0A)

The combinational network reduces to a NOT gate after direct optimisation as shown in Figure 3.1b and Figure 3.1c.
The circuit implementation of the combinational network and the optimised NOT gate is shown in Figure 3.2


The number of transistor for the implementation after direct optimisation due to constant propogation reduces to 2, in comparison to 6 transistor required for the combinational network. 

### Boolean logic optimisation :

Consider the following boolean expression, 


The combinational logic network is shown in Figure 3.3. It consists of 3 MUX having select inputs c, b and a from left to right. The final output is obtained at the third MUX. 


The boolean logic optimisation at each MUX are done as follows, 
At MUX 1, 

![\begin{align*}
Y_1 &= AC + \bar{C}.0 \\
Y_1 &=AC
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AY_1+%26%3D+AC+%2B+%5Cbar%7BC%7D.0+%5C%5C%0AY_1+%26%3DAC%0A%5Cend%7Balign%2A%7D%0A)

At MUX 2, 

![\begin{align*}
Y_2 &= \bar{B}AC + BC \\
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AY_2+%26%3D+%5Cbar%7BB%7DAC+%2B+BC+%5C%5C%0A%5Cend%7Balign%2A%7D%0A)

At MUX 3, The final output is optimised as below, 

![\begin{align*}
Y_3 &= \bar{A}\bar{C} + A (BC + \bar{B}AC) \\
Y_3 &= \bar{A}\bar{C} +ABC + A\bar{B}C \\
Y_3 &= \bar{A}\bar{C} + AC(B + \bar{B}) \\
Y_3 &= \bar{A}\bar{C} + AC
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AY_3+%26%3D+%5Cbar%7BA%7D%5Cbar%7BC%7D+%2B+A+%28BC+%2B+%5Cbar%7BB%7DAC%29+%5C%5C%0AY_3+%26%3D+%5Cbar%7BA%7D%5Cbar%7BC%7D+%2BABC+%2B+A%5Cbar%7BB%7DC+%5C%5C%0AY_3+%26%3D+%5Cbar%7BA%7D%5Cbar%7BC%7D+%2B+AC%28B+%2B+%5Cbar%7BB%7D%29+%5C%5C%0AY_3+%26%3D+%5Cbar%7BA%7D%5Cbar%7BC%7D+%2B+AC%0A%5Cend%7Balign%2A%7D%0A)

Thus, the boolean expression implemented by a combinational network of 3 MUX are optimised to a NOR gate.
The synthesis tool takes into account these steps to provide an optimised combination network for reducing power and area. 

### Sequential logic optimisation :
The techniques for sequential logic optimisation are described below, 
- Sequential constant propogation (Basic) 
- State optimisation
- Retiming 
- Sequential logic cloning (In Floor plan aware) 

### Sequential constant optimisation: 
Consider the example below of a sequential logic with a D-Flop as shown below, 


The logical expression for 
RST = 1, Q = 0 
RST = 0, Q = 0 (since D = 0) 
Y = A0 = A + 0 = A + 1 

In this case it is seen that irrespective of the RST and Clk the output Y is always 1. 
Hence, 

Consider the same sequential with Reset signal replaced with a set signal as below, 

In the above case, the sequential logic cannot be optimised and the D-flop is needed for the logic network. 
Hence, in does not have a sequential constant as compared in example in Figure 3_2. 

Lab



# Day 4 

## Contents 
1. Verification of Gate level Synthesized netlist  
2. Lab Session - Combinational logic optimisation
3. Lab Session - Sequential logic optimisation

## 1. Verification of Gate level Synthesized (GLS) netlist : 
In the Gate level synthesis, the netlist obtained as the output after synthesis is used as the Design under test (DUT) 
The test bench developed for the RTL design code is used for also testing the logical functionality of the netlist. 
GLS is required to verify the logical correctness and ensuring timing specification are met after synthesis.  

The flow diagram of the GLS setup with iVerilog is shown in Figure 4.1


The input to the iverilog is the Netlist generated after synthesis, gate level verilog models and test bench. 
The output of iverilog is the value change dump. The output vcd file is loaded in the GTK waver to check the logical correctness. 
If the timing constraint is given in the 
In the next step, a comparison between a RTL design code and netlist is discussed. 
Consider an example of a combinational network as shown in Figure 4.2. 


The RTL verilog code for the above combinational network is shown below, 


The netlist obtained from the synthesis tool is shown below, 


The testing and comparison of the RTL verilog code and netlist code can help to find and debug any functionality errors.
This improves the quality of the synthesised netlist before the place and route process. 

The mismatch found during the simulations tests done with the synthesised netlist can due to the following reasons,
- Missing sensitivity list 
- Blocking versus non-blocking assignments 
- Non verilog standard coding. 

### Missing sensitivity list 
In the verilog simulator, one of the key parameters that the simulator takes into account is the presence of acitivity, If there is no acitvity in the inputs, the 
Consider the following verilog code, 


In this functionality of the MUX defined in the verilog code, the select input connects the input i0 and i1. 
The output waveform of the above functionality defined in verilog is shown in the 

The conditional statement defined by always (@sel) is only executed when sel is 0 or 1. 
However, always block is not executed 


This can be corrected by evaluating the function with the change of any signal .
The corrected verilog code is shown below, 


Thus the simulation will be corrected as below.




## 2. Lab 1 GLS Synthesis simulation mismatch : 
### Example 1
In this test, the following steps are done, 
- Input the RTL verilog code and testbench to iVerilog
- Output obtained is visualised in gtk and check the functionality
- Perform synthesis and generate netlist.
- Input the netlist, verilog models and testbenct to iVerilog
- Check the output waveform in gtk and compare with pre-synthesis RTL code.  

Verilog code of the RTL design having a 2x1 MUX in this example is shown below, 

```SystemVerilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```
#### 1. Check the functionality of the verilog code of the RTL Design
-  To check the RTL design with the testbench 
 <pre><code><strong>iverilog</strong> ternary_operator_mux.v tb_ternary_operator.v</code></pre>   
-   To create a vcd dump file 
 <pre><code>./a.out</code></pre>
-   Open gtk wave
 <pre><code><strong>gtkwave</strong> tb_ternary_operator_mux.vcd</code></pre>
![Day_4_Commands](Images/Day_4_Lab_GLS_commands.PNG)

The output obtained for the MUX functionality with verilog code in GTKwave is shown below,

![Day_4_Commands](Images/Day_4_Lab_GLS_1.png)

The logical functionality of the 2x1 MUX is found to be correct. 
- Select input sel = 1, Output Y = i0 
- Select input sel = 0, Output Y = i1

#### 2. Perform Synthesis in Yosys
-  Invoke yosys 
 <pre><code><strong>yosys</strong> </code></pre>   
-  Read liberty file 
 <pre><code><strong>read_liberty</strong> -lib my_lib\lib\sky130_fd_sc_hd__tt_025C_1v80.lib </code></pre>
-  Read Verilog File 
 <pre><code><strong>read_verilog</strong> ternary_operator_mux.v</code></pre>
-  Perform synthesis 
 <pre><code><strong>synth</strong> -top ternary_operator_mux</code></pre>

![Day_4_Commands](Images/Day_4_Lab_GLS_2.PNG)

From the above output log after synthesis, the obtained inferred cell is a MUX

#### 3. Generate netlist and save the verilog file after synthesis 
-  Generate netlist 
 <pre><code><strong>abc</strong> -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib</code></pre>   
-  write verilog file
 <pre><code><strong>write_verilog</strong> -noattr ternary_operator_mux_netlist.v </code></pre>
-  Show the schematic
 <pre><code><strong>show</strong></code></pre>
 
![Day_4_Commands](Images/Day_4_Lab_GLS_3.PNG)

From the graphical representation of the netlist the obtained cell is mux2_1

#### 4. Use the synthesised netlist, library verilog models, testbench and check the functionality in iverilog 
-  To check the functionality of the synthesised netlist  
 <pre><code><strong>iverilog</strong> my_lib\verilog_model\primitives.v my_lib\verilog_model\sky130_fd_sc_hd.v ternary_operator_mux_netlist.v tb_ternary_operator.v</code></pre>  -   To create a vcd dump file 
 <pre><code>./a.out</code></pre>
-   Open gtk wave
 <pre><code><strong>gtkwave</strong> tb_ternary_operator_mux.vcd</code></pre>
 
 ![Day_4_Commands](Images/Day_4_Lab_GLS_3.PNG)
 
 The logical functionality of the verilog of the netlist of 2x1 MUX is found to be correct and similar to pre-synthesis functionality. 
- Select input sel = 1, Output Y = i0 
- Select input sel = 0, Output Y = i1
 
 ### Example 2
 In this test, an example of a verilog code for a 2x1 mux is checked for synthesis simulation mismatch. 
 The steps followed is similar to Example 1 as described below, 
- Input the RTL verilog code and testbench to iVerilog
- Output obtained is visualised in gtk and check the functionality
- Perform synthesis and generate netlist.
- Input the netlist, verilog models and testbenct to iVerilog
- Check the output waveform in gtk and compare with pre-synthesis RTL code. 

Verilog code of the RTL design having a 2x1 MUX in Example 2 is shown below, 

```SystemVerilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```
#### 1. Check the functionality of the verilog code of the RTL Design
 <pre><code><strong>iverilog</strong> ternary_operator_mux.v tb_ternary_operator.v</code></pre>   
 <pre><code>./a.out</code></pre>
 <pre><code><strong>gtkwave</strong> tb_ternary_operator_mux.vcd</code></pre>
![Day_4_Commands](Images/Day_4_Lab_GLS_commands.PNG)

The output obtained for the MUX functionality with verilog code in GTKwave is shown below,

![Day_4_Commands](Images/Day_4_Lab_GLS_1.png)

The logical functionality of the 2x1 MUX is found to be correct. 
- Select input sel = 1, Output Y = i0 
- Select input sel = 0, Output Y = i1

#### 2. Perform Synthesis in Yosys
 <pre><code><strong>yosys</strong> </code></pre>   
 <pre><code><strong>read_liberty</strong> -lib my_lib\lib\sky130_fd_sc_hd__tt_025C_1v80.lib </code></pre>
 <pre><code><strong>read_verilog</strong> ternary_operator_mux.v</code></pre>
 <pre><code><strong>synth</strong> -top ternary_operator_mux</code></pre>

![Day_4_Commands](Images/Day_4_Lab_GLS_2.PNG)

From the above output log after synthesis, the obtained inferred cell is a MUX

#### 3. Generate netlist and save the verilog file after synthesis 
 <pre><code><strong>abc</strong> -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib</code></pre>   
 <pre><code><strong>write_verilog</strong> -noattr ternary_operator_mux_netlist.v </code></pre>
 <pre><code><strong>show</strong></code></pre>
 
![Day_4_Commands](Images/Day_4_Lab_GLS_3.PNG)

From the graphical representation of the netlist the obtained cell is mux2_1

#### 4. Use the synthesised netlist, library verilog models, testbench and check the functionality in iverilog  
 <pre><code><strong>iverilog</strong> my_lib\verilog_model\primitives.v my_lib\verilog_model\sky130_fd_sc_hd.v ternary_operator_mux_netlist.v tb_ternary_operator.v</code></pre> 
 <pre><code>./a.out</code></pre>
 <pre><code><strong>gtkwave</strong> tb_ternary_operator_mux.vcd</code></pre>
 
 ![Day_4_Commands](Images/Day_4_Lab_GLS_3.PNG)
 
 The logical functionality of the verilog of the netlist of 2x1 MUX is found to be correct and similar to pre-synthesis functionality. 
- Select input sel = 1, Output Y = i0 
- Select input sel = 0, Output Y = i1

 ## 3. Lab 2 GLS Synthesis simulation mismatch : 
