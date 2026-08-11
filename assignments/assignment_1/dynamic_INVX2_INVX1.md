# Spice File
``` spice
* Skywater PDK Simple Inverter Testbench
* AS = AD = W * (2 * L)
* PS = PD = 2 * (W + 2 * L)

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.param L = 0.15
.param Wn = 0.84
.param Wp = 2.73

XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8
+ l={L}
+ w={Wp}
+ as={Wp*2*L}
+ ad={Wp*2*L}
+ ps={2*(Wp+2*L)}
+ pd={2*(Wp+2*L)}

XM3 n1 vout vdd vdd sky130_fd_pr__pfet_01v8
+ l=0.15
+ w=1.365
+ as=0.4095
+ ad=0.4095
+ ps=3.33
+ pd=3.33

XM2 vout vin 0 0 sky130_fd_pr__nfet_01v8
+ l=0.15
+ w=0.84
+ as=0.252
+ ad=0.252
+ ps=2.28
+ pd=2.28

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
``` bash
  Measurements for Transient Analysis

tr                  =  1.14564e-11 targ=  5.52058e-10 trig=  5.40601e-10
tf                  =  1.22272e-11 targ=  3.50162e-11 trig=  2.27891e-11
tplh                =  1.49270e-11 targ=  5.44927e-10 trig=  5.30000e-10
tphl                =  1.83488e-11 targ=  2.83488e-11 trig=  1.00000e-11
tp                  =  1.66379e-11
```
<img width="732" height="576" alt="image" src="https://github.com/user-attachments/assets/f80e79ba-a3b6-4674-ba39-a1d30ffb669c" />
