# Polynomial Commitment Schemes

And now we arrive at a more complex object, one that is used all over cryptography due to its usefulness, an elliptic curve group. An <mark style="color:purple;">**elliptic curve group**</mark> consists of all the points that satisfy some elliptic curve equation within some specified finite field.

That’s right, we can make a group using elliptic curves, but they look different to the groups we looked at before because, rather than normal numbers, our set consists of points on an elliptic curve. The binary operation is not multiplication or normal addition, but instead the elliptic curve addition we explored earlier. And the identity element is not 0, but the point at infinity that we refer to as $$O$$.

That is the final piece of the puzzle for the most important component of zkSNARKs, a polynomial commitment scheme!

## Mixing Objects: Polynomials & Elliptic Curves

So far we have been evaluating our polynomials of interest at values from fields, such as $$\mathbb{F}_q$$, but that doesn't have to be the case. We could evaluate our polynomials at values from a group too, like an elliptic curve (EC) group.

If our polynomial is $$f(x)=x^2+3x+5$$, then instead of putting in numbers and getting out numbers, like $$f(2)=2^2+3*2+5=4+6+5=15$$, we will put in a number and get out an elliptic curve point. For example if $$G$$ is our base point on the EC, we have that $$f(2)=2^2G+3*2G+5G=15G$$.

You may notice that the evaluation of the above 2 look identical, except that the second one has a $$G$$ at the end. The $$G$$ is very useful because it lets us evaluate polynomials at points of unknown order! What does that mean? Let’s explore.

A point of unknown order is $$T$$ where $$T=nG$$ (so the EC point $$G$$ added to itself $$n$$ times) and nobody knows what $$n$$ is. And thanks to the DLOG assumption, nobody can work it out.

How do we get such a point? We use something called a<mark style="color:purple;"> **trusted setup**</mark>, but that takes some more time to explain, so for now, just imagine that somebody you really trust gave it to you, for example David Attenborough. He went on his computer, made $$T$$, and then deleted $$n$$ from memory, so he can’t even recover it.

Not only did he make $$T$$, he also made $$T^2=n^2G$$, as well as $$T^3=n^3G$$, and many more of these, up to $$T^{10^{10}}=n^{10^{10}}G$$.

Now that we have $$T$$ and its powers, we can create a polynomial such as $$f(x)=x^2+3x+5$$ like we had above and we can evaluate it at an unknown point!

$$f(T)=T^2+3T+5G=n^2G+3nG+5G=(n^2+3n+5)G=P$$

We have the result $$P$$, meaning we know that the polynomial evaluates to $$P$$ somewhere. But where does it evaluate to $$P$$? We don’t know. Well, we know that it is $$n$$, where $$T=nG$$, but nobody knows what $$n$$ is – so it is unknown. Thus we can now evaluate polynomials at points of unknown order.

And that allows us to _commit_ to polynomials and later reveal them. For example, let’s say you and I want to guess the price of three objects, but none of us wants to reveal our guesses before the other. If our three guesses are $37, $65, $134, then we can make a polynomial $$f(x)=37x^2+65x+134$$.

Now, instead of telling me your guesses, you can commit to your polynomial by determining $$f(T)=37T^2+65T+134=P$$ and send me $$P$$. When I receive $$P$$, I have no way of determining what your polynomial was and therefore what your guesses are. Similarly I can send you my version of $$P$$, call my version $$P’$$, which would be the evaluation of my polynomial at $$T$$.

Now that we have both sent each other our commitments, we can reveal what our guesses were. We refer to this process of revealing our polynomials as an _opening_.

Once you commit to a polynomial you cannot change it since the commitment can be used to identify that you are lying. All I would need to do is make the polynomial with your guesses (so this happens after you open your commitment), let's call the polynomial I make from the opening you gave me $$g(x)$$, then evaluate it at $$T$$ and find that $$g(T)$$ does not equal the $$f(T)$$ commitment value that you sent me.

And finally, the Schwartz-Zippel lemma tells us that the chance of guessing the correct $$P$$ is absolutely miniscule. Our polynomials have degree $$d$$ and evaluate to a point in $$\mathbb{F}_p$$ where $$p$$ is absolutely huge. Thus any polynomial we make can evaluate to $$P$$ at at most $$d$$ points. If we want to change our polynomial $$f(x)$$, we have to make sure that the new polynomial $$g(x)$$ evaluates to $$P$$ at $$n$$. But we don’t know what $$n$$ is, so we just have to randomly guess. And since $$d$$ is so small compared to $$p$$, the chances of us guessing correctly are essentially impossible.

To summarize, a <mark style="color:purple;">**Polynomial Commitment Scheme**</mark> (<mark style="color:purple;">**PCS**</mark>) is a process that involves:

1. Creating a <mark style="color:purple;">**point of unknown order**</mark> such as $$T=nG$$ where $$G$$ and $$T$$ are known but $$n$$ isn’t ($$n$$ is kept hidden thanks to the DLOG assumption) via a process known as a <mark style="color:purple;">**trusted setup ceremony**</mark> (David Attenborough in our example).
2. Creating powers of that point of unknown order, $$T^2$$, $$T^3$$, up to $$T^m$$ for some number $$m$$
3. Choosing a polynomial, any we wish
4. <mark style="color:purple;">**Committing**</mark> to that polynomial by evaluating it at the point of unknown order
5. <mark style="color:purple;">**Opening**</mark> that commitment by revealing our polynomial
6. Revealing a polynomial that is different to the one we committed to is impossible, because if we did then the Schwartz-Zippel lemma tells us that when the verifier checks it, they will see that it does not match the commitment we sent them earlier

We only need to do the trusted setup ceremony once, and then we can reuse the result from it again and again every time we want to use the PCS.
