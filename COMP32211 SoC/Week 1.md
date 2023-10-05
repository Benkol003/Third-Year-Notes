- SoC: GPU with a drawing function (free to implement whatever)
- Walkthrough of design process (of ASIC's), verification, use of tools, and using FSM's, using verilog language.
- Produce one functional block as part of a team to then integrate with other blocks via standard interfaces. The complete GPU will then be integrated into a SoC.
- The GPU provides different drawing/plotting functions as different hardware blocks that can be utilised.
- Designs are provided - some working, some broken. Will require to test and diagnose units, and fix errors accordingly. Your allocated design set is called a **CARD**.

- **ALLOCATED CARD NUMBER: *15***


#### Frame Store
Pixel colors are stored in a frame buffer; values for each pixel read out ~60hz. Requires a lot of memory: one 720p frame is 921.6Kb.  machine may not be always be able to write to frame store at all times.
 
- Double buffering: have two framebuffers which are swapped every frame allows to draw to GPU whilst is also updating the screen; This method prevents screen tearing.

#### Line Drawing
Following y=mx+c, naive approach is to use the absolute x position to calculate y, however this is inefficient due to performing divides for every x.
Using Bresenham's line algorithm, we do one division to calculate $\Delta y$ and add this to y for corresponding $\Delta x = 1$ .
Can use antialiasing to have faded pixels around the line to make it smoother.


####lol

