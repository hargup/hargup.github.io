This week I worked on the solvers for the equations with radicals.
Suppose you have to solve

$$ \sqrt{x} + x - 2 = 0 $$.

then you have to move the radical to the right hand side of the equation.

$$ x - 2 = - \sqrt{x} $$

and then square at both sides

$$ x^2 - 4x + 4 = x $$

Now the equation is a polynomial in \\( x \\) can be solved with usual
polynomial solving methods. Note that squaring both sides produce some extra
solutions and we will have to check all the solutions obtained against the
original equation.  If there are more than one radicals involved we may
have to apply the method recursively. For example in solving
\\( \sqrt{2x + 9} - \sqrt{x + 1} - \sqrt{x + 4} = 0 \\)
the method will recurse twice.

To implement the method I tried a pattern matching approach.
The
squaring part is easy the tricky part is identifying which part to move to
the right hand side. First I tried to match the expression with the form
`sqrt(p) + q` but it failed even for case like `4*sqrt(x) + x - 2` because no
pattern matched to it. I had to use `a*sqrt(p) + q` with the condition that the
expression matched to a shouldn't be zero. Now I can simply move the expression
matched with `p` and terms multiplicated with it to the RHS and square both
the sides.

Notice that this method for solving sqrt equation can work with any radical
equation, if it were cube root instead of sqrt I just had to cube both the
sides. OK so how do I mathch that expression? I tried to pattern matching with
assumptions on the wild symbols but it doesn't work.  I tried to match with
somthing like `a*p**Rational(1, m) + q` but this also didn't work out because
Rational(1, m) raises TypeError no matter what the assumption on the variable
are.  There is a proposal for a new pattern matcher, I have not closely checked
the details but it will be able to work with assumption. You can see the
proposal on the wiki
[here](https://github.com/sympy/sympy/wiki/Proposal-for-a-new-pattern-matching)
and if it is implemented then things will be good but I can't wait for it.
I had no other option to check term by term for rational power. Here's the
implementation

```

def _has_rational_power(expr):
    a, p, q = Wild('a'), Wild('p'), Wild('q')
    pattern_match = expr.match(a*p**q)
    if pattern_match is None or pattern_match[a] is S.Zero:
        return (False, None)
    elif isinstance(pattern_match[q], Rational):
        if not pattern_match[q].q == S.One:
            return (True, pattern_match[q].q)

    if not isinstance(pattern_match[a], Pow) or isinstance(pattern_match[a], Mul):
        return (False, None)
    else:
        return _has_rational_power(pattern_match[a])
```
