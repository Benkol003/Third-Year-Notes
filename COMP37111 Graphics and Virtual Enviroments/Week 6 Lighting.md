### The Rendering Equation
Models light for **opaque** surfaces.
The original [paper](https://www.cs.cmu.edu/afs/cs/academic/class/15462-s13/www/lec_slides/86kajiyaRenderingEquation.pdf) here.

In the basic form:
$$L_o(\mathbf{p}, \omega_o) = L_e(\mathbf{p}, \omega_o) + \int_\Omega L_i(\mathbf{p}, \omega_i) \cdot f_r(\mathbf{p}, \omega_i, \omega_o) \cdot (\omega_i \cdot \mathbf{n}) \, d\omega_i$$

- $L_o(\mathbf{p}, \omega_o)$  - The *spectral radiance* of light emitted at point **P** at angle $\omega_o$.
- $L_e(\mathbf{p}, \omega_o)$ - If point **P** is a light emitter, then we calculate its radiance (given the parameters)

- $\int_\Omega...d\omega_i$  - This integral calculates the radiance of all reflected light, considering all incoming directions $\forall \omega_i$ in $\Omega$: A hemisphere around point **P**. The hemisphere lies on the surface at **P**, the surface normal **n** to **P** is perpendicular to the flat face of $\Omega$.

![](misc/Pasted%20image%2020231201201642.png)


Integral Terms:
- $L_i(\mathbf{p}, \omega_i)$ - Spectral radiance of incoming light for the current considered angle  and **P**. The value of which is weighted by the next two terms:
- $f_r(\mathbf{p}, \omega_i, \omega_o)$ - The *bidirectional reflectance distribution function*, (BRDF), which models reflection properties of a surface (&/or material) in terms of ratio of incoming light to outgoing light. There are different BRDF models such as Blinn-Phong, PBR.
- $(\omega_i \cdot \mathbf{n})$ or $cos(\theta_i)$ - Weakening factor - at more oblique angles light reflected will be smeared across a greater area (compared to light reflecting down the surface normal).

Typically with a BRDF the light intensity of light will also decrease with more oblique angles, however this is a property of the reaction of the surface material; The weakening factor is doing this to model *Lambert's cosine law*.

Which can be seen as thus:
![](misc/Pasted%20image%2020231201222350.png) ![](misc/Pasted%20image%2020231201222411.png)
#### Rendering equation with full parameters
We can introduce extra parameters:
- $\lambda$, wavelength - we can model for e.g. RGB wavelengths, or for the full spectrum. 
- $t$, time - For video rendering (as opposed to still images) the scene may change e.g. light sources; Motion blur - done by averaging radiance over a time interval.

New formula thus:
$L_o(\mathbf{p}, \omega_o, \lambda, t) = L_e(\mathbf{p}, \omega_o, \lambda, t) + \int_{\Omega} f(\mathbf{p}, \omega_i, \omega_o, \lambda, t) \cdot L_i(\mathbf{p}, \omega_i, \lambda, t) \cdot (\omega_i \cdot \mathbf{n}) \, d\omega_i$ 

#### Limitations

It is impossible to obtain an analytical solution for the rendering equation;
1. Only models opaque surfaces; doesn't account for transparency of subsurface scattering (see later); and other properties such as [these](https://en.wikipedia.org/wiki/Rendering_equation#Limitations). 
2. We cant integrate over all possible angles in the hemisphere with infinitely accurate precision;
3. The equation itself is infinitely recursive; The radiance of a point may involve its own radiance once reflected off other surfaces; (due to it being a *Fredholm equation of the second kind*?).

We can use numerical methods:
- Monte Carlo methods: path tracing, photon mapping.
- Finite Element Methods (FEM) as used in the radiosity model.

### Meaning of the BRDF

The BRDF for our reflection point **p** is defined as $f_r(\omega_i,\omega_r) = \frac{dL_r(\omega_r)}{dE_i(\omega_i)}$;

- $L_r(\omega_r)$ - Radiance; The (emitted in this case) radiant flux (radiant power) per unit solid angle (*steradian*) per unit area; in the direction of the outgoing angle $\omega_r$.

This shows a solid angle of 1 steradian/square radian:

![](misc/Pasted%20image%2020231202144147.png)

Radiance would be the amount of light power at the circular area, when measured at a unit/radius area of 1m.

- $E_i(\omega_i)$ - Irradiance; The *incoming* radiant flux per unit area, for the surface of the reflection point **p** with our light source positioned at angle $\omega_i$.


The BRDF function of a real material may be measured with a *Gonio-reflectometer*, which allows to sample BRDF values when moving the light source and the observing sensor around the hemisphere.

![](misc/Pasted%20image%2020231202155253.png)

### Other Bidirectional Lighting functions

- BTDF (Bidirectional transmittance distribution function) - models light scattered via *transmission*; that is light passing through translucent surfaces.
- BSSSDF (Bidirectional scattering-surface distribution function) - Describes *subsurface scattering*; (also comes in transmission and reflection variants). light may enter a material, bounce around inside and exit at some later point; Is common in skin.

Subsurface scattering contributes to diffuse reflection (so does the roughness of the material surface).

Diagram of subsurface scattering reflection:

![](misc/Pasted%20image%2020231202160052.png)

The red glow is caused by subsurface scattering:

![](misc/Pasted%20image%2020231202160621.png)

- BSDF (Bidirectional scattering distribution function) - Combines all of the models into a generalised lighting model; Can be formulated as $BSDF = BRDF  + BTDF + BSSSDF$.