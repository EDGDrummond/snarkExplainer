# Section 3: Fundamental Objects

In this subsection we will introduce these mathematical objects: groups and fields. These are the most fundamental mathematical building blocks used in zkSNARKs. In cryptography, we work mainly with values from a field or a group. By constraining the numbers and types of numbers to those in groups and fields, we are able to derive many interesting tools and properties like verifiable computation and security guarantees.

But before we get to groups and fields, we need to talk about modular arithmetic

## Modular Arithmetic
Modular arithmetic is where we take positive, whole numbers and put an upper limit, where reaching the upper limit gets you back to 0, like a clock. It allows us to make a thing finite. Rather than going to infinity, there is an upper limit that takes you back to the start

For example if we make 5 the upper limit, then 5 will be akin to 0. If we were to count with this arithmetic, starting at 0, it would go 0, 1, 2, 3, 4, 0, 1, ...

### Anim

So what happens if we do addition here? We will have to 'loop back' to get numbers between $0$ and $4$. For example if we do $3+4$ we get $7$, but that is not in the range we specified since $5$ is the upper bound. What we do here is keep removing $5$ until we are in our range. Since $7-5=2$ we have that $$3+4=2$. To be even clearer, we should write $3+4=7\equiv2\bmod5$ to also indicate that $5$ is the upper limit.

### Anim

Also $2+3=5=0$. Or more accurately, $2+3=5\equiv0\bmod5$, where the new equals sign $\equiv$ and $\bmod$ name indicate to us that we are doing modular arithmetic. 

Similarly with multiplication, if we had $3*4=12$, we have that $12-5=7$ and $7-5=2$, so $3*4=12\equiv2\bmod5$.

### Anim

## Groups
Groups are relatively simple objects. A group consists of three things: a set, a binary operation, and an identity element. Let's go through each of those and then come back to this definition.

A **set** is any collection, such as $\{0,1,2\}$ or $\{\text{tree},\text{frog}\}$. We will primarily be interested in sets
that contain numbers.

A **binary operation** is some operation that takes in two things and outputs a third, for example addition is a binary operation: $3+4=7$. We are only interested in addition and multiplication, so if we ever mention a binary operation you can just think of addition or multiplication.

An **identity element** is any element (from our set) that, in simple words, makes the operation do nothing. Since we are only interested in addition and multiplication, the identity element of addition is $0$; when we add $0$ to anything we get back the same number. For example, $3+0=3$. Similarly, for multiplication the identity element would be 1.

We can't put any set, binary operation and identity element together to get a group. Groups will require every element in the set to have an **inverse**. What that means is that if addition is our operation, then for every number in the set (call that number $a$), there must exist some other number in the set (call it $b$) such that $a+b=0$. For example, if $a=2$ then there must be some $b$ such that $2+b=0$. The additive inverse is how we get to 0 (the identity element for addition).

You may think that $-2$ is the number that we must add to $2$ to get to $0$, but since we are in modular groups, this is not the whole picture. If we are working $\bmod 5$, then $2+3=5=0$. Thus $3$ is the number we add to $2$ to get $0$ (if we are working $\bmod 5$).

### Anim

And if multiplication is our operation, then we need to get to $1$ so for every number $a$ in the set there must be another number $b$ in the set such that $a*b=1$. Let’s go back to our example in $\bmod 5$ where $a=2$. In this case $2*3 = 6 \equiv 1 \bmod 5$, so $3$ is the inverse of $2$ when multiplying.

### Anim 
Anim: The multiplicative inverse is how we get to 1 (the identity element for multiplication)

Now for the actual definition: A **group** consists of a set, a binary operation, and an identity element on the binary operation; moreover, every element in the set must have an inverse element that is also in the set.

Would the modular set $\{0,1,2,3,4\}$ with addition be a group? Well we have a set, a binary operation and an identity element ($0$), so we just need to check that every element in the set can be added to another element in the set to get $0$. Let’s check:
- $1+4=5\equiv 0\bmod 5$ (so that is true for $1$ and $4$)
- $2+3=5\equiv 0\bmod 5$ (so that is true for $2$ and $3$).

Great, we have a group!

What about the same set but with multiplication where 1 is the identity element? Well let’s check:
- $2*3=6 \equiv 1\bmod 5$ (so that solves $2$ and $3$)
- $4*4=16 \equiv 1\bmod 5$ (that solves $4$, it is its own inverse)

That just leaves us to find a multiplicative inverse for $0$, some element that when multiplied by $0$ returns $1$. But we learnt at school that anything multiplied by $0$ gives us back $0$. So $0$ definitely does not have an inverse; hence it is not a group.

However, if we remove $0$ from the set, then all the remaining elements have an inverse,
meaning that we have a group!

We can write groups like so $(\{0,1,2,3,4\}, +, 0)$, where the numbers in the curly brackets are the set, the $+$ is the binary operation, and $0$ is the identity element. Our second example was $(\{1,2,3,4\}, *, 1)$.

It will be useful to remember that we are primarily interested in groups whose set size is prime. For example $3$, $5$, $11$ and $179$ are prime numbers, but $4$, $10$ or $100$ aren't. We are interested in prime sized groups for security reasons; you don't need to understand why, but it would be good to remember.

## Fields

Both fields and groups are used all over cryptography. Fields are similar to groups. In fact, fields are basically two groups smushed together.

A **field** consists of a set, but it has two binary operations (addition and multiplication), and two identity elements (one for each binary operation, $0$ and $1$ in our case). Every element must have an inverse for both binary operations. The only exception is $0$. The element $0$ does not need to have a multiplicative inverse because it does not have a multiplicative inverse.

In the examples above working with the set $\{0,1,2,3,4\}$, we showed that every element had an additive inverse. In the same set, every element except $0$ had a multiplicative inverse.

Now, if we smush the addition group and multiplication group together, we would get a field. We could write this field as $(\{0,1,2,3,4\}, +, *, 0, 1)$ similar to our notation for groups, but more commonly we will write this as $\mathbb{F}_5$. Whenever you see this $\mathbb{F}$ you should understand it to be a field where addition and multiplication are the 2 binary operations, with $0$ as the additive inverse and $1$ as the multiplicative inverse. The number, $5$ in this case, represents what the set will be; the numbers $0$ to $4$.

$\mathbb{F}_{11}$ would be the same but with $11$ numbers ($0$ to $10$). We will see fields like this a lot in cryptography, but usually they will be absolutely huge, containing something like numbers $10^{77}$ (roughly the number of protons predicted to exist in our observable universe).

