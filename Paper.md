# A Comparison of Parts of Speech Taggers in Rule-based and Statistical Methods

## Background

Parts of speech (referred to as PoS) are labels to grammatically catagorize words to describe their relationships with the words around it.

Note: A misconceptions with parts of speech are that you label the words according to their meaning, like nouns being words that represent objects. While meaning does have a very high correlation on how words are catagorized, this is not the real way of determining parts of speech for a particular word. Part of speech is supposed to represent _grammatical_ relationships rather than _semantic_ ones.

Since the 1st C. BCE, there has been 8 main ways to catagorize parts of speech for European languages that are still mostly true for English to this day.

- Nouns
- Adjectives
- Verbs
- Adverbs
- Pronouns
- Prepositions
- Conjunctions
- Interjections

There are many sub catagories but we won't go into them here.

### Why is part of speech tagging useful?

Using part of speech tags we can abstract a language and use it to understand gramatical structures of a corpus easier especially when translating languages. We can also use the tags to retrieve information more efficently from corpora like taking keywords from nouns or verbs. For text to speech, we can also predict how a word will sound using the part of speech of the word in the context of the sentence.

### Why is part of speech tagging not trivial?

This is a question I first wondered when I wanted to research this topic. Why is this field a big part of NLP when parts of speech for a given word is labeled in the dictionary? Well it turns out its more complicated when you think a little deeper into it.

Well, lets look at an example.
Suppose we have the word **_back_** and the following sentences (These examples were taken from the Stanford Speech and Language Processing complementary slides) [^1]:

1. earnings growth took a **back** seat (ADJECTIVE)
2. a small building in the **back** (NOUN)
3. a clear majority of senators **back** the bill (VERB)
4. enable the country to buy **back** debt (PARTICLE)
5. I was twenty-one **back** then (ADVERB)

As you can see, a word can have multiple parts of speech depending on its context.

Lets look at the two major Parts of Speech classes, called Open and Closed classes, to describe this phenomenon.

![Picture of Classes](images/classes.png)

Closed Classes of words are words with PoS that don't really have much flexiblity in changing from one PoS to another. An example is conjunctions, like the words "and", "or", and "but". These words generally don't have more than one parts of speech.

Open Classes, on the other hand, are words that have multiple PoS. Words that are most commonly Nouns or Verbs have this feature (or bug?).

And it turns out that about 15% of words fall into the closed class, but also the frequency of these words are very high and amount to about 60% of the words used.

---

## Now let's consider some techinques to solve this problem.

First. we want to consider the baseline for the worst case senario or the most naive way to figure out PoS. It turns out to be not too bad.

Suppose we take each word and label it with the PoS of most common usage of the word. It tends to be about 92% correct.

This is pretty good but how much can we get close to a 100% accuracy? And what kinds of techniques are used to get better performance?

Lets compare 2 techniques:

## Rule Based

### TAGGIT

TAGGIT[^5] was the first large attempt at a PoS tagger system with 71 tags and 3,300 rules created by Greene and Rubin in 1971. It still left a lot to be desired because it left 23% of the words in the Brown corpus ambiguous.

### Brill tagger

Lets look at one of the most popular rule based PoS tagger created by Eric Brill at MIT in 1992 for his PhD thesis[^2]. He acknowleges that statistical taggers are easier to craft than rule-based ones which are also not very robust, but he introduces a type of tagger that is lightweight, only needs a small set of rules, and can be easily improved by changing the rules rather than statistical methods that often have obscure parameters.

It starts off by applying some set of initial tagging rules, that dont have any contextual information, to the corpus. For example, one of the rules could be that anything ending with an -ous is an adjective. Then, the labeled corpus is compared to a prelabeled corpus and iteratively improved. Each word keeps track of how many times the word had some tag _a_ but should have gotten tag _b_. A rule (a.k.a patch) from a set of rules (a.k.a. patch template) is picked that reduces the number of errors the most.

![Brill Architecture](images/Brill.png)

