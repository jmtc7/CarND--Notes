# Color and gradient thresholding

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

## Smart edge discrimination

We have already used Canny to detect pixels that, according to their gradient, are very likely to be a part of an edge. However, lane lines have really steep slopes, so we could use this to discriminate other more horizontal edges, such as the ones generated by cars or the landscape.

Canny uses the Sobel operator, which is a convolution kernel that extracts vertical (*Sx*) and horizontal (*Sy*) edges from the image. The bigger we make this kernel, the bigger area will be considered for computing the gradients, so they will be smoother.


In *apply_sobel.py*, a function that applies a sobel in whichever axis is told is implemented. It thresholds the output to keep only the gradients in a certain range.

The *gradient_magnitude.py* script applies Sobel in both axis and computes the combined gradient magnitude. It thresholds the resulting output aswell.


We can apply Sobel in the direction in which the lane lines usually are. The direction of the gradient can be computed by the inverse of the tangent of the y gradient divided by the x one (*arctan(sobely/sobelx)*), in radians, 0 being a vertical line. Using the gradient direction, though, is way more noisy than its magnitude. This is done in the *direction_sobel.py* script.

The script *multi_threshold_sober.py* combines all the masks explained previously (X and Y Sober and magnitude and direction of the gradients) to provide with a more exhaustive thresholding of the image.


## Color spaces

There are more options to represent images than the classical RGB format. Two popular alterative formats are **HLS** (Hue, Lightness and Saturation) and **HSV** (Hue, Saturation and Value). The aim of both of them is to separate color and brightness to make the representation closer to the way we interpret colors. In both spaces *hue* represent color regardless its intensity and brightness. Again, in both spaces, *Saturation* represent the *colorfullness* of the represented tone (from more white/palid to more intense/pure colors). Finally, *Lightness* (in HLS) and *Value* (in HSV) represent how bright they are (far from black).

The main difference between HLS and HSV is that HSV is represented as an inverted cone in the 3D space, while HLS is a bicone (with black at the bottom and white on the top).

The S channel seems to have a really good and robust response when it comes to detect lane limits in complex images (with shadows, illumination changes, etc.), specially for the yellow lines. The white ones are also well detected when using the R channel of an RGB representation.


## Cobining thresholds

Given the results obtained so far, we could mix different thresholding techniques to compensate the weaknesses of one methods with the strengths of others. In the *hls_and_gradient_thresholding.py*, an S-channel and a X-Sobel thresholds are combined to get solid and clear lane lines in the outputted mask. This introduces quite a lot of noise in other zones of the image, but this can be removed by applying a RoI mask.
