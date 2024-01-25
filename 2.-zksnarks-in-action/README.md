---
description: >-
  Core concept: How do we create useful circuits, or how do we turn things we
  care about into useful circuits?
---

# 2. zkSNARKs in Action

We use ZK proof systems as a pragmatic tool. These proof systems can make blockchains much faster, and also allow us to build in more privacy.&#x20;

To recap, zkSNARKs are a form of verifiable computation that take a circuit and a witness and make a proof claiming that the witness satisfies the constraints specified in the circuit. This proof is probabilistic, which means the prover can make the possibility of a fake proof so small that it is considered practically infeasible. Moreover, the prover gets to choose which values in the witness to publicly share, if any at all.

<figure><img src="../.gitbook/assets/3 kopya 2@4x (2).png" alt=""><figcaption><p>Birds eye view of the zkSNARK process. An idealized circuit is sketched out, then turned into actual circuits in code, then a prover can provide a witness and make a proof, then a verifier can check such proofs.</p></figcaption></figure>