Let's have a hypothetical scenario to describe how it works. Let's say you have 200 words and the initial tagger mislabels 90 words as nouns when they should be verbs. Then, a patch is applied that says something like if the word is between two nouns it is a verb (e.g. Man throws ball). After applying this rule, 70 out of the 90 mislabeled words are correctly labeled, but there are 8 new errors that should have stayed the same tag but switched erroneously. There is 62 net decrease in errors and this process is repeated to decrease net errors. This results in a list of patches that describe the grammatical rules of the language, which is portable to other copora in other genres.

Using this method, while results were not 100% representative of a real one to one test against other methods, resulted in around a .5% less accurate performance at around 94.9% than statistical taggers made by DeRose in 1988, but there was some positive side effects using rule-based taggers from their statistical counterparts. Not only were they more lightweight, Idioms were automatically and correctly identified by the Brill tagger when statistical taggers needed to be fine tuned to account for these irregularities in grammar.

This paper is stil highly cited today as a more classical approach to part of speech tagging and ontology matching. Brill also improves upon his work on his 1994 paper [^3].

### RDRPOSTagger

This paper[^4] was written in 2014 by faculty at Vietnam National University, Hanoi and provides some improvements to Brill's work.

One area that the Brill tagger has trouble is managing the multitude of rules and their interactions with one another. When you have hundreds of rules, you can't really know which ones affect different rules, so there is this dependency problem that needs to be solved. RDRPOSTagger uses a Single Classification Ripple Down Rules (SCRDR) tree, which is a way of controlling how different rules affect other rules by organizing them into a tree like structure.

![SCRDR tree](images/SCRDR.png)

Above we can see that certain rules only apply under certain conditions sort of like nested IF-ELSE statements. If a node's rule is satisfied, then the word is passed down the except branch and if it is not satisified, it is passed through to the if-not branch. The last node where the case was satisfied is the tag of the word. By using these exception rules the dependency problem is minimized. There are more complexities when it comes to organizing these rules, but we won't get into the details here.

![RDRPOSTagger](images/RDRPOSTagger.png)

The overall structure of the tagger is very similar to the Brill tagger but with a slightly modified rule section.

The results overall show a 96.49% accuracy on a WSJ Treebank Corpus, 0.03% lead over a Markov model called TnT by Thorsten Brants and 0.04% over Brill's. This model was also tested on a Vietnamese Treebank and achieved 92.59%, better than a SVM based PoS tagging system, showing its ability to adapt to multiple languages and still retain its easy to configure and fast properties.

### Pros and Cons of Rule based tagging

The reason why rule based methods are cited in almost every PoS tagging paper is because their human interpretablity is second to none. It is built on a system that humans can manipulate in ways that make sense to us. This is important because human language is fundamentally built on a set of rules that we agreed upon as the correct way. The downsides of rule based tagging is that, to sufficently describe all the intricacies of a language, it would require tens or maybe hundreds thousands of rules that are handwritten with no automatation. While RDRPOSTagger tries to solve the problem of dependency, there is also the fact that there are a lot of rules that may conflict with each other. The accuracy of these systems are the product of these limitations as they are not that great compared to other methods. If human limitations were not a problem, then this would be the most prescriptive method to use, but unfortunately this is not the case.

## Statistical Methods

Statistical methods are the precursor to the boom of deep learning and transformers we have today. By using some math, we are able to automate and squeeze a lot of performance out of these systems.

### HMM Tagger

A common model to apply statisical methods to determine Part of Speech tags are with a Hidden Markov Model (HMM) using Maximum Likelihood Estimation (MLE).

#### Hidden Markov Model Architecture

A hidden markov model consists of 4 parts.

1. Hidden States
2. Observable States
3. Transitional Probabilities
4. Emission Probabilities

They are similar in nature to a finite state machine.

Tangent: To discribe how a HMM works, Lets suppose that you are told to guess the weather within a far away city with only the air temperature and the time of day there. The possible types of weather are: sunny, cloudy, or raining. Lets say that the air temperature is 60 degrees F (15 degrees C) at 3pm. There is high probability that the weather outside is cloudy but a low probability that it is sunny or cloudy (but not no probability), so you predict that it is cloudy outside. You used some observable information to determine which out of the weather states were most probable and this is what an HMM does.

![Hidden Markov Model](images/hmm.png)

1. The Hidden States (in green) are the states that are the possible outputs of the model's prediction and that are not viewed directly. In PoS tagging this would be the actually PoS tags like NN or VB.


