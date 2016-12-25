
## Asymptotic behavior

From the terms that we are considering, we'll drop all the terms that grow slowly and only keep the ones that grow fast as **n** become larger.

The filter of **"dropping all factors"** and of **"keeping the largest growing term"** is what we call **asymptotic behavior**.

### Rule of thumb

1. Simple programs can be analyzed by counting the nested loops of the program. A single loop over **n** items yields `f(n) = n`. A loop within a loop yields `f(n) = n^2`.

2. Given a series of for loops sequential, the slowest of them determines the asymptotic behavior of the program. Two nested loops followed by a single loop is asymptotically the same as the nested loops alone, because the nested loops dominate the simple loop.

3. Programs with a bigger `Θ` run slower than programs with a smaller `Θ`.

4. It's easier to figure out the O-complexity of an algorithm than its Θ-complexity.

5. While all the symbols O, o, Ω, ω and Θ are useful at times, O is the one used more commonly, as it's easier to determine than Θ and more practically useful than Ω.

6. Improving the asymptotic running time of a program often tremendously increases its performance, much more than any smaller "technical" optimizations such as using a faster programming language.
