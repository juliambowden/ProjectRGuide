---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
toc: true

---

# NMF Projection

NMF decomposes a data matrix of D with N genes as rows and M samples as columns, into two matrices, as D AP. The pattern matrix P has rows associated with BPs in samples and
the amplitude matrix A has columns indicating the relative association of a given gene, where the total number of BPs (k) is an input parameter. CoGAPS and GWCoGAPS seek a pattern
matrix (P) and the corresponding distribution matrix of weights (A) whose product forms a mock data matrix (M) that represents the gene-wise data D within noise limits (ε). That is,

<center>D = M + ε = AP + ε.</center>

The number of rows in P (columns in A) defines the number of biological patterns (k) that CoGAPS/GWCoGAPS will infer from the number of nonorthogonal basis vectors required
to span the data space. As in the Bayesian Decomposition algorithm Wang, Kossenkov, and Ochs (<a href="https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-7-175" target="_blank">2006</a>), the matrices A and P in CoGAPS are assumed to have the atomic prior
described in Sibisi and Skilling (<a href="https://academic.oup.com/jrsssb/article/59/1/217/7083066" target="_blank">1997</a>). In the CoGAPS/GWCoGAPS implementation, αA and αP are corresponding parameters for the expected number of atoms which map to each
matrix element in A and P, respectively. The corresponding matrices A and P are found by MCMC sampling.
