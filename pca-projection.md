---

layout: page
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
toc: true

---

# PCA Projection

Projection of principle components is achieved by matrix multiplication of a new data set by
previously generated eigenvectors, or gene loadings. If the original data were standardized
such that each gene is centered to zero average expression level, the principal components are
normalized eigenvectors of the covariance matrix of the genes. Each PC is ordered according
to how much of the variation present in the data they contain. Projection of the original
samples into each PC will maximize the variance of the samples in the direction of that
component and uncorrelated to previous components. Projection of new data places the
new samples into the PCs defined by the original data. Because the components define
an orthonormal basis set, they provide an isomorphism between a vector space V , and ℜ
n which preserves inner products. If V is an inner product space over ℜ with orthonormal basis
B = v1, ..., vn and vϵV s.t[v]B = (r1, ..., rn), then finding the coordinate of vi
in v is precisely the inner product of v with vi, i.e. ri = ⟨v, vi⟩. This formulation is implemented for only
those genes belonging to both the new data and the PC space. The ```projectR``` function has S4 method for class ```prcomp```.

## Obtaining PCs to Project

```r
# data to define PCs
library(projectR)
data(p.RNAseq6l3c3t)
# do PCA on RNAseq6l3c3t expression data
pc.RNAseq6l3c3t<-prcomp(t(p.RNAseq6l3c3t))
pcVAR <- round(((pc.RNAseq6l3c3t$sdev)^2/sum(pc.RNAseq6l3c3t$sdev^2))*100,2)
dPCA <- data.frame(cbind(pc.RNAseq6l3c3t$x,pd.RNAseq6l3c3t))
#plot pca
library(ggplot2)
setCOL <- scale_colour_manual(values = c("blue","black","red"), name="Condition:")
setFILL <- scale_fill_manual(values = c("blue","black","red"),guide = FALSE)
setPCH <- scale_shape_manual(values=c(23,22,25,25,21,24),name="Cell Line:")
pPCA <- ggplot(dPCA, aes(x=PC1, y=PC2, colour=ID.cond, shape=ID.line,
fill=ID.cond)) +
geom_point(aes(size=days),alpha=.6)+
setCOL + setPCH + setFILL +
scale_size_area(breaks = c(2,4,6), name="Day") +
theme(legend.position=c(0,0), legend.justification=c(0,0),
legend.direction = "horizontal",
panel.background = element_rect(fill = "white",colour=NA),
legend.background = element_rect(fill = "transparent",colour=NA),
plot.title = element_text(vjust = 0,hjust=0,face="bold")) +
labs(title = "PCA of hPSC PolyA RNAseq",
x=paste("PC1 (",pcVAR[1],"% of varience)",sep=""),
y=paste("PC2 (",pcVAR[2],"% of varience)",sep=""))
```

## Projecting prcomp Objects

```r
# data to project into PCs from RNAseq6 l3c3t expression data
data(p.ESepiGen4c1l)
library(projectR)
PCA2ESepi <- projectR(data = p.ESepiGen4c1l$mRNA.Seq,loadings=pc.RNAseq6l3c3t,
full=TRUE, dataNames=map.ESepiGen4c1l[["GeneSymbols"]])
## [1] "93 row names matched between data and loadings"
## [1] "Updated dimension of data: 93 9"
pd.ESepiGen4c1l<-data.frame(Condition=sapply(colnames(p.ESepiGen4c1l$mRNA.Seq),
function(x) unlist(strsplit(x,'_'))[1]),stringsAsFactors=FALSE)
pd.ESepiGen4c1l$color<-c(rep("red",2),rep("green",3),rep("blue",2),rep("black",2))
names(pd.ESepiGen4c1l$color)<-pd.ESepiGen4c1l$Cond
dPCA2ESepi<- data.frame(cbind(t(PCA2ESepi[[1]]),pd.ESepiGen4c1l))
#plot pca
library(ggplot2)
setEpiCOL <- scale_colour_manual(values = c("red","green","blue","black"),
guide = guide_legend(title="Lineage"))
pPC2ESepiGen4c1l <- ggplot(dPCA2ESepi, aes(x=PC1, y=PC2, colour=Condition)) +
geom_point(size=5) + setEpiCOL +
theme(legend.position=c(0,0), legend.justification=c(0,0),
panel.background = element_rect(fill = "white"),
legend.direction = "horizontal",
plot.title = element_text(vjust = 0,hjust=0,face="bold")) +
labs(title = "Encode RNAseq in target PC1 & PC2",
x=paste("Projected PC1 (",round(PCA2ESepi[[2]][1],2),"% of varience)",sep=""),
y=paste("Projected PC2 (",round(PCA2ESepi[[2]][2],2),"% of varience)",sep=""))
```

```r
## Warning: The `guide` argument in `scale_*()` cannot be `FALSE`. This was deprecated in
## ggplot2 3.3.4.
## i Please use "none" instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

![PCA Projection Figure](/images/PCAProjectionFigure.png)
