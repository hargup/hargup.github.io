Week 7, 8
==========

Fu Simplification
-----------------
In the early part of the week 7 was thinking and working on design decisions.
I want the code to be very modular so that it could be easily extended by
people other than me. This came up in a meeting with Matthew and I want the
solvers to be like the Fu simplification explained by Matthew in this [Scipy
Talk](https://www.youtube.com/watch?v=QldxygVVj-s&list=PLYx7XA2nY5GfuhCvStxgbynFNrxr3VFog&index=20).
The idea was that we can see solving an equation as series of transformations.
If we have a lot of small transformations such that the input type is same as
output type, and some notion of what makes a "better" output we can search though
the list of transformations running one on top of other. I also posted about
it on the [mailing
list](https://groups.google.com/forum/#!topic/sympy/42GdMJ9ssyM) which brought
out some flaws in the preliminary design. The idea is pretty crude in the
current stage and I'll have to look deeper into it, but not now.

I also discussed about the implementing a pattern dispached based solver
suggested by [F.B](https://groups.google.com/d/msg/sympy/moSEFHop0n4/e2hBKRQ9WP4J)
on the mailing list. But we decided that it will be better if we finish the
equation solver by the current technique first.


Intersection with S.Reals
-------------------------
I decribed in the last post that one way to solve trigonometric equation is
rewriting them in terms of \\( exp \\). But that is \\( exp \\) in the complex domain and
the solution of \\(exp(x) = a \\) is \\( \\left\\{i \\left(2 \\pi
n + \\arg{\\left (a \\right )}\\right) + \\log{\\left
(\\left\\lvert{a}\\right\\rvert \\right )}\\; |\\; n \\in \\mathbb{Z}\\right\\}
\\). Hence we have to filter out real solutions from the obtained solutions.
The filering is equivalent to the intersection of the solutions with the \\( \\mathbb{R}
\\) set. Suppose \\( g(x) \\) and \\( h(x) \\) are real valued functions and we
have to perform
$$ \\mathbb{R} \\cap \\left\\{g{\\left (n \\right )} + i h{\\left (n \\right )}\\; |\\; n \\in \\mathbb{Z}\\right\\} $$
then the answer will be simply
$$ \\left\\{g{\\left (n \\right )}\\; |\\; n \\in \\left\\{h{\\left (n \\right )} = 0\\; |\\; n \\in \\mathbb{Z}\\right\\}\\right\\} $$

Separate the real and imaginary parts and equate the imaginary to zero
but the problem was with the assumptions on the symbols. For example while
separating real and imaginary parts of the equation.

    In[]: (n + I*n).as_imag_real()
    Out[]:(re(n) - im(n), re(n) + im(n))

That is because `n` is by default complex, even in the `Lambda(..).expr`.
I wrote some code to decide the
assumption on the variable of imageset from the baseset. See [PR
7694](https://github.com/sympy/sympy/pull/7694).
There was another issue that needs to be resolved
`S.Integers.intersect(S.Reals)` doesn't evaluate to `S.Reals`.


LambertW and Multivariate Solvers
---------------------------------
The method to solve equation containing exp and log function is using the
LambertW function. LambertW function is the inverse of \\( x \exp(x) \\).  The
function is multivariate function both for the real and complex
domains and Sympy has only one branch implemented. This also leads us to loss
of solutions. Aaron gave an example
[here](https://github.com/sympy/sympy/pull/2723#issuecomment-33760912). But I'm
pretty unfamiliar with solving by LambertW and LambertW itself and it will take
me some time to build an understanding of them.
As an ad hoc solution I'm using the code in the `_tsolve` in the
`solvers.solvers` module to do at least what the current solvers can do.

When the importing of `_tsolve` method was done. I started working on the
multivariate solvers. Here's how the current multivariate solvers work:

**Solving single multivariate equation**


1. count the number of free symbols in f - no of symbols for
equation. If the equation has exactly one symbol which is not asked for then
use `solve_undetermined_coeffs`, the `solve_undetermined_coeffs` find the
values of the coefficient in a univariate polynomial such that it always
equates to zero.

2. Then for each symbol `solve_linear` is tried which tries to find a solution
of that symbol in terms of constants or other symbols, the docstring says

        No simplification is done to f other than and mul=True expansion,
        so the solution will correspond strictly to a unique solution.

So we don't have to worry about loosing a solution. For every symbol it is
checked if doesn't depend on previously solved symbols, if it does that
solution is discarded.

3. For the symbols for which the above method failed, the `_solve` function is
called for the equation for that variable and as above if the solution contains
a variable already solved then that solution is discarded.

**System of equations in multiple variables**


- Try to convert the system of equations into a system of polynomial equation
  in variables

- If all the equations are linear solve then using `solve_linear_system`, check
  the result and return it. If asked for particular solution solve using
  `minsolve_linear_system`

- If the number of symbols is same as the size of the system solve the
  polynomial system using `solve_poly_system`. In case the system is
  over-determined All the free symbols intersection the variables asked for are
  calculated. Then for every subset of such symbols of length equal to that of
  the system, an attempt to solve the equations by `solve_poly_system` is made.
  Here if any of the solution depends on previously solved system the solution
  is discarded.

- In the case there are failed equations:
    - For every know result:
    - Substitute every thing into the failed equation and see if the equation turns to zero.
      if it does accept the result otherwise put it in the bad_results group.
    - Then try to solve try to solve the failed equation using `solve` for each symbol.
    - If that solution depends on any other previously solved symbols
      discard it.
    - If it doesn't satisfy other equations, discard it.
    - Check if the solution doesn't set any denominator to zero, if it does
      discard that solution.
    - If it satisfies the above conditions substitute this value in know
      solutions and add it as a new result.
