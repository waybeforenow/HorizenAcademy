---
layout: post
type: article
title: "Generating Keys and Addresses"
description: "The Horizen Academy is a free educational platform on blockchain technology, cryptocurrency, and privacy. This chapter is is not available yet. We add content frequently, sign up for our newsletter for notifications when it's released."
permalink: /technology/expert/generating-keys-and-addresses/
topic: technology
level: expert
chapter: "How Does a Blockchain Work?"
published: false
---

When you install and set up a wallet on your phone or computer the first thing you will do is actually fund your wallet by transferring some crypto to your address. In this article we will take a look under the hood of your wallet to understand where your address is coming from and how it is related to your private key.
 
![From Private Key to Address](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/key_to_address_D.jpg)
 
First a private key is randomly generated and in a second step your public key is calculated from it. Addresses are derived form your public key via two main steps: first by hashing your public key and second, by *Base58Check* encoding them.
 
If you have read **our last article** you will know that the security of any public-key cryptography (PKC) scheme relies on a key property: It should be easy to derive a public key from a private key, but it must be infeasible to reverse that operation and derive a private key from a given public key.

With elliptic curve cryptography (ECC) this property is based on the *discrete log problem*, that refers to division on the elliptic curve being *computationally hard* while multiplication is easy. Deriving a public key from an address is also computationally hard, as you would have to find the *preimage* of a **hash function**, which means deriving an input from an output.

This is an important aspect when considering privacy. Using a new address for every incoming transaction makes tracking transactions much harder for an adversary. If you were able to derive a public key from an address, you could derive all addresses used by the same owner (as long as they were produced based on the same public key). This would render change addresses useless in many cases.

Now let's get into the actual process of creating an address from scratch!

### Generating Your Private Key

By now you are aware of the purpose a private key serves: proving ownership of a **UTXO** set by creating digital signatures to authorize spending. But what is a private key?

Private keys are sometimes also called *secret keys* or *spending keys*. When doing ECC math, the spending key is usually abbreviated with *sk* for that reason. A spending key comprises 32 bytes, or 256 bits of data. This has two important implications:

