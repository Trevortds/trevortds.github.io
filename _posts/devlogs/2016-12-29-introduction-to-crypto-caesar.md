---
layout: article
title: "Introduction to Crypto: Caesar"
modified:
categories: devlogs
excerpt:
tags: [crypto, cipher, ciphers]
image:
  feature:
  teaser:
  thumb:
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
In [2]: rotate(alphabet, 1)
Out[2]: 
['b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t',
 'u','v','w','x','y','z','a']
```