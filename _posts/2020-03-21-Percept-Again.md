# Perceptual Generation Redux

This is a follow-up to [my previous post](https://rynmurdock.github.io/2020/03/18/Perceptual_Generation.html) on generating
images given a dataset of only 80 points using a Style-GAN-styled generator and VGG16. I'm posting this because of same major
improvements. The system now has only 1 loss function and produces much more realistic results.

![faces](/images/facs.png)


To get these, I now take the absolute pairwise difference between 2 batches of real data. I then do the same for synthetic, 
generated
images. Finally, I take the mean of the absolute value of the gram matrix of the difference between those two differences.


It's much easier to see what I mean in action, so here is the 
[Colab notebook](https://colab.research.google.com/drive/1LFlWqG3eb0GArkceG8BdR9Ca3Se5-KEE): you'll want to look to the function 
`sim_to_dif` 
for the secret sauce.

There were a couple of additional tricks and changes: I found that the practice of replacing max pooling with
average pooling in VGG16 created artifacts - I either
implemented it incorrectly, or it's a generally bad idea.

I also found using the layers 8, 15, and 22 to be most effective. Adding in 29 for awhile created long, curly artifacts. 
Though there is
definiely a lot of room for playing around with layer choices.

Finally, you can look to my Twitter [@advnoun](https://twitter.com/advadnoun) for some videos of spherical-lerping the latent space. 
Interestingly, the light source 
projecting onto the face rotates as you move through, and the transitions are quite smooth.
