**Animation**: A sequence of images that give the illusion of motion for a model within that image.

### 12 basic principles of animation 
Principles defined by Disney animators on how to make animations more visually appealing.
https://en.wikipedia.org/wiki/Twelve_basic_principles_of_animation

#### Squash-and-Stretch
An object that rebounds off a surface will squash upon contact and stretch after bouncing, to add emphasis to its motion.

![](misc/Pasted%20image%2020231014180423.png)

#### Timing
Relates to no. of frames used for an animation, which relates to the speed/time of the animation; should relate to the situation e.g. heavier objects should take longer to move.


### Keyframes
Describe starting and ending points, or different stages to an animation. The actual frames for performing the animation will be added later (known as **tweening**).

![](misc/Pasted%20image%2020231014180719.png)

### Procedural Animation
The animation is defined algorithmically, e.g. animation of  a clock can be describes as the rate at which the seconds, minutes, hours hands rotate w.r.t each other.

## Skeletal Animation

Also known as **rigging**: We have a model of e.g. a character in two parts; 

- An internal skeleton/armature made up of rigid **bones**.

- An external **skin** or **meshes** that are attached to the skeleton, such that they deform to move with their assigned bones.

- Meshes are attached to and deformed around bones in a process called **skinning**.

Example of a rigged 3D model in different poses:

![](misc/Pasted%20image%2020231015142746.png)

![](misc/Pasted%20image%2020231015142757.png)

### Skeleton Subspace Deformation (SSD)
A skinning technique for how to deform skin meshes.
Vertices that are close to joints are assigned to multiple bones around that joint; their position is a weighted combination of the position of the two bones.

In the image triangles in black at hand joints are assigned to multiple bones.

![](misc/Pasted%20image%2020231015154509.png)

#### Example

![](misc/Pasted%20image%2020231015154633.png)

A point $P_0$ is assigned to two bones $T_1, T_2$.

![](misc/Pasted%20image%2020231015154758.png)

After the joints are moved, we compute $P'_1,P'_2$, which are the position of $P_0$ if it is assigned to only one bone, and we apply the transformations of bones $T_1$ and $T_2$ respectfully.

The new position is calculated as a weighted combination of these: $P'_0=0.5P'_1+0.5P'_2$ - use weights of $1/2$.

Choosing which vertices to apply SSD and their weights is usually done *by hand* & is tedious.

### Kinematics

A double joint with indicated ranges of motion:

![](misc/Pasted%20image%2020231015150738.png)

**Forward Kinematics**: Describes the end position of a bone given the angles of all the connected joints to that bone.

**Reverse Kinematics**: given the end position of a bone, calculate the angles of different connected joints to obtain that position.

A skeleton may have constraints e.g. joint can only be bent to a certain angle.

Kinematics can be sued to manually create animated movement; however is not usually realistic.

### Motion Capture
Process of recording movement of objects or people; outside of graphics is called motion tracking.

#### Mo-Cap Process

1. Recording movement
Say for a person, they will wear passive/active *markers* that are at fixed body position that can be tracked.

![](misc/Pasted%20image%2020231015151741.png)

2. **Retargeting**: Marker positions and their movement are translated to character controls of their skeleton/bones.

![](misc/Pasted%20image%2020231015152007.png)

## Physics-based Animation

Types of realistic physics modelling methods:
- Point/Particle Simulation
- Mass-Spring Models
- Continuum Simulation
- Rigid Body Simulation

Commonly we have an **ordinary differential equation (ODE)** which describes acceleration/velocity at time t; we numerically integrate these to calculate position across time, using e.g. Newtonian formulation.

### Particles Simulation
Use a lot of particles; generated from an *emitter*, e.g. a smoke source such as s gun muzzle. Then apply external forces; gravity, *procedural force* like wind; particles may also have limited lifetime. Then add visual effects for rendering, e.g. fire particles will be red light sources.

May use particles for classical mechanics e.g. double pendulum. 

### Newtonian Formulation of ODE

1. $\vec{F}=m\vec{a}$
2. $\vec{F} = m\frac{d^2\vec{x}}{dt^2}$
3. $$\overline{X}=\begin{pmatrix} \vec{x} \\ \vec{v} \end{pmatrix}$$
4. $$\frac{d}{dt}\overline{X}=f(\overline{X},t) = \begin{pmatrix} \vec{v} \\ \vec{F}(\overline{X})/m \end{pmatrix}$$
5. TEST: \begin{pmatrix} \vec{v} \\ \vec{F}(\overline{X})/m \end{pmatrix}
We calculate the current force in the system using function $\vec{F}(\overline{X})$.

given $\overline{X}$ and the function of its first derivative,$f(\overline{X},t)$, we can use numerical methods such as Euler, Verlet, RK4 to calculate  $\overline{X}_{t+h}$ after a timestep h from $\overline{X}_t$.

- *Lagrangian* and *Hamiltonian* mechanics are two other formulations of classical mechanics that we can use to create ODE's to calculate positions in a system. (Not in slides).

#### Euler Method
Given our 1st-order ODE $\frac{d\overline{X}}{dt}=f(\overline{X},t)$:
- Pick a timestep, h
- $t_1=t_0+h$
- $\overline{X}_1=\overline{X}_0 + hf(\overline{X},t)$
($\overline{X}_1$ is the new calculated position at time $t_1$.)

Euler's method makes the assumption that the gradient is equal through the timestep. This is inaccurate, and this leads to deviation from the actual value (trajectory) over time.

### Vector Fields

![](misc/Pasted%20image%2020231015165526.png)


#### Viscous Damping
Dampens movement of a system; Damping force $\propto$ velocity; Can help stabilise a system - numerical methods on an ODE of a system can be prone to becoming **unstable**. Too much damping makes movement too unreactive.

#### Vector fields
(Called *spatial field* in lectures)
A multidimensional graph; at a given position we have a vector assigned.
A vector field can be used to read the force on an object w.r.t. it's position by finding the force vector at the object's given position.

Example vector field with trajectory:

![](misc/Pasted%20image%2020231015171424.png)



#### Cloth & Hair simulations
These are notoriously difficult (computationally and mathematically) to do properly.

- Mass-spring Model; Simulate the cloth or hair as a large quantity of small masses, connected by 'springs'; the more subdivision the greater the accuracy. For cloth this would be going in 2D.

![](misc/Pasted%20image%2020231015172057.png)


#### Spring forces example
![](misc/Pasted%20image%2020231015173002.png)

$$F(P_i,P_j) = K(L_0-||\vec{P_iP_j}||) \cdot \hat{\vec{P_iP_j}}$$ 
- K - elasticity of spring
- $L_0$ - Length of spring when under no force
- $L_0-||\vec{P_iP_j}||$ - Absolute difference in the length of the spring; This will be a *negative* value if *extended*.
- $\hat{\vec{P_iP_j}}$ - Normalised vector of the direction in which the spring is pointing - indicates the direction of spring force.

https://graphics.stanford.edu/~mdfisher/cloth.html




