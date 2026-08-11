# Spice File
``` spice
* Skywater PDK Simple Inverter Testbench
* AS = AD = W * (2 * L)
* PS = PD = 2 * (W + 2 * L)

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.param L = 0.15
.param Wn = 0.42
.param Wp = 1.365

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

Vin vin 0 DC 0 
.dc Vin 0 1.8 0.1

.control
run

let gain = deriv(v(vout))

meas dc VM when v(vout)=v(vin)

meas dc VIL when gain=-1 cross=1
meas dc VIH when gain=-1 cross=2

let NML = VIL
let NMH = 1.8 - VIH

print VM
print VIL
print VIH
print NML
print NMH

plot v(vin) v(vout)
.endc
```

# Results
``` bash
vm                  =  9.03387e-01
vil                 =  7.15864e-01
vih                 =  1.08647e+00
vm = 9.033868e-01
vil = 7.158638e-01
vih = 1.086474e+00
nml = 7.158638e-01
nmh = 7.135260e-01
```
<img width="732" height="576" alt="image" src="https://github.com/user-attachments/assets/9ec09dd4-a1e8-41e6-baf3-06a14ac2b627" />
