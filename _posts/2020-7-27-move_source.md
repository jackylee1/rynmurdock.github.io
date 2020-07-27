# Index-Differentiable Image Rolling

![A Kardashian](/images/256er.png)
![Kardashian flowery collages](/images/flowery_K.png)
![Kardashian flowery collages](/images/flowery_K1.png)


Over the past several weeks, I've been experimenting with image collaging using Artificial Neural Networks (ANNs). 
Directly learning parameters to reveal portions of an image
is relatively simple, only requiring you to learn some functions and create surfaces from them.
However, learning parameters to sample *from different portions of an image* is extremely difficult. 

As an example, it isn't too hard to take an image of a dog and learn to hide or show different portions of it; you could easily reveal the nose
and hide the paws based on some objective. On the other hand, learning to move the nose and show it in a different location using backpropogation
seems impossible. You end up wanting to somehow slice or roll the image, but have no way to learn the required indices!

## CPPNs & Memorization

I told this to [Kaai Kauwe](https://twitter.com/stevenkauwe), and within a day he had a clever solution: memorize the image using a special ANN 
that takes pixel coordinates as inputs and that outputs pixel values to form an image, then tweak those coordinates! 
These special ANNs, known as CPPNs (Compostional Pattern-Producing Networks 
[[see hardmaru's post for an in-depth intro](https://blog.otoro.net/2016/03/25/generating-abstract-patterns-with-tensorflow/)]), have been
around for awhile, but were recently brought to many researchers' attention by [SIREN](https://arxiv.org/abs/2006.09661). We've used SIREN networks in this work as
they seem to
do a better job at image memorization than previous CPPN variants. In particular, I utilized 
[this PyTorch repo's](https://github.com/dalmia/siren) implementation by Aman Dalmia.

![A Kardashian](/images/256er.png)
![Kardashian city collages](/images/city_K.png)
![Kardashian city collages](/images/city_K1.png)

## Input Constraints & Learned Rolling

So, the process begins by memorizing a set of images using a SIREN network. Next, we need to figure out a way to alter these coordinates.
If we allow the input to range from negative infinity to infinity,
we may run outside of the learned image coordinates. In fact, for this work, the coordinates were
pre-processed to be fractional, ranging from zero to one. In order to constrain the coordinates to reasonable values, we have to stay
within that range. In addition, we would like to sample from the image while keeping it intact as a whole. It would certainly be possible
to learn each pixel of the image, and then sample from those pixels in a way that doesn't keep the gestalt of the image; for this task,
we'd like for it to remain recognisable regardless of input magnitude. 
To this end, we decided to roll the image, restarting at zero (one side of the image) when the coordinates rise above one (the other side).

Our constraints to learn parameters of a function to roll an image are then as follows: output range bounded from zero to one, 
input domain from negative infinity to infinity, and periodicity that rolls from zero to one to zero to one, repeating forever.

![Sawtooth waves from Mathematica user Dadan Ari Wibowo](https://i.stack.imgur.com/FbFAt.jpg)


These constraints lead us to a wacky cross-disciplinary solution: if you're familiar with music synthesis, the sawtooth wave is a function that meets
this description quite well! 
An approximation (found on [this StackExchange question](https://mathematica.stackexchange.com/questions/38293/make-a-differentiable-smooth-sawtooth-waveform)
along with the image above) provided by [J. M.'s technical difficulties](https://mathematica.stackexchange.com/users/50/j-m-s-technical-difficulties)
can be utilized that allows the function to retain these qualities (mostly) while remaining differentiable. With only a small amount of translation and scaling,
the function provides proper inputs for any real number used to shift the input. Some warping can be seen around the seems of the rolled image - in the future it may
be possible to define a piecewise function and its gradients or even just find a closer approximation to reduce the warping.




## Tying it Together

![Unrolled](/images/rolly.png)
![Rolled image](/images/roller.png)

Putting the SIREN network and this particular sawtooth function together, we can simply create two tensors of linearly-spaced numbers from zero to one
that represent x & y coordinates, then add some learned parameters to these two vectors, shifting and rolling them with the function. 
Ultimately, we repeat the x & y coordinates and concatenate them to create a vector of shape (rows, columns, 2) where the rows and columns represent
each pixel, and the last dimension represents the pixel's coordinates.

Doing all of this allows us to learn to roll an image based on some objective. This is extremely useful for my collaging project, but
we expect it could
also be useful for others' purposes as well. For instance, one could learn a translation/roll of an image adversarially in an attempt to increase
a classifier's robustness to such perturbations. Future directions for this work are mainly bottlenecked by the requirement of learning
all the images with a CPPN - a step that requires some time and a significant amount of memory. On the other hand, this step may become increasingly 
less difficult as raw compute continues to increase, and is already fairly easy with small images.



