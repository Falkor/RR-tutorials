### Hand-on 1: DIY Approach

* Let's see how to reproduce a simple yet practical example!

\wbegin{Challenge 1: Reproduce the Build of these Slides}

* Several **tricky** issues illustrating previous best practices
     - grab the sources                 \hfill{}[`git`](https://git-scm.com/)
     - use of a constrained environment \hfill{}[Vagrant](https://www.vagrantup.com)
     - installing the prerequisite software environment \hfill{}`apt-get`
          * [un]common mix here: `make`, `latex-beamer`, `biber`, `pandoc`...
          * generally the _major challenge_ in reproducing computations...

\wend

. . .

\begin{tcolorbox}\centering\scriptsize
\url{http://rr-tutorials.readthedocs.io/en/latest/hands-on-01/}
\end{tcolorbox}

\scriptsize
\hfill{}**\alert{IF NOT YET DONE:}** \myurl{http://rr-tutorials.readthedocs.io/en/latest/setup/   }

### Grab the [Code/Data] Source

\begin{textblock}{0.15}(0.8,0.15)
  \imgw{}{logo_github.png}
\end{textblock}


* You should have now [Git](http://git-scm.com) installed
* Get the `RR-tutorials` repository from [Github](https://github.com/Falkor/RR-tutorials)

~~~bash
$> git clone https://github.com/Falkor/RR-tutorials.git
$> cd RR-tutorials
$> make setup  # OR git submodule init && git submodule update
~~~

* _Notable_ elements within this cloned repository:
    -  the \LaTeX\ slides sources \hfill{}`slides/2016/cloudcom2016/src/`
    - Documentation sources \hfill{}`mkdocs.yml` and `docs/`
    - [Vagrant](https://www.vagrantup.com) configuration for **this** project\hfill{}`Vagrantfile`
    - [Bats](https://github.com/sstephenson/bats) unit tests \hfill{}`tests/`
    - Continuous Integration settings through [Travis-CI](https://travis-ci.org/Falkor/RR-tutorials)\hfill{}`.travis.yml`

### Use a Constrained Environment

\hfill\myurl{http://vagrantup.com/}

\imgw{}{screenshot_vagrant}
