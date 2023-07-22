## creating the document

UD CAS allows latex upgraded subscriptions! make sure you get the upgraded subscription thorugh UD so you can activate the "track changes" and invite more than one coauthor

Invite me (and the core coauthors) as an author.

Make sure you name your file something specific: while "thesis" may be unique for you, it is not for me since I am on all of FASTLab thesis documents!

Add the following macros to your header (the top of the document before \begin{document}):

\newcommand{\new}[1]{{\color{blue} #1}}

\newcommand{\question}[1]{{\color{red} #1}}

\newcommand{\ie}{\emph{i.e.}}

\newcommand{\eg}{\emph{e.g.}}

Then use \new{...} to change or add new text after discussions and suggestions from authors. Authors can use \question{...} to put questions in the text or to flag things that need to be discussed or changes, or checked and add a comment.

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


