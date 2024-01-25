---
description: >-
  Core concept: idealized circuits represent the core idea of what a computation
  should do without any detail
---

# Idealized Circuits

Creating a fully usable circuit requires a good deal of skill and experience to do correctly and securely. There number of experts who can do this reliably is relatively small. But nearly anyone can create an idealized circuit, a useful stepping stone to a real circuit.

Idealized circuits represent the core idea of what a computation should do without any detail. They turn complex functions such as checking if someone can vote into a black box that can be easily moved around and manipulated. These simplified circuits allow us to focus more on design decisions and less on the nitty gritty computational details.&#x20;

Here are a couple sketches of idealized circuits for things we said we might want to prove earlier.

<figure><img src="../.gitbook/assets/Çalışma Yüzeyi 15 kopya@4x.png" alt="" width="375"><figcaption><p>Left circuit is a voting circuit: check that the voter id is a valid voting id and that the signature is valid, combine with vote for data. Right circuit is a check that Alice has more than 1 Eth: check that Ethereum state input is valid and that the specified account has enough Eth</p></figcaption></figure>

Coming up with idealized circuits is easy, which is great because it allows us to discuss them quickly without needing to add in tonnes of detail. But we do have to be careful. Firstly, making any circuit takes a lot of work, so bear that in mind when trying to find useful circuits. Secondly, not all ideas can actually be represented as computational circuits. In fact it takes some understanding of zkSNARKs to be able to understand what circuits are possible and what are not.

<details>

<summary>What is an idealized circuit?</summary>

It is a simplified version of a circuit wherein all the details are black-boxed

</details>
