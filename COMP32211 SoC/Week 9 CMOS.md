### Silicon Doping

Pure silicon (4-valency) is covalently bonded and by itself does not conduct electrons.

However silicon can be doped with typically boron (3-valency) or phosphorus (5-valency) which leaves an *electron hole* or spare electron respectfully to carry charges. Electrons can move throughout the material to effectively allow the hole/electron to 'move' and pass charge. (The position doesn't actually move, but they allow electrons to pass through them).

![](misc/Pasted%20image%2020240110195332.png)

#### Doping Levels
Increasing the amount of doping material in silicon increases the number of charge carriers; This reduces the threshold voltage to allow current flow & has a faster switching speed, aswell; however will take longer to switch off and is more prone to leaking charge (power dissipation). High-threshold transistors are better used for low-power applications.

## CMOS

Complementary metal–oxide–semiconductor (CMOS) uses complementary pairs of p-type & n-type *metal–oxide–semiconductor field-effect transistors (MOSFETs)* to produce logic gates.
Our circuits are designed with complementary use of PMOS and NMOS transistors. PMOS are good at pulling outputs high & NMOS good at pulling low.

CMOS is now commonly used but before the 1980's it was common to use NMOS-only logic due to faster switching speeds.

NMOS/PMOS diagram:

![](misc/Pasted%20image%2020240110172422.png)

We have 4 connections:

- Source, $V_s$ (of electron carriers)
- Drain, $V_d$ (of electron carriers)

Note that source & drain are the same as the MOSFET is symmetrical on silicon.

- Gate, $V_g$ - potential voltage controls current flow across the MOSFET.
- Substrate (optional): Used to control the *body effect* - we can change the threshold voltage.

- $V_{dd}$ & $V_{ss}$ denote source and drain power supply voltage. Typically $V_{ss}$ is connected to ground / 0Vand $V_{dd}$ to power source ($V_{cc}$).

Diagram of NMOS/PMOS silicon:

![](misc/Pasted%20image%2020240110175007.png)

- Gate Oxide known as a *dielectric*; It is an electrical insulator but is also capable of transmitting the electric field from the gate to the bulk to attract charge carriers - known as *polarisation*; negative/positive charges in the material orient towards the positive/negative terminal. Dielectrics are a key component in capacitors as transmitting the electric field is what keeps the opposite polarity charges held by each other in the plates.
- The top of the CMOS is coated in an insulator (silicon dioxide)

#### Depletion Region
TODO
![](misc/Pasted%20image%2020240112131322.png)

When below the threshold voltage opposite charge , creates a barrier of very high resistance. When we apply our gate voltage the depletion region shrinks, lowering the resistance, allowing current to flow. (However the depletion region never fully disappears).


A MOSFET can be in *enhancement or depletion mode*, depending on the following voltage relationships:

![](misc/Pasted%20image%2020240110173044.png)

The threshold voltage is typically $\pm 3V$. 

- Enhancement mode: Transistor is off (infinite resistance) at zero voltage across gate-source.
- Depletion mode: Transistor is on (near-zero resistance) at zero voltage across gate-source.

CMOS uses exclusively enhancement mode logic.

Effectively in our CMOS logic you will see PMOS transistors conduct at logical low and NMOS conduct at logical high.


- CMOS diode junctions: These implicitly exist anywhere we have a boundary between p- & n-type semiconductors.

- Gate threshold: Threshold voltage where gate opens and can pass current.

- Logic levels - voltage ranges for minimum/maximum input/output voltages for a low/high signal. In between the ranges is when it is *high-Z/floating/tri-state* (equivalent to verilog Z signal).


![](misc/Pasted%20image%2020240110234236.png)

- By using both PMOS and NMOS we can guarantee that CMOS will always have a low resistance s.t. our logic signal are always strongly driven; therefore low power dissipation (/voltage drop) across the CMOS circuitry; CMOS only has high power usage during switching & higher clock frequencies ~ higher power draw.
- 
- NMOS is good at transmitting a 0 and PMOS good for 1; however they are bad at transmitting the opposite bit as the transistor as the transistor turns off / approaches being an (undesirable) open circuit / high resistance.

- Transistors are connected through metal *interconnects* made of copper; there are multiple layers of interconnects for different parts of the circuit, with interconnects joined vertically through layers.

![](misc/Pasted%20image%2020240111014604.png)

[Notes on CMOS lithography process](https://www3.nd.edu/~kogge/courses/cse60742-Fall2018/Public/Files/KernelPaper/CMOS_Process.pdf).

#### CMOS Inverter

![](misc/Pasted%20image%2020240110202502.png)

#### Pass Transistor logic

A pass transistor is a transistor used as-is as a switch, and does not amplify or drive the incoming signal. Pass Transistor Logic uses transistors to switch logic signals; however the will be voltage drop in the signal (across the transistor) & he voltage difference between high & low becomes reduced, & therefore cannot be used for complicated logic.

#### SRAM Cell 

![](misc/Pasted%20image%2020240111002930.png)

We have two pass transistors that switch when to set the two inverters; When the cell is disconnected The inverters output opposite bit values & drive each other; The inverters are small w.r.t. transistors so that the bit can easily be set; However because of the weak signal a *sense amplifier* must be used upon read.

#### Transmission Gate

With the following circuit & symbol:

![](misc/Pasted%20image%2020240111003248.png)

They allow current to flow in both directions like a mechanical switch, & transmits an analogue voltage. Remember that MOSFET transistors are symmetrical, we use NMOS/PMOS to transmit a 0/1 in either direction. Would use to build multiplexors.

#### Transparent Latch

![](misc/Pasted%20image%2020240111012018.png)

We have the following effective open circuits for enable states:

![](misc/Pasted%20image%2020240111012027.png)

### Standard cells

Logic gates are designed with a fixed height but variable width (usually a multiple of a constant to allow for a grid layout) such that we can tesselate them together i.e. drop in our individual cells where needed into rows; you will see that the wiring for signals & power are pre-placed between cell rows & flow down. 

![](misc/Pasted%20image%2020240110204026.png)

- All basic CMOS gates are inverting - we have NOT, NAND & NOR to use as building blocks.

### Transistor sizes

We can increase clock speed by having multiple devices/cells in parallel, which reduces the load & parasitic capacitance across the circuit, at the cost of increasing the load across the power source. 
Lower impedance leads to faster switching speed / speed at which the voltage level or the impedance of a transistor switches at.
- We mitigate increased impedance from transistors being in series by increasing transistor width (physically or use parallel devices).

Example of parallel inverter cell layouts:

![](misc/Pasted%20image%2020240111010914.png)

- PMOS transistors are typically *twice as wide* as NMOS transistors; electrons in NMOS are more mobile than holes in PMOS, therefore we have to compensate to get equivalent resistance between NMOS & PMOS. 


### Macrocell Arrays

A large circuit that has already been pre-designed; Typically are already transformed and optimised to a circuit layer (known as *hardening*). We take the prebuilt macrocell and integrate it into our design.

E.g. Multi-core processors, each core may be a macrocell which we then repeat and interconnect later. This may not be as optimal but easier to work with, optimise layout; timing can be optimised more easily on a per-core basis.

(SRAM) Memory often generated as macrocell, using an optimally small SRAM cell and 2D grid layout.

#### Analogue components
E.g. for creating a digital clock signal, and controlling a Voltage-controlled oscillator, or digital-to-audio (or vice versa). 
A *mixed-signal* design has both digital and analogue components; Typically analogue components will use the same MOSFETS for ease of production.