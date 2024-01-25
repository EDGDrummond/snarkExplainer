---
description: 'Core concept: zkSNARKs give probabilistic claims'
---

# Mathematical guarantees&#x20;

Math can be thought of as a complex logical game where the basic rule is consistency. Contradictions are not allowed, which means one statement and its opposite cannot both be true in the same context. With consistency and some basic assumptions called <mark style="color:purple;">**axioms**</mark>, we can derive new truths and objects in the form of <mark style="color:purple;">**theorems**</mark> via proofs. By playing around with definitions or moving different objects around in new ways, mathematical areas and tools can appear - these allow us to build things that were not possible before as beautifully demonstrated by this video here:

{% embed url="https://www.youtube.com/watch?v=B1J6Ou4q8vE" %}

<mark style="color:purple;">**Proof systems**</mark> will give us mathematical proofs that say something like “with 99.9999999999999999% likelihood, the following statement is true: Alice owns more than 1 ETH.” But the value of this mathematical proof is in its relation to the real world.&#x20;

For additional examples, a ZK proof could tell us that:

* Alice is over a certain age
* Alice can vote in a certain jurisdiction
* Alice has no criminal record

The percentage likelihood means <mark style="color:purple;">**ZK proofs**</mark> are probabilistic. They are not true with absolute certainty, but the chances of them being wrong or broken are practically zero.

As an analogy, we often work in realms where there are 2^256 values, which is about 10^77 (10 with 77 zeroes after it). That’s roughly the number of particles in the entire observable universe, and we quite simply will never have traditional computers that can search a space this large to find a hidden value. And that’s the basis of a lot of our security. You can check out a more beautifully presented example of this size in this video here:

{% embed url="https://www.youtube.com/watch?ab_channel=3Blue1Brown&v=S9JGmA5_unY" %}

The logical consistency of the mathematical system in which ZK proofs are created, combined with the probability of the assumptions that underlie that system give us a remarkable degree of certainty in the truth of the proof. In other words, proofs can be trusted because of strong logic and very high probabilities.&#x20;

These assumptions are based purely on mathematical ideas, and mathematical proofs make those ideas inherently true, so there is less room for a discussion of whether a belief is justified. Because the assumptions are considered secure or very unlikely to be broken, then the proofs that are derived from the assumptions also inherit the property of being secure or hard to break.&#x20;

With ZK proofs, we will take their claims and say “since the probability is so high, we can assume that, for all intents and purposes, Alice really does have 1 ETH, really is over a certain age, or really can vote.”&#x20;

As we delve deeper we will see in more detail what kinds of statements we will be making proofs for, and why these statements hold value. But first, let's cut to the chase and give you the core idea of what zkSNARKs are.

<details>

<summary>What sort of claim does a zk proof make?</summary>

"With 99.99...% likelihood, the following claim is true: Alice has >1 Eth"

</details>
