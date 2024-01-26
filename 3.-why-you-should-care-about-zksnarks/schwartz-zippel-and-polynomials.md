# Schwartz-Zippel and Polynomials

We now want to introduce an idea called the Schwartz-Zippel lemma. A lemma is a proven statement used for proving other statements or theorems. The names Schwartz and Zippel reflect the names of some of the discoverers of this lemma.

This idea is statistical and it is going to be pinnacle in allowing us to check that a prover's computation has been done correctly. By choosing a random number and evaluating a polynomial there, we will be able to see whether a prover did their work in a valid way. Since the prover doesn't know what this number will be, they cannot 'prepare' their polynomial for it.

Before we formally define it, we want to touch on polynomials a little bit more. Most of what happens in zkSNARKs is to do with polynomials so spending more time on them is always worthwhile.

## Polynomial degree

To recap, a polynomial is a formula where the variables have powers with positive, whole numbers, like $$f(x)=3x^2+4$$ or $$g(x,y)=x^3y^7+6x^3$$.

We often work with certain types of polynomials where there is an additional restriction. In SNARKs, we are interested in polynomials where the coefficients are all elements of a field.

Coefficients are numbers in front of the variables. In $$f(x)=3x^2-8x+4$$ the coefficients are: $$3$$ for $$x^2$$ and $$-8$$ for $$x$$.

Let's say our field is $$\mathbb{F}_{11}$$, then $$f(x)=3x^2-8x+4$$ would be a relevant polynomial (remember that in $$\mathbb{F}_{11}$$ we have that $$8+3=11\equiv0\bmod11$$, and therefore $$-8\equiv3\bmod11$$), but $$f(x)=3.4x^2-8x+4$$ wouldn't because it contains a fraction.

In terms of notation, we usually use $$\mathbb{F}_{11}[X]$$ to indicate the polynomials that we are interested in. $$\mathbb{F}_{11}$$ indicates that we want the coefficients to be in $$\mathbb{F}_{11}$$, whilst $$[X]$$ the says that this will be a polynomial with $$x$$ as a variable. The polynomials can have powers of $$x$$ that are arbitrarily high, there is no limit on that.

We refer to the highest power of $$x$$ in a polynomial as the <mark style="color:purple;">**degree**</mark> of the polynomial. For example the degree of $$f(x)=3x^7+5^2+2$$ is $$7$$. The degree will play an important part in the Schwartz-Zippel lemma.

As will almost always be the case in cryptography, we are primarily interested in evaluating it on values from a field or a group. For example we might choose to evaluate these polynomials only at values within $$\mathbb{F}_q$$ where $$q$$ is some huge prime.

## Schwartz-Zippel Lemma

The <mark style="color:purple;">**Schwartz-Zippel lemma**</mark> essentially tells us that if we have two polynomials that have the same degree (the highest power term), for example $$f(x)=3x^7+5x^2+2$$, and some $$g(x)$$ whose highest power is 7 (so its degree is also 7), then if we evaluate them at some randomly chosen point, let's say $$a\in\mathbb{F}_q$$ (the $$\in$$ notation simply means the thing on the left is some element from the set on the right), and they have the same evaluation, so $$f(a)=b$$ and $$g(a)=b$$, then it is safe to assume that the polynomials are actually the same, i.e. that $$f(x)=g(x)$$.

But why is it safe to assume they are the same polynomial if they have the same evaluation? There are two primary things we need to think about.

Firstly, we need to consider the degree of our polynomials. If a polynomial $$f$$ has degree $$d$$, then $$f$$ can cross the x-axis (i.e. evaluate to 0) at at most $$d$$ points. In fact, $$f$$ cannot evaluate to any value $$b\in\mathbb{F}_q$$ more than $$d$$ times. Let's explore that visually a bit.

If we have a constant polynomial, i.e. where there are no powers of $$x$$, for example $$f(x)=4$$, then the graph is just a horizontal line. Okay this actually breaks our rule since $$f(x)=0$$ crosses the x-axis everywhere. But the case of a constant polynomial (there are no powers of $$x$$, so the degree is $$0$$) is a special case.

