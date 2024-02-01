# Elliptic Curves and DLOG

## Elliptic Curves & DLOG

Elliptic curves are widespread in cryptography and zkSNARKs because they have certain properties that make them extremely useful. But they are not straightforward. We will do our best to introduce them at a high level in such a way that you can reason about them without getting bogged down in the details.

You probably drew mathematical curves at school on little graphs or axes. Well elliptic curves (ECs) are like those curves but a bit more complicated. They are equations of the form $$y^2=x^3+Ax+B$$.

<figure><img src="https://media1.giphy.com/media/9geO1boG3PLwc4VYBM/giphy.gif" alt=""><figcaption></figcaption></figure>

The $$A$$ in our case will mean any number from a field like $$\mathbb{F}_{11}$$. In practice, we usually use very large numbers and say $$\mathbb{F}_p$$ with $$p$$ being some really large prime number rather than write out that huge number every time we write the field. Now when we draw these curves out we get a pretty little symmetric curve like this.

<figure><img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ29ua2JxbnkzNDF4MW5zajV3cDV5eHVxcnk5bGNla3B3bTY0YnNoMSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Q1GNIuvGh4sHbyNbdI/giphy.gif" alt=""><figcaption></figcaption></figure>

## Elliptic Curve Addition

ECs have a very strange property – if you take any two points on the elliptic curve and draw a line through them, that line will intersect the curve in a third place. From this property, we get a binary operation called elliptic curve addition.

<figure><img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExcXhhZnk0ZzJicnZzOWh5MWxucTM1MW51czd1ZTlpaWZuZTdrcnlxeCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/7NDwoJ4AEZVTMzbKue/giphy.gif" alt=""><figcaption></figcaption></figure>

<mark style="color:purple;">**Elliptic curve addition**</mark> is a process that takes in two points on the curve, draws a line through them to find a third, then returns the reflection of the third point about the x-axis.

Elliptic curve addition will take in two curve points and draw a line through them to find the third point, but it won't return that third point. You probably noticed that the EC drawings above were symmetric about the x-axis, and that is actually true for all ECs (since the formula looks for $$y^2$$, not $$y$$), so the operation instead returns the reflection of the third point across the x-axis, which we know is on the curve.

<figure><img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExZWR5cnMxb3h1c3h2a2k2MG9jMzJnd2RuZzY1MjRhcmcwa3VrZGhoOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/WW3RAoQJkoizDCVsIl/giphy.gif" alt=""><figcaption></figcaption></figure>

The core idea is if we know some point $$G$$ that is on the curve, and somebody takes that point and adds it to itself $$n$$ times, they will produce $$P=nG$$. Now if we are given this point $$P$$, even though we know $$G$$ and how this new point was created, we cannot figure out what $$n$$ is, regardless of the amount of computational power we have. This is where the security of many online cryptographic systems come from. In theory we can brute force work out what$$n\$$ is, but by choosing our parameters in a certain way (i.e. ensuring the groups we work in are large enough), we make it completely infeasible to ever succeed in that brute force attack.

### Elliptic Curves over Finite Fields

Now we're going to do something a bit more complicated with our curve, and in return get the cryptographically useful object we actually want. Rather than have a curve that stretches to infinity, we want to bound it.

In the curves above, we sort of assumed that the x- and y-axes stretched out to infinity. We want to restrict that. Let's first remove all negative x values (everything on the left side of the vertical axis). Then instead of stretching the right side of the x-axis to infinity, let it only go as far as $$p$$ from $$\mathbb{F}_p$$.

Now remember that fields are modular. They wrap around. For example we have $$p\equiv 0\bmod p$$. So when the x-axis reaches $$p$$, we can imagine it teleporting back to $$0$$.

Similarly, rather than letting y stretch all the way to infinity, we will remove all negative y values (everything below the horizontal axis), and take the top part of the y-axis only as far as $$p$$.

Lastly, we cannot draw our curve in this reduced plane as we did before. Remember that the fields only contain elements, and all the elements are the whole numbers – not the decimals between them. So we cannot draw the curve with a line. Instead, we only draw the parts of that curve that go through points in the plane whose coordinates are both whole numbers.

