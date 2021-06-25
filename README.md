# Static-CMOS-Inverter

The contents are organized in the below metioned sequence.

1. Inverter Schematic
2. Test Schematic 
3. Transient Analysis 
4. DC Analysis: VTC Curve
5. Netlist Generation
6. Layout
7. Swap NMOS and PMOS: Transient Analysis, VTC

Software of Use: Cadence Virtuoso

# Background
A simple inverter inverts the logically high input to low output and vice versa. It is of immense significance in clock generation, generation of delays, memories to store the data, improving the circuit's noise immunity, and much more. It is a simple two transistor device. For input 0, PMOS turns ON, and NMOS stays OFF. This charges the output capacitance, thus making output logic HIGH. Whereas for input 1, PMOS is OFF, and NMOS is ON. The charged capacitance now discharges, making output logic LOW. On a periodic application of pulse, an inverted pulse is obtained at the output. The in-depth analysis of the output on logic 0 to 1 at the input is summed up by its Voltage Transfer Characteristics (VTC).

# 1. Inverter Schematic

<img width="327" alt="Inverter Schematic" src="https://user-images.githubusercontent.com/59061427/123095880-277c6980-d44c-11eb-95e7-2a85430fdf51.PNG">

# 2. Test Schematic 

<img width="724" alt="Test Schematic" src="https://user-images.githubusercontent.com/59061427/123097233-8db5bc00-d44d-11eb-96cb-1bda38fa9550.PNG">

# 3. Transient Analysis 

<img width="824" alt="Output Waveform" src="https://user-images.githubusercontent.com/59061427/123097318-ab832100-d44d-11eb-91e8-2a85e8ddade3.PNG">

# 4. DC Analysis: VTC Curve

A) Electrically symmetric inverter (Wp=4um; Wn=2um)

<img width="760" alt="VTC Curve wp=4u wn=2u" src="https://user-images.githubusercontent.com/59061427/123097452-c81f5900-d44d-11eb-8931-64a1b50ec908.PNG">

B) Inverter with stronger PMOS (Wp=8um; Wn=2um)

<img width="759" alt="VTC for Stronger PMOS wp=8u wn=2u" src="https://user-images.githubusercontent.com/59061427/123097515-d8cfcf00-d44d-11eb-8c09-a331b6831b3f.PNG">

C) Inverter with stronger NMOS (Wp=2um; Wn=2um)

<img width="760" alt="VTC for Stronger NMOS wp=2u wn=2u" src="https://user-images.githubusercontent.com/59061427/123097658-f866f780-d44d-11eb-968a-53687020be36.PNG">

# 5. Netlist Generation

// Generated for: spectre
simulator lang=spectre
global 0 vdd!
parameters Vbias
include "analog_labs/models/spectre/gpdk.scs" section=stat

subckt Inverter IN OUT
    NM0 (OUT IN 0 0) nmos1 w=(2u) l=180n as=1.2p ad=1.2p ps=5.2u pd=5.2u \
        m=(1)*(1)
    PM0 (OUT IN vdd! vdd!) pmos1 w=(4u) l=180n as=2.4p ad=2.4p ps=9.2u \
        pd=9.2u m=(1)*(1)
ends Inverter
// End of subcircuit definition

I0 (net3 OUT) Inverter
V1 (vdd! 0) vsource dc=5 type=dc
V0 (net3 0) vsource dc=Vbias type=dc
C0 (OUT 0) capacitor c=10f
simulatorOptions options reltol=1e-3 vabstol=1e-6 iabstol=1e-12 temp=27 \ tnom=27 scalem=1.0 scale=1.0 gmin=1e-12 rforce=1 maxnotes=5 maxwarns=5 \ digits=5 cols=80 pivrel=1e-3 sensfile="../psf/sens.output" \ checklimitdest=psf 
modelParameter info what=models where=rawfile
element info what=inst where=rawfile
outputParameter info what=output where=rawfile
designParamVals info what=parameters where=rawfile
primitives info what=primitives where=rawfile
subckts info what=subckts  where=rawfile
saveOptions options save=allpub

# 6. Layout
(Work In Progess with UMC Library; DRC, LVS Checks)

# 7. Swap NMOS and PMOS: Transient Analysis, VTC

The circuit is modified by swapping NMOS and PMOS with each other. The expected behaviour of the circuit is a buffer with weak output logics. 

Input and Output waveform for transient analysis:

<img width="818" alt="Output Waveform" src="https://user-images.githubusercontent.com/59061427/123412772-9d5d0e00-d5cf-11eb-84e6-1a0b863aa091.PNG">

Notably, we can observe reduction in the voltage swing as expected.

VTC - Plot between input and output:

<img width="755" alt="VTC" src="https://user-images.githubusercontent.com/59061427/123412816-a8b03980-d5cf-11eb-9a7c-c137d22600d8.PNG">




