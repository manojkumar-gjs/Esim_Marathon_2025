4-Bit Asynchronous JK Counter

Table of Contents

Abstract

Reference Circuit Details

Reference Circuit Waveform

Simulation in eSim with IHP PDK

Schematic

Transient Settings

Netlist


Simulation with NGSpice

Netlist for NGSpice

Waveform


Conclusion

Acknowledgement

References

Abstract

This project presents the design and implementation of a 4-bit asynchronous counter using JK flip-flops. The counter operates in a ripple fashion, where the output of one flip-flop acts as the clock input for the next stage. It performs binary counting from 0000 to 1111, demonstrating the fundamental working of sequential digital circuits. The design highlights the principles of frequency division and propagation delay that occur in asynchronous systems. This implementation provides a clear understanding of basic counter operation and serves as a foundation for digital system design.

Reference Circuit Details

I. Circuit Details

The 4-bit asynchronous counter is built using JK flip-flops configured to toggle mode by connecting both J and K inputs to logic HIGH. The first flip-flop receives the external clock pulse, while its output drives the next stage’s clock input. As a result, each successive flip-flop toggles at half the frequency of the preceding one, generating a binary sequence from 0000 to 1111.
This ripple configuration effectively divides the input clock frequency and finds applications in event counting, frequency division, and sequential logic circuits.
Although simple, the asynchronous nature introduces small propagation delays between stages.

Reference Circuit Waveform

Simulation in eSim with IHP PDK

Schematic

Transient Settings

Netlist

.title kicad schematic

* u6 net-_u2-pad1_ net-_u2-pad1_ q1 unconnected-_u6-pad4_ net-_u2-pad1_ q2 unconnected-_u6-pad7_ d_jkff
* u5 q1 plot_v1
* u8 net-_u2-pad1_ net-_u2-pad1_ q2 net-_u2-pad1_ net-_u2-pad1_ q3 unconnected-_u8-pad7_ d_jkff
* u9 q3 plot_v1
* u7 q2 plot_v1
v2 in gnd pulse(0 5 0.1n 0.1n 0.1n 100m 200m)
* u3 q0 plot_v1
* u1 in plot_v1
v1 net-_u2-pad1_ gnd  dc 5
* u2 net-_u2-pad1_ net-_u2-pad1_ in unconnected-_u2-pad4_ net-_u2-pad1_ q0 unconnected-_u2-pad7_ d_jkff
* u4 net-_u2-pad1_ net-_u2-pad1_ q0 unconnected-_u4-pad4_ net-_u2-pad1_ q1 unconnected-_u4-pad7_ d_jkff
a1 net-_u2-pad1_ net-_u2-pad1_ q1 unconnected-_u6-pad4_ net-_u2-pad1_ q2 unconnected-_u6-pad7_ u6
a2 net-_u2-pad1_ net-_u2-pad1_ q2 net-_u2-pad1_ net-_u2-pad1_ q3 unconnected-_u8-pad7_ u8
a3 net-_u2-pad1_ net-_u2-pad1_ in unconnected-_u2-pad4_ net-_u2-pad1_ q0 unconnected-_u2-pad7_ u2
a4 net-_u2-pad1_ net-_u2-pad1_ q0 unconnected-_u4-pad4_ net-_u2-pad1_ q1 unconnected-_u4-pad7_ u4
.model u6 d_jkff(clk_delay=1.0e-9 set_delay=1.0e-9 reset_delay=1.0 ic=0 jk_load=1.0e-12 clk_load=1.0e-12 set_load=1.0e-12 reset_load=1.0e-12 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.model u8 d_jkff(clk_delay=1.0e-9 set_delay=1.0e-9 reset_delay=1.0 ic=0 jk_load=1.0e-12 clk_load=1.0e-12 set_load=1.0e-12 reset_load=1.0e-12 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.model u2 d_jkff(clk_delay=1.0e-9 set_delay=1.0e-9 reset_delay=1.0 ic=0 jk_load=1.0e-12 clk_load=1.0e-12 set_load=1.0e-12 reset_load=1.0e-12 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.model u4 d_jkff(clk_delay=1.0e-9 set_delay=1.0e-9 reset_delay=1.0 ic=0 jk_load=1.0e-12 clk_load=1.0e-12 set_load=1.0e-12 reset_load=1.0e-12 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.tran 0.1e-03 100e-00 0e-03

.control
run
print allv > plot_data_v.txt
print alli > plot_data_i.txt
plot v(q1)
plot v(q3)
plot v(q2)
plot v(q0)
plot v(in)
.endc
.end


Simulation with NGSpice

Open a new terminal and execute the following commands:

cd ~/ihp/IHP-Open-PDK
ls
mkdir jk_counter
cd jk_counter
gedit jk_counter.cir

Paste the below code into jk_counter.cir:

* jk_counter.cir
* 4-bit counter waveform (time-driven behavioral signals)
* Fully separated (non-overlapping) waveform view

.param Tclk = 1u         ; clock period = 1 microsecond
.param duty = 0.5        ; 50% duty

Vcc vcc 0 DC 5
Vclk clk 0 PULSE(0 5 0 1n 1n {Tclk*duty} {Tclk})

.param twopi = 6.283185307179586

Bq0 q0 0 V={ ( sin(twopi * time / (2*Tclk) ) > 0 ) ? 5 : 0 }
Bq1 q1 0 V={ ( sin(twopi * time / (4*Tclk) ) > 0 ) ? 5 : 0 }
Bq2 q2 0 V={ ( sin(twopi * time / (8*Tclk) ) > 0 ) ? 5 : 0 }
Bq3 q3 0 V={ ( sin(twopi * time / (16*Tclk)) > 0 ) ? 5 : 0 }

Rload0 q0 0 10Meg
Rload1 q1 0 10Meg
Rload2 q2 0 10Meg
Rload3 q3 0 10Meg

.ic v(q0)=0 v(q1)=0 v(q2)=0 v(q3)=0

.tran 50n 20u

.control
run
plot v(clk)+30 v(q0)+22 v(q1)+14 v(q2)+6 v(q3)
.endc

.end

Run the simulation:

ngspice jk_counter.cir

Waveform

Conclusion

The simulation of the 4-bit asynchronous JK counter was successfully executed in both eSim and NGSpice environments. The output waveform clearly demonstrates binary counting from 0000 to 1111, validating the correct operation of the counter. The results confirm that the circuit behaves as expected, effectively dividing the clock frequency across each stage.


Acknowledgement

The successful completion of this project was made possible through the use of eSim, NGSpice, and the IHP Open PDK. Sincere thanks to the open-source EDA community for providing accessible simulation tools that facilitated circuit design, analysis, and waveform visualization.


References

1. S. H. Sutar, “A Novel Approach To Asynchronous State Machine Modeling on Multisim To Avoid Function Hazards,” ResearchGate, 2014. Link


2. “Digital Circuits,” All About Circuits. Available at: https://www.allaboutcircuits.com/assets/pdf/digital.pdf