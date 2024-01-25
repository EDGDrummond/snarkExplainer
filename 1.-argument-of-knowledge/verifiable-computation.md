---
description: >-
  Core concept: zkSNARKs are a type of VC, and that is a simpler way to think
  about them conceptually
---

# Verifiable computation

<mark style="color:purple;">**zkSNARKs**</mark> are an example of <mark style="color:purple;">**Verifiable Computation**</mark> (VC), which means a system where computational work is checked to see if it was done correctly. The proof that is created is evidence that suggests with 99.999…% probability, the relevant computation was done correctly.

Of course there is a naive way to check any computation with 100% probability, and that is to simply redo the computation yourself and see if you get the same result. But it turns out that you can check the validity of a result without redoing the computation.&#x20;

And it turns out we can go a step further, and check the validity of a computation without knowing all the data that was used for it. For example, if you (the <mark style="color:purple;">**verifier**</mark>) give your friend (the <mark style="color:purple;">**prover**</mark>) a Rubik’s cube and they solve it, when they show you the solved Rubik’s cube you will be convinced that they indeed did solve the computation, but you have not learned the solution of how to solve a Rubik’s cube. Your friend, the prover, has shown that they know an answer without revealing any other information. The proof is zero-knowledge and preserves privacy.&#x20;

zkSNARKs also turn out to have another very valuable property: the process of verifying the computation is far quicker than redoing the computation. We call this succinctness or scalability.

There is a lot more detail we need to add to understand this better, but this is a good catalyst to grow your understanding around. The rest of this course is aimed at adding memorable chunks of detail to this basic idea.

<details>

<summary>What is VC?</summary>

VC is a process where the outcome of a computation can be verified; of course this is only interesting when the verification does not involve redoing the computation

</details>
