---

layout: page
title: projectR
subtitle: An R package for creating projection matrices from gene weights extracted from high dimensional genomic analysis.
hero_image: /projectRGuide/images/projectRHero.jpg
<!-- hero_height: is-fullwidth -->
hero_darken: true
show_sidebar: false
hero_link: https://github.com/genesofeve/projectR
hero_link_text: GitHub Repository

---

Technological advances continue to spur the exponential growth of biological data as illustrated by the rise of the omics—genomics, transcriptomics, epigenomics, proteomics, etc.—each with their own high throughput technologies. In order to leverage the full power of these resources, methods to integrate multiple data sets and data types must be developed. The reciprocal nature of the genomic, transcriptomic, epigenomic, and proteomic biology requires that the data provides a complementary view of cellular function and regulatory organization; however, the technical heterogeneity and massive size of high-throughput data even within a particular omic makes integrated analysis challenging. To address these challenges, we developed **projectR**, an R package for integrated analysis of high dimensional omic data. projectR uses the relationships defined within a given high dimensional data set, to interrogate related biological phenomena in an entirely new data set. By relying on relative comparisons within data type, projectR is able to circumvent many issues arising from technological variation. For a more extensive example of how the tools in the projectR package can be used for in silico experiments, or additional information on the algorithm, see [Stein-O’Brien, et al](https://www.biorxiv.org/content/10.1101/395004v2).
