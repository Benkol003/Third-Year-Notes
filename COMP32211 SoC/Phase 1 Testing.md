### Process for line drawing modules

convert pixel addresses to framestore addresses:

framestore is 640 bytes / 160 words (4-byte) wide
pixel is 1-byte color

```
import math
def px_addr(x,y):
    return y*160 + math.ceil(x/4)
```
for de_byte the x mod 4 th bit will be low

scan line is 160 words - 640 bytes/pixels

**clock edge is posedge?**

## Req / Ack

Produce a req at any time, but must occur across a clock edge
- test producing exactly on clock edge

testing ack:
- if recieved
- if occurs on clock edge, for one clock cycle only
- if only one ack is produced

## Framestore memory

Check:
- Unit waits for de_ack before changing its outputs
- Unit waits for de_ack before de_req goes low
- check that de_req switches high-low-high before next memory write
- unit does not change outputs while de_req high

## Input Params

Remove immediately when ack asserted, (or on next clock edge?), & test if still functions / data is stored.

Check:
- de_data at every de_ack = input colour (R6?) at ack - only applicable for the active de_byte wires enabled
## Busy
check:
- goes high at same time as ack
- is high until all memory operations are finished - till the last de_ack (check goes low after this)
- there is only one busy signal

## Line Drawing
Check:
- Edge cases - horizontal & vertical lines, lines at the edges of the screen
- At least the two input pixels are drawn to memory.
- Checking intermediary pixels - preset array or bresenham's algorithm in verilog? or precompute and compare later? - do this last if have time


