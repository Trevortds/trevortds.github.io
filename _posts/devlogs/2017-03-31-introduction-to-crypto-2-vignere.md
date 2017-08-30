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
date: 2017-03-31T13:27:09-07:00
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
<b>vhkgem</b>jagiokknycnmtywkvbuihpsttefqvbpgmqwttdlvhx<b>vhkge</b>igadurxdeedalg
</pre>


Next time: we consider an addition to Vignere that renders it invulnerable to cryptanalysis: One Time Pad Cypher. 