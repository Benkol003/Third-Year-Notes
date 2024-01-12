Adjusting various chip properties affects power draw as follows:

![](misc/Pasted%20image%2020240111165733.png)

- Switching activity factor ($\alpha$) - Probability of a transistor or output switching (at a given time e.g. clock edge). Can be estimated from functional simulations.
- The supply voltage at a circuit will be lower than spec / power supply due to voltage drop across internal wires to the circuit; this can again be estimated with CAD tools.



#### Voltage Drop / Load
- Electrical Load - the electrical energy consumed by a device, you will see this as a voltage drop across the device.

#### Decoupling
CMOS power demand is not constant, being high at clock edge switching, this is further exacerbated by *self induction* as power supply wires resist change in current flow. This can cause fluctuation in voltage drop & therefore voltage provided to other devices. We can use *decoupling capacitors / bypass capacitors* to hold charge & absorb noise.

### Power Dissipation

For a CMOS inverter circuit

![](misc/Pasted%20image%2020240111230112.png)


Dynamic / switching power dissipation (on clock edge):
- When switching a circuit current must flow in the PMOS or NMOS part of the circuit ($f \alpha CV_{dd}^2$)
- Power dissipation when both pull-up & pull-down circuits (PMOS & NMOS logic) are active during switching between them, causing a short circuit ($fV_{dd}V_{th}$) - **subthreshold leakage**

Static power dissipation (on clock high/low state) (lower than dynamic power dissipation):
- Static  leakage / power dissipation through both PMOS & NMOS, when the entire circuit is not active / all transistors are off (again a short circuit)($V_{dd}V_{th}$) - **gate leakage**.
- Static power dissipation when one of the PMOS / NMOS is active & not switching ($V_{dd}$)

We can increase transistor threshold to reduce power dissipation but this reduces switching speed.

#### Transistor Thresholds
This is the voltage threshold at which the transistor switches & starts conducting current. (different from gate threshold which defines voltages for logical low/high.)

Increasing the concentration of dopants in silicon decreases the transistor threshold (& vice versa).

Variation in transistor thresholds, *Random Dopant Fluctuation (RDF)* is increasing with smaller nanometre chips; a variation of a few atoms increases the threshold variation as transistors shrink to sizes of fewer atoms. It is therefore good practice to have changes signal edges/ changes be fast to reduce the impact of varied thresholds (short circuit occurs for less time).

Voltage diagram of input ($t_{pd}$) & output

![](misc/Pasted%20image%2020240111232740.png)

By decreasing the threshold the opposite circuit switches on earlier (faster switching) but there is greater time when both PMOS & NMOS conduct thus greater power dissipation.

#### Floating Inputs
Aka high-impedance; in this state the input to a CMOS circuit can take any voltage level; is likely to be between thresholds & cause power dissipation / short circuit; therefore good practice to set up logic inputs to CMOS circuits as soon as possible.



### Power Gating

We use *pass transistors* (switch) on the voltage supply & ground to turn off gates when they are inactive (dark silicon), to remove static power dissipation.

### Dynamically changing transistor thresholds

- [Body effect](https://www.electronics-tutorial.net/Analog-CMOS-Design/MOSFET-Fundamentals/Body-Effect/) (see also [this]([https://siliconvlsi.com/body-effect-in-mosfet/](this))); typically the substrate is connected to ground (0V) but if we apply a voltage to it, $V_{SB}$ source-body voltage then we can change the transistor threshold; this basically affects the size of the depletion region.

#### FinFETs

![](misc/Pasted%20image%2020240112134523.png)

The gate lies on multiple sides of the channel / substrate body which is in a fin shape, such that the gate is more efficient (more electric field around the channel); switching speed is increased & subthreshold leakage reduced.

### Power Domains
Different parts of the chip may run at different voltages; we may use lower voltages for slower circuits. Each voltage supply requires its own distribution network (i.e. more complicated chip.)

There is an issue of interpreting logic signals of different voltages across voltage domains; this can be done with a *level shifter*.

#### Level shifter

Without a level shifter:

![](misc/Pasted%20image%2020240112153828.png)

Both complementary transistors are on causing loss of voltage at the output signal.

![](misc/Pasted%20image%2020240112145051.png)

The bottom NMOS transistors themselves will not fully turn on / off when supplied with the 0.8V high signal; however both voltage domains have ground / 0V in common as the voltage level for logical low to respond well to.

The NMOS transistor that is off drains voltage to ground, providing a weak logical 0 to a PMOS transistor; the other NMOS is off and passes the inverted signal (logical 1) to the other PMOS which then fully turns off & provides a stronger logical 0 signal in a loop.

The issue here is that when a transistor is provided with a tri-state signal it transmits the correct voltage but with weak current (instead of weak voltage), causing a slow switching speed.


### DVFS

DVFS is becoming less useful as supply voltages reduce, leaving less headroom. However if chip activity is reduced we can reduce voltage - forces us to reduce frequency - we spread out less activity into longer time / less chip idle time.

![](misc/Pasted%20image%2020240112154035.png)

![](misc/Pasted%20image%2020240112154024.png)

