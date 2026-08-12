# SPICE File 
``` spice
* Skywater PDK Simple Inverter Testbench
* AS = AD = W * (2 * L)
* PS = PD = 2 * (W + 2 * L)

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.param L = 0.15
.param Wn = 0.84
.param Wp = 2.6754

XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8
+ l={L}
+ w={Wp}
+ as={Wp*2*L}
+ ad={Wp*2*L}
+ ps={2*(Wp+2*L)}
+ pd={2*(Wp+2*L)}

XM3 n1 vout vdd vdd sky130_fd_pr__pfet_01v8
+ l=0.15
+ w=1.3377
+ as=0.40131
+ ad=0.40131
+ ps=3.2754
+ pd=3.2754

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
## Terminal output
``` bash
vm                  =  8.96224e-01
vil                 =  7.12925e-01
vih                 =  1.08597e+00
vm = 8.962244e-01
vil = 7.129251e-01
vih = 1.085974e+00
nml = 7.129251e-01
nmh = 7.140260e-01
```
## Voltage transfer curve 
<img width="732" height="576" alt="image" src="https://github.com/user-attachments/assets/d16dfc6a-c727-4fa2-a7d3-f4b68e7b7000" />