2. The Observable States (in blue), like it's name suggests, is the observable part of the model. It is like the data that is output given a particular hidden state. With sunny weather, we might observe temperatures in the 80s F (26.6 C). We might also see cold temperatures even when sunny, so we have to include those as well, and it becomes all the possible observable states no matter how unlikely it may be. In PoS tagging these would be words with the particular PoS of the connected hidden state. 


3. The Transitional Probabilities (arrows pointing between each hidden states) are the probabilities that define the possiblity of switching from a current (or initial) hidden state to another hidden state. An example is the probability of going from a sunny Monday to a rainy Tuesday regardless of any given data. In the PoS scenario, it would be how likely it is to get a verb after a noun in a sequence of words.


4. The Emission Probabilities (arrows pointing from hidden state to observable state) are the probabilities that given a certain hidden state, how likely is it to output that observable state. In PoS tagging, this would be given a part of speech like a verb what are the probabilities of a specific word being observed.


#### Maximum Likelihood Estimation

Now we understand the structure of the HMM, lets see how we are able to utilize it using Maximum Likelihood Estimation.

MLE is a way of finding the best possible model that fits the data.

Generally speaking, for MLE, we have a parameter we adjust to fit a probability density function to a set of given data points and the parameter that matches the data distribution the closest is considered the most optimal parameter.

![MLE](images/MLE.png)

In the case of this HMM, we have this equation to represent the best model to predict PoS given a sequence of words and its corresponding tags.

![HMM Equation](images/Equation.png)

Here, x represents the sequence of words and y represents the sequence of corresponding PoS tags for x.

This equation is saying the joint probability of these two sequences x and y are equal to the product of this "q" term and this "e" term (We will go more in depth on these two letters). Having the maximum joint probability tells us that those sequence of words and tags are the most likely to correlate to each other, so that is our goal.

First, lets consider we have a list of tags availible to us to label words which we will arbitrarily call the "tag pool" and a training set of words that are prelabeled with tags we will call the "dictionary". Now we can jump into the calculations.

The "q" term in the equation represents the Transitional Probabilities that we saw in the HMM earlier. This term describes the probability of having a certain tag y<sub>i</sub> given the two previous tags in the sequence y<sub>i-1</sub> and y<sub>i-2</sub> . In other words, from our "tag pool", what is the probability of having a certain 3rd tag come after tag 1 and tag 2 in the sequence. Note that tags are just hidden states within a HMM, in the context of PoS taggers, so these probabilities describe how likely you are to go from one hidden state to another a.k.a Transitional Probabilities.

The "e" term in the equation represents the Emission Probabilities. The term says that "e" is the probability of a certain word x<sub>i</sub> given the corresponding tag y<sub>i</sub>. In simpler terms, from our "dictionary", what is the chance of having a certain word like "dog" show up when the tag is a noun. Here we are trying to find words that are likely to have a given tag.

To find the joint probability of these we take the products of each "e" and "q" terms for all n words and tags in the sequence, x and y.

![argmax HMM Equation](images/argmax.png)

The argmax of this joint probability is the best prediction that the model has come up with of these x and y sequences complimenting each other.

![Viterbi Algorithm](images/viterbi.gif)

A fun note is that since there can be around 50 tags in a typical "tag pool" and thousands of words in the prelabeled training set, the time complexity can be very costly, but there is a clever way to drop the unnecessary stuff using the Viterbi Algorithm.

This chains the sequences with the highest probabilities and drops all the other probabilities to go from O(tags^length of x) to O(length of x*tags^2) time complexity.



---

## References:

[^1]: https://web.stanford.edu/~jurafsky/slp3/
[^2]: https://dl.acm.org/doi/pdf/10.3115/974499.974526
[^3]: https://arxiv.org/pdf/cmp-lg/9406010.pdf
[^4]: https://aclanthology.org/E14-2005.pdf
[^5]: https://books.google.com/books?id=yl6AnaKtVAkC&lpg=PA219&ots=_VWd78CFIi&dq=%20part%20of%20speech%20tagging&lr&pg=PA223#v=onepage&q=part%20of%20speech%20tagging&f=false
