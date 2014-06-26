
Solving Trigonometric Function (part I)
---------------------------------------
This week I spend time on making trigonometric solvers work.
Every trigonometric function can be written in terms of tan.

$$ sin(x) = \frac{2*tan(x/2)}{tan^{2}(x/2)} $$

$$ cos(x) = \frac{-tan^{2}(x/2) + 1}{tan^{2}(x/2) + 1} $$

$$ cot(x) = \frac{1}{tan(x)} $$

A basic technique to solve trigonometric equations can be rewriting the equation in terms of tan.
And if the equation is made by addition, multiplication or quotient of
trigonometric functions then the transformed equation is a equivalent to a rational
function in tan. That equation can be solved by the usual polynomial
solving techniques.

Taking the example from the [doc](https://github.com/sympy/sympy/wiki/solvers)
\\( cos(x) + sin(x) \\) gets converted to
\\( \frac{-tan^{2}(x/2) + 2*tan(x/2) + 1}{tan^{2}(x/2) + 1} \\)

The solution of this equations is \\( tan(x/2) = 1 +- sqrt(2) \\).
Since the inverse of tan is
\\( \\left\\{2 \\pi n + \\operatorname{atan}{\\left (y \\right )}\\; |\\; n \\in \\mathbb{Z}\\right\\} \\)
the solution of the given equation is
$$ \\left\\{2 \\pi n - \\frac{\\pi}{8}\\; |\\; n \\in \\mathbb{Z}\\right\\} \\cup \\left\\{2 \\pi n + \\frac{3 \\pi}{8}\\; |\\; n \\in \\mathbb{Z}\\right\\} $$

Though it appears this technique should work universally for trigonometric
equation it fails for even \\( sin(x) = 0 \\). From the table above
\\( sin(x) = \frac{2*tan(x/2)}{tan^{2}(x/2)} \\)
So, the \\( sin(x) = 0 \\) occurs at \\( tan(x/2) = 0 \\) which has solution
\\( \\left\\{2 \\pi n\\; |\\; n \\in \\mathbb{Z}\\right\\} \\)
But the solution is \\( \\left\\{ \\pi n\\; |\\; n \\in \\mathbb{Z}\\right\\} \\)
. Why are we missing some solutions? The
reason is \\( sin(x) = 0 \\) also occurs when denominator tends to \\( \infty \\),
i.e.,
the values where \\( tan^{2}(x/2) + 1 \\) tends to \\( \infty \\).
We had encountered a similar problem for the solution of
$$ \\frac{1}{\\left(\\frac{x}{x + 1} + 3\\right)^{2}} $$

here \\( x = -1 \\) is not a point in the domain of the of the equation. The solver
simplifies the equation to

$$ \\frac{\\left(x + 1\\right)^{2}}{\\left(4 x + 3\\right)^{2}} $$

which extends the domain to include the point \\( x = -1 \\) which is also the
solution to the transformed equation. There we wrote a sub procedure
`domain_check` to verify if the returned solution is part of the domain of the
original equation. The problem here is slightly different in the sense that
transforming the equation decreases the domain of the solutions and not increase
it.

To find such solution we have allow \\( \infty \\) to be solution to equations, we
will be working on extended reals instead of just reals.  I think this change
will simplify a lot of things.

Another thing which should be taken care off is that we cannot naively search
for the values for which the denominator tends to infinity as for the same
value numerator might also attain infinitely large value, we will have to
conceder the limiting value of the equation.

<!--
A technique followed by current solvers is.
- Git Rebase
- The tan technique
- The problems with tan technique
- the Exp technique
- The problem with exp technique
- Complex logarithms and multivalued(set valued) functions
-->
