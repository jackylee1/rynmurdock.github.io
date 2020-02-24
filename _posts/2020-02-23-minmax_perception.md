# Min-Maxing Perceptual Losses

While working on my glyphs, I mentioned [in this post](https://rynmurdock.github.io/2020/02/18/1CCN_SVD.html) that it would be
interesting to try perceptual loss as a means of enforcing image discriminability. 
After trying it, I've put aside glyphs for the time being 
in favor of just using perceptual
loss to create interesting generative Neural Networks (NNs).

![Maxing image difference](/images/percep_vgg.png)

The images above are made by maximizing the perceptual difference between each image. Unfortunately, they all are quite saturated
and a bit bright. The discriminability for humans isn't very high.

So how can we make these images less saturated and more natural? We continue maximizing the difference between each output
but we minimize the difference between the outputs and images from a dataset.

![Min-maxing realness and difference](/images/minmax.jpeg)

The results are, in my opinion, visually quite interesting. 

For the sake of clarity, here are some additional details: this method adopts the use of a BigGAN conditional 
discriminator instead of vgg16 or vgg19. Also, 
there is an additional loss term to minimize noise,
much like you would see in DeepDream images. This can be thought of as a technique that is very akin to DeepDream in some ways,
though I think the aesthetic of the results is quite distinct.

Finally, here are the results of this technique being applied with one additional loss term: patches of the images are 
randomly sampled
and compared to other patches within the same image. This means the NN will try to make the image as perceptually diverse
across pixels as possible.


![Min-maxing realness and difference](/images/wowie.jpeg)
