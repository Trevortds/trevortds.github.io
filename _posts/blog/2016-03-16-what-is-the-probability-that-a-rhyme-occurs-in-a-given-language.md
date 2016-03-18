---
layout: article
title: What Is the Probability That a Rhyme Occurs in a Given Language?
modified:
categories: blog
excerpt: Mandarin Chinese only has a few syllables, whereas English or German have far more possibilities of combining sounds. However, grammar only allows certain sound combinations, so not all of them are employed in language. Is it correct to say that in Chinese there will be more rhymes than in English? 
tags: [linguistics, poetry, phonology]
image:
  feature: ozzy.jpg
  teaser: chinese.jpg
  thumb: 
date: 2016-03-16T10:33:25-07:00
---

{% include toc.html %}

## Original Question:

### What Is the Probability That a Rhyme Occurs in a Given Language?
Mandarin Chinese only has a few syllables, whereas English or German have far more possibilities of combining sounds. However, grammar only allows certain sound combinations, so not all of them are employed in language. Is it correct to say that in Chinese there will be more rhymes than in Engl.? 

This is actually a very interesting question. I can't give you an actual number, because as far as I'm aware, a study like this has never been done, and it would be kinda hard to do, but we can make predictions.

Secondly, I'm going to assume that you meant, "probably that a rhyme occurs on a given word in a given language", because every language is going to have at least one rhyme, which would make your answer 100%, and that would be boring.

So, first of all, we need to define "rhyme". The poetic definition is a bit wibbly-wobbly, so I'll go with the linguistic definition. A syllable has three parts, an onset, nucleus, and coda. The nucleus and coda together are called the Rhyme or Rime (I'll use the second spelling, for the sake of clarity). Two words are said to "rhyme" when the final syllable of each word shares the same rime.

To reiterate, for the purposes of this answer (which has now become an essay), rime is a linguistic object consisting of a vowel and some number of consonants, and rhyme is a relationship between two words such that they share the same rime.
Kapeesh?
Okay, let's go. 

So, armed with this, let's look at some languages.

## Rhyme cross-linguistically

So, first, we need to get inflecting languages out of the way. Many, many languages have nominal and verbal inflection that takes the form of suffixes on words. In these languages, if two words share the same gender, or number, or case, or tense, they will rhyme, by strict definition. This is where we encounter our first cross-linguistic problem: in some languages this rhyme counts for poetry, in others it doesn't. So in English

>When Summer to the earth is bringing     
>birds of air and flowers blooming

is a kind of disappointing rhyme. If I were in a poetry class, that would get me a solid C, only because I kept the meter. However, in Persian

>Morde bodam, zende shodam gerye bodam khande shodam     
>dowlat e eshgh âmad o man dowlat e pâyande shodam

is totally fine. In fact, many Persian speakers can recite this poem by heart, it's one of the most famous poems by one of the most famous poets, Rumi. Every line ends with "-am" which is the marker for 1st person singular verb agreement, and a Persian speaker would call that rhyme.

So, what do we do? For the purposes of this discussion, I'm going to go ahead and say that Persian rhyming rules don't count. I say so at my own peril, but I think that far more languages are like English, and forbid suffix rhyme than like Persian where it counts. This means that we're only going to be looking at stems.

Whew, now that that's out of the way.

## Defining Rime

### Syllable Structure
The next thing to look at is syllable structure, because in addition to having different phonemic inventories, as you have pointed out, different languages have different syllable structure. Linguists classify this in terms of maximal forms. The maximal form is a generalized illustration of the syllable structure of a language by the largest possible number of sounds. The English maximal form is (C)(C)(C)V(C)(C)(C)(C)(C). The maximal example is "strengths", which is phonemically /strɛŋkθs/. Three onset consonants, one vowel nucleus, and five coda consonants. The parentheses indicate items that are optional: in English you can have a syllable as small as V, as in "I" or "a". 

Chinese, on the other hand, has the maximal form (C)(G)V(X)<sup>T</sup>. That is, an optional consonant, an optional glide, a mandatory vowel, an optional nasal, and a mandatory tone (which is part of the rime). Naturally, this is the greatest limiter to the size of syllables and the number of possible rimes, but not the only one.

### The Sonority Principle:
Now that we've got a description of the shape of syllables, there are some rules about how they can be made. Now, each language has it's own rules, for example, in English you can't have a /ŋ/ in an onset, but there is one generalizable rule that almost every language follows:  the Sonority Hierarchy Principle. This principle simply states that sounds with more sonority will be closer to the center of the syllable, and sounds with less sonority, will be further from the center. The sonority hierarchy looks like this (if you'll excuse the formatting):

