### Electromagnetic Radiation & colour Reproduction

- All EM radiation in the visible spectrum has a wavelength of 450-700nm.
- The colour of an object depends on the *illumination spectrum* combined with the *reflectance spectrum of the material.*

![](misc/Pasted%20image%2020231014143248.png)

- Human eyes have 3 cones in their eyes to see; **L, M and S**. These see colours of blue, green, and reddish-orange respectfully.

![](misc/Pasted%20image%2020231014145152.png)

### Colour Mixing

WE can form any visible colour using Red, Green and Blue colours.

For illuminating light such as coloured light sources we use additive colour:

![](misc/Pasted%20image%2020231014145401.png)

For Reflected light such as paints (or most surfaces), we use subtractive colour - CMYK colour is commonly used for this:

![](misc/Pasted%20image%2020231014145918.png)

 #### Cone Colour stimulus
 WE can see that the response curves for cells overlap with each other:
 
![](misc/Pasted%20image%2020231014150128.png)

I.e. for blue we still stimulate M and L cones by a small amount.
Colour mixing and choice of primary colour focuses of the *ratio* of stimulus for each cone to achieve the desired colour.

### Measurement of light

**Light intensity**: In units of **Lux**, is measured as the area under an intensity graph.

Light spectra of varying intensity:

![](misc/Pasted%20image%2020231014153711.png)

**Hue**: Describes the overall colour of a spectra; is measured by finding the *mean*.

Hues of blue, yellow and red:

![](misc/Pasted%20image%2020231014153828.png)

**Saturation, or Chroma**: Amount of *achromatic* light - light that deviates from the main hue. Is measured as the deviation of the spectra from its hue. A colour with a *higher* saturation will have less deviation from the hue and appear less white/more colourful.

![](misc/Pasted%20image%2020231014154029.png)

### Colour Spaces

Is a specific organisation or model of colours, usually 3-dimensional.

RGB is an example - we represent a colour via 3 parameters that control the amount of red, green and blue primaries.

Not all colour spaces can reproduce *all* possible colours that are visible to the human eye.

**CIE XYZ colour space**: Colour space designed to be able to reproduce all visible colours. Often used as a reference by other colour spaces to indicate how well they can reproduce all colours.

**Gamut** is a subset of visible colours; different colour spaces will have better gamut's to represent more colours.

![](misc/Pasted%20image%2020231014161157.png)

### CIE RGB & XYZ colour spaces

(https://reflectives.averydennison.com/en/home/learn/learn-sheeting/cie-1931.html has a good overview)

Experiments by the CIE in 1900's were used to test the ability to accurate reproduce colours from RGB primaries:

#### Experiment Overview

![](misc/Pasted%20image%2020231014162016.png)

An observer would have a reference colour of light of a known wavelength;
In the Adjustment field there were 3 sources of RGB colour lights; the person would adjust their intensity to try reproduce the original colour.

It was noted that certain cyan colours could not be reproduced; so red was added to the reference field and asked to adjust both fields until they matched.

![](misc/Pasted%20image%2020231014162258.png)

The results of the experiments were used to make **colour matching functions**.

**Colour matching functions:** For a wavelength we use these functions to calculate the amount of red, green and blue light needed. This is known as the **CIE RGB colour space**.

![](misc/Pasted%20image%2020231014162550.png)

For colours that *cannot* be reproduced you can see we have a *negative* amount of red light. 

This is fixed by the using a different colour space, **CIE XYZ**.

We have three new *colour matching functions*, $\bar{x}(\lambda), \bar{y}(\lambda), \bar{z}(\lambda)$.   

![](misc/Pasted%20image%2020231014163325.png)

(What are X,Y & Z? No fucking clue!)

X, Y, & Z are known as *tristimulus values*.

Are obtained by some complex math via integration of the colour matching functions $\bar{x}(\lambda), \bar{y}(\lambda), \bar{z}(\lambda)$ (DON'T need to know this.)
(https://en.wikipedia.org/wiki/CIE_1931_color_space#Computing_XYZ_from_spectral_data)

This is the visible colours on a CIE xy chromaticity diagram:

![](misc/Pasted%20image%2020231014163826.png)

Where

![](misc/Pasted%20image%2020231014163904.png)

x,y,& z add up to 1. (z not displayed on graph as z=1-x-y i.e. fixed).

**Converting between CIE RGB & XYZ**: We use the following set of matrices:

![](misc/Pasted%20image%2020231014163530.png)

(Can do this for other colour spaces e.g. converting to sRGB).


### Gamma Correction

Because human eyes are sensitive to *ratios* of colour or light;
For a linear encoding of intensity, human eyes are more sensitive to changes in dark light (Weber-Fechner law).

**Gamma correction**: Uses a non-linear / power mapping for intensity values so that the *perceived intensity change* happens linearly. 

You can see a linear encoding has larger banding at low intensity, but the gamma corrected version is consistent.

![](misc/Pasted%20image%2020231014165732.png) 

For Gamma value $\gamma$, scaling occurs by $V_{out} = AV_{in}^\gamma$. (A is a scaling factor where usually $A=1$.) $\gamma=2.2$ is commonly used.