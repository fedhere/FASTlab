# Name - Week of MM/DD/YYYY

## 1. At least 2 of the 3 subsections should have material

### 1.1 Papers Read

\<link1\> : Couple of sentence summary of paper 1.

\<link2\> : Couple of sentence summary of paper 2.

.
.
.
### 1.2 Code Written

foo1.py : Summary of what you did to foo1.py (or if it is new, what it does) with link to github.

foo2.py : Summary of what you did to foo2.py (or if it is new, what it does) with link to github.

.
.
.
### 1.3 Other (algorithm, discussion with experts, went to conference)

## 2. Figures (at least 1 figure)

Figure 1 : Figure 1 with a caption that describes what is being plotted. Add link to code on github that created the plot.

Figure 2 : Figure 2 with a caption that describes what is being plotted. Add link to code on github that created the plot.

.
.
.
### 3 Results (required)

This section should be a discussion of what you did, how you did it, why you did it, and what you found.  For example,

_In order to explore the extent to which dimensionality reduction plus a classifier can be used to tag each spectrum with a known spectral type, I wrote pca_classify.py (link to code) which first whitened the spectra and then used scikit-learn’s PCA functionality to decompose the sample into N=20 principle components.  The first 5 are shown in Figure 1.  The figure shows that these components are dominated by only a few spectral lines.  Figure 2 shows the explained variance as a function of the number of included principle components and suggests that N=20 is insufficient.  Nevertheless, by projecting onto these components and training an SVM classifier (using grid search on the parameters Cand ; see Figure 3) with training and testing sets of size 3,500 and 1,500 respectively, I achieved an accuracy of 80% on the testing set._
