---
layout: post
type: article
title: "Public Key Cryptography"
description: "The Horizen Academy is a free educational platform on blockchain technology, cryptocurrency, and privacy. This chapter is is not available yet. We add content frequently, sign up for our newsletter for notifications when it's released."
permalink: /technology/expert/public-key-cryptography/
topic: technology
level: expert
chapter: "How Does a Blockchain Work?"
published: false
---

An integral part of blockchain technology is public-key cryptography (PKC). There are several PKC schemes in existence but the two most popular ones are RSA (after Rivest, Shamir and  Adleman) and elliptic curve cryptography (ECC). In blockchains elliptic curve cryptography is used to prove and validate ownership of data.

The inner workings of ECC are often treated as a black box and we want to shed some light on the inner workings of the type of cryptography that gave *crypto*currencies their name. To make this topic more digestible we split the topic into three parts.

<div class="row mt-5">
    <div class="col-md-3">
        <a href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-1-elliptic-curve-cryptography %}">
            <img src="/assets/post_files/technology/advanced/what-is-a-blockchain/VT2.svg" alt="Protocols" />
        </a>
    </div>
    <div class="col-md-9">
        <h5 class="intro-article-title">Elliptic Curve Cryptography</h5>
        <p class="mb-1">
            First, we will explain ECC in general and show you the basics of how math on an elliptic curve works. We look at basic mathematical operations and examine the complexity of performing those operations.
        </p>
        <p class="mb-0">
            <a class="font-weight-bold" href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-1-elliptic-curve-cryptography %}">Read Article</a>
        </p>
    </div>
</div>


<div class="row mt-5">
    <div class="col-md-3">
        <a href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-2-generating-keys-and-addresses %}">
            <img src="/assets/post_files/technology/advanced/what-is-a-blockchain/VT2.svg" alt="Protocols" />
        </a>
    </div>
    <div class="col-md-9">
        <h5 class="intro-article-title">Generating Keys and Addresses</h5>
        <p class="mb-1">
            Next, we demonstrate how we use ECC to first derive a public key from your private key and in a second step your address from your public key.
        </p>
        <p class="mb-0">
            <a class="font-weight-bold" href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-2-generating-keys-and-addresses %}">Read Article</a>
        </p>
    </div>
</div>


<div class="row mt-5">
    <div class="col-md-3">
        <a href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-3-digital-signatures %}">
            <img src="/assets/post_files/technology/advanced/what-is-a-blockchain/VT2.svg" alt="Protocols" />
        </a>
    </div>
    <div class="col-md-9">
        <h5 class="intro-article-title">Digital Signatures</h5>
        <p class="mb-1">
            Lastly, we explain how one can prove ownership of a private key without revealing any information about it by creating a digital signature with it. We also show how a verifier can validate the digital signature only knowing your public  key.
        </p>
        <p class="mb-0">
            <a class="font-weight-bold" href="{{ site.baseurl }}{% post_url /technology/expert/2022-02-04-3-digital-signatures %}">Read Article</a>
        </p>
    </div>
</div>
