
### Global illumination / diffuse in ray tracing
For a perfectly reflective surface, for any reflected light; incoming angle = outgoing angle.

![](misc/Pasted%20image%2020231203160339.png)

And in basic ray casting, every time a ray hits a surface we may bounce one singular ray according to this. However ray casting is not capable of
- Global illumination: for rough surfaces, with any kind of diffuse/global this is not the case, and the reflection looks more like this:

![](misc/Pasted%20image%2020231203160825.png)

- Soft shadows: not all light sources are point lights; therefore a point may be illuminated by a partial area of a light:

![](misc/Pasted%20image%2020231203162817.png)


These factors are modelled by the BRDF (but not utilised by ray casting): The amount of light reflected from one angle of incidence, for any angle of reflection will be captured within an accurate BRDF of the surface.

A naïve way to model this is when reflecting a ray, to add some noise within a certain variance to the surface normal or outgoing angle; we use a surface *roughness* value as the variance of the noise value used. This will result in a noisy image if we only use a single output ray.

### Path Tracing
(Also appears in Kajiya’s original paper for the rendering equation.)
([Youtube video](https://www.youtube.com/watch?v=NIpC53vesHo&t=1255s) comparing ray and path tracing.)
([Game dev blog](https://www.scratchapixel.com/lessons/3d-basic-rendering/global-illumination-path-tracing/global-illumination-path-tracing-practical-implementation.html))
This improves upon the previous point by producing multiple reflected rays in a range; we use a *Monte Carlo* approach and the angles of the reflected rays are chosen *randomly*.
Naively we would use a uniform random distribution in all directions; this is ideal for diffuse surface.
But for reflective surfaces will produce noisy specular highlights, ideally you would want to use a gaussian distribution; 
*Importance sampling* will use a *probability distribution function* (PDF) based on the form of the surface BRDF.

You can see that the image noise with a higher number of samples:

![](misc/Pasted%20image%2020231203162639.png)

and you can notice this effect if you have a complex scene in blender and move the camera around.
Instead of using higher samples, a denoising algorithm may be used, which is computationally faster (but is not as good visually).
### Texture Baking

Ray tracing methods, especially path tracing, take an especially long time to render; even with real time ray tracing with RTX it only used for components such as [ambient occlusion](https://youtu.be/Rk5nD8tt_W4?si=nokY0bY17lkf_Ve_&t=241) specular highlights, caustics; & rasterization is used for general shading. See this [Nvidia video](https://youtu.be/Rk5nD8tt_W4?si=DdkNJSmTOR04HNhd&t=276). (In the ambient occlusion case we produce rays from all pixels as background light and test how many rays are capable of reaching an escape distance if not in shadow).

The ray traced lighting can be baked into textures such as this floor plane here; 
TODO how is it done? I'm presuming on the texture we store the colour for rays sent from the ray to the texture, or we could do path tracing from each pixel?


![](misc/Pasted%20image%2020231203180719.png)

However the lighting is no longer dynamic as we can see here by the shadow not moving:

![](misc/Pasted%20image%2020231203180749.png)
