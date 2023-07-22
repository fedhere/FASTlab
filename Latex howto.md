If you have never used LaTeX, start with this "LaTeX in 30 min" webpage offered by Overleaf https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes
Overleaf is the platform on which we can collaboratively edit latex-based articles. When you want to start a new paper or report, go to [overleaf.com](https://www.overleaf.com)

## creating the document

UD CAS supports Overleaf upgraded subscriptions! make sure you get the upgraded subscription thorugh UD so you can activate the "track changes" after you finish your first draft and invite more than one coauthor.

Make sure you name your file something specific: while "thesis" may be unique for you, it is not for me since I am on all of FASTLab thesis documents!

Invite me (and the core coauthors) as an author.
<img width="540" alt="Screen Shot 2023-07-22 at 12 24 40 PM" src="https://github.com/fedhere/FASTlab/assets/1696902/0de7a002-71eb-4a87-944d-35baf16e4fd0">

Add the following macros to your header (the top of the document before \begin{document}):

```

\newcommand{\new}[1]{{\color{blue} #1}}

\newcommand{\question}[1]{{\color{red} #1}}

\newcommand{\ie}{\emph{i.e.}}

\newcommand{\eg}{\emph{e.g.}}
```
Then use \new{...} to change or add new text after discussions and suggestions from authors. Authors can use \question{...} to put questions in the text or to flag things that need to be discussed or changes, or checked and add a comment. This is what it will look like

<img width="390" alt="Screen Shot 2023-07-22 at 12 26 56 PM" src="https://github.com/fedhere/FASTlab/assets/1696902/2cf94f4c-cdb5-415d-b1e5-c15bc7f82dfd">
<img width="368" alt="Screen Shot 2023-07-22 at 12 27 17 PM" src="https://github.com/fedhere/FASTlab/assets/1696902/83aaaeca-e01d-4376-95be-3e8004085615">


Consider adding a macro with a different color (like the ones defined above as \new and \question) for each coauthor if its a small number of coauthors.

## latex and common and editorial syntax rules

- use \autoref{label} instead of \ref: that adds the Figure or Table word before the number as a part of the clickable element

- photometric filter lables (e.g. g) should be italic $g$

- numbers below 10 should be in letters (in most cases anyways)

- quotes should be `` ' ' instead of “” in latex

## citations
- Use \citep{} if you want the citation to appear in parenthesis: *the LSST \citep{ivezic19} => the LSST (Ivezić et al. 2019)*, this is the most common case.
- If you however want to integrate the citation in the narrative use \citet{}: *As in \citet{Ivezic19} => As in Ivezić et al. (2019)*. 
- Where you have a parenthetical that also requires a citation use \citealt{} instead of \citep{} *(as discussed in \citealt{ivezic19}) => (as discussed in Ivezić et al. 2019)*

If your paper is extensively discussing Rubin/LSST include the lsst bib file as one of your reference files. The file is here https://github.com/lsst/lsst-texmf/blob/main/texmf/bibtex/bib/lsst.bib
<img width="880" alt="Screen Shot 2023-07-22 at 12 19 14 PM" src="https://github.com/fedhere/FASTlab/assets/1696902/6ad15861-ea25-4abc-bb57-b15705e9720e">


