### Environment Management

\bbegin{Controlling/Providing your Environment}

* An _environment_ is a \alert{set of tools and materials} that permits a \alert{complete
reproducibility} of \textit{part/whole} experiment process.

\bend

. . .

\wbegin{}\centering

**Q1**: \alert{How to describe/provide the software environment used?}

\wend

> "I used [OpenFOAM](http://www.openfoam.com/) with [OpenMPI](https://www.open-mpi.org/) on [Debian](https://www.debian.org/)"

* Obvious solution: _Virtual Machines_
     - Easy way to [automatically] test recipes \hfill{}
     - **Yet** provides **only** the final result, _not the logic behind_



### Practical Environment Generation

\exbegin{}

* Accurate, organized and _easy-to\{read|take\} Docs_
    - [ Markdown](https://guides.github.com/features/mastering-markdown/), [mkdocs](http://www.mkdocs.org/), [org-mode](http://doc.norang.ca/org-mode.html), [Read the Docs](https://readthedocs.org/)...

. . .

* _Sharing Code and Data_
    - [`git`](https://git-scm.com/), [Github](https://github.com/), [Bitbucket](https://bitbucket.org/), [Gitlab](https://about.gitlab.com/)...

. . .

* _Mastering your environment clean and automated_ by:
    - Using common building tools \hfill{make, cmake etc.}
    - Using a constrained environment
          * Sandboxed Ruby/Python,[Vagrant](https://www.vagrantup.com), [Docker](https://www.docker.com/)
    - Automate its building through cross-platform recipes
    - Automatically test your recipes for Environment configuration

\exend

\just{4}{
  \begin{textblock}{0.7}(0.2,0.2)
  \begin{turn}{45}
  \begin{tcolorbox}\Large\centering\alert{\textbf{All covered in this tutorial!}}\end{tcolorbox}
  \end{turn}
\end{textblock}
}
