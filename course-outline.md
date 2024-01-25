---
description: Curriculum sketch
---

# Course outline

## General Outline <a href="#docs-internal-guid-28f6ca63-7fff-84c0-4d8a-731f574c1a19" id="docs-internal-guid-28f6ca63-7fff-84c0-4d8a-731f574c1a19"></a>

1. Argument of Knowledge
2. zkSNARKs in Action
3. Why you should care about zkSNARKs?&#x20;
4. Math (to get to PLONK)
5. PLONK
6. Proof Systems

\


Part 1: Verifiable computation (sections 1-3)

Part 2: A deep dive into PLONK (sections 4-5)

Part 3: SNARK ecosystem overview (section 6)

\


## Course Syllabus

In this outline there is a cut-off after section 3 such that people who are time-strapped or math-phobic can stop and walk away with a good high level basis of SNARKs. They will know what SNARKs do and what tradeoffs have to be considered. Then section 4 onward would provide a mathematical basis for people to understand plonk as a base system on which to catalyze their mathematical understanding.

### 1. Argument of Knowledge

Knowledge is about relationships holding true

1. Mathematical guarantees\
   \- zkSNARKs give probabilistic claims
2. Verifiable Computation\
   \- zkSNARKs are a type of VC, and that is a simpler way to think about them conceptually
3. Computer Circuits\
   \- circuits are constraints that a set of values must satisfy
4. ZK-Circuits\
   \- zkSNARKs are claims that a prover knows a valid witness to some set of constraints

### 2. zkSNARKs in Action

How do we create useful circuits, or how do we turn things we care about into useful circuits?

1. Idealized Circuits\
   \- idealized circuits represent the core idea of what a computation should do without any detail
2. Useful Circuits\
   \- zk-proofs have value based on what real-world computation it claims to have done
3. Polynomials\
   \- zkSNARKs largely come down to doing operations on polynomials
4. Polynomials in zkSNARKs\
   \- zkSNARKs make polynomials, commit to them and then query them to make proofs

### 3. Properties of zkSNARKs (Why should you care?)

1. Privacy (zk)
   1. Use cases
      1. Private money, private voting, private identity
2. Scaling (SNARKs)
   1. Overview: succinctness as a property&#x20;
      1. Succinctness vs scaling&#x20;
         1. Succinctness refers to verifier time / proof size
         2. Scaling on the application level
      2. Succinctness as selection pressure and a primary use case driver&#x20;
   2. Use cases
      1. zkEVMs



**Things I've forgotten to include but should be added somewhere:**&#x20;

1. the impact of circuit size on proof generation.
2. what it means for some computation to be (or not be) snark friendly

————— WELL DONE FOR LEARNING THE BASICS ————— &#x20;

————— CONTINUE TO SECTION 4 FOR MATH  —————

————— CONTINUE TO SECTION 6 FOR PROOF SYSTEMS   —————

### 4. Math (to get to PLONK)

1. Mathematical objects
   1. Groups, fields, polynomials, ECs
2. Mathematical properties
   1. DLOG, q-strong DLOG, Schwartz-Zippel (probabilistic checks), Fiat-Shamir?
3. Cryptographic tools
   1. Polynomial commitment scheme (and openings), hashing

### 5. PLONK

1. Plonkish arithmetization
2. Commit to witness & copy constraints
3. Proof creation
4. Verifier checks

### 6. Proof Systems&#x20;

1. Overview comparing and contrasting different system trade-offs and factors
2. Example image![](https://lh7-us.googleusercontent.com/rt9o\_ncmxk-5JZBaek1FV\_mtrwI27iCmGsBUYz5q10LSOZdtEc9LwX5JeQ3krwlxfsZTKCDH7GpgrILb5JELVGqVJ7JANNnI6dZySMtcpq1-IWyBc2J\_-CwQ\_mE4S7l8\_qchU-meewfdZytPl0w\_cek)
3. PCD, lookup singularity, etc. or more general discussions that don't fit with the perspective we've taken
