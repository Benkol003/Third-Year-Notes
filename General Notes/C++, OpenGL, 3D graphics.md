# C++
## Templates
Templates allow generic typing of classes & functions:
```cpp
template<typename T> T func(T t){
	return ++T;
}
```
And create template instances
```cpp
func<int>(1);
func<float>(2.5f);
```
- Each template instance is considered a different type.
- The compiler will expand the template create a class/function with each type signature that is used in code.

- template instances for functions are equivalent to function overloading (but done with less code!)

## Typename
`typename` keyword:
- can be used interchangeably with `class` in `template <typename A>` <==> `template <class A>`, with the exception of template template arguments pre-C++17
- Used for instantiating specific template specialisations
- accessing elaborated template types

### Template specialisation
https://www.ibm.com/docs/sr/zos/2.4.0?topic=only-explicit-specialization-c
#### Full template specialisation

Allows you to create an override for a template with specific parameters. You don't even need to define the primary template:
```c++
template<class T> class A; //declaration of primary template
template<> class A<int>;//specialisation dec

template<> class A<int> { /* ... */ };
```
The compiler will then use the specialisation instead of generating one from the primary template.

#### Partial specialisations
I.e. reducing the no. of template arguments:
```cpp
template<class T, int i> class A {
   int x;
};
template<class T> class A<T, 5> {
   short x;
};
```
- Partial specialisation can only be done for classes; for functions this is equivalent to function overloads.

Invalid:
```cpp
template<class T, class U> int A() {
    return 0;
}
template<class T> int A<T, int>() {
    return 0;
}
```

valid function overloads:
```cpp
template<class T,class U> int A(){
	return 0;
}

template<class T> int A(){
	return 0;
}
```

### template template arguments
https://www.ibm.com/docs/en/zos/2.4.0?topic=only-template-template-arguments-c
https://en.cppreference.com/w/cpp/language/template_parameters (template template arguments section)
- You can pass a template, as an argument to another template!

- Template specialisations of a class are not considered when passed as template template arguement.
This code is valid:
You can use the template specialisation in code
```cpp
template<class T, class U> class A {
public:
   int x;
};
template<class U> class A<int, U> {
public:
   short x;
};

template<template<class T, class U> class V> class B {
   V<int, char> i;
   V<char, char> j;
};
B<A> c;
```
but this code is not:
cant use the specialisation as a template parameter
```cpp
template<class T, int i> class A {
   int x;
};
template<class T> class A<T, 5> {
   short x;
};
template<template<class T> class U> class B1 { };
B1<A> c; //the a<T,5> specialisation is not considered.
```

### Elaborated template types
https://en.cppreference.com/w/cpp/language/elaborated_type_specifier
```cpp
template<typename param_t>
class Foo
{
	Foo(){
	typename param_t::value_t sub;
	}
};
```
where you are accessing a scoped typedef
```cpp
class param_t{
typedef unsigned int value_t;
}
```

Without the typename specifier, compiler thinks `param_t::value_t` is accessing a static variable.

### Variadic templates
https://en.cppreference.com/w/cpp/language/parameter_pack
Man, this is a whole another beast of syntax
Done via use of C's ellipsis (`...`) to specify *(template) parameter packs*:

```cpp
template<class... Us>
void f(Us... pargs) {}
template<class... Ts>
void g(Ts... args){
    f(args...); // “args...” is a pack expansion
}
```

- Can get parameter time at compile time via `sizeof...` operator

# OpenGL
## Computing camera axii
to do this calculate the cameras axis from the cross product of a general up vector $u$ (0,1,0), the direction vector $d$ i.e. where the camera is looking at.
- The up vector doesn't have to be perpendicular to the direction axis - what matters is its orientation around the direction axis

using cross products:
- camera right axis (local x axis)$r$ = $u\times d$ 
- camera local y axis = $r\times d$

## Transform matrices

given an object model in local space - cantered around origin, scale 1 - We have Model - View - Projection transform matrices.

Move object to world space - transform into camera space (rotation, scale, translation) - apply type of camera projection.

#### Interaction of rotation, translation, scaling

Invariancy:
- rotation is invariant to later scaling
- scaling is invariant to later translation

For moving a model from local space to world space, you wanna do translate X rotation X scaling.

- To iteratively update a model in world space you need to use quaternions, otherwise you will get [gimbal lock](https://en.wikipedia.org/wiki/Gimbal_lock). You may / not use this for camera rotation aswell as gimbal lock is intuitive for a camera as in fps games.

Cameras don't have scaling, instead a projection matrix.

Camera transform is projection X translation X rotation.

Scaling, which is typically applied to a local space model, should typically be applied before rotation - in the case where theres different scaling on different axis, applying the rotation afterwards also rotates the scaling axes and will stretch along the wrong axis.

### Camera Transforms / caveats
There is a caveat for updating camera transforms - you want to do them with the local axis reference, not the xyz axis. apply the current orientation to translates/rotations you want to apply. to rotate xyz axis to local axis.

To apply the camera transformation we apply the inverse camera matrix/ inverse transform to get to camera.
We can avoid directly computing the inverse matrix:
- Apply reverse rotations and transforms  i.e. -angle,-direction
- apply in order *translation X rotation* to world models
- Camera zoom e.g. 8X is achieved by dividing the FOV. (you could also do this via scaling)



## Regular polyhedra & icospheres

An icosahedron is a polygon of 12 equilateral triangle faces.

![](misc/Pasted%20image%2020240327181904.png)

from [wolfram](https://mathworld.wolfram.com/RegularIcosahedron.html);
An icosahedron can be constructed by placing a top and bottom point e.g. (0,+-1,0); then constructing the remaining verticies by:
1. Placing two circles at heights $\pm\frac{\sqrt{5}}{5}$ 
2. Construct 5 equally spaced vertices ($72\textdegree$) on each circle - the points on each circle differ by phase $36\textdegree$.

![](misc/Pasted%20image%2020240327182025.png)

#### Icosphere

Each vertex on the icosahedron already lies on the surface of a sphere around the centre, with radius 1.

An icosphere is produced by subdividing these faces and lifting the new triangles to sphere surface / triangle centre to object centre equals radius.

![](misc/Pasted%20image%2020240327182621.png)

We can generate triangles all of the same size for a consistent surface and minimal triangle count.

![](misc/Pasted%20image%2020240327183015.png)

This is actually part of the family of [geodesic polyhedrons](https://en.wikipedia.org/wiki/Geodesic_polyhedron); we can also get a sphere from subdivision of a regular tetrahedron/pyramid (but requires more subdivision) - this is easier to generate.




## Vertex Array Objects

A VAO is just a specification on how to send data as input arguements to your shader program.


A VAO stores:
 - A set of bound buffers
 - Access patterns set via `glVertexAttribPointer` to those buffers to map data to you input variables in you shader program,. e.g. `layout (location = 0) in vec3 pos;`

A VAO is used when you have different vertex formats (& accompanying buffers) (may/may not be for different shader programs.)

### Buffer changes without restating vertex format

If you have buffers in the same format, don't use different VAOs. Instead

(This is only available in GL 4.3 - graphics cards from 2013 >GTX 7 series)

1. specify the buffer format with `glVertexAttribFormat`
2. create intermediate *bind points* with `glVertexAttribBinding`
3. You can then swap out buffers without costly changes to the vertex format with `glBindVertexBuffer` to bind your buffer to the bind point.