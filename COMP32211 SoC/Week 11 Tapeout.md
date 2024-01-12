- Tapeout is the final process of creating a CAD chip layout that can be sent for chip fabrication to produce the photomask.

### Chip packaging


### Mixed Simulation
- Infeasible to simulate the entire chip physically accurately on a simulation run; use simulation models that accurately simulate only certain properties that are important to different functional blocks.

![](misc/Pasted%20image%2020240112162856.png)

#### Low-level simulation
SPICE is commonly used to provide an accurate electrical / analogue simulation of circuits. Typically we would use SPICE to simulate standard cells to derive characterisations, (e.g. switching speed, propagation delay) to use in a higher-level simulation of the chip. 

#### Process Corners
There range of variability of different conditions that a chip must be able to operate under. These extreme values are known as corners.

- Process - Variations in chip manufacture e.g. transistor sizes
- Voltage - power supply fluctuations
- Temperature - expected range of conditions; chip may also head itself up to extreme temperatures

These factors affect transistor properties as *FEOL corners*, affecting the switching speed of transistors - FF,FS,SF,SS denote the range fast/slow switching speeds of NFET (NMOS) & PFETs.
(We also have T - typical, but this is not a corner).


- Cell Phantoms: TODO

### Placement & Route
- Placement phase attempts to place connected chips as close to each other as possible, based on utilisation & area constraints - certain space amount must be left for wiring etc.; there is pull in multiple directions to different other chips it is connected to.
- Routing attempts to then place wire connections in the spare space to devices.

Often Done multiple times, with relaxing the constraints at each iteration

#### Floorplanning
Again we may do placement & route for macrocells e.g. processor cores and then perform placement & route for the larger chip with the hardened macrocell.

#### Buffer insertion
If wiring is excessively long then latency is increased; we can hide this by placing data buffers along the wire to hide the latency into multiple clock periods.

#### Design Rule Checks
Checks multiple rules - e.g. minimum spacing is preserved between between components, well contacts exist within certain distance of transistor; other rules such as electrical & antenna checks.

#### Electrical Rule Checks
E.g.
- Power connected
- I/O is not connected directly to power
- No short circuits
- 

#### Antenna Effect
Or *plasma induced gate oxide damage*; connections to a gate may be made vertically; therefore the gate is not connected to it's driving wire till late in the fabrication process as each layer has to be deposited; this causes issues as the fabrication process (namely plasma) causes charge build-up in unconnected wire (i.e. it acts like an antenna), that if not dissipated shorts the dieletric.

![](misc/Pasted%20image%2020240112205118.png)

This can be resolved by 

1. Redesigning such that the transistor is only connected to short wires before it's connected to the driver / power rail
2. Add an extra diode near the transistor, this can dissipate charge into the substrate; however this increases capacitance
3. Use of more leaky gate oxide; it can dissipate more built up charge, however static power dissipation will be increased

![](misc/Pasted%20image%2020240112205524.png)

### Production Testing

Typically only 30% of chips on a silicon wafer actually work; For CPU production ones that are produced may have different highest possible clock frequencies and these are sorted by performance in a process called *binning*.

- *Design for test* means designing chip circuitry so that it can be easily tested.

#### Automatic Test Pattern Generation
Problem of generating test data input sequences that can be used to test the chip (and know which parts it tests) with test equipment, such that we can get as close to 100% test coverage. Ideally we want to test as many functional blocks at the same time for a given sequence; however some chips may not expect to have all components turned on at once and may blow up even if it works correctly.

#### Built-in self test
Integrated circuitry that can be run that checks the chip is functional by itself; in e.g. CPU may be used with either a pseudo-random generator or a test ROM of ATPG's.
A tester then only has to run the BIST and wait for a 'pass test' signal from the chip.

#### Scan chains
Used typically for flip-flops; Typically we replace our flip-flops with *scan flip-flops*, which have two mutexed inputs;

In the second (testing mode) of operation we place wiring that connects all the flip flops together in a *scan chain*, forming a long shift register / data queue with scan bit in/out connections to the chain. chain connection is usually arbitrary & try to minimise wiring. Testing then consists of inputting a bit sequence & waiting to observe we see that sequence on the output.

![](misc/Pasted%20image%2020240112211727.png)

It's better to test flip-flops first as it makes testing / test patterns easier + we can isolate issues to either the flip-flops or logic blocks.

- Chip buses provide an easy way to set up a scan chain & test interface, along the length of the bus:

![](misc/Pasted%20image%2020240112214204.png)

- Test interface may be connected to onboard RAM which can then execute test code on the device via the bus, basically close to normal operation


Test pins on a board are commonly used to multiplex other pins so alternate testing mode, as chips are limited on the number of available pins.

![](misc/Pasted%20image%2020240112214703.png)

You will commonly see these test pins as 'tie to ground' on spec sheets, manufacturers do not disclose how to utilise the built-in testing functionality.

### Boundary Scan / JTAG

- Boundary Scan, Test Access Port (TAP) and Joint Test Action Group (JTAG) are specified in [IEEE Standard 1149.1](https://standards.ieee.org/ieee/1149.1/4484/)

This concerns testing circuitry now outside the chip; We have flip flops around the boundary of the chip connected to pin lines; the boundary flips flops are then connected in series, called a *boundary scan register*. Testing becomes a matter of reading/writing data (via shifting bits in/out of the register) to the BSR.

![](misc/Pasted%20image%2020240112212443.png)

![](misc/Pasted%20image%2020240112212454.png)

JTAG connectors:

![](misc/Pasted%20image%2020240112212718.png)

- In labs the Xilinx spartan boards actually use JTAG to configure the FPGA & load a bit design. It is common for JTAG to be used for software-controlled configuration & debugging purposes.

JTAG has multiple registers:

- Boundary Scan register
- Instruction Register - there are multiple different modes of operation for different testing scenarios, e.g. fetching IDCODES of chip devices, 
- Data Register / Test Register
- Bypass Register

We can only program the instruction & data register, but instructions exist to program the other registers; bypass register instruction may be 0xFF...

#### TAP controller

Has the following state diagram:

![](misc/Pasted%20image%2020240112212345.png)

- Bypass path: This allows to bypass boundary scan cells in the BSR; data is simply shifted through the flip flop instead. Allows for debugging certain parts of the chip, reducing test time.