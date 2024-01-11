## AMBA 
[ARM link](https://developer.arm.com/Architectures/AMBA)
Is a set of bus protocols by ARM for on-chip communications between SoC functional blocks. We will cover the APB, AHB & AXI protocols.

### AMBA Advanced Peripheral Bus (APB) specification
[ARM spec link](https://developer.arm.com/documentation/ihi0024/latest/).

![](misc/Pasted%20image%2020240110014117.png)

We have our different peripherals (functional blocks / modules) at different addresses on the bus; Our bus master or *APB bridge / requester* controls read/write requests to peripherals. Typically the APB bridge is attached as an AHB device.

Some of the signals:
(The signals are named Pxxx as peripheral xxx)
(There are more signals for waiting and handling error but we are only covering read/write, go look at the spec)
- PCLK (clock)
- PADDR - address for r/w, within peripherals internal memory space.
- PWRITE, PREAD (request read/write)
- PDATA - bus data to be transferred.
- PSELx - the APB bridge has a select signal to each x'th peripheral to select it for the transfer operation.#
- PREADY - peripheral signals that it is ready to do r/w. It is held high but can be set low by the peripheral if it needs more time to do an operation.
- PENABLE - allows the peripheral to take/place data onto the PDATA bus.

The APB transfer occurs in two phases; the peripheral is first provided with the PADDR address and allowed to ready itself for the second phase; where data is transferred on the PDATA bus.

Write transfer:

![](misc/Pasted%20image%2020240110021328.png)

Write transfer with *wait states* (if the peripheral needs more time):

![](misc/Pasted%20image%2020240110021347.png)

APB takes a minimum of two clock cycles to perform a data transfer.

### AMBA Advanced High-performance BUS (AHB)
[ARM spec link](https://developer.arm.com/documentation/ihi0033/latest/)

AHB block:

![](misc/Pasted%20image%2020240110024853.png)

AHB is a *pipelined bus*, meaning multiple transactions (two in this case) can occur at the same time.

![](misc/Pasted%20image%2020240110030128.png)

As you can see the read/write data is provided on the next clock, however the command and address will be changed for the next transaction. This allows for greater data throughput.
However, other subordinate devices now need to watch for other devices inserting either wait states or an error; other devices will need to then wait themselves, or in the case of the abort the next transfer requested aswell, so that the error can then be handled.

### AMBA Advanced eXtensible Interface (AXI) specification
[ARM spec link](https://developer.arm.com/documentation/ihi0022/latest)

More complicated design, more akin to a networking protocol.

General block layout:

![](misc/Pasted%20image%2020240110033715.png)

- Allows for multiple master or *manager* devices.
- Outstanding transactions may be completed out of order, rather than a transaction being immediately fulfilled.
- Data bursts: For a transaction, allows multiple data *beats* to be sent back from a single provided address, e.g. for memory / cache accesses we would expect to access continuous data.

![](misc/Pasted%20image%2020240110033730.png)



#### AXI Data Queue
(The notes call this a pipeline (its a queue))
We can buffer data elements to & from elements, e.g. the communication line between CPU & AXI interface, or from AXI manager to a device. Two reasons to do this:
- We can potentially decrease wiring length that data has to cover, for faster clocking -> better throughput
- Spreading out latencies in data transmission so that they are less apparent


![](misc/Pasted%20image%2020240110120230.png)

Ready to indicate  back empty flip-flop; valid to indicate forward that a flip-flop can send data.
Consider this: Although we have time do indicate between registers whether to move data, we **do not have time** to propagate these signals up the pipe if we want a decent clock; therefore we cant shuffle all elements forward like we would for a typical queue.

We can implement this with:

- One buffer per stage:

![](misc/Pasted%20image%2020240110120502.png)

However we have to leave half the queue elements empty to avoid data loss like so:

![](misc/Pasted%20image%2020240110120615.png)

- Using two buffers per stage

![](misc/Pasted%20image%2020240110120654.png)

This way we don't start to lose data until we have double-filled all buffer elements.

In both cases we have redundant storage to use; this starts to get filled backwards from where the blockage in the queue has occured.


The following bus hierarchy for an ARM atmel chip:

![](misc/Pasted%20image%2020240110121149.png)

- TCM - tightly coupled memory - we use high-speed on-chip SDRAM (as used for CPU caches) that is physically close to the CPU core, without a cache for a constant access time.
- As you can see we have the APB bridge as a AHB device.
- Other devices can become bus masters if they want to perform *first-party DMA* operations.

### Network on Chip (NoC)

Chips may be connected together in a 2D/3D grid manner:

![](misc/Pasted%20image%2020240110154841.png)

As is the case for SSDs,

or more random where we use packets or circuit switching e.g. multiplexers

![](misc/Pasted%20image%2020240110154954.png)

### Globally Asynchronous, Locally Synchronous (GALS)

allow different Intellectual property (IP) blocks to be clocked independently - arbitrary phase and frequency. Interconnects than have to handle crossing clock domains. Allows for better per-block performance, & reduction in power usage.

Can also reduce power noise: In a synchronous circuit a there will be stronger electromagnetic interference as gates will switch in sync with the clock, creating a regular AC signal; if different blocks are at different frequencies & phases then this is reduced.

Due to the requirement to have a *handshake protocol* such as AXI, synchroniser & maybe buffering there is added added latency to communication.


### Serial Bus

These use a single wire for communication and transmit data one bit at a time. These are typically only used for communication off-chip / off-board where electrical interference is a concern.


such as:
- Ethernet
- USB
- I$^2$C
- CAN bus

USB pinout:

![](misc/Pasted%20image%2020240110152502.png)

A *differential pair* is used; in USB there is a 2.6V differential using voltages >2.8V & <0.3V; for logical high D+ takes >2.8V & logical low D- takes >2.8V.

If there is electrical interference, the voltage difference should be preserved (*common-mode rejection?*), compared to using a single line with high/low voltage, which would more likely to transfer an errored bit.




