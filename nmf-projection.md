---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
toc: true

---

# NMF Projection

NMF decomposes a data matrix of D with N genes as rows and M samples as columns, into two matrices, as D AP. The pattern matrix P has rows associated with BPs in samples and the amplitude matrix A has columns indicating the relative association of a given gene, where the total number of BPs (k) is an input parameter. CoGAPS and GWCoGAPS seek a pattern matrix (P) and the corresponding distribution matrix of weights (A) whose product forms a mock data matrix (M) that represents the gene-wise data D within noise limits (ε). That is,

1.
<center>D = M + ε = AP + ε.</center>

<p>
The number of rows in P (columns in A) defines the number of biological patterns (k) that CoGAPS/GWCoGAPS will infer from the number of nonorthogonal basis vectors required to span the data space. As in the Bayesian Decomposition algorithm Wang, Kossenkov, and Ochs (<a href="https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-7-175" target="_blank">2006</a>), the matrices A and P in CoGAPS are assumed to have the atomic prior described in Sibisi and Skilling (<a href="https://academic.oup.com/jrsssb/article/59/1/217/7083066" target="_blank">1997</a>). In the CoGAPS/GWCoGAPS implementation, αA and αP are corresponding parameters for the expected number of atoms which map to each matrix element in A and P, respectively. The corresponding matrices A and P are found by MCMC sampling.
</p>

Projection of CoGAPS/GWCoGAPS patterns is implemented by solving the factorization in 1. for the new data matrix where A is the fixed nonorthogonal basis vectors comprising the average of the posterior mean for the CoGAPS/GWCoGAPS simulations performed on the original data. The patterns P in the new data associated with this amplitude matrix is estimated using the least-squares fit to the new data implemented with the lmFit function in the limma package (<a href="https://bioconductor.org/packages/3.17/bioc/html/limma.html" target="_blank">*limma*</a>). The ```projectR``` function has S4 method for class ```Linear Embedding Matrix, LME```.

```r
library(projectR)
projectR(data, loadings,dataNames = NULL, loadingsNames = NULL,
NP = NA, full = FALSE)
```

## Input Arguments

The inputs that must be set each time are only the data and patterns, with all other inputs having default values. However, inconguities between gene names–rownames of the loadings object and either rownames of the data object will throw errors and, subsequently, should be checked before running.

The arguments are as follows:

**data** a target dataset to be projected into the pattern space

**loadings** a CoGAPSResult object

**dataNames** rownames (e.g. gene names) of the target dataset, if different from existing rownames of data

**loadingsNames** loadingsNames rownames (e.g. gene names) of the loadings to be matched with dataNames

**NP** vector of integers indicating which columns of loadings object to use. The default of NP = NA will use entire matrix.

**full** logical indicating whether to return the full model solution. By default only the new pattern object is returned.
