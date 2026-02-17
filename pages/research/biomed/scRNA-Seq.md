---
layout: page
title: scRNA-Seq Tutorial
date: 2026-02-17
---

# I. Introduction

Single-cell RNA sequencing allows researchers to profile the gene expression of individual cells within a heterogeneous sample, such as a tissue or tumor.

Bulk sequencing was useful for characterizing the overall gene expression or DNA variation in a sample, but it couldn’t reveal the diversity and functional differences among individual cells within that population.

Tang et al 2009 introduced a technique of microfluidics to isolate single cells and perform RNA sequencing on them.

Thereafter Numerous SCS methods were developed, including scDNA-
seq, scATAC-seq, scMethyl-seq, Smart-seq2, and 10× Genomics. These methods expanded the scope of single-cell analysis beyond transcriptomics, into genomics, epigenomics, etc.

Feature = gene
Barcode = cell

## Count Matrix : From Raw Counts to UMIs

In bulk rna seq the count matrix obtained by running feature counts on the FaSTQ output of the bulk sequencer contains count values which signify both the unique number of mRNA molecules in the sample plus the number of PCR duplicates, and because we have enough number of cells, the proportion of PCR duplicates is usually not too high. When you use DESeq2 on bulk data, it uses a Size Factor normalization it somewhat tries to reduce this PCR noise effect.

In Single cell RNA seq  on the other hand we have limited number of cells which demands greater no. of PCR cycles before the sequencer can detect it. which is why sometimes the number of PCR duplicates can far exceed the number of unique molecules (PCR bias). Therefore, unique molecule identifier or UMI are attached at the library preparation stage by CellRanger in SCRNA seq which eliminates The contribution of PCR duplicates in the count matrix.

Single cell RNA-seq data may be generated using the Cell Ranger (10X genomics) platform which contains two types of (raw & filtered) feature-barcode matrices. Each element of the matrix is the number of UMIs associated with a feature (row) and a barcode (column).

**Note on Multi-Sample Data:** If your dataset is a merger of multiple samples (e.g., different conditions, mice, or batches, stored in orig.ident), batch effects can dominate the analysis. So a modified workflow incorporating Harmony for batch correction, making it universal for both single-sample (e.g., PBMC) and multi-sample data (e.g., MC38 tumor models) will then be needed.

# II. First steps


# Step 1: Loading Packages
Load necessary packages, including Harmony for batch integration.
```r
library(dplyr)
library(Seurat)
library(patchwork)
library(openxlsx)
library(writexl)
library(tidyverse)
library(ggplot2)
library(harmony)
```

## Step 1 : Reading Data

We start by reading in the data. The `Read10X()` function reads in the output of the cellranger pipeline from 10X, returning a UMI count matrix

```r
pbmc.data = Read10X(data.dir = "filtered_gene_bc_matrices/hg19/")
```

## Step 2 : Create Seurat Object

We next use the count matrix to create a Seurat object.
```r
pbmc = CreateSeuratObject(counts = pbmc.data, min.cells = 3, min.features = 200)
```
Genes detected in fewer than 3 cells are removed, and cells with fewer than 200 genes are removed.
This results in a Seurat object pbmc with approximately 22000+ features across 10,000+ cells.