- the SHA256 hash function used in most cryptocurrencies produces 256 bit outputs, which makes them a suitable candidate for private keys
- the elliptic curve used in many cryptocurrencies, scp256k1 has an [order of 256 bits](https://bitcoin.stackexchange.com/questions/21907/what-does-the-curve-used-in-bitcoin-secp256k1-look-like), which means it consumes 256 bit inputs and produces 256 bit outputs.

A private key can be represented in many formats, such as a binary string of 1's and 0's, a [*Base64*]\(https://en.wikipedia.org/wiki/Base64) string, a *mnemonic phrase* or a hex string.

![Key Formats](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/key_formats_D.jpg)

In order for a private key to be secure it needs to be a random number. Randomness is also referred to as *entropy* which is a term coming from the world of physics and chemistry describing the level of disorder in a system. Guessing a private key is so unlikely no sane human would try to attempt it and stealing them is also infeasible assuming good opsec. Exploiting the source of randomness used to create a private key and then recreating it can be a viable strategy for attackers in some cases. It's important to note that randomness should be viewed on a scale, rather than on a binary basis. A good resource to find out more about randomness is [random.org](https://www.random.org).

Many programming languages have a random number generation (RNG) module build in, but few of them are made with cryptographic purposes in mind. Some, like pythons secret module are optimized for truly random number generation. How "good" a RNG module is, depends on the source of entropy used. It can be collected from many sources: the processes running on your processor, [mouse movements](https://zenpaperwallet.com/), camera and microphone inputs or a mix of the above.

If you would like to go through the process of creating a private key for yourself, [here is an article](https://www.freecodecamp.org/news/how-to-generate-your-very-own-bitcoin-private-key-7ad0f4936e6c/) guiding you through the process in python.

### Your Mnemonic Phrase

For security reasons it is good practice to save your private key in an analog format but copying and writing down you private key in one of the formats above is very error prone. In 2013 a *Bitcoin Improvement Proposal* (BIP) was submitted suggesting the use of **mnemonic phrases** to represent private keys.

![Mnemonic Phrase](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/mnemonic.png)

> *"A mnemonic code or sentence is superior for human interaction compared to the handling of raw binary or hexadecimal representations of a wallet seed. The sentence could be written on paper or spoken over the telephone. This guide is meant to be a way to transport computer-generated randomness with a human readable transcription." - [BIP 0039](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), Palatinus, Rusnak, Voisine, Bowe*

In a first step randomness is generated using one of the methods described above. In a second step the initial randomness is split into groups of 11 bits each. 11 bits can represent the values from 0 to 2047. Next each group is mapped to a word list of 2048 common and easily identifiable words. Similar words such as woman/women or build/built are excluded to avoid confusion.

The list of words can be extended by a password if desired and is lastly converted using a key derivation function (PBKDF2) and a hash function to produce your private key. A private key can deterministically  be generated from a mnemonic phrase, but because of the one-wayness or *preimage resistence* you can't compute a mnemonic from a private key.

### Multiplication with Private Key

Now that we have generated a private key $sk$ we need to derive a public key $PK$ from it. In our **last article** we already distinguished between *scalars* and *vectors*. A scalar is something that only has a magnitude. Simply speaking, any number is a scalar. A vector has a magnitude and a direction and is represented by a *tuple* of values. If we are looking at a two-dimensional plane, a vector can be interpreted as an arrow with a certain length, the magnitude, and a direction, the angle relative to the positive x-axis. This means it is a tuple comprising two values, a *double*. In order to represent a vector in three dimensional space one would use a *triple* of values, one for the magnitude and two for the direction (angle relative to x- and z-axis). It is a convention that scalars are written with small letters, like $sk$, while vectors are written with capital letters, like $PK$.

![Scalar vs. Vector](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/scalar_vector_D.jpg)

We added this little discourse because it might help you to keep in mind what are points on the curve and what are just "regular values" or scalars when following along. Especially in our next article on **digital signatures** this will be helpful.

A *generator point* or *base point* is defined for the secp256k1 curve and its coordinates are

$$x = 55066263022277343669578718895168534326250603453777594175500187360389116729240$$

and

$$y = 32670510020758816978083085130507043184471273380659243275938904335757337482424$$

The product of your private key *sk* with the base point *P* gives you your *elliptic public key*, *PK*. If you wonder how computationally expensive it is to multiply such a large number (*sk*) with a point on the curve you can check out our last article on **elliptic curve cryptography** where we calculate the number of computational steps needed. An extra byte is added to the front to give you your *full public key*. The extra byte depends on the specific blockchain you are generating an address for. For Bitcoin it is 0x04, for Horizen it is 0x ?? **TKKG**

![Keys](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/keys_D.jpg)

We will follow along with an example: Our private key needs to have 32 bytes, which equals 32 UTF-8 characters.

    Horizen Academy Testing Address!

The phrase above has 32 characters and converted to hex format it gives us

    486f72697a656e2041636164656d792054657374696e67204164647265737321

We'll use this as our private key for demonstrative purposes.

**TKKG** do this with our test private key from top to bottom.

### Compression

We already mentioned that the elliptic curve being symmetric about the *x*-axis is a property, that makes  compressing a point on the curve very handy. For every *x*-value, there are only two possible *y*-values and they only differ in their sign.

$$y = \pm \sqrt{x^3 + ax + b}$$

If you throw out the *y*-value all together and add a byte indicating the sign of *y* a reduction in size by almost 50\% is achieved.

### From Public Key to Address

Your compressed public is now hashed twice in a row, first using SHA256 and second using RIPEMD160. The second round of hashing accomplishes a compression by 37.5% (from 256 to 160bit). What you have now is your *encrypted public key*.

The next step to getting your address is adding a network byte to the front. Depending on which blockchain you are talking about this can also be two bytes. They indicate if the address is meant to be used on the production network or one of the testnets. Horizen uses two network bytes, which causes the *Base58Check* encoded transparent addresses to start with *zn* while shielded addresses start with *zc*.

After we have added the network byte, a checksum is calculated and concatenated with the address. This serves as a security measure to prevent addresses that were corrupted during transmission to be used by a wallet. The checksum is obtained by hashing the address including the network byte twice and adding the first 4 bytes of the result to the end.

    SHA256(SHA256(network byte+encrypted public key))

Now this is a valid address, but there is one last step to it. *Base58Check* is a way to convert bits into alphanumeric characters, but it excludes the four characters 0, O, I, and l. Base58Check removes these characters from your address to reduce errors when copying addresses manually and proofreading them.

### Hierachichal Deterministic Address Generation}

Most wallets today are HD wallets - hierarchical deterministic wallets. They use a single seed or *master private key* which can be derived from your mnemonic phrase, to generate a number of private keys. This is done by adding a counter to the seed and incrementing it by one for every new private key generated.

![Hierarchical Deterministic Wallet](/assets/post_files/technology/expert/2.3.2-keys-and-addresses/hd_wallet_D.jpg)

These private keys can be used for different blockchains, because the underlying concept is always the same: the private key is a source of randomness that when multiplied with the base point yields your public key. In our **chapter on wallets** we will look at [HD wallets](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) more closely.

### Summary

To get from your private key, *sk* to your address you first perform multiplication on the curve with the base point *P*. Next you add a leading byte, 0x04 (only bitcoin? what about horizen?) **TKKG** , omit the *y*-value while adding a byte for its sign and perform two hash operations in a row, SHA256 first and RIPEMD160 second. An extra byte is added to indicate if the address is meant for the main- or testnet, giving you the *mainnet encrypted public key*. A checksum is calculated by performing SHA256 twice and the first 4 bytes of that checksum are added to the end of the *mainnet encrypted public key*.
Lastly, the characters 0, O, I, and l are removed via Base58Check.

In our next article, we will look at how digital signatures are produced and verified without revealing any knowledge about the private key used to produce the signature.

\url{https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki} mnemonic bip

\url{https://medium.freecodecamp.org/how-to-create-a-bitcoin-wallet-address-from-a-private-key-eca3ddd9c05f}