less sonorous < < < < < < < < < < < < < < < < < < < < < < < < < < < more sonorous   
stops < < < delayed release < < continuants < < sonorants < approximants < < vowels   
[p t k]  < < < <  [b d ɡ] < < <  [s f θ] <  [z v ð] < < [m n ŋ] < < < [l] < [r] < < < [i u] < [e o] < [a]

and [s] is the exception to everything, because it can appear at the beginning of any onset and at the end of any coda without regards to sonority.

## Counting Rhymes:
This is the part where we need to actually look at the languages, with all the rules, and try to figure out what is and isn't legal. Now that we have these basic rules, we can make lists of possible nuclei and codas. Thankfully, some very bored people have already done the tedious work for us, and online you can find exhaustive lists of all the possible codas or nuclei of certain languages. Let's stick with Chinese and English, since you brought them up.

### Chinese
In Chinese, the syllable structure is rather simple, you only get one onset consonant, one vowel, and one coda syllable, so sonority hierarchy doesn't actually matter. Various institutions have created Pinyin Tables, that contain an exhaustive list of possible syllables, ignoring tone. On this Pinyin Table on Wikipedia (<a href="https://en.wikipedia.org/wiki/Pinyin_table">Pinyin table</a>), the rhymes are on the Y axis. We can count the number of rimes simply by counting how many rows there are. But wait! In Chinese, the optional glide is not considered part of the rime (depending on who you ask), so we have to look carefully at the table, and only count the rows without glides. By my count, that gives us 21 rimes. Hooray!

### English
Jesus Christ, how are we going to count English rimes? Well, we'll have to do some combinatorics, there are simply too many possibilities for anybody to make a table that means anything. There are a few tables we can use, though.

On this wikipedia page, you'll find a list of possible codas in English, ignoring the final /s/: <a href="https://en.wikipedia.org/wiki/English_phonology#Coda">English phonology</a>. The creators of this list already took into account the maximal form and sonority hierarchy, as well as a couple more specific rules that English has. If we count all of those, we should get a total number of possible codas of 97 . Since you can stick an /s/ or a /t/ or /d/ on any of these as inflectional markers, we're going to go ahead and ignore those, since up above, remember, we said we're not going to count inflection. Since you can also have no coda, we're going to stick an extra possibility on there for a total of 98.

So now we've got a count of codas, but a rime is a coda put next to a vowel. English, depending on your dialect, has around 15 vowels (normally I would count almost twice that, but our list of codas counts rhoticity as a consonant, I think that's wrong, but whatever). If we naively assume that any vowel can be stacked with any coda to produce a rhyme, that gives us 15*98=1470 rimes. Holy shit. This language, man, this language.

## Oh dear god he's still going
Not for much longer, I promise.

You'd think that those numbers above should give you your answer. Since Chinese has fewer rimes than English, you would expect there to be more rhymes. And that's almost certainly correct. But that's not what you asked. You asked for a probability of finding a rhyme. And for that, you need to know something about the lexicon.

The lexicon is a funny thing. Language follows Zipf's law in almost everything. This means that there are a small number of items with a very high distribution of occurrence, and a large number of things with a very low distribution of occurrence. For example, there are hundreds of words with the rhyme /ə/, but only one with the rhyme /ɪnd͡ʒ/ (orange). This means that the probability of finding a rhyme depends on the word you're starting with. In English, for some words, it's 0%. For other words, it could be upwards of 5%. We can presume that other languages will follow a similar distribution. Chinese might not have any singletons, like English does, but I guarantee that there's a zipfian distribution, since, if you go back and look at the pinyin table, you'll see that some codas are full across the row, and others have only one measly column and the rest is empty.

Therefore, to solve this problem once and for all, we would need to do a survey of all the words in the language, and build some kind of statistical model from that survey, whereby we can query a particular rhyme and find out how often it occurs, and what percentage of the probability space it occupies.


## Conclusion
I hope this answered your question. I had fun writing it, and I hope you learned something. To briefly summarize: your likelihood of finding a rhyme depends on what language you're in. In particular, that language's syllable structure, the interaction between sonority hierarchy and phonemic inventory, and any additional special phonotactic rules.

The likelihood of finding a rhyme also depends on what word in particular is being investigated. By Zipf's law, some rimes are much more common than other rimes, and uncommon rimes are more numerous than common ones (I may be wrong on that part, I haven't done the experiment).

To find this probability, you need to know the language, and you need to have a model of the syllables of that language, and you need to know what word you are attempting to rhyme.