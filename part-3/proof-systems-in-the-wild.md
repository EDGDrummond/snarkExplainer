# A Bird’s Eye View of PLONK

## A Bird’s Eye View of PLONK

We did all that work to explore the mathematical components behind PLONK and other proof systems, so let’s fit them together to see how they interact.

<figure><img src="../.gitbook/assets/3 kopya 3@4x.png" alt=""><figcaption><p>A bird's eye view of PLONK.</p></figcaption></figure>

The starting point of any proof system is the same: "What computation do we need to know has been done correctly?" Once we have that we can turn it into an [idealized circuit](../part-1/zksnarks-in-action.md).

In PLONK, we use plonkish arithmetization to turn the idealized circuit into a detailed zk-circuit. So we now have addition and multiplication gates connected by wires to represent our computation. Some of the wire values are decided, but some will have to be inserted by the prover.

The prover will fill in all the empty wire values using their private data. With that done, we have a filled out circuit. We then list all the left input wires from the gates (_a_ in the image below represents all the left input wires), and get something like $$(\text{value}_1, \text{value}_2, …, \text{value}_n)$$. We represent this list as a polynomial by using them as coefficients. As a result, we get:

$$(f(x)=\text{value}_1 + \text{value}_2 * x + \text{value}_3 * x^2 + \cdots + \text{value}_n * x^{n-1})$$.

<figure><img src="../.gitbook/assets/1 kopya@3x (2).png" alt="" width="375"><figcaption><p>An example multiplication gate.</p></figcaption></figure>

We can do that for the right input wires too, as well as the output wires. And then we can commit to the polynomials using our [PCS](../part-2/polynomial-commitment-schemes.md). And now we can make proofs about our computation by opening commitments (called querying since we query polynomials to get their openings). And this will keep all our data secret thanks to the [DLOG assumption](../part-2/elliptic-curves-and-dlog.md).

Lastly we will use the openings to our commitments and [elliptic curve cryptography](../part-2/elliptic-curves-and-dlog.md) to check that they satisfy the expected relationship. And if they satisfy the expected relationship, then by the [Schwartz-Zippel Lemma](../part-2/schwartz-zippel-and-polynomials.md), we can conclude that the computation was indeed valid.

## Constraint Count

Before diving into the systems, we briefly need to explore one concept that is going to help us understand the idea of efficiency as it relates to circuits. This is the concept of a constraint count.

The constraint count determines how big the circuits are. The bigger the circuit, the longer it will take to generate the proof and the more data is required to store the proof. Lower constraint counts mean smaller circuits and faster proving (or proof generation) times, as well as less data needed to store the proof.

<figure><img src="../.gitbook/assets/plonk table 1.png" alt=""><figcaption><p>Table from the <a href="https://eprint.iacr.org/2019/953.pdf">PLONK paper</a>.</p></figcaption></figure>

During the arithmetization process, the idealized circuit had to be turned into addition and multiplication gates. Each gate adds 1 to the constraint count. Generally speaking, a low constraint count is good and a high constraint count is bad. Most circuit developers are aiming for a minimal constraint count. An optimal circuit should contain enough constraints to verify a computation in a secure and valid way, but not so many that proving time, storage, and computational requirements become too burdensome.

<figure><img src="../.gitbook/assets/Çalışma Yüzeyi 15 kopya@4x.png" alt="" width="563"><figcaption><p>Idealized circuits from our <a href="../part-1/zksnarks-in-action.md">idealized circuits</a> subsection.</p></figcaption></figure>

Remember that circuits are reusable. Both the prover and verifier will know the layout and logic of the circuit in advance. This circuit will contain many wires with known values and some wires with undecided values. The prover will be putting private information into those undecided wires, for example inserting their private key, their address, or who they are voting for. In the backend, the prover will take the circuit, fill in their private values and make a proof; then the verifier will take in this original circuit and check whether the proof behaves as it is supposed to. Because circuits may be reused millions or even billions of times, the right constraint count is very important.

The constraint count will also vary depending on the type of arithmetization you use. For example, to represent the computation of voting, we can use Plonkish arithmetization or binary arithmetization (what computers do), which would result in different constraint counts. And there are types of problems that are more suited to using Plonkish arithmetization, and types of problems that are more suited to other methods. Comparing Proof Systems
