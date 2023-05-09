---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
toc: true

---

# Correlation Based Projection

Correlation based projection requires a matrix of gene-wise correlation values to serve as the Pattern input to the ```projectR``` function. This matrix can be user-generated or the result of
the ```correlateR``` function included in the projectR package. User-generated matrixes with each row corresponding to an individual gene can be input to the generic ```projectR``` function.
The ```correlateR``` function allows users to create a weight matrix for projection with values quantifying the within dataset correlation of each genes expression to the expression pattern
of a particular gene or set of genes as follows.

## correlateR

```r
library(projectR)
correlateR(genes = NA, dat = NA, threshtype = "R", threshold = 0.7, absR = FALSE, ...)
```

### Input Arguments

The inputs that must be set each time are only the genes and data, with all other inputs having default values.

The arguments are as follows:

|                | Arguments                                                                                                      |
|----------------|----------------------------------------------------------------------------------------------------------------|
| **genes**      | gene or character vector of genes for reference expression pattern dat                                         |
| **data**       | matrix or data frame with genes to be used to calculate correlation                                            |
| **threshtype** | Default "R" indicates thresholding by R value or equivalent. Alternatively, "N" indicates a numerical cut off. |
| **threshold**  | numeric indicating value at which to make threshold                                                            |
| **absR**       | logical indicating where to include both positive and negatively correlated genes                              |
| ***...***      | addition imputed to the cor function                                                                           |

### Output

The output of the ```correlateR``` function is a ```correlateR``` class object. Specifically, a matrix of correlation values for those genes whose expression pattern in the dataset is correlated
(and anti-correlated if absR=TRUE) above the value given in as the threshold arguement. As this information may be useful in its own right, it is recommended that users inspect the
```correlateR``` object before using it as input to the ```projectR``` function.

## Obtaining and Visualizing ```correlateR``` Objects

```r
# data to
library(projectR)
data("p.RNAseq6l3c3t")
# get genes correlated to T
cor2T<-correlateR(genes="T", dat=p.RNAseq6l3c3t, threshtype="N", threshold=10, absR=TRUE)
cor2T <- cor2T@corM
### heatmap of genes more correlated to T
indx<-unlist(sapply(cor2T,rownames))
indx <- as.vector(indx)
colnames(p.RNAseq6l3c3t)<-pd.RNAseq6l3c3t$sampleX
library(reshape2)
pm.RNAseq6l3c3t<-melt(cbind(p.RNAseq6l3c3t[indx,],indx))
## Using indx as id variables
library(gplots)
library(ggplot2)
library(viridis)
## Loading required package: viridisLite
pCorT<-ggplot(pm.RNAseq6l3c3t, aes(variable, indx, fill = value)) +
geom_tile(colour="gray20", size=1.5, stat="identity") +
scale_fill_viridis(option="B") +
xlab("") + ylab("") +
scale_y_discrete(limits=indx) +
ggtitle("Ten genes most highly pos & neg correlated with T") +
theme(
panel.background = element_rect(fill="gray20"),
panel.border = element_rect(fill=NA,color="gray20", size=0.5, linetype="solid"),
panel.grid.major = element_blank(),
panel.grid.minor = element_blank(),
axis.line = element_blank(),
axis.ticks = element_blank(),
axis.text = element_text(size=rel(1),hjust=1),
axis.text.x = element_text(angle = 90,vjust=.5),
legend.text = element_text(color="white", size=rel(1)),
legend.background = element_rect(fill="gray20"),
legend.position = "bottom",
legend.title=element_blank()
)
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## i Please use `linewidth` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
## Warning: The `size` argument of `element_rect()` is deprecated as of ggplot2 3.4.0.
## i Please use the `linewidth` argument instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

![Gene Heatmap](/images/GeneHeatmap.png)

## Projecting correlateR Objects

```r
# data to project into from RNAseq6l3c3t expression data
data(p.ESepiGen4c1l)
library(projectR)
cor2ESepi <- projectR(p.ESepiGen4c1l$mRNA.Seq,loadings=cor2T[[1]],full=FALSE,
dataNames=map.ESepiGen4c1l$GeneSymbols)
## [1] "9 row names matched between data and loadings"
## [1] "Updated dimension of data: 9 9"
```
