---
layout: article
title: "Introduction to Crypto 2: Vignere"
modified:
categories: devlogs
excerpt:
tags: [crypto, cipher, ciphers]
image:
  feature: north.jpg
  teaser: /devlogs/tabula_recta.png
  thumb: /devlogs/tabula_recta.png
date: 2016-12-29T13:27:09-07:00
---


This is the second of a short series on cryptographic methods and their history, for no other reason than that I really like cryptography. 

Last time, I introduced the Caesar cipher, the first ever cipher. It involves changing the method from one alphabet to another, following some *rotation*. The number of letters by which the alphabet is rotated is the key. 

For a very long time, this was the state of the art in cryptography, and it has many advantages: it is very easy to explain, and very easy to implement: no tools required. For this reason, it was popular long after it was outdated, most lately used in a military scenarion in 1915 in the Russian Army (much to the delight of German and Austrian cryptanalysts). But, as we saw, it has weaknesses. 

The weakness of the Caesar cipher are twofold: 

1. It is a **single substitution cipher**. This means that whenever a symbol appears in the plaintext, that same symbol always appears in that position in the ciphertext. This makes the cipher vulnerable to **frequency analysis**.

2. The search space of keys is very small, only 26. If a cryptanalyst catches on that a message might be Caesar encrypted, they can brute-force it in only 26 tries. 

To solve these problems, one might attempt to construct a **multiple subsitution cipher** (or **polyalphabetic**)

## Vignère Cipher

The Vignère Cipher was invented several times over the centuries, first by Giovan Battista Bellaso in 1553, but it didn't achieve widespread adoption until Blaise de Vignère's 1586 book, *Traicté des Chiffres ou Secrètes Manières d'Escrire*. 

The Vignere cipher is a polyalphabetic cipher. This means that it translates letters in the plaintext into several other alphabets, not only one. Encryption is still just barely simple enough that it could be done without a tool, but a tool is just as easy to make as with a Caesar cipher. 

<img src="{{ site.url }}/images/devlogs/tabula_recta.png" width="400">

This is a "Vignere Square". Note that each of the rows represents a Caesar rotation alphabet. The key tells you which row to use. Simply repeat the key through the length of the message. 

```
 plaintext: thisisasecretmessage
       key: catcatcatcatcatcatca
ciphertext: vhbuilcsxerxvmxustie
```

Each letter in the plaintext message gets its own key. This is the key to the security of Vignere. 

In our python implementation, it looks like this. 

```python
def encrypt(plaintext, key, alphabet=alphabet):
    if isinstance(key, str):
        key = key_parse(key)
    ciphertext = ""
    key_counter = 0 # which key letter to encrypt with
    for char in plaintext:
        ciphertext += caesar.encrypt(char, key[key_counter], alphabet)
        key_counter = (key_counter + 1) % len(key)
    return ciphertext
```

There is a small edit to our previous cryptographic system, where an alphabet passthrough, but it is otherwise identical. 

Simply put, the above code will turn a word key into a list of caesar rotations with `key_parse`, then iterate over the text, using a different caesar rotation on each letter, with wraparound when the `key_counter`  overcomes the length of the `key`. 

Just like in Caesar, the decryption scheme is identical to the encryption scheme, but with the key inverted. 

```python
def decrypt(ciphertext, key, alphabet=alphabet):
    if isinstance(key, str):
        key = key_parse(key)
    key = [len(alphabet) - x for x in key]
    return encrypt(ciphertext, key, alphabet)
```


## Weakness: Frequency Analysis

Unfortunately, Vignere is also not perfect. We will not run through the entire example here, but looking closely at the above, you may have noticed the issue. 

```
 plaintext: thisisasecretmessage
       key: catcatcatcatcatcatca
ciphertext: vhbuilcsxerxvmxustie
```

Notice that whenever a letter in the plaintext meets the same match in the key, its result in the ciphertext is also the same. In the above message, t lines up with c resulting in v twice, and e lines up with t resulting in x three times. If the message is long enough, and the key short enough, the same method can be used to crack it. 

