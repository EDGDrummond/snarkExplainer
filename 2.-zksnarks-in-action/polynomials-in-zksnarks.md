---
description: >-
  Core concept: zkSNARKs make polynomials, commit to them and then query them to
  make proofs
---

# Polynomials in zkSNARKs

<figure><img src="../.gitbook/assets/3 kopya 3@4x.png" alt=""><figcaption><p>Birds eye overview of zkSNARKs, but with polynomials</p></figcaption></figure>

Proof systems generally involve the prover committing to their witness, and then the rest of the protocol is about probing this commitment to see if it 'behaves like' a valid witness.&#x20;

The proof creation process primarily consists of committing to a polynomial and then querying the committed polynomial in order to see if the polynomial behaves as we expect it to.

Here we want to outline a few more details of how a generic ZK proof system will work, what is done with the circuit and witness, and elaborate a bit on what we do with polynomials. There will be some simple polynomials and evaluations sketched out, but not much beyond that.&#x20;

zkSNARKs largely come down to doing operations on polynomials so understanding the basic concepts in the rest of this section will really solidify your understanding of how SNARKs work.&#x20;

To get to our ZK proof, the basic steps are:

1. Make the polynomials
2. Commit to the polynomials
3. Query the committed polynomials&#x20;

### Making polynomials

This is a basic circuit we can use to help think about the steps:

<figure><img src="../.gitbook/assets/1 kopya 5@4x.png" alt="" width="188"><figcaption><p>A simple zk-circuit consisting of 3 constraints (1 multiplication and 2 addition) connected in a particular way</p></figcaption></figure>

Remember that a witness is a set of values that the prover claims satisfies the circuit, so we can place all the witness values in the wires that they correspond to. In this example, we decide to use 3 and 1 as the inputs, but since circuits are reusable, we could use other values if we wanted.&#x20;

<figure><img src="../.gitbook/assets/3 kopya 4@4x.png" alt="" width="375"><figcaption><p>Left: The original circuit with a satisfying witness below. Right: The same idea but put the witness values in their corresponding wires</p></figcaption></figure>

Now we see that every gate has two values going into it, and one value coming out. Take each of the output values and put them in their own list. That would give us:&#x20;

{21, 6, 27}

| Then do the same for the left input wires: | and the right input wires: |
| ------------------------------------------ | -------------------------- |
| {3, 1, 21}                                 | {7, 5, 6}                  |

Now we have three sets of values and can make the polynomials.

For each list, we see the first value as the constant in the polynomial. Then we see the next value in the list as the coefficient we multiply $$x$$ by in the polynomial. Then the third value in the list is what we multiply by $$x^2$$. So for the output wires set, we get the polynomial:

$$\text{output} = (\text{third value})x^2 + (\text{second value})x + \text{first value}$$

$$\text{out}(x) = 27x^2 + 6x + 21$$

Then for the others we will get:

| $$\text{left}(x) = 21x^2 + x + 3$$ | $$\text{right}(x) = 6x^2 + 5x + 7$$ |
| ---------------------------------- | ----------------------------------- |

And that’s it, we have now made some polynomials!

Right now we only have three values because we have three gates. But if we had more gates, we could just continue this process, and we would have polynomials with higher powers, such as $$x^{100}$$ or $$x^{1000}$$. An important thing to note about this process is each gate represents one step in the circuit’s computation, so having the correct values is not enough. The values must be presented at the correct place in the correct order. Any misstep or errors will result in a failing proof, and this is one thing that makes it nearly impossible to come up with the correct values via fraudulent means.&#x20;

### Committing polynomials

Now that we have made the polynomials, we want a way to commit to them. Metaphorically, we want to put these polynomials – as they are – into a locked box so we can later come back to them and make sure they haven’t been changed. In order to do this, we use something called a <mark style="color:purple;">**polynomial commitment scheme**</mark>.

We will take each polynomial and evaluate them at a random, unknown value. If that value becomes known, the prover will be able to make invalid proofs pass with ease, so it always has to remain hidden. For our purposes here we can treat the process of evaluating polynomials at a random unknown value as a black box.

The random, unknown value that our black box evaluates polynomials at never changes, once the box is made it is set for life. And the box works only on polynomials; so for example $$1/x$$, which is a function but not a polynomial, would not be evaluated by the box. Moreover, like a one-way hash function, it is impossible to take an output of the box from any input polynomial and work out what the value was.

$$\text{out}(?) = 27?^2 + 6? + 21 = 142$$

| $$\text{left}(?) = 973891$$ | $$\text{right}(?) = 369$$ |
| --------------------------- | ------------------------- |

The random point selected here will never be known. At this stage, we are only interested in getting out the commitments.&#x20;

Later on, when we query the polynomial, we will evaluate it at a different value that is known. After we query it, we can combine those values together to get out another value, and we will see if that value behaves as expected.

These evaluations that we just made with the unknown value are what we use as the commitments, the locked boxes. Later on when we want to use these polynomials, we can use these commitments to ensure that the polynomials being used are the same polynomials we started with, and not some new ones that were cooked up to try to convince us.

We can black box this again, as the details of how we can use this commitment to ensure the same polynomial is used is very math-heavy. The core idea is that the commitment can be used later and can’t be changed by the prover.&#x20;

These polynomial commitment schemes are a huge part of any ZK proof system, in general. They are what decides whether the proof system is <mark style="color:purple;">**post-quantum secure**</mark>**,** and arguably more importantly, they are the <mark style="color:purple;">**computation bottleneck**</mark> in proof creation.

### Querying polynomials

Now that the polynomials are made and committed to, the verifier will want to query those polynomials at certain values to see if they ‘behave’ as expected. So we will select a random value and ask for the evaluation of the polynomials at this new value - it is imperative that the prover have no prior knowledge as to what this value will be, so that is why it is chosen randomly. For example, the verifier might ask for the evaluation at 10.

$$\text{out}(10) = 27*10^2 + 6*10 + 21 = 2781$$

| left(10) = 2113 | right(10) = 657 |
| --------------- | --------------- |

The chosen values when querying the polynomials are known – as opposed to the values used in the commitment, which remain unknown. In the example above, we use a value that is very straightforward.

We will use the commitments from earlier to check that the new evaluations the prover provides are indeed correct. We also mix the polynomials together. So there are a lot of checks going on. But the core idea here is checking that the newly produced values were made by the same polynomial we committed to.

We will do this a number of times, and the values we get out will be the ZK proof (in combination with the earlier values). The verifier can then check if the expected relationship between all these values holds - if they do it implies everything was done in a valid manner and therefore that the provider knows (and used) a satisfying witness to create the proof with overwhelming likelihood.

The intuition to emphasize here is that we can multiply and add polynomials together; and since we made these polynomials from a structured circuit we had earlier, there are relationships between the polynomials that we know should hold true.

<details>

<summary>What role do polynomials play in zkSNARKs</summary>

zkSNARKs first need to make polynomials to represent the computation, then commit to them so that they can't change them. Then these polynomials are queried, and these values are combined and checked to see if they behave as expected

</details>

<details>

<summary>What do we use to commit to polynomials?</summary>

We use polynomial commitment schemes, which are sort of like putting them in a locked box to be used later to see whether new values come from the same polynomial

</details>
