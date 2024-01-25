---
description: >-
  Core concept: zk-proofs have value based on what real-world computation it
  claims to have done
---

# Useful circuits

In the blockchain world, it is more likely that people who are not SNARK experts (maybe you?) will come up with ideas for useful circuits, and then SNARK experts will turn those desired circuits into code. Let’s explore some circuits we actually use in the industry and discuss their value.

#### Transaction circuits

A circuit that checks whether you have a balance available to spend, an address you want to send some amount to, a check that that amount is less than your balance, and an authorization to send the amount to that address.

If the system is designed to not need to reveal anything from the witness, we will be able to send crypto completely anonymously - anybody looking at the proof won’t know which account the funds came from, where it was sent, which asset was sent or how much was sent. True anonymity.&#x20;

#### Smart contract circuits

Rather than conduct all the computation of a smart contract on-chain, we can make circuits that represent the computation that will happen when a smart contract is called, and reveal the relevant witness values. Now, a proof will demonstrate that a smart contract was executed correctly, and that the result of the computation can be used. But the gas cost of putting this proof verification on-chain can be a lot cheaper than directly running the smart-contract on-chain, depending on what the smart contract did. The value is again clear, you will need to spend less gas to do things on-chain.&#x20;

For some example values, verifying a ZK proof from the proof system plonk (used by Aztec for example) costs somewhere around 300,000 gas, whereas a proof from the StarkWare proof system costs around 5,000,000 gas. So proof system choice is important, and if your circuit is not providing you anonymity you will only get the scaling benefit if the proof represents a computation that would have cost more gas to do on-chain directly. But proofs can represent huge computations since the gas const doesn’t increase much with a larger computation.

#### EVM circuits&#x20;

Nodes in a blockchain need to check that any new block coming from other nodes were made correctly, so they have been re-executing all the computation to check that this new block is correct. Rather than that, we can have a circuit that represents the checking of a block being valid, and reveal witness values that should be public.

A valid proof of a circuit like this with revealed witness values of the block’s values will remove the need for nodes to re-execute all new blocks they are sent, they can simply check the proof and save themselves a lot of time and computing power. Again, this is a clear use case. And this is what [“type-1” zkEVMs](https://vitalik.ca/general/2022/08/04/zkevm.html) are trying to achieve.&#x20;

<details>

<summary>Why do ZK-proofs have value?</summary>

Because they demonstrate the correct execution of a computation that has real world relevance, for example correctly processing a transaction, or smart contract, or correctly verifying that a block is valid

</details>
