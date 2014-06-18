<!--
.. title: week 4
.. slug: week-4
.. date: 2014/06/18 11:29:01
.. tags: sympy, GSoC
.. link:
.. description:
.. type: text
-->

Intersections of Infinitely indexed sets
----------------------------------------

This week I implemented a method to do intersection of imagesets using the
solutions of Diophantine equations at [PR](https://github.com/sympy/sympy/pull/7587).

Say you have
to find the intersection of sets `2*n| n in Integers` and `3*m| m in Integers`.
The intersection of these sets is the set of the common values in the two sets,
which in this case is equivalent to the values of `n` for which the equation `2*n - 3*m` has
some integral solution in `m`. Or the values of `m` for which the `2*n - 3*m`
has some integral solution in `n`. Diophantine equations are equations for
which only integral solutions are searched for.
The Diophantine module was written by
[Thilina](https://github.com/thilinarmtb) as his GSoC project last year.
It gives the parametric solution for such equation.

    In [17]: diophantine(2*n - 3*m)
    Out[17]: {(-2*t, -3*t)}

The Solution is sorted according to alphabetic order of the variables involved.
So the value of LHS (`2*n`) for which the equation is `2*(-3*t)` that is `-6*t`
and it is the intersection of the sets described above `-6*t| t in Integers`.
Since `-6*t| t in Integers` is same as `6*t| t in Integers` I also wrote some
simplification rules for the imagesets with Integers as baseset.


Sets for Invert Function
-------------------------

The sets module turned out to be better than I expected. I had a perception
that substitutions doesn't work properly with sets and I have even opened an
[issue](https://github.com/sympy/sympy/issues/7483) for that but it turned out
I hadn't looked closely enough. It worked well for the free variables and it
didn't worked for the things it shouldn't work i.e., the bound variables in the
imagesets.

Using sets simplified the code. All the list comprehensions like this
`[i.subs(symbol, symbol/g) for i in _invert(h, symbol)]` were converted to
simple substitutions for sets and other sets operations.  `_invert(h,
symbol).subs(symbol, symbol/g)`

Just by changing the output of invert to sets, then by adding the inverse of
trigonometric function and writing the code to rewrite then as tan I was able
to return all the solutions of the equations like `cos(x) + sin(x) == 0` it
turned to out to easier than I thought. Using sets as output makes thinking
about the mathematics of the solvers much more easier and the code comes
out to be pretty natural. Now when we can see the results I can surely say
there can be no better output for solvers than sets.

This week I'll study LambertW function and then code additional techniques to
solve real equations. I'll also try to figure out techniques to perform Union
on infinitely indexed sets.
