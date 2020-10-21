# Rotation-Scale-invariant-CNN
Convolutional neural network (2D index) approximate invariant properties implicitly by max-pooling. Thus filters fail to generalize with drastic changes in scale and rotation. In this page, I show 2D convolution on **`Frequency`** &amp; **`Direction`** is a natural way to think of scale and rotation.

# Pros & Cons

* Pros:
  1. Images size are normalized at preprossing stage, filters are now capturing patterns between images of different sizes.
  2. GlobalMaxPooling makes pattern recognition simple, no longer need to stack deep convolutional networks.

* Cons:
  1. Fourier series are not `correctly` used here. Obvious, geometric frequency series is not orthogonal. 
     This could be approximately solved by [`Equal Temperament`](https://en.wikipedia.org/wiki/Equal_temperament) & `dilation convolution` on Frequency dimension.
  2. Smaller propotional area of the desired pattern in the image gives a weaker signal. I try to adjust it by quadratic growth rewards.

# Background
Fourier transform is a decomposition method that can decompose any function into weighted sum of series of characteristic functions of the heat equation.
In 2D, chararcteristic functions are wave-like, each wave has two parameters **`Frequency`** and **`Direction`**.

<img src="icon/characteristic.png" width=600>

# Image Preprossing
Instead of using ![](https://latex.codecogs.com/svg.latex?cos(k\omega),%20k\in%20N),
geometric decay series was used here for scaling property.


![](https://latex.codecogs.com/svg.latex?T:%20img%20\mapsto%20|F|%20\times%20|D|)

In preprossing stage, images were decompose into linear combination of series of waves with differ frequencies and directions. Store them as a 2D array.

![](https://latex.codecogs.com/svg.latex?a%20\cdot%20cos(\omega)%20+%20b%20\cdot%20sin(\omega)%20=%20\sqrt{a^2+b^2}%20\cdot%20cos(\omega%27)=c%20\cdot%20cos(\omega%27))

Notice that only `c` are considered features. **Dropping phase parameter is actually a hash method of location invariant**, because pattern in different locations are now in the same bucket (feature combination).

<img src="icon/padding.png" width=600>

# Invariant Properties

If we apply 2D convolution on these feature map, with `kernel size = (n, d)`. #`d` had better equal to number of direction.

We can see that left-shift of filter on **`Direction`** is a clockwise rotation, up-shift on **`frequency`** means scaling up. Since we choose a small decay rate, almost all  scales are considered.

<img src="icon/conv.png" width=600>

# Equal Temperament

Equal temperament are first invented for [harmonic melody](https://www.youtube.com/watch?v=cyW5z-M2yzw&ab_channel=3Blue1Brown), due to its approximation of simple whole number ratio. 

Take 12-ET as example: 
+----+----+
|d^0|1|
|d^12|2|
|d^19|3|
|d^24, d^28, d^31, d^34, 36, 38|

# Stacked filter 
Since composition of convolution operator is another convolution(another filter on original space), of course you can stack multiple filters.

# Challenge 
If only part of the image shown in the picture, signal maynot strong enough for filters. 
Need some localization pattern detecting, Which is time domain convolution (traditional one), to detect wave-like pattern.
