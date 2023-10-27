# Simulation

### Rigid Body Simulation
For objects we have an actual geometry/shape.
**Degrees of freedom**: Whereas a particle system has 3 degrees of freedom (DOF) in (x,y,z); A rigid body model has 6 DOF's for both position and rotation about axes (x,y,z).

![](misc/Pasted%20image%2020231026160827.png)

#### Collisions
For collisions we model forces at specific points on the geometry to cause a change in velocity, but which also induces rotation/torque on the body.
Because forces are applied over time we can combine this into *impulse-based collisions*; We can also use impulse on mass=spring systems.

#### Rest in Contact
For the rock resting on the surface, if we calculate impulses continuously it may become unstable & will bounce/wobble on the surface; For a resting object we stop calculating impulses until there is an external action.

![](misc/Pasted%20image%2020231026162301.png)

We can also stack multiple objects to be in rest as the same time.


#### Articulated rigid body simulation

![](misc/Pasted%20image%2020231026162754.png)
Used for simulation of humans, robots, cars...
Each arm has 6 DOF; however the joint has a *joint constraint*; only 1 DOF being the rotation/angle between the two arms in one axis. The articulated system has a total of 7 DOF.

### Deformable Object Simulation
Used for simulation of elastic materials:
![](misc/Pasted%20image%2020231026164419.png)

#### Finite Element Methods (FEM)
This is used to compute partial differential equations over time;

#### Tetrahedralization
We take a 3D object and decompose into 3D tetrahedrons (similar to triangulation).
![](misc/Pasted%20image%2020231027130833.png) ![](misc/Pasted%20image%2020231027130904.png)
Tetrahedron:
![](misc/Pasted%20image%2020231027131013.png)
We then use FEM to calculate forces/impulses that cause tetrahedron deformation:
![](misc/Pasted%20image%2020231027131129.png)

#### Fracture & Cloth simulations
![](misc/Pasted%20image%2020231027133650.png)
![](misc/Pasted%20image%2020231027133702.png)

Both of these can be modelled with either FEM or mass-spring effectively.

### Fluid Simulation
https://quangduong.me/notes/eulerian_fluid_sim_p1/
https://www.cs.cornell.edu/courses/cs5643/2015sp/a1PositionBasedFluids/
#### 1. Particle based
We use particles to simulate the behaviour of the fluid; at rendering we wrap all the particles in a mesh to make it look fluid.
This is known as a **Lagrangian fluid simulation**.

![](misc/Pasted%20image%2020231027134101.png)
![](misc/Pasted%20image%2020231027134111.png)

#### 2. Grid based
We discretize the space into cells, and track different fluid properties such as velocity & pressure; is known as **Eulerian fluid simulation**.

#### 3. Hybrid Technique
These use both models simultaneously, where we usually model most of the mass of the fluid with eulerian, as it is accurate enough; but then for parts of the fluid near the surface we apply the Lagrangian simulation to model the surface mesh properly.

# Collisions

#### Detecting Collisions
Because we are checking for collisions are discretized timesteps (every frame), we will detect collisions after they occur.
![](misc/Pasted%20image%2020231027141354.png)
Therefore once we detect a collision, we can:

1. backtracking to the time when the collision occurred.
![](misc/Pasted%20image%2020231027141447.png)
2. Fixing the position to the surface at the time we detect the collision. Is more of a hack and is inaccurate, but computationally faster.
![](misc/Pasted%20image%2020231027141540.png)

### Pruning possible colliding objects.
For N objects, we can naively assume that they may all possibly collide with each other; this leaves us to check $O(N^2)$ possible collisions. However we can improve this.

- Static objects: DO not need to check if they collide with any other objects, or between static objects (if they are not moving); Collisions will only happen with other moving objects,
- We can make an assumptions about when objects wont collide with each other; if two objects are far apart and moving slowly, or moving away from each other.

### Bounding Volumes
Method to allow us to efficiently check whether two objects are overlapping (& therefore have collided). We encase the object mesh in a bounding volume, which can be different shapes.
Listed in increasing complexity order (For checking collisions):
![](misc/Pasted%20image%2020231027142411.png)
They are very efficient especially where we have an object with a mesh of complex geometry.
![](misc/Pasted%20image%2020231027142554.png)
However calculating the exact time/point of intersection is inaccurate as is, so we usually use backtracking after this.

#### Bounding Volume Hierarchies (BVH's)
![](misc/Pasted%20image%2020231027142920.png)

For a complex object we can have bounding volumes within a larger bounding volumes for more accurate collision detection; as it allows us to calculate a local area on the mesh where the collision occurs; called a **Bounding Volume Hierarchy (BVH)**.
These are also more typically used to have bounding volumes to encase groups of objects that are close together, and check to see if there are collisions with that group.
![](misc/Pasted%20image%2020231027143409.png)

