# A Bird’s Eye View of PLONK

We did all that work to explore the mathematical components behind PLONK and other proof systems, so let’s fit them together to see how they interact.

<figure><img src="../.gitbook/assets/3 kopya 3@4x.png" alt=""><figcaption><p>A bird's eye view of PLONK.</p></figcaption></figure>

The starting point of any proof system is the same: "What computation do we need to know has been done correctly?" Once we have that we can turn it into an [idealized circuit](../part-1/zksnarks-in-action.md).&#x20;

In PLONK, we use plonkish arithmetization to turn the idealized circuit into a detailed zk-circuit. So we now have addition and multiplication gates connected by wires to represent our computation. Some of the wire values are decided, but some will have to be inserted by the prover.

The prover will fill in all the empty wire values using their private data. With that done, we have a filled out circuit. We then list all the left input wires from the gates (_a_ in the image below represents all the left input wires), and get something like $$(\text{value}_1, \text{value}_2, …, \text{value}_n)$$. We represent this list as a polynomial by using them as coefficients. As a result, we get:&#x20;

&#x20;$$(f(x)=\text{value}_1 + \text{value}_2 * x + \text{value}_3 * x^2 + \cdots + \text{value}_n * x^{n-1})$$.

<figure><img src="../.gitbook/assets/1 kopya@3x (2).png" alt="" width="375"><figcaption><p>An example multiplication gate.</p></figcaption></figure>

We can do that for the right input wires too, as well as the output wires. And then we can commit to the polynomials using our [PCS](../part-2/polynomial-commitment-schemes.md). And now we can make proofs about our computation by opening commitments (called querying since we query polynomials to get their openings). And this will keep all our data secret thanks to the [DLOG assumption](../part-2/elliptic-curves-and-dlog.md).

Lastly we will use the openings to our commitments and [elliptic curve cryptography](../part-2/elliptic-curves-and-dlog.md) to check that they satisfy the expected relationship. And if they satisfy the expected relationship, then by the [Schwartz-Zippel Lemma](../part-2/schwartz-zippel-and-polynomials.md), we can conclude that the computation was indeed valid.
