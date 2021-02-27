# Thoughts on DeepDaze, BigSleep, and Aleph2Image


![A painting of a building (possibly in my area?)](/images/EujAWlfUUAI_PuA.png)

For the last month or so, I've been working on text-to-image using pretrained models. The repeating theme of this work is using different networks to generate 
images from a given description by matching the images' and description's agreement using a neural netowrk called CLIP. 

For those who don't know, CLIP is a model that was originally intended for doing things like
searching for the best match to a description like "a dog playing the violin" among a number of images. By pairing a network that can produce images (a "generator" of some sort)
with CLIP,
it is possible to tweak the generator's input to try to match a description.

The first iteration of this idea used a SIREN network as the generator, and was called "DeepDaze" in part because the images have a fever-dream, hazy quality.

The next used BigGAN and was called BigSleep as an allusion to DeepDream and the surrealist film noir, *The Big Sleep*. The second reference is due to its strange, dreamlike quality.

Finally, Aleph2Image uses the VQ-VAE discrete decoder from DALL-E. It's named as such to reference The Aleph, a short story by Borges about 
an object that can let you see all of the world all at once. In addition, it includes a quote on surprise from the short story:

> "I was afraid that not a single thing on earth would ever again surprise me"

## Surprise

This was not only to joke about how generative artists often become 'immune' to some of the awe experienced by people first confronted with neural or generative art, 
but also to point towards 
the general idea of surprise and what makes this kind of experience interesting. There are classic ideas from the likes of Turing and Lovelace about what makes
humans special. Particularly Lovelace zeroed in on ideas surrounding original creation and surprise. 

One of the first times I've felt truly surprised by a system in quite awhile was
upon typing `a horse with four eyes` and seeing it interpret the prompt as a horse having glasses. Not only were the capabilities interesting, but seeing the system
riff on and misinterpret the prompt just blew me away! But the fact that I had no intent of creating this humorous image raises questions.

![a horse with four eyes from BigSleep](/images/horsefoureyes.png)


## Authorship

I have no idea if I can really say that I've "made" any of these images. I would still argue that the user owns the output, but it's difficult to say that they
originated it when the system can so radically control the process.


![an Aleph2Image moonlit scape](/images/EvNZmvtUcAEqvh2.png)

## To conclude

If you'd like to "create" a surprising image from a description 
feel free to use these notebooks! 

https://colab.research.google.com/drive/1oA1fZP7N1uPBxwbGIvOEXbTsq2ORa9vb?usp=sharing

https://colab.research.google.com/drive/1NCceX2mbiKOSlAd_o7IU7nA9UskKN5WR?usp=sharing

https://colab.research.google.com/drive/1FoHdqoqKntliaQKnMoNs3yn5EALqWtvP?usp=sharing



## Notes & thanks

1. Thanks to the creators of BigGAN, DeepMind, and to huggingface! https://github.com/huggingface/pytorch-pretrained-BigGAN (Andrew Brock, Jeff Donahue, & Karen Simonyan)

1. Thanks to the authors of SIREN! https://github.com/vsitzmann/siren (Vincent Sitzmann*, Julien N. P. Martel*, Alexander W. Bergman, David B. Lindell, & Gordon Wetzstein)

1. Thanks to OpenAI for sharing CLIP! https://github.com/openai/CLIP (Alec RadfordIlya Sutskever, Jong Wook Kim, Gretchen Krueger, Sandhini Agarwal)

1. And DALL-E's decoder! https://github.com/openai/DALL-E (Aditya Ramesh, Mikhail Pavlov, Gabriel Goh, Scott Gray [PRIMARY AUTHORS] Mark Chen, Rewon Child, Vedant Misra, Pamela Mishkin, Gretchen Krueger, Sandhini AgarwalIlya Sutskever [SUPPORTING AUTHORS])

1. As a good launching point for future directions based on feature visualization and to find more related work, see https://distill.pub/2017/feature-visualization/.

1. Thanks to Alex Mordvintsev for creating Deep Dream, which this work pulls on in terms of name and concept.

1. Props to Ceyuan Yang, Yujun Shen, & Bolei Zhou for separating the latent vector by layer. https://openreview.net/forum?id=Syxp-1HtvB

1. & the authors of GANspace as well: Erik Härkönen1, Aaron Hertzmann, Jaakko Lehtinen, Sylvain Paris. https://github.com/harskish/ganspace

