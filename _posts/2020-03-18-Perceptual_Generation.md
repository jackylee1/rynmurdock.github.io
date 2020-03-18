# Perceptual Generation

I haven't posted anything here for awhile because I've wanted to nail this down.

After creating images that serve as sorts of archetypes of their
datasets, I wondered if something similar could be done to generate regular images. Yes, *like a GAN.* 
Except attempting to use 80 images and several hours, instead of 100s!

![tree-ish](/images/treesish.png)


I started with the idea that VGG16 could enforce diversity in style and/or content by doing a pairwise comparison of each image
within a batch. This could be useful for the current idea or for trying to prevent mode collapse in GANs by pulling features from
either a VGG model or the discriminator.
Later on, I realized that I could do a pairwise comparison between two generated batches, 
do the same for real data *and take the distance
between the pairwise differences.* 

This meant I could create diverse data with VGG16, a couple of images, and a generator (which happens to be a la StyleGAN here).

At first this wasn't very successful. But throw a to_gram_matrix() function around 
each difference and you're comparing non-localized
style. This works great if you're just trying to get the general feel of something, say a tree (see above), 
but it doesn't work so well
when you want faces instead of eerily skin-and-eyeball-like textures (see my Twitter! [@advadnoun](https://twitter.com/advadnoun)).

I thought that I'd be able to balance low- and high-level features in such a way that I could use just one loss function.

That didn't work!

So I wanted to make a pairwise, feature-based comparison that could place the different stylized elements within the image. 
But I wanted it to be compatible with my diversity function instead of constantly fighting it. 
I don't want another adversarial loss in my life ;)

To do this, I take the pairwise absolute difference between one batch of outputs and a double-stuffed batch of real data 
(it just has double the batch size number of images in it). 
I iterate through each output's distance from each real data point. I take the lowest 5 averages of those*, and if that 
real-data image hasn't already had its distance from a generated image used in the loss function, I add it and move on to
the next synthetic image. If the real image has been used, I always skip it.
After going through each successive synthetic image, if I haven't found a batch of image-difference means,
I start to add random ones.

*Inspiration in part from the Top-K GAN paper [here](https://arxiv.org/abs/2002.06224).


So what does this even mean? I am incentivising the generator to be similar to a particular photo in the double-stuffed dataset...
But not the same particular photo as the rest of the generated images! Otherwise, it's all randomly matched from there.

In other words, if the generated image has a match in its top 5 averages, that's good, and if it happens to be at the top of the
generated batch, then it'll immediately be paired with it. If it's in the middle, still pretty likely. If it's at the end, 
and the batch is varied,
its lowest best 5 errors should still match *something* that hasn't been already seen. 
Otherwise, we pull some image pairs that may be very different and suboptimal for the generator.

I've yet to do ablation studies, but this seems to work very well in aligning the diversity function and feature-similarity
function in terms of their ends.

![faces (not great)](/images/faces.png)
![faces](/images/ooalaldf.png)
![p good faces](/images/bac.png)


Finally, after tweaking the weight of some VGG16 layers (I'm looking at you, layer 3/relu1_2!) and creating a model that's
pretty similar to StyleGAN's generator, I've gotten things stable. This is my first PyTorch project, so be moderately kind when
viewing the source in this Colab notebook, which I've used to trained on 80 images from CelebA! It's totally undocumented
and probably very bad in some ways. I'll correct the documentation issue tonight, to some extent.

[Colab Notebook](https://colab.research.google.com/drive/1I8qkEsfWz3NaFDt_RkEup3otpfGEYvYa)

The technique is, I think, appealing in its speed and ability to work with small data. But it isn't perfect, as shown above!
Given more time, data*, or whatever else, it may be improvable. If you'd like to contribute, I'd love to hear from you.

*I've yet to even try it on more than 80 datapoints or with a batch size above 8 - who knows if it scales to be great or fails.

If you have any suggested modifications, tweaks, or questions, feel free to DM me on Twitter. 





