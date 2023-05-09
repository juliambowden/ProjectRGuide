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