<figure><img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExOXZzbDgzNnQxbmlkZHliOTRjNHRxcms3NHRqemp3OTNxN2RtNThleCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/gF3UmRoGBj6IBssPrv/giphy.gif" alt=""><figcaption></figcaption></figure>

What about if the degree is $$1$$, like $$f(x)=x$$? Well this curve will just be an upwards slope of $$45^\circ$$. According to our rule, this line cannot cross any horizontal line more than once.

<figure><img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExaHhxdmF6ZTVvY2lscWgzYnY3dzJiMGJpajljYmYwYTFleDluNm9ncCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/9geO1boG3PLwc4VYBM/giphy.gif" alt=""><figcaption></figcaption></figure>

If we were to look at $$f(x)=x^2$$, we would have a more bendy curve. But let's try the same thing, draw horizontal lines and try to see if the curve crosses these lines at more than two points. Again, we won't find any.

<figure><img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExeWYzczE5czY3eDlpNG5uM3JvcHFsNXcxMGZhNnppaGp2cjdiajc1ZCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/nNC7jjAPHyFWSq8Rni/giphy.gif" alt=""><figcaption></figcaption></figure>

These examples show that for a degree $$d$$ polynomial, any horizontal line in the graph cannot be crossed by the polynomial’s curve more than $$d$$ times. If $$a$$ is genuinely chosen at random after the polynomials are created, then it is not feasible to engineer $$g$$ to evaluate to the same value as $$f$$ at $$a$$. Since the only way to arrive at the same value is by guessing, all we are left with is probability.

You might think that we can engineer $$g$$ so that its curve is the same as the curve of $$f$$ in most places, but make them differ in a few places.This is not possible - the curves of $$f$$ and $$g$$ can only cross in at most $$d$$ places. If they crossed at more places, then we could look at their difference $$f-g$$. Since $$f$$ and $$g$$ are both degree $$d$$ polynomials, $$f-g$$ will also be of degree $$d$$. But if $$f$$ and $$g$$ crossed each other more than $$d$$ times, $$f-g$$ will equal $$0$$ more than $$d$$ times, violating the rule we introduced above.

So if $$f$$ and $$g$$ are different polynomials, meaning their curves cross each others in at most $$d$$ places, how likely is it that they evaluate to the same value at $$a$$?

If $$p$$ is some number on the order of $$10^{80}$$, and the degree $$d$$ of our polynomials is on the order of $$10^{10}$$, then the chance of choosing something from the $$10^{10}$$ bucket within the $$10^{80}$$ bucket is roughly $$\frac{10^{10}}{10^{80}}=\frac{1}{10^{70}}$$. How small is that? $$10^{70}$$ is roughly the number of atoms in our galaxy, the Milky Way. It is so unlikely that it is completely unreasonable to ever expect it to happen. The numbers we chose here reflect standard values we use in our zkSNARKs.

Putting that all together, we can conclude that:

1. If $$f(x)=g(x)$$, then $$f(a)=g(a)$$ for all values of $$a\in\mathbb{F}_q$$
2. If $$f(x)\neq g(x)$$, then the probability that $$f(a)=g(a)$$ for a randomly chosen $$a$$ is vanishingly small

Because of the above two situations, the Schwartz-Zippel lemma follows that if two polynomials of the same degree evaluate to the same value on a randomly chosen point, then they are the same polynomial. Neat!

Note: We said that $$a$$ would have to be chosen at random after the two polynomials $$f$$ and $$g$$ were already chosen. There is another scenario where this also works. If there is a way to keep $$a$$ invisible but still evaluate our polynomials there, then we can keep reusing this invisible value $$a$$. A trusted setup lets us create this invisible value $$a$$, which lets us create polynomial commitment schemes that allow us to check a prover’s proof, as we will see in a later section.
