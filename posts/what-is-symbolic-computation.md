<!--
.. title: what is symbolic computation
.. slug: what-is-symbolic-computation
.. date: 2014/05/21 01:10:52
.. tags:
.. link:
.. description:
.. type: text
.. draft:
-->

- Intro
- The fundamental limit
- Numerical Computation
- Symbolic Computation

<!-- Seek an expert advice -->

<!--
I can have distance as a quantity which takes values from the real uncountable
set. Can I build a computer which computes on a uncountable set, something in
which I can code the direchlet's function??
-->

You have got function in maths, functions map some values to some other values.
sin(x) is a function cos(x) is a function. Then we have got operators which map
these functions to some other functions, say differentiation, integration then
we have operations like multiplication of matricies.

<!--
Get a better beginning.
Explain what do you mean by a computation.
-->

People have to do computations by hand but what if you want to harness the
power of modern computers. But if you want to do computations on functions
which have real numbers as as their inputs you first need a method to represent
them on computers. But it turns out it impossible represent all the real numbes
or a finite subset <!-- What do you mean by finite subset?? --> of real numbers
on a computer. In fact it can proved that it is impossible to reprsent real
numbers on a modern computer working on 0s and 1's or any computer by the way,
the one bases on turing machines, I don't know about the quantum computers.

I can represent integers on a computer. How do I do it? The binary
representation of integers. If I have infinitely large memory I can represent
any integer. How do I represent a rational number I can have a pair of
integers. I can say the odd bit corresponds to the first integer and the even
bit corresponds to the second.

So, what are representations. They are maps one-to-one maps. I give you some
integers and you should be able to tell what is the corresponding bit string.
I should be able to tell the corresponding integer value if given a bit string.
Similarly the representations should have corresponding operators. I know how
to add two integers then I should know how to add two representation of
those integers. In this case the corresponding operator turns out to be fairly
simple, the binary addition. Now if I need to represent real numbers on
a computer I need one to one mapping from the set of real numbers to binary
strings, that's equivalent to have a mapping from real number to the set of
natural numbers. That's impossible, yeah impossible and it was shown
beautifully by George Cantor in what is called the Cantor's diagonal argument.
<!-- Read about Cantor -->
I won't explain it here you read it from the links given in the end.
So, how the hell will I do compuations on real numbers when I cannot even
represent them. And we need real valued functions every where, from rocket
science to the social science every where. <!-- Better examples -->

<!-- TODO: explain the non computability of the operators too -->
Here's what we do, we replace the real numbers in question by their floating
point representations <!-- Numerical Analysis doesn't need the number to
representable, the argument is flawed. --> and we replace the usual operators
by the approximate can compute.

Differentials by finite difference operators.
    d(f(x)     lim f(x + h) - f(x)
    ------ =   h->0 --------------
     dx                   h

     In general we don't know how to do the limit so we replace the h by a very
     small value. Here the error is of order h. For better result you do

    d(f(x)     lim f(x + h) - f(x - h)
    ------ =   h->0 --------------
     dx                   2*h

This is numerical computation. You replace all the values and operators by the
ones for which you how to deal with. Derivatives by finite differences,
integrals by reimann's summation. The input is in terms of something you know
how to work with and output is in the same format.

So, let's have an example. Say you have to compute the derivative of sin(x) at
pi/3. So the first thing you will do is to approximate pi with something you
can work with, that can be done by truncating the decimal or binary expansion
of pi at some value. Let's do for 5 digits.

Then you have to do differentiation you have to choose del x, let's that is 0.001
Then you don't even *know* what sin(x) is, so what you do? You approximate
sin(x) by truncating it's taylor series expansion at 0 to say 4 terms.
Now you plug the values and get the result.

It may seem that numerical compuations are useless, that they are too much
approximation but it's cool because it work, it appears crude but it works.
Numerical analysis is all about refining the techniques to be faster and more
robust, say by bounding and estimating the error. and don't ever need to have
a computer to do numerical analysis.

<!-- Show the calculations -->

Hey but why can't I simply output the exact answer, that is 1/2. I know already
the derivative of sin to be cos and we know the value of cos(pi/3) to be 1/2.
Why can't I use that knowledge to do the computation.

Now you are entering the domain of symbolic computation. You take the exact
representation of rational number, not the floating point. Then you got
a function which you name as `sin`. and you have got some properties associated
with it.
For Pi you don't its exact value some placeholder symbol will do
You 'know' the derivative of that function as some other function
`cos` whoes some properties you know. These properties include the values of
the function at some point.


When you want the program to compute the derivative of sin(pi/3) it uses it's
knowdege to get cos(pi/3), then it knows cos(pi/3)


By knowdege I mean simple conditional, if the argument is pi/3 then the value
of cos(pi/3) is 1/2.

Say in the computation you somehow got sin(1/10) you don't know what exact
value it corresponds to. You can't simply output a rational number for it or
somehting like sqrt(2)/sqrt(71). So, what do you do? You leave it
unevaluated. sin(1/10) is simply sin(1/10). Does it have any use this way? Yes,
because it can change forms and you have got rules to combine sin(1/10) with
other values, e.g., if you get somewhere get `sin(1/10)**2 + cos(1/10)**2` you
simplify it to 1 from the rules of trigonometry.

Similarly for derivatives you `know` the derivative of elementary functions,
you 'know' how to get the derivative of the combination of then from the chain
rule and multiplication and quotient rule.

So, you cna come with an algorithm for symbolic differentiation. That's the
essense of symbolic computation. You have the functions and operators and their
know properties. And you use this knowledge in the techniques of computation.

<!-- Can possibly include Richard's problem here -->

<!--
What is a representation, Representation is an one to one mapping of the object
to the bit strings in the computer.
-->
<!-- TODO: add links to Cantor's Diagonal Argument -->
