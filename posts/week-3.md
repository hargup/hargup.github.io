<!--
.. title: week 3
.. slug: week-3
.. date: 2014/06/09 20:20:06
.. tags: sympy, GSoC
.. link:
.. description:
.. type: text
.. author: Harsh Gupta
-->

Hi,
At the start of this week I wrote a method to check if a given solution
lies in the domain of the equation or not. The problem it targeted was equations like `(x
- 1)/(1 + 1/(x - 1))` though at `x = 1` the eqution has value zero the point is
not present in the domain of the given equation. Though there is some
disagreement on the implementation, the idea was to traverse the expression
tree and check if any of the subexpression goes unbounded for the given
value.  Later I observed that caching was messing with the tests. Sometimes
the tests passed and sometimes the same test suit run without any
modification in the code failed. To do the check on the subexpressions I need
to have a copy of the original equation before the solver performs any
simplification on it. Being not sure of the mutability of the equations
I used deepcopy to copy the original equation to a variable. Later in a
meeting we figured out that deepcopy was messing with caching. Aaron told me
that every expression in sympy is immutable so don't need to perform deepcopy
on anything.

This week we will restart working on the sets to represent infinite solutions,
Almost every equation in complex domain other than polynomials and rationals
has infinitly many solutions. For example even the simple equation `exp(x) ==
1` has infinitely many solutions that is `i*2*n*pi` where `n` is an integer.
To return these solutions we first need to have infrastructure to handle them.
The old(current) solvers does it wrong, it implicity mixes up the complex and real
domains, the answer it returns to `exp(x) == 1` is only `[0]`. Earlier I was also doing it wrong
so I decided that I should have two seperate solvers for reals and complex
instead of one. This also simplifed the code for the reals solvers to a large
extent.
