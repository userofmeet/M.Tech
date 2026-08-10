## Spice File
``` spice
* Skywater PDK Simple Inverter Testbench
* AS = AD = W * (2 * L)
* PS = PD = 2 * (W + 2 * L)

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.param L = 0.15
.param Wn = 0.42
.param Wp = 1.2693

XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8
+ l={L}
+ w={Wp}
+ as={Wp*2*L}
+ ad={Wp*2*L}
+ ps={2*(Wp+2*L)}
+ pd={2*(Wp+2*L)}

XM2 vout vin 0 0 sky130_fd_pr__nfet_01v8
+ l={L}
+ w={Wn}
+ as={Wn*2*L}
+ ad={Wn*2*L}
+ ps={2*(Wn+2*L)}
+ pd={2*(Wn+2*L)}

XM3 n1 vout vdd vdd sky130_fd_pr__pfet_01v8
+ l=0.15
+ w=1.2693
+ as=0.38079
+ ad=0.38079
+ ps=3.1386
+ pd=3.1386

XM4 n1 vout 0 0 sky130_fd_pr__nfet_01v8
+ l=0.15
+ w=0.42
+ as=0.126
+ ad=0.126
+ ps=1.44
+ pd=1.44

VDD vdd 0 DC 1.8

Vin vin 0 PULSE(0 1.8 0 20p 20p 500p 1n)

.tran 1p 5n

* Rise time
.measure tran tr
+ TRIG v(vout) VAL=0.36 RISE=1
+ TARG v(vout) VAL=1.44 RISE=1

* Fall time
.measure tran tf
+ TRIG v(vout) VAL=1.44 FALL=1
+ TARG v(vout) VAL=0.36 FALL=1

* Propagation delays
.measure tran tplh
+ TRIG v(vin) VAL=0.9 FALL=1
+ TARG v(vout) VAL=0.9 RISE=1

.measure tran tphl
+ TRIG v(vin) VAL=0.9 RISE=1
+ TARG v(vout) VAL=0.9 FALL=1

.measure tran tp PARAM='(tphl+tplh)/2'

.control
run
plot v(vin) v(vout)
.endc
.end
```
## Results 
``` bash
Circuit: * skywater pdk simple inverter testbench

Doing analysis at TEMP = 27.000000 and TNOM = 27.000000

Using SPARSE 1.3 as Direct Linear Solver

Initial Transient Solution
--------------------------

Node                                   Voltage
----                                   -------
vout                                       1.8
vin                                          0
vdd                                        1.8
n1                                 1.17225e-07
vin#branch                                   0
vdd#branch                        -6.44593e-11

 Reference value :  4.91450e-09
No. of Data Rows : 5065

  Measurements for Transient Analysis

tr                  =  1.84482e-11 targ=  5.61076e-10 trig=  5.42628e-10
tf                  =  1.86186e-11 targ=  4.32237e-11 trig=  2.46052e-11
tplh                =  1.96882e-11 targ=  5.49688e-10 trig=  5.30000e-10
tphl                =  2.28976e-11 targ=  3.28976e-11 trig=  1.00000e-11
tp                  =  2.12929e-11

ngspice 1 -> 
```
<img width="732" height="576" alt="image" src="https://github.com/user-attachments/assets/f98bb3ca-8bed-4989-8822-67711d3daecf" />