Refer [here](https://biostatsquid.com/seurat-objects-explained/) to understand better about the structure of Seurat objects.

Basically its a nested structure like `object @ slot @ assay @ layer`.
The layer is the end dataframe you work on.

After creating your Seurat object, the next steps are data preprocessing, batch integration (if applicable) and dimensionality reduction. 

# III. Data Preprocessing

## Step 1 : QC (Data cleaning)

1. Calculate percentage of mitochondrial genes
```r
pbmc[["percent.mt"]] <- PercentageFeatureSet(pbmc, pattern = "^MT-")
```

2. Visualize QC metrics in violin plot to decide thresholds
```r
VlnPlot(pbmc, features = c("nFeature_RNA", "nCount_RNA", "percent.mt"), ncol = 3)
```

3. FeatureScatter used to visualize feature-feature relationships
```r
plot1 = FeatureScatter(pbmc, feature1 = "nCount_RNA", feature2 = "percent.mt")
plot2 = FeatureScatter(pbmc, feature1 = "nCount_RNA", feature2 = "nFeature_RNA")
plot1
plot2
```


4. Filter cells based on decided thresholds (adjust based on your violin plots)
```r
pbmc <- subset(pbmc, subset = nFeature_RNA > 200 & nFeature_RNA < 2500 & percent.mt < 5)
```


## Step 2 : Data Normalisation 

```
This step addresses differences between cells by adjusting for sequencing depth or other technical variations, ensuring that gene expression values are comparable across cells.
```

After removing unwanted cells from the dataset, the next question we ask is: If Gene X has 10 UMIs in Cell A and 100 UMIs in Cell B, is Gene X expressed 10x higher in Cell B? Though we eliminated **PCR bias** by starting with "UMI" based count matrices instead of the raw counts, a **sequencing-depth bias** remains as follows : What if Cell B was just sequenced deeper (captured more mRNA molecules overall) ?

To fix this, the next step is to normalize the data. Here, to fix the sequencing depth bias, we normalise each UMI count by dividing by the sum of UMI counts for a particular cell, and then multiplying this by a scale factor (10,000 by default)

But even after normalisation it is observed that the normalised counts within a cell are highly skewed (right-skewed infact). So to make those values closer, we do "log" on the values, which compresses the dynamic range so highly expressed genes don't dominate numerically (log turns multiplicative changes into additive ones, reduces skewness). This complete **log-normalisation** is done by the following command:
```r
pbmc = NormalizeData(pbmc)
```
Normalized values are stored in `pbmc[[“RNA”]]@data`.

### Eliminating Housekeeping Genes

Most genes in a cell are "housekeeping genes" (like GAPDH or ACTB). They are characterised by a near uniform expression across almost all cells.

The `vst` (variance-stabilizing transformation) method looks for genes that have high variance relative to their mean expression level. We can then eliminate those by keeping just the top 2000 most variable genes.

```r
pbmc = FindVariableFeatures(pbmc, selection.method = "vst", nfeatures = 2000)

# Peek at the top 10
head(VariableFeatures(pbmc), 10)

# Plot variable features with and without labels
plot1 <- VariableFeaturePlot(pbmc)
plot2 <- LabelPoints(plot = plot1, points = top10, repel = TRUE)
plot1 + plot2
```

The low-variance genes (black dots in the above plot) are automatically eliminated by the `FindVariableFeatures()` function.


## Step 3 : Scaling

```
This step addresses differences between genes by adjusting for varying ranges in expression levels across genes, making sure that no single gene with very high expression dominates the analysis.
```

Our goal is PCA clustering. 

But even after log-normalising the counts, PCA would still be heavily driven by the most highly expressed genes, because their numerical variation is larger in absolute terms.

So now instead of focussing on the entire cohort of genes in a cell (as in the log-norm step), we will work these successive steps on each gene separately across all cells:
- Subtract the mean expression of that gene → centers to mean = 0
- Divide by the standard deviation of that gene → scales to variance = 1 (z-score transformation)

This process is called scaling. With this, every gene now has mean = 0 and variance = 1 across cells. Moreover a 2-fold change in a lowly expressed gene (e.g., from 0.1 → 0.2) becomes mathematically as "large" as a 2-fold change in a highly expressed gene (e.g., from 5 → 10). Also no gene dominates downstream math just because its raw/log values were larger.

```r
all.genes = rownames(pbmc)
pbmc = ScaleData(pbmc, features = all.genes)

# Sanity check
pbmc@assays$RNA@scale.data[1:20, 1:5]
```

# IV. Dimensionality Reduction with Batch Integration

## Step 1 : PCA

We've got the top 2000 high variance genes (HVGs) from the preceeding step, but 2000 is still a lot of dimensions. So we will try to reduce it to say 2 principal component axes

```r
pbmc <- RunPCA(pbmc, features = VariableFeatures(object = pbmc))
```

This will print a summary of the top gene list in the +ve and -ve axes of each PC.

**THEORY** :

Suppose after HVG selection:

- 2000 genes
- 3000 cells

You now have a matrix:

$$ X_{3000 \times 2000} $$

PCA computes eigenvectors of covariance matrix:

$$ \Sigma = \frac{1}{n} X^T X $$

It finds directions:

$$ \text{PC1} \to \text{direction of maximum variance} $$

$$ \text{PC2} \to \text{next orthogonal variance} $$

$$ \text{PC3} \to \text{next} $$

$$ \dots $$

Each cell now becomes:

$$ \text{Cell}_i = (\text{PC1}_i, \text{PC2}_i, \text{PC3}_i, \dots, \text{PC10}_i) $$

Instead of:

$$ \text{Cell}_i = (\text{Gene}_1, \text{Gene}_2, \dots, \text{Gene}_{2000}) $$


After running PCA, you can inspect the Cells × PCs matrix using :
```r
Embeddings(pbmc, "pca")
# or
head(pbmc@reductions$pca@cell.embeddings)
```

Then to visually decide how many PCs to use for clustering run :
```r
ElbowPlot(pbmc)
```

## Step 2 : Batch Integration with Harmony (Universal for Single or Multi-Sample) 

If your data has multiple batches (orig.ident), Harmony corrects for them. It works even for single-batch data (no change).
```r
pbmc <- RunHarmony(pbmc, group.by.vars = "orig.ident")
# sanity check
head(pbmc@reductions$harmony@cell.embeddings)
```

The `RunHarmony()` embeddings values are actually quite close to the `RunPCA()` embeddings values. 

# V. Clustering

## Step 1 :

Suppose from the elbow plot, you decide you'll go ahead with first 10 PCs.

Let $ PC_{ik} $ denote the k-th principal component value of cell i.

Then, treating each cell as a point in 10-dimensional space, the Euclidean distance between 2 cells i and j will be 

$$
d_{ij} = \sqrt{\sum_{k=1}^{10} (PC_{ik} - PC_{jk})^2}
$$

Using this formula, for each cell, Seurat can find the its K nearest neighbors (default k = 20). This data is stored internally in `pbmc@graphs$RNA_nn` and is called the **KNN graph.** The edge weights of these k neighbours should then be normalised to create the **SNN graph** stored in `pbmc@graphs$RNA_snn`
. All of this is done using the single function `FindNeighbours()`

```r
pbmc <- FindNeighbors(pbmc, reduction = "harmony", dims = 1:10)  # Use "pca" if no Harmony or just skip "reduction" parameter
```

## Step 2 :

With ther above SNN graph, we need to group nodes (cells) into communities based on **dense internal connections** and **sparse external connections**. Then our clustering will be done. This is done using th `FindClusters()` function.

```r
pbmc = FindClusters(pbmc, resolution = 0.5)
```
This function is essentially a "community detection" algorithm (like how Facebook suggests friends). If Cell A is neighbors with Cell B, and Cell B is neighbors with Cell C, the algorithm starts to see a neighbourhood (cluster). It essentially creates a new "cluster" column and assigns a cluster no. to each cell in the dataset. 

The Louvain algorithm used by Seurat for the above clustering determines the number of clusters automatically based on your data's structure and the resolution you set.

You can check the number of clusters made in your Seurat object using:
```r
length(levels(pbmc))
```

The resulting cluster assignments are stored in the `Idents(pbmc)` slot of your object.

# VI. Non-linear dimensionality reduction

We have the SNN graph (depicting clusters) now.

But the above clusters are still in the 10-dimensional space (because we chose PC_1 to PC_10). But humans can only visualize in 2D. So before you can see those clusters on a 2D map (DimPlot), you must run a non-linear dimensional reduction like UMAP or t-SNE to reduce embedding for each cell to 2 axes : UMAP_1 & UMAP_2. This calculates the coordinates needed for the Dimplot

```r
# Calculate UMAP coordinates
pbmc <- RunUMAP(pbmc, reduction="harmony", dims = 1:10) # skip "reduction" parameter for single sample
```

Results stored in `@reductions$umap@cell.embeddings`

# VII. UMAP Visualization of Clusters in 2D

```r
DimPlot(pbmc, reduction = "umap", group.by = c("orig.ident", "seurat_clusters"), label = TRUE)
```

# VIII. Marker identification

**Identifying markers of each cluster relative to all other clusters**

When the code runs `pbmc.markers <- FindAllMarkers(pbmc, ...)`, it performs a differential expression analysis (typically a Wilcoxon Rank Sum test) for every gene. This test compares the expression of a gene in one cluster against its expression in all other clusters combined. It will generate a large output `pbmc.markers` and takes some time and RAM.

```r
pbmc.markers = FindAllMarkers(pbmc, only.pos = TRUE, min.pct = 0.25, logfc.threshold = 0.25)

# Write results to file
write.csv(pbmc.markers, "results/pbmc_markers.csv", quote = F)
```

`pbmc.markers` dataframe contains cluster-wise list of marker genes and also the adj. p-value of each gene which is symbolic of its differential presence in given cluster w.r.t. all other clusters.

N.B. For multi-batch data, use `FindConservedMarkers()` instead of `FindAllMarkers()` to find markers consistent across batches.

# IX. Assigning cell type identity to clusters


```r
# Show the top 2 marker genes for every cluster
a = pbmc.markers %>% group_by(cluster) %>% top_n(n = 2, wt = avg_log2FC)
# Extract all entries in the "gene" column into a single vector "genes"
genes = a %>% pull(gene)
```

After you find these marker genes, you usually run a `FeaturePlot()` to see where those genes "light up" on your UMAP:

```r
FeaturePlot(pbmc, features = genes[1:2], split.by = "orig.ident")
```