---
layout: article
title: "Introduction to Crypto: Caesar"
modified:
categories: devlogs
excerpt:
tags: [crypto, cipher, ciphers]
image:
  feature: north.jpg
  teaser: caesar.jpg
  thumb: caesar.jpg
date: 2016-12-29T13:27:09-07:00
---


This is the first of a short series on cryptographic methods and their history, for no other reason than that I really like cryptography. 

Cryptography is the art of concealing information in plain sight. Cryptography is subdivided into two main types. **Codes** and **Ciphers**. 

**Codes** are the bread and butter of *semantic cryptography*. What is *encoded* (note, not *encrypted*), is the meaning of the message, not the message itself. The following is an encoded message. 

> The Eagle has left the Nest, repeat, the Eagle has left the Nest, over.



> This is Yorkshire Terrier, I'm on my way to The Library to check out Three Books. 

You have probably noticed that the message itself is plain to see, but the details of it are obscured by *codewords*. Unlike encipherment, encoding is immediate, baked into the message. This makes it fast, perfect for short-wave radio communication, like what you were imagining in your head reading the above. Coding is interesting in its own right, but it's not what this series is about. 

**Ciphers** are methods used in *textual cryptography*, which is what we aim to explore. The thing that is *encrypted* is the message itself, rather than its content. The following is an encrypted message. 

> Htslwfyzqfyntsxdtzajzsufhpjiymjxjhwjyrjxxflj.Ltrfpjdtzwxjqkflttixfsibnyhmfxfbjqqijxjwajiwjbfwi

You can't even begin to guess what this one means, but by the time you hit the bottom of this page, you'll know where to start. 

But before we begin, let me introduce the cast

----------

## Dramatis Personae

Before we begin, as part of the time-honored tradition of cryptography, we need to establish some characters who are going to be sending secret messages. 

#### Alice

Alice is a field agent of the Fingian rebels, deep undercover in the heart of the Thangor Empire. Her public face is as a reporter, to the limited extent that reporters exist amidst the oppressive regime, but it still allows her a small degree of additional freedom of movement, which is all she really needs. 

#### Bob

Bob is Alice's handler back at the secret rebel base far in the outskirts of the empire. Bob needs to be able to receive and understand Alice's messages so that the Rebels can avoid Thangorian crackdowns and strike where they are weakest. 

#### Eve

Eve is Alice's roommate. She has no particular love for the Empire, but she is loyal nonetheless, and more than a bit nosy. As a good citizen, she is encouraged to spy on her neighbors by the government, and she often eavesdrops on Alice's outbound mail. If Eve discovers the nature of Alice's messages, she will report her to the authorities, and all will be lost. 

---------


## The Caesar Cipher

Alice needs to send a message to Bob, but she does not want Eve to be able to read it. One of the oldest and most often cited methods for this task is the Caesar Cipher. 

The Caesar Cipher is very simple. If you are like me, you probably played with it as a child with one of these "decoder rings". that were often found as prizes in cereal boxes.  

![Decoder Ring from the 30s]({{ site.url }}/images/devlogs/decoderring.jpg)

All you have to do is shift the alphabet. Augustus Caesar used a shift of 1 in his military dispatches, so A becomes B, B becomes C, C becomes D, and so on. Julius Caesar, who is credited with inventing the cipher (though there is evidence it predates him) used a shift of 4. 

Here is my implementation of the shift. 

```python
alphabet = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p',
            'q','r','s','t','u','v','w','x','y','z']
def rotate(alphabet, key):
    key = key % len(alphabet)
    return alphabet[key:] + alphabet[:key]
```

Simple enough, right? Let's see what Augustus's rotation looked like. 

```python
In [1]: rotate(alphabet, 1)
Out[1]: 
['b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t',
 'u','v','w','x','y','z','a']
```

The encryption method is as follows. 

```python
def encrypt(plaintext, key):
    ciphertext = ""
    alpharotation = rotate(alphabet, key)
    plaintext = plaintext.lower()

    for char in plaintext:
        if char in alphabet:
            ciphertext += alpharotation[alphabet.index(char)]
        else:
            ciphertext += char

    return ciphertext
```

Most of this is preparation and edge cases (characters in the message not in the alphabet, etc), the core element is this loop. 

```python
    alpharotation = rotate(alphabet, key)

    for char in plaintext:
        ciphertext += alpharotation[alphabet.index(char)]
```

Note the core piece is that each character in the plaintext is replaced by a single ciphertext character, dependent only on the plaintext character itself. That's why this technique is a **Single Substitution Cipher**. Every character is replaced by exactly one other character. 

```python
In [2]: encrypt("this is a secret message", 1)
Out[2]: 'uijt jt b tfdsfu nfttbhf'
```

The decryption scheme is simply to do the same thing in the other direction. 

```python
def decrypt(ciphertext, key):
    return encrypt(ciphertext, len(alphabet)-key)
```

