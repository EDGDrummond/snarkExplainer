# zkSNARKs in Action

We use ZK proof systems as a pragmatic tool. These proof systems can make blockchains much faster, and also allow us to build in more privacy.

To recap, zkSNARKs are a form of verifiable computation that take a circuit and a witness and make a proof claiming that the witness satisfies the constraints specified in the circuit. This proof is probabilistic, which means the prover can make the possibility of a fake proof so small that it is considered practically infeasible. Moreover, the prover gets to choose which values in the witness to publicly share, if any at all.

<figure><img src="../.gitbook/assets/3 kopya 2@4x (2).png" alt=""><figcaption><p>Birds eye view of the zkSNARK process. An idealized circuit is sketched out, then turned into actual circuits in code, then a prover can provide a witness and make a proof, then a verifier can check such proofs.</p></figcaption></figure>

## Idealized Circuits

Creating a fully usable circuit requires a good deal of skill and experience to do correctly and securely. There number of experts who can do this reliably is relatively small. But nearly anyone can create an idealized circuit, a useful stepping stone to a real circuit.

Idealized circuits represent the core idea of what a computation should do without any detail. They turn complex functions such as checking if someone can vote into a black box that can be easily moved around and manipulated. These simplified circuits allow us to focus more on design decisions and less on the nitty gritty computational details.

Here are a couple sketches of idealized circuits for things we said we might want to prove earlier.

<figure><img src="../.gitbook/assets/Çalışma Yüzeyi 15 kopya@4x.png" alt="" width="563"><figcaption><p>Left circuit is a voting circuit: check that the voter id is a valid voting id and that the signature is valid, combine with vote for data. Right circuit is a check that Alice has more than 1 ETH: check that Ethereum state input is valid and that the specified account has enough ETH</p></figcaption></figure>

Coming up with idealized circuits is easy, which is great because it allows us to discuss them quickly without needing to add in tons of detail. But we do have to be careful. Firstly, making any circuit takes a lot of work, so bear that in mind when trying to find useful circuits. Secondly, not all ideas can actually be represented as computational circuits. In fact it takes some understanding of zkSNARKs to be able to understand what circuits are possible and what are not.

<details>

<summary>What is an idealized circuit?</summary>

It is a simplified version of a circuit wherein all the details are black-boxed

</details>

## Useful circuits

In the blockchain world, it is more likely that people who are not SNARK experts (maybe you?) will come up with ideas for useful circuits, and then SNARK experts will turn those desired circuits into code. Let’s explore some circuits we actually use in the industry and discuss their value.

**Transaction circuits**

A circuit that checks whether you have a balance available to spend, an address you want to send some amount to, a check that that amount is less than your balance, and an authorization to send the amount to that address.

If the system is designed to not need to reveal anything from the witness, we will be able to send crypto completely anonymously - anybody looking at the proof won’t know which account the funds came from, where it was sent, which asset was sent or how much was sent. True anonymity.

**Smart contract circuits**

Rather than conduct all the computation of a smart contract on-chain, we can make circuits that represent the computation that will happen when a smart contract is called, and reveal the relevant witness values. Now, a proof will demonstrate that a smart contract was executed correctly, and that the result of the computation can be used. But the gas cost of putting this proof verification on-chain can be a lot cheaper than directly running the smart-contract on-chain, depending on what the smart contract did. The value is again clear, you will need to spend less gas to do things on-chain.

For some example values, verifying a ZK proof from the proof system plonk (used by Aztec for example) costs somewhere around 300,000 gas, whereas a proof from the StarkWare proof system costs around 5,000,000 gas. So proof system choice is important, and if your circuit is not providing you anonymity you will only get the scaling benefit if the proof represents a computation that would have cost more gas to do on-chain directly. But proofs can represent huge computations since the gas cost doesn’t increase much with a larger computation.

**EVM circuits**

Nodes in a blockchain need to check that any new block coming from other nodes were made correctly, so they have been re-executing all the computation to check that this new block is correct. Rather than that, we can have a circuit that represents the checking of a block being valid, and reveal witness values that should be public.

