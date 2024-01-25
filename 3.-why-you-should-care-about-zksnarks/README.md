# 3. Why you should care about zkSNARKs

Well done for making it this far and for deciding to join us on this more mathematical exploration of zkSNARKs.

We are going to review the mathematical objects and relationships required to understand a popular proof system called [PLONK](https://eprint.iacr.org/2019/953.pdf).

The thinking here is that by going over one proof system at a high level, you will get to both see
a proof system used across the industry, and also put yourself in a position to more easily understand any other proof system you come across such as Halo2, Groth16, or STARKs as. This is because zkSNARKs are modular; you can more or less swap their components out and get a different proof system with different tradeoffs.

A bird's eye view of what we need to do is:

1. Introduce the objects we will be manipulating.
2. Explore some theories about how we can manipulate these objects
3. Build systems of how to manipulate these objects that do useful things for us.

For example, by discovering materials like metals and rubber, learning how to manipulate them, and then putting them together, we create cars. We will do something similar with mathematics, but instead of getting out cars or physical items, we will get out a computational tool that allows you to prove to me that you did some calculation correctly.

The next few sections will cover the mathematical objects and relationships below:

1. Fundamental Objects: Modular Arithmetic, Groups, and Fields
2. Schwartz-Zippel Lemma and Polynomials
3. Elliptic Curves and the Discrete Logarithm Problem (DLOG)
4. Polynomial Commitment Schemes (PCS)
