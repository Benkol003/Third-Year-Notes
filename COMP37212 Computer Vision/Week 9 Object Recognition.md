we can treat object recognition as a search problem similar to text search i.e. a ML/NLP problem.

## Indexing with local features
primary way to search for an object or image

another application might be searching for a specific object in a video 

![](misc/Pasted%20image%2020240502195934.png)

have a database of images with descriptors built from e.g. harris+SIFT 

![](misc/Pasted%20image%2020240502192511.png)


build a reverse index s.t. given a descriptor / token / *visual word* can find images that contain it (similar to searching for text in documents)

![](misc/Pasted%20image%2020240502192309.png)

![](misc/Pasted%20image%2020240502192641.png)

we will have a lot of descriptors - descriptors of the same feature or object might be similar but not identical e.g. illumination/scale differences.

Build a model by quantizing the search space (this is more unsupervised learning?) or clustering similar descriptors - week 4 content - use GMM or mean shift models...

then only need to save the 'model' and match visual words to the best class in that model - reduces the database size.

![](misc/Pasted%20image%2020240502193012.png)

we can then use this model to build our *inverted file index* - we have a known set of words and we link images containing those words.

![](misc/Pasted%20image%2020240502193056.png)

for image search lookup features in the search image in the database, to get a list of candidate image results. can use correlation of images between words to rank i.e. sort images by max. number of hits or matching visual words to the target image.

![](misc/Pasted%20image%2020240502193135.png)

### Spatial verification

 two images may have lots of matching features but may not be the same:

![](misc/Pasted%20image%2020240502193809.png)

this can happen especially if we have a lot / bad visual words that are ambiguous.

the images should be the same if the features are arranged spatially similarly across both images.

![](misc/Pasted%20image%2020240502194037.png)

(i cant see how hough transform is relevant here or how its used with local features isnt explained...)

general system:

![](misc/Pasted%20image%2020240502195358.png)



### sampling strategies

![](misc/Pasted%20image%2020240502195446.png)

- using sparse, unique points e.g. harris will give low false positive rate, however low coverage and may be less successful at finding e.g. occluded/partial images
- use multiple operators for more coverage, but is still unique /reliable

- can use random or dense/grid locations to build descriptors from - high coverage and likely to successfully find image in search, however likely to be ambiguous and increase false positive rate; however has been employed successfully...

![](misc/Pasted%20image%2020240502195715.png)

![](misc/Pasted%20image%2020240502195951.png)


## Object categorisation

given an object that has been detected in an object, we want to categorise / describe and identify it.

![](misc/Pasted%20image%2020240502200127.png)

usually only have a small number of training samples for a category (as have lots of categories). we want to test if our image is or is not in a category.

![](misc/Pasted%20image%2020240502215500.png)

#### Object categories

![](misc/Pasted%20image%2020240502220044.png)

have lots of categories with lots of relations, tree structure...

typically describe an object at individual level i.e. most specific description e.g. dog and then more general abstract categories e.g. animal...

![](misc/Pasted%20image%2020240502220249.png)


some categories dont have a consistent definition or image either:

![](misc/Pasted%20image%2020240502220348.png)

(a horse could be a type of chair)

![](misc/Pasted%20image%2020240502220416.png)

we have two requirements of our model:

![](misc/Pasted%20image%2020240502220445.png)

![](misc/Pasted%20image%2020240502220502.png)

(annotating data is tedious)

### Bag-of-Word representations

![](misc/Pasted%20image%2020240502222733.png)

an image can be defined as a bag of 'visual words' i.e. bag of local features.

instead, given a set of possible local features (by e.g. quantising the feature space)

![](misc/Pasted%20image%2020240502222932.png)

and form a histogram or vector (using the set of defined features as dimensions).
we can still have irrelevant features in a different image e.g. for a picture of a violin there may be human face local features of the musician which are not relevant.
however on average the value will still be low.
Analogous to bag of words representation commonly used for documents.

**the histogram gives a representation vector that represents the image**, dependent on the number and quality of the visual words.

### Implementation of BoW model
#### 'Visual Categorisation with bags of keypoints'
[Visual Categorisation with bags of keypoints paper](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/csurka-eccv-04.pdf)

the paper explains it way better than the lecture slides

they have ~2000 images and 7 classes.

They compute SIFT descriptors on their images, get around a total of 600,000 for the entire dataset. They then reduce this down by performing k-means clustering - use the mean vector of each cluster i.e. k visual words to form our histogram.

then when you want to test an image you take all it's SIFT descriptors, match them to the nearest cluster, add it to the histogram / visual word vector which represents the image.

they then train both naive bayes and SVD models (SVD words well on very high-dimension data as here) on the dataset to map image vector of visual words to the correct categorie/s

![](misc/Pasted%20image%2020240502231543.png)

this graph shows the trade-off between the error rate and the number of visual words or k clusters for k-means in the model (for naive bayes anyway).


### Recognition ML models

i.e. use a model to map visual word vector to classification.  theres nothing much to learn here if you've done ML - lecture notes just recap the different models.

![](misc/Pasted%20image%2020240502234059.png)

![](misc/Pasted%20image%2020240502234126.png)

SVM's - as used in the paper for best performance

![](misc/Pasted%20image%2020240502234219.png)

#### spatial representation with BoW model

![](misc/Pasted%20image%2020240502234502.png)

very similar to NLP?. again they haven't told us how the fuck we do this...


![](misc/Pasted%20image%2020240502234637.png)