A valid proof of a circuit like this with revealed witness values of the block’s values will remove the need for nodes to re-execute all new blocks they are sent, they can simply check the proof and save themselves a lot of time and computing power. Again, this is a clear use case. And this is what [“type-1” zkEVMs](https://vitalik.ca/general/2022/08/04/zkevm.html) are trying to achieve.

<details>

<summary>Why are ZK-proofs valuable?</summary>

Because they demonstrate the correct execution of a computation that has real world relevance, for example correctly processing a transaction, or smart contract, or correctly verifying that a block is valid

</details>

## Polynomials

We now want to introduce <mark style="color:purple;">**polynomials**</mark>. If even the mention of math makes your eyes glaze over, try black boxing these tools a bit; try thinking of it as a black box that has magical properties. You accept that you don’t understand the details at this moment, but you know what the box does and how to use it. For example you may not know how the internet works in any detail, but you know what it does and how to use it. And with time and dedication, you can learn the details.

<figure><img src="../.gitbook/assets/1 kopya 6@4x.png" alt="" width="375"><figcaption><p>Polynomial means “many terms”</p></figcaption></figure>

Polynomials are equations where the exponents or powers of the variables must be positive whole numbers, such as $$x^2$$. Functions are also labeled, so $$f(x) = x^2$$ is also a polynomial. All this means is the function is called $$f$$, and it has $$x$$ as a variable.

We could also have $$f(x,y)=2xy^2$$. Now we have a second function, also called $$f$$, but it has $$x$$ and $$y$$ as variables. In general we will use different names when we have multiple functions in a single context to avoid confusion. Here are some more examples to clarify:

| Polynomials              | Not polynomials  |
| ------------------------ | ---------------- |
| $$x^3$$                  | $$x^{2/3}$$      |
| $$f(x) = 3x^2 + x + 4$$  | $$1/x = x^{-1}$$ |
| $$g(x,y) = 5xy^2 + 3xy$$ | $$3x$$           |

Polynomials are a very useful mathematical object. It is because of polynomials that we get the succinctness and zero-knowledge properties that are central to zkSNARK tech. They can hold a lot of information and we can check that the information is what we expected with a very high degree of certainty by only looking at a single value. This little black box is light in weight but heavy in data, and it can hold secrets. In later sections we will explore why this is true and elucidate more clearly what it means.

Because polynomials are such [useful tools and building blocks](https://app.streameth.org/devconnect/progcrypto/session/why\_you\_should\_care\_about\_polynomials), all zkSNARKs make use of polynomials in some fashion. More precisely, we will be representing our witness values and constraints in polynomials, and then we will commit to these polynomials via something known as a polynomial commitment scheme.

<details>

<summary>What is a polynomial?</summary>

A function in any number of variables but where the variables have powers that positive, whole numbers

</details>

<details>

<summary>Why are polynomials relevant?</summary>

Polynomials are useful building blocks since they can capture a lot of data and allow us to compress it and keep it hidden

</details>

## Polynomials in zkSNARKs

<figure><img src="../.gitbook/assets/3 kopya 3@4x.png" alt=""><figcaption><p>Birds eye overview of zkSNARKs, but with polynomials</p></figcaption></figure>

Proof systems generally involve the prover committing to their witness, and then the rest of the protocol is about probing this commitment to see if it 'behaves like' a valid witness.

The proof creation process primarily consists of committing to a polynomial and then querying the committed polynomial in order to see if the polynomial behaves as we expect it to.

Here we want to outline a few more details of how a generic ZK proof system will work, what is done with the circuit and witness, and elaborate a bit on what we do with polynomials. There will be some simple polynomials and evaluations sketched out, but not much beyond that.

zkSNARKs largely come down to doing operations on polynomials so understanding the basic concepts in the rest of this section will really solidify your understanding of how SNARKs work.

To get to our ZK proof, the basic steps are:

1. Make the polynomials
2. Commit to the polynomials
3. Query the committed polynomials

#### Making polynomials

This is a basic circuit we can use to help think about the steps:

<figure><img src="../.gitbook/assets/1 kopya 5@4x.png" alt="" width="188"><figcaption><p>A simple zk-circuit consisting of 3 constraints (1 multiplication and 2 addition) connected in a particular way</p></figcaption></figure>

Remember that a witness is a set of values that the prover claims satisfies the circuit, so we can place all the witness values in the wires that they correspond to. In this example, we decide to use 3 and 1 as the inputs, but since circuits are reusable, we could use other values if we wanted.

<figure><img src="../.gitbook/assets/3 kopya 4@4x.png" alt="" width="375"><figcaption><p>Left: The original circuit with a satisfying witness below. Right: The same idea but put the witness values in their corresponding wires</p></figcaption></figure>

Now we see that every gate has two values going into it, and one value coming out. Take each of the output values and put them in their own list. That would give us:

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

Right now we only have three values because we have three gates. But if we had more gates, we could just continue this process, and we would have polynomials with higher powers, such as $$x^{100}$$ or $$x^{1000}$$. An important thing to note about this process is each gate represents one step in the circuit’s computation, so having the correct values is not enough. The values must be presented at the correct place in the correct order. Any misstep or errors will result in a failing proof, and this is one thing that makes it nearly impossible to come up with the correct values via fraudulent means.

#### Committing polynomials

Now that we have made the polynomials, we want a way to commit to them. Metaphorically, we want to put these polynomials – as they are – into a locked box so we can later come back to them and make sure they haven’t been changed. In order to do this, we use something called a <mark style="color:purple;">**polynomial commitment scheme**</mark>.

We will take each polynomial and evaluate them at a random, unknown value. If that value becomes known, the prover will be able to make invalid proofs pass with ease, so it always has to remain hidden. For our purposes here we can treat the process of evaluating polynomials at a random unknown value as a black box.

The random, unknown value that our black box evaluates polynomials at never changes, once the box is made it is set for life. And the box works only on polynomials; so for example $$1/x$$, which is a function but not a polynomial, would not be evaluated by the box. Moreover, like a one-way hash function, it is impossible to take an output of the box from any input polynomial and work out what the value was.

$$\text{out}(?) = 27?^2 + 6? + 21 = 142$$

| $$\text{left}(?) = 973891$$ | $$\text{right}(?) = 369$$ |
| --------------------------- | ------------------------- |

The random point selected here will never be known. At this stage, we are only interested in getting out the commitments.

Later on, when we query the polynomial, we will evaluate it at a different value that is known. After we query it, we can combine those values together to get out another value, and we will see if that value behaves as expected.

These evaluations that we just made with the unknown value are what we use as the commitments, the locked boxes. Later on when we want to use these polynomials, we can use these commitments to ensure that the polynomials being used are the same polynomials we started with, and not some new ones that were cooked up to try to convince us.

We can black box this again, as the details of how we can use this commitment to ensure the same polynomial is used is very math-heavy. The core idea is that the commitment can be used later and can’t be changed by the prover.

These polynomial commitment schemes are a huge part of any ZK proof system, in general. They are what decides whether the proof system is <mark style="color:purple;">**post-quantum secure**</mark>**,** and arguably more importantly, they are the <mark style="color:purple;">**computation bottleneck**</mark> in proof creation.

#### Querying polynomials

Now that the polynomials are made and committed to, the verifier will want to query those polynomials at certain values to see if they ‘behave’ as expected. So we will select a random value and ask for the evaluation of the polynomials at this new value - it is imperative that the prover have no prior knowledge as to what this value will be, so that is why it is chosen randomly. For example, the verifier might ask for the evaluation at 10.

$$\text{out}(10) = 27*10^2 + 6*10 + 21 = 2781$$

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

## Recap

zkSNARKs are a valuable tool because we can represent <mark style="color:purple;">**useful**</mark> computations such as: <mark style="color:purple;">**transaction circuits**</mark> that can allow users to transact on blockchains with almost complete anonymity; <mark style="color:purple;">**smart contract circuits**</mark> that run computations off-chain thus saving on gas costs; and <mark style="color:purple;">**EVM circuits**</mark> that can verify Ethereum blocks without redundantly re-running all the transactions. With <mark style="color:purple;">**idealized circuits**</mark>**,** more use cases can be imagined by SNARK non-experts and it becomes possible to discuss how to use SNARKs without knowing all the details. But experts will still be needed to understand how to make <mark style="color:purple;">**polynomials**</mark>, commit to them via <mark style="color:purple;">**Polynomial commitment schemes**</mark>**,** and query the polynomials so the rough ideas can turn into practical and secure ZK proofs.

### Terms

<mark style="color:purple;">**Transaction circuit**</mark>**:** A circuit that represents the computation required to check a valid transaction call on a particular blockchain, for example on Ethereum

<mark style="color:purple;">**Smart contract circuit**</mark>**:** A circuit that represents the computation within a smart contract, where all the inputs to the smart contract function will be inputs in the circuit

<mark style="color:purple;">**EVM circuit**</mark>**:** A circuit that represents the computational work involved in checking whether a block is valid

<mark style="color:purple;">**Idealized circuit**</mark>**:** A rough sketch of a circuit’s logic and functionality where the computational work involved is black boxed

<mark style="color:purple;">**Polynomials**</mark>: A function where variables (like x or y) have powers that are whole numbers. Polynomials have the useful property of being able to hide and compress values

<mark style="color:purple;">**Polynomial commitment scheme**</mark>: A process for creating a value from a polynomial that can later be used to check other values come from the same polynomial (sort of like a locked box)
