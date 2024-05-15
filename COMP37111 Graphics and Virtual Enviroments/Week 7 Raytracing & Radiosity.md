## Ray Tracing

Family of algorithms that draw rays into a scene to calculate it's illumination; typically we mean ray casting.
Full ray tracing is used in realistic CGI; Real time graphics for video games recently can utilise RTX graphics cards for real-time ray tracing for parts of the lighting model such as global illumination & reflections (but still perform rasterization).
Ray tracing from light sources is a model of how illumination occurs physically, but is typically not done as its too computationally expensive; & most rays traced will not find their way to the camera.

![](misc/Pasted%20image%2020231202180755.png)

You can see every time a traced ray interacts with a surface, it may be reflected (specular), pass through a translucent surface (transmission), or scatter/diffuse.

### Ray casting
(This is the [recursive ray tracing algorithm](https://en.wikipedia.org/wiki/Ray_tracing_(graphics)#Ray_casting_algorithm)).
[Whittard ray tracing model](https://dl.acm.org/doi/pdf/10.1145/358876.358882)
Instead we draw rays from the camera viewpoint, for every pixel; In a direction converging to the camera's focal point (viewpoint projection), or perpendicular to the pixel array (orthographic projection).
every time a ray intersects an object we draw a *shadow rays/shadow feelers* to all light sources; if their path is blocked by another object then the light does not illuminate the ray at that point.

![](misc/Pasted%20image%2020231202181339.png)

We keep doing this until we run out of compute or the ray exits the scene.
Realistically light will not always be perfectly reflected on a surface (especially for diffuse); some random noise may be added to the surface normal, within a certain variance to simulate [microfacets](https://www.pbr-book.org/3ed-2018/Reflection_Models/Microfacet_Models).
## Radiosity
[Useful VIdeo](https://www.youtube.com/watch?v=-v3-l8Xi9UA&t=1366s)
[Original Paper](https://dl.acm.org/doi/pdf/10.1145/358876.358882).
Radiosity was originally used to model thermal radiation, but has applications for rendering; we measure light as the heat exchange between surfaces; 

1. Break down our scene into small patches of discretised area; for our scene $S$ we have lots of small patches, represented using $\delta A$, with centre points $x$.

![](misc/Pasted%20image%2020231202210010.png)

3. Using the radiosity equation thus:



$$B(x)\delta A = E(x)\delta A + \rho(x)\int_S B(x')F(x,x')\delta A'$$ 
In layman's terms, the energy emitted from point x/patch A is: the emitted energy (if it is a light source), + the sum of emitted energy, for all other patches in the scene, that (their points) are visible to x.

-  $\delta$ is used in this context to represent a small change area around a point.
- $B(x)$ is the total radiosity for our patch $A$ - Amount of radiant flux *leaving* per surface area; (This is different from irradiance which is for the receiving case). Multiplying by the area $\delta A$ gives rise to:
- $B(x)\delta A$ - The total energy emitted for patch area A.
- $E(x)\delta A$ is the total energy *emitted* by patch area $\delta A$ -applies for light sources, when we say *emitted* we mean energy introduced into the system. (E(x) is the radiosity of the light source).

- $\rho(x)$ is the reflectance (reflected energy ratio) at this point/patch.

- $\int_S$ - we integrate over all other surfaces/patches in the scene to give integration variables $(x',\delta A')$
- $B(x')$ - Radiosity for patch $A'$. The $\delta A'$ term will transform this into the total emitted energy of patch $A'$.
- $F(x,x')$ - The view factor between $x,x'$, taking a value $0\le F(x,x') \le 1$:  The proportion of energy from $x'$ that strikes $x$. This is dependent on the angles, distance, & difference in size of $x,x'$ .

The radiosity equation self-recurses infinitely/is Fredholm equation of second kind if $\delta A$ infinitely small i.e. no analytical solution; 
But if we have a limited number of patches the radiosity equation is more commonly written as $B_{i}=E_{i}+\rho _{i}\sum _{{j=1}}^{n}F_{{ij}}B_{j}$ for $n$ patches in the scene.
We can then produce an $n\times n$ matrix of linear equations; We can solve to find the radiosity of each patch using *Jacobi iteration* or the *Gauss-Seidel method*.

#### View form function
The following function in the videos is the form factor equation for between two surfaces, as derived from radiation physics:

$$ F_{ij} = \frac{1}{A_i}\int_{A_i}\int_{A_j}\frac{\cos\theta_i\cos\theta_j}{\pi r^2}V_{ij}\cdot dA_i\cdot dA_j$$


It starts to get confusing here; $A_i$ represents a patch; (I assume we perform further discretisation of the patch with $dA_x$)

- $r$ - distance between the two areas
- $V_{ij}$ - View function; =1 if two areas are visible to each other, else =0.
- $\theta_1,\theta_2$ -The angle between the line joining the two areas and their surface normals (normal value is same across patch).


![](misc/Pasted%20image%2020231203000645.png)


(TODO Why cant we calculate this without the integral? & do something like this $F_{ij}=\frac{\cos\theta_i\cos\theta_j}{\pi r^2}V_{ij}\delta A_j$   if we don't break down the patch further)

#### Nusselt Analog

A visualisation of view factor; The patch is transformed to unit hemisphere and then to a unit circle.

![](misc/Pasted%20image%2020231203091251.png)

#### Alternative method for calculating view factor

- Sampling: Generate points on the unit circle, transform them onto the hemisphere surface, then trace rays from (the centre point &) these points to the other patch and obtain a radiosity value from the point landed on.
- [Hemicube](https://en.wikipedia.org/wiki/Hemicube_(computer_graphics)): TODO
#### Radiosity vs Ray Tracing

- For static scenes, we can pre-render the radiosity (to produce a lightmap); We can then use this dynamically e.g. if the camera is moving around the scene.
- Radiosity is good at rendering diffuse (or global illumination), colour bleeding; ray tracing is better at rendering reflections & spectral highlights.