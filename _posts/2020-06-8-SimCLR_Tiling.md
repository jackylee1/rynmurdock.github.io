# SimCLR for Tiled Collages

![Whomst lady is this? Tiled cuts of faces](/images/download%20-%202020-04-28T175142.405.png)

This post outlines how to use SimCLR[1], 
specifically this[2] pre-trained model,
to create tiled collages[3] using Perceptual Loss[4]. 

This is interesting for two reasons:
1. It has not been previously shown (to my knowledge) that SimCLR can be used effectively for Perceptual Losses.
1. The collages this produces can be quite visually interesting!

Assuming you know a thing or two about Artificial Neural Networks,
for background let's begin by briefly explaining how SimCLR operates and how Perceptual Losses are used: 

### SimCLR


1. A set (preferrably quite large) of images is taken from a dataset. These images are augmented twice so that each augmented 
image has a corresponding, paired
augmented image within the batch that came from the same original image.
1. A ResNet with some fully-connected layers on top of it produces a representation of each image.
1. These representations are compared and a loss is created such that the network's error is based on how similar those paired 
images are
to each other and how different they are from other images. The network learns to create very similar representations for matched
augmented images and very different representations for augmented images originating from different images.

Ultimately, this leads to a neural network with useful, information-rich representations that can be used to classify images
with very little fine-tuning, as well as
a number of other tasks.

### Perceptual Loss

Now, Perceptual Loss entails taking a trained neural network (often VGG16, but we use SimCLR here), pulling the activations of 
intermediate layers of the network, and comparing
those activations created by different images. Why do this instead of pixel loss? That is, why not just subtract the different
Red-Green-Blue
values between images?
The reason: Perceptual Loss is not blurry, and it picks up on higher-level features. The latter of these points is demonstrated here.

## Tiling Collages

This work uses SimCLR to construct a Perceptual Loss Function that matches patches from some dataset (or datumset, if you'd rather 
use only a single image) to tiles of some target image, producing a new image. 
This allows us to retain a gestalt of the target image with tiles that contain
semantically related content, but with details that can drastically deviate from the target image.

![Whomst lady is this? Tiled cuts of faces](/images/download%20-%202020-05-31T213139.780.png)


The actual process is pretty straight forward: pull a couple of patches from random locations in images in the source dataset; run them
through SimCLR and pull intermediate layers' activations; compare those activations to those of the tiles of the target image;
update the produced image if any of the new patches are a better fit to the target image than previous patches.

It may be tempting to some to call this "evolutionary computing", but it's really just brute-forcing/random-searching!
If you'd like more details regarding the process, 
see this[5] Colab notebook!

![Whomst lady is this? Tiled cuts of faces](/images/download%20-%202020-05-31T230129.853.png)

## Notes

1. https://arxiv.org/abs/2002.05709
1. https://github.com/sthalles/SimCLR
1. Robbie Barrat (https://twitter.com/videodrome) has done previous work on "repairing" 
and collaging artwork using, by my understanding, GAN discriminators - their work heavily influenced this.
1. https://arxiv.org/pdf/1603.08155.pdf
1. https://colab.research.google.com/drive/16FCAltI9NeIN9vtIxMLY6NbY-tsEvRu6?usp=sharing


