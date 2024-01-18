
#### Moore's Law
- Number of transistors doubles every two years

![](misc/Pasted%20image%2020240113163607.png)

Transistor lengths have also been steadily decreasing, however this trend is expected to end as transistors are now approaching size limit, quantum tunnelling effects become more prevalent; atoms are ~0.1nm thick; for a 22nm process the transistor is ~200 atoms wide, & the dielectric a few atoms thick.


- As transistor size decreases so does their required supply voltage, reducing noise margins. Chip voltages have reduced from 5V to 3.3V #

#### Reliability
Variability in sizes of parts of a transistor increase; etching process has roughly same variability, but when applied to smaller components is more apparent. This reduces reliability and requires larger margins as transistor behaviour becomes more unpredictable.

We can mitigate for unreliability in different ways; processor chips may have redundant cores if one stops working; manufacturers may also just disable broken cores & ship is as a CPU with less cores. DRAM uses error-correcting codes; this is more due to instability from e.g. radiation, rather than broken chips.

#### High-K dielectric

These are materials with a high dielectric constant, $\kappa$, compared to silicon oxide; such as hafnium or zirconium. These reduce gate leakage and allow for thinner dielectric layer; However are harder to construct as the dielectric material has to be placed, compared to using silicon oxide where the underlying silicon wafer material is oxidised.

#### 3D / stacked chips

DRAM chips already utilise 3D space by having a well to hold charge (for data):


![](misc/Pasted%20image%2020240113171628.png)

CPU's are increasing in terms of the number of layers of wire interconnects they use as they become more complex.


### Through-Silicon Vias (TSV's)

- A *via* is an electrical connection that goes through two or more physical layers
- Through silicon vias are high speed interconnects that pass through all layers of a chip; are utilised in creating 3D (stacked) chips.
![](misc/Pasted%20image%2020240113172725.png)

(interconnected chips)

![](misc/Pasted%20image%2020240113183532.png)

- Are formed using *Deep reactive-ion etching*  ( - etched with plasma ion beam & then coated with passive layer e.g. teflon).