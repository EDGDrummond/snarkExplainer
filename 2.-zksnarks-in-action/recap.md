# Recap

zkSNARKs are a valuable tool because we can represent <mark style="color:purple;">**useful**</mark> computations such as: <mark style="color:purple;">**transaction circuits**</mark> that can allow users to transact on blockchains with almost complete anonymity; <mark style="color:purple;">**smart contract circuits**</mark> that run computations off-chain thus saving on gas costs; and <mark style="color:purple;">**EVM circuits**</mark> that can verify Ethereum blocks without redundantly re-running all the transactions. With <mark style="color:purple;">**idealized circuits**</mark>**,** more use cases can be imagined by SNARK non-experts and it becomes possible to discuss how to use SNARKs without knowing all the details. But experts will still be needed to understand how to make <mark style="color:purple;">**polynomials**</mark>, commit to them via <mark style="color:purple;">**Polynomial commitment schemes**</mark>**,** and query the polynomials so the rough ideas can turn into practical and secure ZK proofs.

## Terms

<mark style="color:purple;">**Transaction circuit**</mark>**:** A circuit that represents the computation required to check a valid transaction call on a particular blockchain, for example on Ethereum

<mark style="color:purple;">**Smart contract circuit**</mark>**:** A circuit that represents the computation within a smart contract, where all the inputs to the smart contract function will be inputs in the circuit

<mark style="color:purple;">**EVM circuit**</mark>**:** A circuit that represents the computational work involved in checking whether a block is valid

<mark style="color:purple;">**Idealized circuit**</mark>**:** A rough sketch of a circuit’s logic and functionality where the computational work involved is black boxed

<mark style="color:purple;">**Polynomials**</mark>: A function where variables (like x or y) have powers that are whole numbers. Polynomials have the useful property of being able to hide and compress values

<mark style="color:purple;">**Polynomial commitment scheme**</mark>: A process for creating a value from a polynomial that can later be used to check other values come from the same polynomial (sort of like a locked box)
