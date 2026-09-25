The **totient of a number \(n\)**, written **φ(n)** (“phi of n”), counts how many positive integers below \(n\) **have no common factor with \(n\) other than 1**.

Those numbers are called **coprime to \(n\)**. They don’t have to be prime themselves.

For example, take **12**:

| Number below 12 | Coprime to 12? |
| --------------- | -------------- |
| 1               | Yes            |
| 2, 3, 4         | No             |
| 5               | Yes            |
| 6               | No             |
| 7               | Yes            |
| 8, 9, 10        | No             |
| 11              | Yes            |

The qualifying numbers are **1, 5, 7 and 11**, so **φ(12) = 4**. We count 1 because its only positive factor is 1.

**For a prime, it’s particularly easy**

If \(p\) is prime, every number from 1 to \(p-1\) is coprime to it. There are no smaller positive multiples of \(p\).

Therefore:

$$
\varphi(p)=p-1
$$

For example, **φ(13) = 12**.

**For the product of two distinct primes**

Your spreadsheet uses:

$$
n=13\times23=299
$$

Because the only prime factors of 299 are 13 and 23, a number fails the coprime test **only if it is divisible by 13 or by 23**.

Let’s count:

| Step                                   |   Count |
| -------------------------------------- | ------: |
| Start with all integers from 1 to 298  |     298 |
| Remove multiples of 13: 13, 26, …, 286 |     −22 |
| Remove multiples of 23: 23, 46, …, 276 |     −12 |
| Numbers remaining                      | **264** |

We haven’t removed anything twice: the first positive number divisible by **both** primes is 299, which is outside our list.

For any two distinct primes \(p\) and \(q\), that same calculation becomes:

$$
\begin{aligned}
\varphi(pq)
&=(pq-1)-(q-1)-(p-1)\\
&=pq-p-q+1\\
&=\boxed{(p-1)(q-1)}
\end{aligned}
$$

So the spreadsheet’s calculation is:

$$
\varphi(299)=12\times22=\boxed{264}
$$

**Why does this count matter for encryption?**

Here’s the remarkable connection: **this count also tells us about the behaviour of powers modulo \(n\)**.

Euler’s theorem says that, when \(m\) is coprime to \(n\):

$$
m^{\varphi(n)}\bmod n=1
$$

For your “H”, whose byte value is 72:

$$
72^{264}\bmod299=1
$$

Multiply by another 72, and therefore:

$$
72^{265}\bmod299=72
$$

That’s why we choose encryption and decryption exponents whose product is **one more than a multiple of the totient**:

$$
e\times d=1+k\varphi(n)
$$

Your keys have \(e=5\) and \(d=53\), so:

$$
5\times53=265=1+264
$$

Encrypting and then decrypting effectively raises the original number to the power 265:

$$
(72^5)^{53}=72^{265}
$$

Taking remainders along the way preserves the final remainder, so **we get 72 back**.

One subtlety: Euler’s theorem directly covers only messages coprime to \(n\). RSA also works for the other message values; that follows by considering the remainders modulo each prime separately.

So the totient supplies the **exponent relationship that lets the two keys undo one another**. Knowing the two primes makes that totient very easy to calculate.
