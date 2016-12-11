### Reproducibility axes

\begin{textblock}{0.4}(0.6,0.22)
  \imgw{}{RR-3-axis.png}
\end{textblock}

* Always keep track of:
    - your \emph{methodology}
    - your \emph{code}
    - your (input) \emph{data}

* Can you later come back and:
    - reproduce you experiment
    - including its environment
    - ... and obtain the same results?

\pause
\vspace{1.8em}

* If \emph{not}, then \emph{now} is the best time to start
    - documenting your processes
    - describing your environment (software \emph{and hardware}!)
    - versioning and tagging your code and data
    - (... and keep backups of it all)

### Reproducibility levels

\begin{textblock}{0.4}(0.65,0.11)
  \smartdiagramset{
   planet size=2.5cm,
   satellite size=2.3cm,
   distance planet-text=0,
   distance planet-satellite=3cm, 
   /tikz/connection planet satellite/.append style={<-}
  }   
  \centering\scalebox{0.5}{\smartdiagram[constellation diagram]{\emph{Research},
  \hspace*{-0.3em}\mbox{Reviewable}, \hspace*{-0.3em}\mbox{Replicable}, 
  \hspace*{-0.5em}\mbox{Confirmable}, \hspace*{-0.2em}\mbox{Auditable}, \hspace*{-0.7em}\mbox{Reproducible}}}
\end{textblock}

\emph{Is your research}\footnote{ICERM Report 2013: "Reproducibility in Computational and Experimental Mathematics"}:

* reviewable
    - desc. of your methods can be independently assessed?
* replicable
    - are the tools available to duplicate the results?
* confirmable
    - can the main conclusions be attained independently of your tools?
* auditable
    - do you have records such that your research can be later defended?
    - ... or differences between independent confirmations resolved?
* \emph{open or reproducible}, such that
    - the procedures can be fully audited \emph{and}
    - the results can be replicated or independently reproduced \emph{and}
    - the results can be extended or the method applied to new problems
