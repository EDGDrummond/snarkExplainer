---
description: >-
  Core concept: zkSNARKs are claims that a prover knows a valid witness to some
  set of constraints
---

# ZK-Circuits

A <mark style="color:purple;">**zk-circuit**</mark> is a collection of constraints where allowed values and operations are clearly defined. In general we will simply call them circuits because there are no other circuits to confuse the zk-circuits with.

Boolean algebra creates the basic logic of computer circuits. But what if rather than the numbers 0 and 1 as in boolean algebra, we use numbers between 0 and 10, and instead of boolean gates, we use addition or multiplication? The change in definitions for values and operations allows us to create the zero-knowledge circuits used in zero-knowledge proofs.

Creating ZK circuits by laying out arithmetic gates like addition and multiplication is called <mark style="color:purple;">**arithmetization**</mark>.

In a ZK circuit, some values will be determined by constraints, and some will be variable. For example, if we have the constraint: $$a*b=c$$,  then $$c$$ has to be the value we get when we multiply $$a$$ by $$b$$.

<figure><img src="../.gitbook/assets/1 kopya@3x (2).png" alt="" width="375"><figcaption><p>A single constraint written as simple math and also drawn as a gate</p></figcaption></figure>

The image above gives you two perspectives on the gate. The first is like the boolean gates we drew earlier that were visually nice, and the second is the same constraint but written more mathematically. We show you both interpretations because some proof systems use constraints that can’t be drawn in a visually pretty way like this multiplication gate.

What values do a and b take? There are 2 possibilities – either they are variable values where we can place any number when filling out the circuit, or they are predetermined and the circuit specifies the value.&#x20;

In this example, if a is variable and b is predetermined to be 23, then c must be 23 times whatever value a is chosen to be.

<figure><img src="../.gitbook/assets/1 kopya 2@3x (2).png" alt="" width="375"><figcaption><p>The same constraint as above, but here b is specified to be 23</p></figcaption></figure>

When we fill out a circuit, what we are doing is plugging in values for all the variable values that the circuit had, as well as determining any downstream values.&#x20;

In the example above we said that $$a$$ was variable, so when we fill out the circuit we will set a value for it, let’s say 3. Then we need to determine the downstream values, which in this case is c. The predetermined value for b is 23, so c then must be 69.

In the example above we said that a was variable, so when we fill out the circuit we will set a value for it, let’s say 3. Then we need to determine the downstream values, which in this case is c. The predetermined value for b is 23, so c then must be 69.

<figure><img src="../.gitbook/assets/1 kopya 3@3x (2).png" alt="" width="375"><figcaption><p>A filled out or satisfied instance of the above 2 constraints (either every value was variable, or b was set to 23)</p></figcaption></figure>

We refer to the collection of all values that aren’t predetermined by the circuit as the <mark style="color:purple;">**witness**</mark>. A witness is considered to be valid or invalid depending on whether it can fill out the circuit correctly.&#x20;

In the above example, c would be a part of the witness, whereas b wouldn’t because it was already specified by the circuit.

<figure><img src="../.gitbook/assets/1 kopya 4@3x (2).png" alt="" width="375"><figcaption><p>2 examples of a constraint (left) with a valid witness (right)</p></figcaption></figure>

The above circuits each with a valid witness because the math checks out. In the first circuit b is a part of the witness because it is not specified by the circuit.

As you may have noticed, even in our simple circuit example, there are many possible witness values that you could plug in to make the circuit work. This is why circuits can be reused. Once the order of operations and constraints have been defined, the witness can be changed to serve different purposes.&#x20;

And, of course, this being zero-knowledge proofs, witness values can be kept secret.&#x20;

SNARKs are really a tool for making proofs about circuits having a valid witness. If a prover is able to provide a valid witness, they are on their way to making a credible and verifiable claim without revealing the actual data or inputs used.

From these simple relationships and some other basic tools, which we will discuss throughout this series, we can start building arguments of knowledge or justifications for why something is true (why your witness satisfies the circuit).

<details>

<summary>What is a ZK-Circuit?</summary>

Like a normal computer circuit, it is a collection of constraints - best imagined as a series of gates connected with wires

</details>

<details>

<summary>What does a ZK-proof actually claim?</summary>

"With 99.99...% likelihood, the prover knows a witness that satisfies the constraints of a circuit"

</details>

