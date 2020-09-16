# Does GPT-2 Parse Ambiguous Sentences Similarly to Humans?
## A Preliminary Investigation<sup>1</sup>

> The niece of the waitress who hurt herself on the bicycle was angry about the incident.

What does the above sentence<sup>2</sup> mean? There was some kind of incident, the niece was angry --
it tells us a number of things, but what is of interest is what it definitely *does not tell us*.
You probably automatically inferred that either the 
niece or the waitress hurt herself on the bicycle. Neither of these interpretations is more correct than the other, though the latter is more common in English.<sup>2</sup>
This type of ambiguous sentence (which we'll call a globally ambiguous relative clause sentence [GARCS] after a paper<sup>3</sup> that studied it with humans)
serves as an interesting window into how reading works in people as well as, I would argue, language models.


There are a number of theories as to what makes some people more apt to choose the niece (the first noun phrase or NP1) or the waitress (the second noun phrase or NP2) as the
person who hurt herself (the relative clause). When "the niece" is chosen over "the waitress", we would call that NP1 attachment. NP2 attachment is the opposite, when
"the waitress" is chosen as the person "who hurt herself."

It could be that people with higher verbal working memory capacity are able to
hold the representation of NP1 in their minds longer, enabling them to couple it with the action of "hurting herself on the bicycle" more easily. Conversely, it would 
take less work for people who need to conserve working memory resources to couple NP2, the closer noun, with the relative clause<sup>4</sup>.

On the other hand, the opposite
effect may exist. It has been proposed that people with lower working memory capacity may "chunk" or break up sentences in ways that follow prosodic patterns (i.e., 
are related to the rhythm of speech), which leads them to be more likely to choose NP1 <sup>5</sup>. With our example, this looks like reading "The niece of the waitress" as one unit and
attaching "who hurt herself" to the beginning of that unit, that is, "the niece." 

![A GAN clock](/images/clock.png)



## The Model

This brings us to GPT-2. I began testing GPT-2's parsing of syntactically difficult sentences because I thought it might be possible to use GPT-2 as a drop-in for actual human subjects.
I figured if I could replicate some effects well enough, I might be able to do exploratory analyses without collecting data from a single real person. 
That would've been too simple and convenient, of course. 

Before getting into the results of my playing around, let's consider the possible outcomes of comparing GPT-2 to humans with respect to GARCSs
and what they may mean. 

GPT-2is a language model that is not constrained by working memory
in the case of sentences with a number of words that can easily fit within its context window (its input). It sees every word as a token and makes each-to-each
comparisons that incorporate word order, but, importantly, **GPT-2 has no cost for utilizing long-range dependencies.** The whole purpose of transformers is to use attention
in order to make "the niece" as connected to every other portion of the sentence as "the waitress."

So, while GPT-2 can handle long-range dependencies, there's one thing that it can definitely not do: prosody. 
There is no rhythm intrinsic to written text. These observations lead to the following possible outcomes for this comparison:

#### H1. GPT-2 is just like humans.

If GPT-2 prefers resolving GARCSs by attaching the clause to NP2 exactly like humans, then that means GPT-2 could work just like humans because it is trained to do so.
All GPT-2 really "wants" to do is minimize its outputs' dissimilarity from the training data. As mentioned, this is not what I found.

#### H2. GPT-2 prefers NP2 attachment even more than humans.

If GPT-2 prefers attaching NP2 to attaching NP1 even more than human English readers, then this could support the prosody theory: GPT-2 has no need for chunking, 
and further couldn't chunk, so it would be less likely to mimic NP1-attachment behavior.

#### H3. GPT-2 prefers NP1 attachment, contrary to most human English speakers.

If GPT-2 flips the script and prefers attaching NP1 to attaching NP2, then this could support the working memory theory: GPT-2 has no working-memory limitations,
and so if low working memory making long-range dependencies more difficult drives some people to attach NP2, then the opposite may be true for GPT-2, leading it to attach to NP1.

I feel most comfortable with hypotheses one and two. H1 is not at all surprising; H2 is compelling and I think quite plausible. H3, on the other hand, makes some loose sense, 
but doesn't accord well with what (admittedly little relative to others!) I've read so far in the literature. 

Naturally, I found that H3 won out, and GPT-2 does the opposite of what humans generally do, strongly preferring NP1 attachment to NP2 attachment. It looks like the niece
may be the one who biffed it on the bike after all.


## The Method

"How can a neural network express preference anyway?" you may be wondering. I decided to use two measures to do this. Firstly, I compared the sentence-likelihood using
lm-scorer<sup>6</sup> by writing them like so "The niece of the waitress who hurt herself on the bicycle was angry about the incident. The niece hurt herself." This may
seem like a rudimentary way to go about it, because it is. To follow up, I prompted a model like so:

>"The brother of the count who crippled himself by falling off a horse took a long time to get over it.

>Who crippled himself by falling off a horse?

>  A. The count

>  B. The brother

>The answer is letter"

And then I compared the likelihoods of  A and  B given the prompt. To account for GPT-2 always preferring  A, I flipped the order like so:

>"The brother of the count who crippled himself by falling off a horse took a long time to get over it.

>Who crippled himself by falling off a horse?

>  A. The brother

>  B. The count

>The answer is letter"

And I only counted an answer as more likely if it was higher relative to the other noun in both the A and B positions.

I wish there were and will continue thinking of a better way to query GPT-2 like this, but these two methods seem reasonable and converge on an extreme preference for NP1 
attachment.

## The Conclusion

So, it seems that GPT-2, free of working-memory limitations, prefers NP1 attachment. This is a very surprising finding because that pattern is more commonly seen, in some
recent research, in people with *low working memory!* <sup>3</sup>, and this result cannot easily be explained as simple mimicking, because NP1 attachment is less common on
average! It may be that in online communication, GPT-2's primary data source, people are more likely to phrase Q&A's like the one above with NP1 attachment as a correct answer
for some strange reason, and that people are less likely to write sentences with obvious, repetitive claims like
"The niece of the waitress who hurt herself on the bicycle was angry about the incident. The niece hurt herself." Such a gap between how people write online
and how people comprhend cannot be ruled out for now.

Future directions for this work include trying different attention mechanisms and model sizes,
doing similar analyses with LSTMs,
and finding new ways to assess attachment preference. 

If anyone would like to get me access to GPT-3, that'd be pretty rad. Based on some runs where GPT-2-small was used instead of GPT-2-xl, I doubt scaling will
make any significant difference. On the other hand, using a significantly more powerful model would facilitate some other preference-measuring methods I've considered.

If you have any questions or ideas about this work, feel free to reach out on Twitter [@advadnoun](https://twitter.com/advadnoun)!

### Notes
1. "Investigation" as opposed to study, experiment, etc. because this is explicitly not formal research: I'm playing around and sharing where my ideas have come from and
what I've found.
1. [Underspecification of syntactic ambiguities: Evidence from self-paced reading](https://link.springer.com/content/pdf/10.3758/MC.36.1.201.pdf)
1. [Individual differences in syntactic processing: Is there evidence for reader-text interactions?](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6350810/)
1. [The nature of working memory in sentence comprehension: Evidence against domain-specific working memory resources](http://web.mit.edu/evelina9/www/Downloads/Journal%20pubs/Fedorenko_et_al_2006_JML.pdf)
1. [Psycholinguistics Cannot Escape Prosody](https://www.researchgate.net/profile/Janet_Fodor/publication/2891257_Psycholinguistics_Cannot_Escape_Prosody/links/09e41507c2300f3cca000000.pdf)
1. [LM-Scorer's GitHub](https://github.com/simonepri/lm-scorer)


## Update: Future Directions and Different Prompts

Nothing seems to increase the likelihood of me wanting to do another write-up than finishing a write-up! I've found an arguably better prompt than the originals used here.
An example of it would be: "The uncle of the general who sacrificed himself for the cause was the subject of a biography.
Who sacrificed himself for the cause? The"; it seems to work well and is consistent with my previous findings, showing a massive NP1-attachment preference. However,
I seem to have been completely wrong about scale, as I'm finding that the small version of GPT-2 is much more likely to choose NP2-attachments (at least with this method).
So in addition to the future directions stated above, it looks like I'll have to try larger models as well. I've also realized that comparing Spanish- or Dutch-trained models 
could be interesting, as those languages generally have a fair NP1-bias. There could be some very interesting results from multi-lingual models as well. There is much to do!

## Update: Online Attachment Bias is Very Different! (Probably)

I've confirmed over and over again that when asked questions about a sentence in the form of multiple choice, True/False, etc., GPT-2 chooses high attachment, the opposite of average humans; however, my brilliant advisor pointed out that I could let GTP-2 resolve the ambiguity of these relative clause 
sentences by having it indicate the gender of the noun the clause applies to. This is commonly referred to as "online" completion, in contrast to the "offline" questions I've been focusing on previously. 

For example, we compare the probabilities of GPT-2 outputting "himself" or "herself" given this sentence: 

> The brother of the princess who injured

and this allows us to infer a decision in which either the brother or the princess injured themself. It turns out that GPT-2's online attachment "preference" is the complete opposite of its offline behavior, showing a serious bias towards low attachment. There is a small caveat: for the online assessment, I've yet to use a corpus with a human baseline. It could be that the items are strange in some way, but I will likely be able to collect a corpus where
I can directly compare to see if the bias is higher than in English speakers. I've also found that there is quite a bit of possibility for pushing and pulling this behavior 
around by slightly altering the length, the nouns, and other features of the sentence, so, again, I'll have to find a baseline for comparison.

Whatever the case, this is a good example of how important the materials/prompts you use are, especially if you're using them with language models such as GPT-2.


