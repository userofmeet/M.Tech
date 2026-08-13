# SPICE File
``` spice
* Skywater PDK Simple Inverter Testbench
* AS = AD = W * (2 * L)
* PS = PD = 2 * (W + 2 * L)

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.param L = 0.15
.param Wn = 0.42
.param Wp = 1.3378974
* wp = 3.18547 * wn

XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8
+ l={L}
+ w={Wp}
+ as={Wp*2*L}
+ ad={Wp*2*L}
+ ps={2*(Wp+2*L)}
+ pd={2*(Wp+2*L)}

XM3 n1 vout vdd vdd sky130_fd_pr__pfet_01v8
+ l={L}
+ w={Wp}
+ as={Wp*2*L}
+ ad={Wp*2*L}
+ ps={2*(Wp+2*L)}
+ pd={2*(Wp+2*L)}

XM2 vout vin 0 0 sky130_fd_pr__nfet_01v8
+ l=0.15
+ w=0.42
+ as=0.126
+ ad=0.126
+ ps=1.44
+ pd=1.44

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

# Results
## Terminal output
``` bash
  Measurements for Transient Analysis

tr                  =  1.79216e-11 targ=  5.60296e-10 trig=  5.42374e-10
tf                  =  1.93651e-11 targ=  4.43276e-11 trig=  2.49625e-11
tplh                =  1.92308e-11 targ=  5.49231e-10 trig=  5.30000e-10
tphl                =  2.35977e-11 targ=  3.35977e-11 trig=  1.00000e-11
tp                  =  2.14143e-11
```
## Transient output
<img width="732" height="576" alt="image" src="https://github.com/user-attachments/assets/3e19ecc2-7fef-41df-b81c-4c1a54e1220f" />
