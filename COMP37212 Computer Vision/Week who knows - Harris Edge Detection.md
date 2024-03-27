
### Suppressing local minima

For each pixel, if that pixel is the maxima in the local region (e.g. 7x7 window) centered over the pixel, retain, otherwise suppress. I.e. 

![](misc/Pasted%20image%2020240325154352.png)

This is different to dilation (cv2.dilate) or sliding max filter - we consider the pixel at the centre of the window with its surrounding - we *don't* choose the maximum in the region.