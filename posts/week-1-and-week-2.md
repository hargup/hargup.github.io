<!--
.. title: Week 1 and Week 2
.. slug: week-1-and-week-2
.. date: 2014/06/02 17:15:54
.. tags: sympy, GSoC
.. link:
.. description:
.. type: text
-->

Hi, As I told you about in the previous post, My work for this summer will be
improving the current equation solvers of sympy. There are basically three
thing I'll have to do.

1. Create a basic set infrastructure for solvers to return sets.
2. Rewrite the current solvers to be clean and robust.
3. On the top of 1 and 2 write new solvers and function, they will returning
   and handling all the solutions of equations like `sin(x) == 0` and a general
   singularity finder.

You can look at the details in my
[proposal](https://github.com/sympy/sympy/wiki/GSoC-2014-Application-Harsh-Gupta:-Solvers.)

A started working on the sets module in the community bonding period, basically
writing a set difference class at [this
PR](https://github.com/sympy/sympy/pull/7462) , I try to get it merged by the
end of this week.

Set are a pretty general mathematical constructs, and you can solve a lot of
hard problems if you could compute general set operations. The whole number
theory can be defined in terms of sets. So, computing general set operations,
e.g, intersection, unions is pretty hard problem too and we should not aim to
do all them.  In a meeting with Matthew and Sergey we discussed that it would
    be better if complete the part 2 first. It will give a general idea about
    what capabilities of sets we need.  Also some sets problems can be reduced
    to equations and vice versa.  <!-- Explain -->

I've started working on the univariate solvers [at
PR](https://github.com/sympy/sympy/pull/7523).  My aim for this
week will be.

1. Completing the rational solvers
2. Getting the open PR for the sets merged.
