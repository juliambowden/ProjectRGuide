---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
toc: true

---

# Clustering Projection

As cannonical projection is not defined for clustering objects, the projectR package offers two transfer learning inspired methods to achieve the “projection” of clustering objects. These
methods are defined by the function used to quantify and transfer the relationships which define each cluster in the original data set to the new dataset. Briefly, ```cluster2pattern``` uses
the correlation of each genes expression to the mean of each cluster to define continuous weights. These weights are output as a pclust object which can serve as input to ```projectR```.
Alternatively, the ```intersectoR``` function can be used to test for significant overlap between two clustering objects. Both ```cluster2pattern``` and ```intersectoR``` methods are coded for a
generic list structure with additional S4 class methods for kmeans and hclust objects. Further details and examples are provided in the following respective sections.

## cluster2pattern

```cluster2pattern``` uses the correlation of each genes expression to the mean of each cluster to define continuous weights.

```r
library(projectR)
cluster2pattern(clusters = NA, NP = NA, Data = NA)
```

### Input Arguments

The inputs that must be set each time are the clusters and data.

The arguments are as follows:

|              | Arguments                                                              |
|--------------|------------------------------------------------------------------------|
| **clusters** | a clustering object                                                    |
| **NP**       | either the number of clusters desired or the subset of clusters to use |
| **data**     | data used to make clusters objects                                     |

### Output

The output of the ```cluster2pattern``` function is a ```pclust``` class object; specifically, a matrix of genes (rows) by clusters (columns). A gene’s value outside of its assigned cluster is zero. For
the cluster containing a given gene, the gene’s value is the correlation of the gene’s expression to the mean of that cluster.

## intersectoR

```intersectoR``` function can be used to test for significant overlap between two clustering objects. The base function finds and tests the intersecting values of two sets of lists, presumably the genes associated with patterns in two different datasets. S4 class methods for hclust and kmeans objects are also available.

```r
library(projectR)
intersectoR(pSet1 = NA, pSet2 = NA, pval = 0.05, full = FALSE, k = NULL)
```

### Input Arguments

The inputs that must be set each time are the clusters and data.

The arguments are as follows:

|           | Arguments                                                                                                                            |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------|
| **pSet1** | a list for a set of patterns where each entry is a set of genes associated with a single pattern                                     |
| **pSet2** | a list for a second set of patterns where each entry is a set of genes associated with a single pattern                              |
| **pval**  | the maximum p-value considered significant                                                                                           |
| **full**  | logical indicating whether to return full data frame of significantly overlapping sets. Default is false will return summary matrix. |
| **k**     | numeric giving cut height for hclust objects, if vector arguments will be applied to pSet1 and pSet2 in that order                   |

### Output

The output of the ```intersectoR``` function is a summary matrix showing the sets with stastically significant overlap under the specified p-val threshold based on a hypergeometric test. If ```full==TRUE``` the full data frame of signigicantly overlapping sets will also be returned.
