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

(1.)

<p style="text-align: center;">D = M + ε = AP + ε.</p>

The number of rows in P (columns in A) defines the number of biological patterns (k) that CoGAPS/GWCoGAPS will infer from the number of nonorthogonal basis vectors required to span the data space. As in the Bayesian Decomposition algorithm Wang, Kossenkov, and Ochs (<a href="https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-7-175" target="_blank">2006</a>), the matrices A and P in CoGAPS are assumed to have the atomic prior described in Sibisi and Skilling (<a href="https://academic.oup.com/jrsssb/article/59/1/217/7083066" target="_blank">1997</a>). In the CoGAPS/GWCoGAPS implementation, αA and αP are corresponding parameters for the expected number of atoms which map to each matrix element in A and P, respectively. The corresponding matrices A and P are found by MCMC sampling.

Projection of CoGAPS/GWCoGAPS patterns is implemented by solving the factorization in (1.) for the new data matrix where A is the fixed nonorthogonal basis vectors comprising the average of the posterior mean for the CoGAPS/GWCoGAPS simulations performed on the original data. The patterns P in the new data associated with this amplitude matrix is estimated using the least-squares fit to the new data implemented with the lmFit function in the limma package (<a href="https://bioconductor.org/packages/3.17/bioc/html/limma.html" target="_blank">*limma*</a>). The ```projectR``` function has S4 method for class ```Linear Embedding Matrix, LME```.

```r
library(projectR)
projectR(data, loadings,dataNames = NULL, loadingsNames = NULL,
NP = NA, full = FALSE)
```

## Input Arguments

The inputs that must be set each time are only the data and patterns, with all other inputs having default values. However, inconguities between gene names–rownames of the loadings object and either rownames of the data object will throw errors and, subsequently, should be checked before running.

The arguments are as follows:

|                   | Arguments                                                                                                             |
|-------------------|-----------------------------------------------------------------------------------------------------------------------|
| **data**          | a target dataset to be projected into the pattern space                                                               |
| **loadings**      | a CoGAPSResult object                                                                                                 |
| **dataNames**     | rownames (e.g. gene names) of the target dataset, if different from existing rownames of data                         |
| **loadingsNames** | loadingsNames rownames (e.g. gene names) of the loadings to be matched with dataNames                                 |
| **NP**            | vector of integers indicating which columns of loadings object to use. The default of NP = NA will use entire matrix. |
| **full**          | logical indicating whether to return the full model solution. By default only the new pattern object is returned.     |

## Output

The basic output of the base projectR function, i.e. ```full=FALSE```, returns ```projectionPatterns``` representing relative weights for the samples from the new data in this previously defined feature space, or set of feature spaces. The full output of the base projectR function, i.e. ```full=TRUE```, returns ```projectionFit```, a list containing ```projectionPatterns``` and ```Projection```. The ```Projection``` object contains additional information from the proceedure used to obtain the ```projectionPatterns```. For the the the base projectR function, ```Projection``` is the full lmFit model from the package <a href="https://bioconductor.org/packages/3.17/bioc/html/limma.html" target="_blank">*limma*</a>.

## Obtaining CoGAPS Patterns to Project

```r
# get data
library(projectR)
AP <- get(data("AP.RNAseq6l3c3t")) #CoGAPS run data
AP <- AP$Amean
# heatmap of gene weights for CoGAPs patterns
library(gplots)
##
## Attaching package: 'gplots'
## The following object is masked from 'package:stats':
##
## lowess
pNMF<-heatmap.2(as.matrix(AP),col=bluered, trace='none',
distfun=function(c) as.dist(1-cor(t(c))) ,
cexCol=1,cexRow=.5,scale = "row",
hclustfun=function(x) hclust(x, method="average")
)
```

![CoGAPS Projected Pattern Figure](/images/CoGAPSProjectedPatternFigure.png)

## Projecting CoGAPS Objects

```r
# data to project into PCs from RNAseq6l3c3t expression data
library(projectR)
data('p.ESepiGen4c1l4')
## Warning in data("p.ESepiGen4c1l4"): data set 'p.ESepiGen4c1l4' not found
data('p.RNAseq6l3c3t')
NMF2ESepi <- projectR(p.ESepiGen4c1l$mRNA.Seq,loadings=AP,full=TRUE,
dataNames=map.ESepiGen4c1l[["GeneSymbols"]])
## [1] "93 row names matched between data and loadings"
## [1] "Updated dimension of data: 93 9"
dNMF2ESepi<- data.frame(cbind(t(NMF2ESepi),pd.ESepiGen4c1l))
#plot pca
library(ggplot2)
setEpiCOL <- scale_colour_manual(values = c("red","green","blue","black"),
guide = guide_legend(title="Lineage"))
pNMF2ESepiGen4c1l <- ggplot(dNMF2ESepi, aes(x=X1, y=X2, colour=Condition)) +
geom_point(size=5) + setEpiCOL +
theme(legend.position=c(0,0), legend.justification=c(0,0),
panel.background = element_rect(fill = "white"),
legend.direction = "horizontal",
plot.title = element_text(vjust = 0,hjust=0,face="bold"))
labs(title = "Encode RNAseq in target PC1 & PC2",
x=paste("Projected PC1 (",round(PCA2ESepi[[2]][1],2),"% of varience)",sep=""),
y=paste("Projected PC2 (",round(PCA2ESepi[[2]][2],2),"% of varience)",sep=""))
## $x
## [1] "Projected PC1 (18.32% of varience)"
##
## $y
## [1] "Projected PC2 (17.12% of varience)"
##
## $title
## [1] "Encode RNAseq in target PC1 & PC2"
##
## attr(,"class")
## [1] "labels"
```
