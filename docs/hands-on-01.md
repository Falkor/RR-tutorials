
# Challenge 1: Reproduce the Build of these Slides

The objective of this first exercise is to familiarize you with [Vagrant](https://www.vagrantup.com) and see how it can be used to bundle an environment out of a recipe.

In this context, we will:

1. try to _reproduce_ an environment that permits to build the slides used for this tutorial, knowing that this relies on a complex set of software ([LaTeX](https://www.latex-project.org/), [Beamer](http://www.ctan.org/pkg/beamer), [Pandoc](http://pandoc.org/) etc.)
2. create a _recipe_ that would be used to _provision_ an empty box automatically
3. commit and repackage the modified box for further sharing on [Vagrant Cloud](https://vagrantcloud.com/)