There is an interesting consequence here. Since the decryption is simply encrypting with `26 - key` as the key, whenever the number of letters in the alphabet is even, there is a symmetric scheme halfway through. This is why Caesar with a shift of 13, or **ROT13** is commonly used for encrypting and decrypting quickly, it's the same procedure both ways. 

```python
In [3]: rotate(alphabet, 13)
Out[3]: 
['n','o','p','q','r','s','t','u','v','w','x','y','z',
 'a','b','c','d','e','f','g','h','i','j','k','l','m']

In [4]: encrypt("this is a secret message",13)
Out[4]: 'guvf vf n frperg zrffntr'

In [5]: encrypt("guvf vf n frperg zrffntr",13)
Out[5]: 'this is a secret message'


```


## Weakness: Frequency Analysis

This scheme is effective for a certain amount of time, but eventually, Eve's curiosity gets the best of her. She takes a piece of Alice's outbound mail, and sees this.

> 'p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol yvhk'

Eve knows a little bit about language, and she knows that some letters appear more often than others. She checks the frequency of the letters in the ciphertext. 

    'l': 15,
    'a': 14,
    'v': 13,
    'h': 11,
    'o': 8,
    'y': 7,
    's': 6,
    'z': 6,
    'p': 6,
    'u': 5,
    'i': 4,
    'j': 4,
    'k': 4,
    'd': 4,
    'b': 4,
    't': 4,
    'n': 3,
    'f': 3,
    'm': 2,
    'r': 2,
    'w': 1,
    'q': 0,
    'g': 0,
    'x': 0,
    'e': 0,
    'c': 0,


The four most common letters in English by far are 'e', 't', 'o', and 'a'. Eve guesses that these are the top four letters in the message. 

```
   a  t  ee t      o  t e    a  o t o  t e  a  ta   a e    t   o   e     o e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  t e   e e ta      a o t  o  o   e     o   a   e a  e to  at   t e  o  t e
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
 oa 
yvhk'
```

Since this is a short message, frequencies other than the top ones can't be relied on too heavily, but some of these seem to make sense. "ee" is a common sequence, which is encouraging. She tries to substitute 'o' for the next most common letter, 'n'. This is a stretch, but it might work. 

```
   a  tn ee t      o  tne    a  o t o  tne  a  ta   a e    tn  o   e     o e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  tne   e e ta      a o t  o  o   e     o   a   e a  e to  at n tne  o  tne
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
 oa 
yvhk'
```

This doesn't seem to improve things. The sequence "tne" appears five times, and this is practically unheard of in English. But there is another letter that appears in that position very often: 'h'

```
   a  th ee t      o  the    a  o t o  the  a  ta   a e    th  o   e     o e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  the   e e ta      a o t  o  o   e     o   a   e a  e to  at h the  o  the
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
 oa 
yvhk'
```


Based on this she can make some more guesses. What if the third word is "three"?

```
   a  three tr     o  the r  a  o t o  the  a  ta   a e    th  o   er    o e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  the   ere ta      a o t  o  o   er    o   a   e a  e to  at h the  o  the
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
roa 
yvhk'
```

The third word in the second line could be "here" or "were", but we already think that 'o' represents 'h', so we guess that d is 'w'

```
   aw three tr     o  the r wa  o t o  the  a  ta   a e  w th  o   er    o e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  the  were ta      a o t  o  o   er    o   a   e a  e to  at h the  o  the
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
roa 
yvhk'
```

Now that second word, there's only one reasonable possibility, it must be "saw"

**Notice how each guess gives you more clues that lead to more cascading guesses in other places?**

```
  saw three tr   s o  the r wa  o t o  the  a  ta   a e  w th so   ers  so e
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
o  the  were ta      a o t  o  o s er    o   a   e a  e to  at h the  o  the
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
roa 
yvhk'
```

The rest falls in very quickly as you fill the gaps. At this point, Eve could continue as before and keep guessing individual letters based on patterns. 

Or, if she believe that she're dealing with a Caesar cipher, she can guess the key, there is only one rotation that maps 't' to 'a' and 'e' to 'l'. Checking against a table of Caesar rotations will quickly give Eve the key, at which point cracking the rest is trivial. 

```
i saw three trucks on their way out of the capital laden with soldiers  some
p zhd aoyll aybjrz vu aolpy dhf vba vm aol jhwpahs shklu dpao zvskplyz, zvtl
of them were talking about gondolsberg  you may be able to catch them on the
vm aolt dlyl ahsrpun hivba nvukvszilyn. fvb thf il hisl av jhajo aolt vu aol
road
yvhk'
```

The reply from Bob is left as an exercise to the reader. 

    'leuvijkffu rxvek rcztv kyviv riv ivsvc xlvizccrj fe kyvzi nrp kf zekvitvgk. nvcc ufev. r evn zewfidrek nveup yrj druv tfekrtk nzky lj, dvvk yvi rk 1700 ze kyv jhlriv kfdfiifn'
