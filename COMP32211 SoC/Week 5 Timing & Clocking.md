### D-type flip flop
Holds state or a binary value (0/1). Are commonly used to construct registers to hold binary data.

![](misc/Pasted%20image%2020240106180116.png)

- $T_{su}$: Data set up time - data must be set up before this and held at a stable value for the duration of the set up time window. 
- $T_{hold}$: Data hold time - data must be held at a stable value *after the clock edge* for this period of time

Set up and hold time both require the data to be stable but differ in that the occur before and after the clock edge.

- $T_{pd}$: Propagation delay: The delay from the clock edge and the circuit to update internally, or for data to travel from input to output - in this case to store the value.

### Clock distribution

- Clock generation - the clock signal is produced from a source e.g. oscillator.

- Clock tree - The original clock signal needs to be split and and distributed to a large number of components. However this can introduce *clock skew*, which we want to reduce as much as possible.
	- Clock skew: the clock signal (or edge) reaches components at different times due to physical factors/delays.

There is a large amount of fan-out of the original signal, in terms of a **H-tree**:

![](misc/Pasted%20image%2020240106181409.png)

The h-tree allows to distribute our clock signal with *theoretical* minimal skew (all leaf nodes are at the same depth.)

The h-tree splits our signal as needed. At the nodes we need to split and amplify the clock signal, which is performed by *clock buffers* or *clock drivers*.

Physical distance of wire from the clock signal to clocked devices also introduces delay which cause skew. (A h-tree is only theoretically optimal if all our devices are the same distance from the clock signal.)

### Timing closure

Timing closure is about designing our chip layout to optimise it's minimum clock period, & trying to meet timing requirements. E.g. looking at critical paths & trying to reduce them, *routing* (or wiring) for minimal delay, etc. Operating temperature may affect timings, or too high a clock may cause our chip to overheat and become unstable.
We may also increase/decrease the clock period if we have timing slack.

- Negative slack: If our chip has a minimum clock period that is longer than the one desired, this value is *negative slack*.

### Time Stealing/borrowing

![](misc/Pasted%20image%2020240107222806.png)

1 GHz clock - period 1000ps. 1.11 GHz clock - period 900 ps.
In the top pipeline, the second stage waits for 200 ps for an input from the first stage.

In the bottom pipeline, the clock signal arrives 100ps later to the second stage compared to the first stage; this gives the first stage 1000ps to do its computation.

However we must guarantee that no logic path in the first stage takes less than 100 ps, otherwise that data can be captured by the same (delayed) clock edge at next stage.

**Question**: What I don't understand is that the first stage is being clocked at 900ps, so we are effectively changing the inputs before it produces an output - wont this make it metastable?. I assume this is allowed as long as we still meet the set up & hold times?

#### Transparent latches
We could also use a *latch*, which captures it's input when the clock is high, rather than on the clock edge; the signal from the first stage will be complete 100ps into the 'clock high' signal.



### Clock domains

Different chip modules may use clocks at different frequencies; e.g. cpu, gpu, or a module may have a variable clock. these modules are in different *clock domains*.

- Dark silicon: The percentage of a chip that cannot be turned on at any given time due to thermal or power constraints - modern chips will power down units that are not in use.

### Crossing clock domains

Note we're gonna be using a d-type flip flop as an example component to observe it's behaviour in this scenario.

We have the case where we have two circuits of different clock frequencies, and a signal crosses this domain; the signal appears to be asynchronous to the circuit at a different clock frequency.

It is **impossible** to do this with 100% reliability, unless the two clocks are harmonics of each other (an **isochronous circuit**). The setup/hold times of a circuit can be violated somewhere; or (discussed later) it could potentially take forever to resolve. However we can improve reliability with synchroniser, or an *asynchronous arbiter.*



#### Metastability

If we violate signal stability during set up or hold times - i.e. the signal changes during these periods, then our flip flip will become *metastable*: the output voltage behaviour is undefined - it may lie between the two thresholds, or oscillate.
There is also **no perfect method** for bringing a device out of metastability - the device itself is internally unstable. However we can use synchronisers which are less likely to go metastable and resolve quicker.

Example 2 flip-flop synchroniser used to cross a clock domain:

![](misc/Pasted%20image%2020240107153023.png)

Basically we use two flip-flops as a buffer to copy the data across the clock domain.

![](misc/Pasted%20image%2020240107200434.png)

If the first flip flop does go metastable then it has an entire clock period to resolve this before it is copied to the second flip flop. We could add more flip flops to reduce the chance of metastability. However the synchroniser adds latency of a few clock cycles to the signal crossing the domain.

Metastability can be a serious problem - if a metastable output is used as an input to another element e.g. flip flop, then that flip flop will also become metastable - metastability propagates in an infectious manner.

An *asynchronous arbiter* detects metastability and delays its output signal until this is resolved - however metastability can take potentially forever to resolve.

We have a trade-off between risk of metastability being transmitted, and the latency + bandwidth of transmission, across the clock domain.

#### Buffering

The latency of a synchroniser typically is applied to each individual item of data transmitted across the clock domain. If the receiving circuit is clocked slower than the transmitting one, then the transmitter spends time waiting on the receiver to collect the data.

- Single buffer: Single bucket of memory is filled with data; then signal the receiving circuit, which will synchronise and empty the buffer. However this increases latency as we have to wait for the buffer to be filled.
- Double buffer: We have two memory buckets; Whilst one is being synchronised/emptied the other is used and filled up with data. You will have come across this in computer graphics, where the framebuffer to screen which we render to is implemented as a double buffer. We can achieve much higher bandwidth with this (but requires double the memory).
- Decoupling FIFO: Here we have a counter for how many elements are currently in the queue; we increase/decrease with writes/reads; Synchronisation is not necessary unless the FIFO is empty i.e. both circuits are reading from the same place in the FIFO. This is best used when one of the clock frequencies is unknown, e.g. a baud rate; using a double buffer here we may see both **buffer underrun & buffer overrun**.

### Clock generation

Clock division (reducing the frequency) is simple and just requires a FSM (*prescaler*) with the clock signal as input.

#### Clock multiplication (increasing the frequency)
For e.g. a CPU internal clock, is much harder; we use a *phase locked loop*.

- Voltage-controlled oscillator: Circuit capable of generating a high-frequency signal, but wont be very accurate. Crystal oscillators are accurate but can only generate frequencies in the MHz range. A VCO typically uses capacitors to be able to generate frequencies in the GHz range.

Example VCO circuit:

![](misc/Pasted%20image%2020240109233957.png)


- A phase locked loop uses a voltage-controlled oscillator (VCO) to generate a low-accuracy high-frequency signal, and controls it with *phase comparator* to keep it in phase with the accurate reference low-frequency signal (typically generated from a crystal oscillator).
- 
![](misc/Pasted%20image%2020240109234356.png)




