---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false

---

# Getting Started

For automatic Bioconductor package installation, start R, and run:

```r
BiocManager::install("projectR")
```

## Methods

Projection can roughly be defined as a mapping or transformation of points from one space to another often lower dimensional space. Mathematically, this can described as a function φ(x) = y : ℜ D 7→ ℜd s.t. d ≤ D for x ∈ ℜD, y ∈ ℜd Barbakh, Wu, and Fyfe (<a href="https://link.springer.com/chapter/10.1007/978-3-642-04005-4_3" target="_blank">2009</a>). The projectR package uses projection functions defined in a training dataset to interrogate related
biological phenomena in an entirely new data set. These functions can be the product of any one of several methods common to “omic” analyses including regression, PCA, NMF, clustering. Individual sections focusing on one specific method are included in the vignette. However, the general design of the projectR function is the same regardless.

## The Base projectR Function

The generic projectR function is executed as follows:

```r
library(projectR)
projectR(data, loadings, dataNames=NULL, loadingsNames=NULL, NP = NULL, full = false)
```

### Input Arguments

The inputs that must be set each time are only data and loadings, with all other inputs having default values. However, incongruities in the feature mapping between the data and loadings, i.e. a different format for the rownames of each object, will throw errors or result in an empty mapping and should be checked before running. To overcome mismatched feature names in the objects themselves, the /code{dataNames} and /code{loadingNames} arguments can be manually supplied by the user. 

The arguments are as follows:

**```data```** a dataset to be projected into the pattern space

**```loadings```** a matrix of continuous values with unique rownames to be projected 

**```dataNames```** a vector containing unique names, i.e. gene names, for the rows of the target dataset to be used to match features with the loadings, if not provided by ```rownames(data)```. Order of names in vector must match order of rows in data.
