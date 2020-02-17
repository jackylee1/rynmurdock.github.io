# CCNs and Audio

We've applied CCNs [as explained here](https://rynmurdock.github.io/2020/02/05/CCN.html) to images --- but what about signals that
have only one dimension? 

Although audio is one of many examples of this, and the sound presented here is arguably 
sonification of information that isn't specific to hearing, it only makes sense to treat it this way. These networks are
communicating and the most convenient way to understand this process is through the primary way that we communicate similar
signals. 

I will say that it would be amazing to somehow constrain the format in how these signals are being "heard" and created
in a way similar to humans, somehow requiring them to be limited to tongues and cochlea. Such constraints would likely lead to
even more interesting results, but how that would be accomplished is beyond me for now. (Maybe some kind of fourier trick?
I remember something like most information is conveyed in the 2nd formant through vowels when humans speak.)

With these points in mind, I will walk through what changed in the network:

1. Not much of substance
1. Seriously, just a conv2D to conv1D change
1. And shape-wrangling tricks, of course
1. It's surprisingly easy to use audio with iPython
1. If interest is shown, I can definitely just share the notebook but
1. really not much changed

--- Quite a list! But anyways, you really just need to know that the original CCN was taken and instead of sliding 2-dimensional, 
learned "filters" over an image, 1-dimensional filters were used instead. Can you guess what the results sound like?


Here's the hint: consider an early means of communication in the 1800s. 

And here's [the audio](https://github.com/rynmurdock/rynmurdock.github.io/blob/master/images/outputone.mp3).

I honestly did not expect this. But it makes sense: drowning in noise, of course you're going to use what you best can to rise 
against it. Similar to the white/black extremes in images, extreme blips and dips are easier to "hear." It is interesting that
the network, instead of converging on some ancient trick like glyphs, converges on a relatively recent technology.