The resulting image looks very different to what we might expect at first.

<figure><img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExbm54Z2RzYnRtcXc3MmsxaGUwYm11MG9haDE0MW42dG84OGI3MnFiaSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/8lQCOKh0UVIeOPndvB/giphy.gif" alt=""><figcaption></figcaption></figure>

Rather than plot all the points that satisfy the equation from the entire number line, we are now plotting all the points that satisfy the equation from the finite field we chose to work in. And since fields have this very different structure, where for example $$2*3=6\equiv 1\bmod 5$$, the curve actually ends up looking very different.

Moreover, the elliptic curve addition (which is a binary operation) where we draw lines through the points still works!

<figure><img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExYTd4cnJjdndpMnhzdW9lMDFlY2pqdnUwMWZkbjhzOXp0cWt0cjUydiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/7gfWCiWONNyChhlm0n/giphy.gif" alt=""><figcaption></figcaption></figure>

Our set is now made up of all the points that are left on the curve (all the dots in the image above), and one more point. We refer to this set as $$EC(\mathbb{F}_p)$$. That other point we include is called <mark style="color:purple;">**the point at infinity**</mark> (also referred to as $$O$$), and you can imagine it to be due north of the graph at all times. It allows us to turn our set and this EC addition we showed into a group!

To touch briefly on the point at infinity, imagine drawing a line from due north onto any point. And since every point on the axes has a sibling point that is its reflection in the horizontal line in the middle of our graph, it means that the vertical line will have its third intersection with the curve be the sibling point. Now when we reflect back across the horizontal line in the middle, we will get back to the point we chose. Thus $$O$$ does nothing when we add it to other EC points, just as we said an _identity element_ needed to back when we defined groups.

We refer to this group as an <mark style="color:purple;">**elliptic curve group**</mark>.

If we choose our elliptic curve equation correctly, along with the size of our finite field $$\mathbb{F}_p$$, then $$(EC(\mathbb{F}_p), \text{EC addition}, O)$$ will be a huge group that will be very hard to break. So hard to break that we will be able to use it for secure online communication.

## Discrete Logarithm Problem (DLOG)

With EC introduced, we can move onto one of the most important ideas in all of cryptography, the Discrete Logarithm (DLOG) problem. This is an example of a one way function: it is very easy to go in one direction, but essentially impossible to go in the other direction. It’s hard to solve yet easy to verify.

When we have an EC point $$G$$, and we are given some other EC point $$P=nG$$ (so $$P$$ is just $$G$$ added to itself $$n$$ times), we refer to the problem of identifying $$n$$ as the <mark style="color:purple;">**discrete logarithm problem**</mark> (<mark style="color:purple;">**DLOG**</mark>).

Of primary importance is an assumption, called the <mark style="color:purple;">**DLOG assumption**</mark>, that this problem is hard. Really hard. So hard in fact that there is no way to solve the problem, except via brute force methods. And if we ensure that we work with large enough values, it is safe to assume that nobody can ever work out what $$n$$ is for a given \$$P4 since it would take the world’s best supercomputers millions of years to solve.

Interestingly, there is no formal proof that this problem is hard. We assume it is hard because we have been trying for hundreds of years, and nobody has found a way to solve the problem more quickly than brute force.

There is even some possibility that we will never prove that the discrete logarithm problem is hard because mathematics simply might not be capable of producing such a proof. Alternatively, this hardness assumption may be wrong, there might be quicker ways to solve this problem, and online security would be in some serious jeopardy!

## Crypto Wallets & DLOG

If you are involved in crypto you probably have a wallet, something like metamask. So you probably also know that your wallet has an address, some really long number like $$0x172c4b2378\ldots$$ This address is actually a point on an elliptic curve! What happened is, the creators of Ethereum said let's use an EC called BLS, and let's use $$G$$ as the base point. Then in order for us to make addresses on those wallets, we have to come up with some random number $$sk$$ - and then $$skG=P$$ would be our address.

The only people who can spend the money in the address $$P$$ are people who know $$sk$$. And because DLOG is assumed to be hard, nobody can take $$P$$, your public address, and figure out what $$sk$$ is. DLOG is everywhere in crypto.
