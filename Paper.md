# Comparing Parts of Speech Taggers in Rule-based and Statistical Domains

## Background

Parts of speech (referred to as PoS) are labels to grammatically catagorize words to describe their relationships with the words around it.

Note: A misconceptions with parts of speech are that you label the words according to their meaning, like nouns being words that represent objects. While meaning does have a very high correlation on how words are catagorized, this is not the real way of determining parts of speech for a particular word. Part of speech is supposed to represent _grammatical_ relationships rather than _semantic_ ones.

Since the 1st C. BCE, there has been 8 ways to catagorize parts of speech for European languages that are still mostly true for English to this day.

The main 8 are:

- Nouns
- Adjectives
- Verbs
- Adverbs
- Pronouns
- Prepositions
- Conjuctions
- Interjections

There are many sub catagories but we won't go into them for the time being.

### Why is part of speech tagging not obvious?

So this is a question I first wondered when I wanted to research this topic. Why is it so difficult when you can just grab the parts of speech of any word from a dictionary?

Well, lets look at an example
Suppose we have the word **_bat_** and the following sentences:

1. bat (Animal) is a noun
2. bat (Baseball) is a noun
3. to bat (to hit) is a verb
4. off the bat (idiom) is a ?
5. to bat an eye (idiom) is a verb

Lets look at the two major Parts of Speech classes that these catagories fall under called Open and Closed classes.

![Picture of Classes](images/classes.png)

Closed Classes are words with PoS that don't really have much flexiblity in changing from one PoS to another. An example is conjunctions, like the words "and", "or", and "but". There aren't conjunctions that are also different PoS.

Open Classes are words that are Nouns or Adjectives that fall into multiple other PoS catagories.

## References:

- https://web.stanford.edu/~jurafsky/slp3/
  - More specifically Chapter 8
  - Contain slides from Christopher Manning's video lecture
