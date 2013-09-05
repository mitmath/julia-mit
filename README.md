Julia for Numerical Computation in MIT Courses
==============================================

Several MIT courses involving numerical computation, including
[18.06](http://web.mit.edu/18.06/www/),
[18.303](http://math.mit.edu/~stevenj/18.303/),
[18.330](http://homerreid.ath.cx/teaching/18.330/),
[18.335/6.337](http://math.mit.edu/~stevenj/18.335/), and
[18.337/6.338](http://beowulf.csail.mit.edu/18.337/index.html), are
beginning to use [Julia](http://julialang.org/), a fairly new language
for technical computing.  This page is intended to supplement the
[Julia documentation](http://docs.julialang.org/en/latest/) with some
simple tutorials on installing and using Julia targeted at MIT
students.

In particular, we will be using Julia in the
[IJulia](https://github.com/JuliaLang/IJulia.jl) browser-based
enviroment, which leverages your web browser and
[IPython](http://ipython.org/) to provide a rich environment
combining code, graphics, formatted text, and even equations, with
sophisticated plots via [Matplotlib](http://matplotlib.org/).

## Why Julia?

Traditionally, these sorts of courses at MIT have used
[Matlab](https://en.wikipedia.org/wiki/MATLAB), a high-level
environment for numerical computation.  Other possibilities might be
[Scientific Python](http://www.scipy.org/) (the [Python
language](http://python.org/) plus numerical libraries), [GNU
R](http://www.r-project.org/), and many others.  Julia is another
high-level free/open-source language for numerical computing in the
same spirit, with a rich set of built-in types and libraries for
working with linear algebra and other types of computations, with a
syntax that is superficially reminiscent of Matlab's.

High-level dynamic programming languages (as opposed to low-level
languages like C or static languages like Java) are essential for
interactive exploration of computational science.  The allow you to
play with matrices, computations on large datasets, plots, and so on
without having to worry about managing memory, declaring types, or
other minutiae—you can open up a window and start typing commands to
immediately get results.

The traditional problem with high-level dynamic languages, however, is
that they are slow: as soon as you run into a problem that cannot
easily be expressed in terms of built-in library functions operating
on large blocks of data ("vectorized" code), you find that your code
is suddenly orders of magnitude slower that equivalent code in a
low-level language.  The typical solution has been to switch to
another language (e.g. C or Fortran) to write key computational
kernels, calling these from the high-level language (e.g. Matlab or
Python) as needed, but this is vastly more difficult than writing code
purely in a high-level language and imposes a steep barrier on anyone
hoping to transition from casual experimentation to "serious"
numerical computation.  Julia mostly eliminates this issue, because it
is carefully designed to exploit a "[just-in-time
compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation)"
called [LLVM](http://llvm.org/), making it possible to write
high-level code in Julia that achieves near-C speed.  (It also means
that we can perform meaningful performance experiments easily in
courses where this matters, e.g.  in 18.335.)

Speed, while avoiding the "two-language" problem of requiring C or
Fortran for critical code, is the initial draw of Julia, but there are
a few other nice points.  Unlike Matlab, it is free/open-source
software, which eliminates licensing headaches and allows you to look
inside the Julia implementation to see how it works (since Julia is
mostly written in Julia, its code is much more readable than a
language like Python that is largely implemented in low-level C).  For
calling existing code, it has easy facilities to [call external C or
Fortran
libraries](http://docs.julialang.org/en/latest/manual/calling-c-and-fortran-code/)
or to [call Python libraries](https://github.com/stevengj/PyCall.jl).
[Multiple
dispatch](http://docs.julialang.org/en/latest/manual/methods/) makes
it especially easy to overload operations and functions for new types
(e.g. to add new vector or numeric types).  Julia's built-in
[metaprogramming](http://docs.julialang.org/en/latest/manual/metaprogramming/)
facilities make it easy to write code that generates other code,
essentially allowing you to extend the language as needed.  And so on...

## Installing Julia and IJulia

First, [install IPython](http://ipython.org/install.html) and related
scientific-Python packages (SciPy and Matplotlib).  The simplest way
to do this on Mac and Windows is by [downloading the Anaconda
package](http://continuum.io/downloads), running its installer, and
then running the following commands on the command line (the
[Terminal](https://en.wikipedia.org/wiki/Terminal_%28OS_X%29) program
in MacOS or the [Command
Prompt](https://en.wikipedia.org/wiki/Command_Prompt) on Windows):

```
conda update conda
conda update ipython
```

Second, [download Julia](http://julialang.org/downloads/) *version 0.2*
(currently in prerelease) and run the installer.  Do *not* download
version 0.1.   Then run the Julia application (double-click on it); a
window with a `julia>` prompt will appear.  At the prompt, type:
```
Pkg.add("IJulia")
Pkg.add("PyPlot")
```

## Running Julia in the IJulia Notebook

Once you have followed the installation steps above, open up the
command line (Terminal or Command Prompt) and type:
```
ipython notebook --profile julia
```

A "dashboard" window like this should open in your web browser:

![IJulia dashboard](dashboard.png "IJulia Dashboard Window")

Now, click on the *New Notebook* button to start a new "notebook".  A
notebook will combine code, computed results, formatted text, and
images; for example, you might use one notebook for each problem set.
The notebook window that opens will look something like:

![IJulia notebook](notebook-1.png "IJulia empty notebook")

You can click the "Untitled0" at the top to change the name, e.g. to
"My first Julia notebook".  You can enter Julia code at the `In[ ]`
prompt, and hit **shift-return** to execute it and see the results.
If you hit **return** *without* the shift key, it will add additional
lines to a single input cell.  For example, we can [define a variable](http://docs.julialang.org/en/latest/manual/variables/)
`x` (using the built-in constant `pi` and the built-in function
`sin`), and then evaluate a polynomial `3x^2 + 2x - 5` in terms of `x`
(note that, unlike Matlabor Python, we don't have to type `3*x^2` if
we don't want to: a number followed by a variable is automatically
interpreted as multiplication without having to type `*`):

![IJulia notebook](notebook-2.png "Renamed IJulia notebook with a result")

The result that is printed (in `Out[1]`) is the *last* expression from
the input cell, i.e. the polynomial.  If you want to see the value of
`x`, for example, you could simply type `x` at the second `In[ ]` prompt
and hit shift-return.

See, for example, the [mathematical operations in the Julia
manual](http://docs.julialang.org/en/latest/manual/mathematical-operations/)
for many more basic math functions.

## Plotting

There are several plotting packages available for Julia.  If you
followed the installation instructions, above, you already have one
full-featured Matlab-like plotting package installed:
[PyPlot](https://github.com/stevengj/PyPlot.jl), which is simply a
wrapper around Python's amazing [Matplotlib](http://matplotlib.org/) library.

To start using PyPlot to make plots in Julia, first type `using
PyPlot` at an input prompt and hit shift-enter.  `using` is the Julia
command to load an [external
module](http://docs.julialang.org/en/latest/manual/modules/) (which
must usually [be
installed](http://docs.julialang.org/en/latest/manual/packages/)
first, e.g. by the `Pkg.add("PyPlot")` command from the installation
instructions above).

Then, you can type any of the [commands from
Matplotlib](http://matplotlib.org/api/pyplot_api.html), which includes
equivalents for most of the Matlab plotting functions.  For example:

![IJulia notebook](notebook-3.png "IJulia notebook with a plot")

