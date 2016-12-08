### Validation in (Computer) Science { .t }
<!-- Courtesy L.Nussbaum -->

\begin{textblock}{0.25}(0.7,0.2)
  \imgw{}{BigBangTheory.jpg}

  \imgw{}{The-Large-Hadron-Collider.jpg}
\end{textblock}


* Two classical approaches for validation:
    - _Formal_: equations, proofs, etc.
    - _Experimental_, on a scientific instrument
* Often a mix of both:
    - In Physics
    - In Computer Science
* Quite a lot of _formal_ work in Computer Science
* But also quite a lot of experimental validation
    - Distributed computing, networking
         * testbeds: [IoT-LAB](https://www.iot-lab.info/), [Grid'5000](https://www.grid5000.fr)...
    - Language/image processing $\leadsto$ evaluations using large corpuses

. . .

\wbegin{}\centering

\alert{How good are we at performing experiments?}

\wend


### (Poor) State of Experimentation in CS

* _1994_: survey of 400 papers\footcite{Lukowicz94}
    - among published CS articles in ACM journals
    - _40\%-50\%_ of those **\alert{requiring}** an experimental validation **\alert{had none}**
* _1998_: survey of 612 papers\footcite{zelk98}
    - too many papers have **no experimental validation at all**
    - too many papers use an informal (assertion) form of validation
    - 2009 update: situation is improving\footcite{zelk09}

### (Poor) State of Experimentation in CS

* Most papers _do not use_ even basic statistical tools
    - Papers published at the Europar conference\footnote{Study carried out by E. Jeannot.}

\scriptsize

|          Year | #Papers | With error bars | Percentage |
|---------------|---------|-----------------|------------|
|          2007 |      89 |               5 |       5.6% |
|          2008 |      89 |               3 |       3.4% |
|          2009 |      86 |               2 |       2.4% |
|          2010 |      90 |               6 |       6.7% |
|          2011 |      81 |               7 |       8.6% |
|---------------|---------|-----------------|------------|
| **2007-2001** | **435** |          **23** |   **5.3%** |


### (Poor) State of Experimentation in CS

* _2007_: [Survey](http://www.comp.brad.ac.uk/het-net/tutorials/P37.pdf) of simulators used in P2P research\footcite{Naicken:2007:SPS:1232919.1232932}
     - 287 papers surveyed on P2P networking subject
     - _141_ of these papers _reports the use of a simulator_
          * 30% use a custom tool
          * 50% don't report the used tool!

\centering
\piechart{0.4}{
  30.5/Custom,
  50.4/Unspecified,
  5.6/NS-2,
  5/Chord (SFS),
  8.5/Others
}

### (Poor) State of Experimentation in CS ###

\begin{textblock}{0.6}(0.4,0.31)
  \imgw{}{Collberg_Total.png}
\end{textblock}

* _2015_: **\alert{601 papers}** from ACM conferences and journals analysed
\footcite{Collberg:Repeatability2015}
    - _Obj._:  attempt to locate any source code that backed up the published results;
    _if found, try to build the code_.
    - EM$^\text{no}$ (__146 papers!__): code cannot be provided!
    - Original study: _80% of non reproducible work_


\piechart{0.4}{% total: 601
  32.2/OK,
  24.3/EM$^\text{no}$,
  3.8/OK$^\text{Authors}$,
  33.1/Excluded,
  5/Authors don't answer,
  1.6/Build Fails
}


### And in Other Sciences? ###

* _2008_: Study shows lower fertility for mices exposed to transgenic maize ([AFSSA report](https://www.anses.fr/en/system/files/BIOT2008sa0361EN.pdf)\footnote{Opinion of the French Food Safety Agency (Afssa) on the study by Velimirov et al.})
     - Several calculation errors have been identified
     - led to a false statistical analysis \& interpretation

. . .

* _2011_: CERN Neutrinos / OPERA Experiment
    - [_faster-than-light neutrinos_](https://en.wikipedia.org/wiki/Faster-than-light_neutrino_anomaly)
        * People started gossiping about relativity violation...
    - caused by timing system failure in 2012

. . .

\vspace*{-1em}
\exbegin{}

* \alert{\frownie}: Not everything is perfect
* \smiley: But some errors are properly identified
    - Stronger experimental culture in other (older?) sciences?
    - Long history of costly experiments, scandals, \ldots

\exend

\just{2}{
\begin{textblock}{0.2}(0.77,0.35)
  \imgw{}{Faster-than-light-neutrino-anomaly.png}
\end{textblock}
}
