---
description: 'Core concept: zkSNARKs largely come down to doing operations on polynomials'
---

# Polynomials

We now want to introduce <mark style="color:purple;">**polynomials**</mark>. If even the mention of math makes your eyes glaze over, try black boxing these tools a bit; try thinking of it as a black box that has magical properties. You accept that you don’t understand the details at this moment, but you know what the box does and how to use it. For example you may not know how the internet works in any detail, but you know what it does and how to use it. And with time and dedication, you can learn the details.

<figure><img src="../.gitbook/assets/1 kopya 6@4x.png" alt="" width="375"><figcaption><p>Polynomial means “many terms”</p></figcaption></figure>

Polynomials are equations where the exponents or powers of the variables must be positive whole numbers, such as $$x^2$$. Functions are also labeled, so $$f(x) = x^2$$ is also a polynomial. All this means is the function is called $$f$$, and it has $$x$$ as a variable.&#x20;

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