There are sneakier methods though that don't require a weak key to make cryptanalysis feasible. 

## Weakness: Kasiski Examination

In a military text, it is likely that words will appear multiple times, and whenever this happens, there is a likelihood that the ciphertext will also repeat equal to `1/len(key)`

<pre>
<b>vhkgem<\b>jagiokknycnmtywkvbuihpsttefqvbpgmqwttdlvhx<b>vhkge<\b>igadurxdeedalg
<pre>



----------

## Dramatis Personae

Before we begin, as part of the time-honored tradition of cryptography, we need to establish some characters who are going to be sending secret messages. 

#### Alice

Alice is a field agent of the Fingian rebels, deep undercover in the heart of the Thangor Empire. Her public face is as a reporter, to the limited extent that reporters exist amidst the oppressive regime, but it still allows her a small degree of additional freedom of movement, which is all she really needs. 

#### Bob

Bob is Alice's handler back at the secret rebel base far in the outskirts of the empire. Bob needs to be able to receive and understand Alice's messages so that the Rebels can avoid Thangorian crackdowns, and strike the Imperial Forces where they are weakest. 

#### Eve

Eve is Alice's roommate. She has no particular love for the Empire, but she is loyal nonetheless, and more than a bit nosy. As a good citizen, she is encouraged to spy on her neighbors by the government, and she often eavesdrops on Alice's outbound mail. If Eve discovers the nature of Alice's messages, she will report her to the authorities, and all will be lost. 

---------


## The Caesar Cipher

Alice needs to send a message to Bob, but she does not want Eve to be able to read it. One of the oldest and most often cited methods for this task is the Caesar Cipher. 

The Caesar Cipher is very simple. If you are like me, you probably played with it as a child with one of these "decoder rings", that were often found as prizes in cereal boxes.  

![Decoder Ring from the 30s]({{ site.url }}/images/devlogs/decoderring.jpg)

All you have to do is shift the alphabet. Augustus Caesar used a shift of 1 in his military dispatches, so A becomes B, B becomes C, C becomes D, and so on. Julius Caesar, who is credited with inventing the cipher (though there is evidence it predates him) used a shift of 4. 

Here is my implementation of the shift. 

```python
alphabet = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p',
            'q','r','s','t','u','v','w','x','y','z']
def rotate(alphabet, key):
    key = key % len(alphabet) # in case the key is 27 or higher for some reason
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
            ciphertext += char # leave punctuation and spaces as is

    return ciphertext
```

Most of this is preparation and edge cases (characters in the message not in the alphabet, etc), the core element is this loop. 

```python
    alpharotation = rotate(alphabet, key)

    for char in plaintext:
        ciphertext += alpharotation[alphabet.index(char)]
```

Note that the central piece of the encryption process is to replace each character in the plaintext with a single ciphertext character, dependent only on the plaintext character itself. That's why this technique is the prime example of a class of ciphers known as **Single Substitution Ciphers**. Every character is replaced by exactly one other character. 

Other single substitution ciphers may involve performing scrambles other than rotations to the alphabet, or changing to another alphabet entirely (as in Pigpen or Numeric Caesar), but they all function in fundamentally the same way, and share the same weaknesses. 

But I digress, let me run this code for you and see how it works. 

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

Since this is a short message, frequencies other than the top ones can't be relied on too heavily, but some of these seem to make sense. "ee" is a common sequence, which is encouraging. She tries to substitute `o` for the next most common letter, 'n'. This is a stretch, but it might work. 

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

The third word in the second line could be "here" or "were", but we already think that `o` represents 'h', so we guess that `d` is 'w'

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

Or, if she believe that she's dealing with a Caesar cipher, she can guess the key: there is only one Caesar rotation that maps 't' to `a` and 'e' to `l`. Checking against a table of Caesar rotations will quickly give Eve the key, at which point cracking the rest is trivial. 

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


-------

Next time, an extension of Caesar, **The Vignere Cipher**