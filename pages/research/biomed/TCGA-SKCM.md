---
layout: page
title: TCGA-SKCM Project
date: 2026-01-11
---

The transcriptome encompasses all the mRNA molecules produced by a cell at a specific moment. Researchers extract RNA from tumor biopsies to see which genes are "turned on" (upregulated) or "turned off" (downregulated). This reflects the activity of oncogenes (like BRAF or NRAS) and tumor suppressors.

The steps involved are :
1. **RNA Extraction & Fragmentation:** RNA is isolated and broken into small fragments.
2. **cDNA Library Preparation:** RNA is converted into stable cDNA and amplified.
3. **Sequencing:** High-throughput machines (Illumina) read these fragments, producing millions of short "reads."
4. **Alignment (STAR):** These reads are mapped back to the human reference genome (GRCh38). The pipeline used by TCGA is called STAR-Counts.

# Bulk RNA-Seq analysis :

I went to TCGA GDC and built a cohort of TCGA-SKCM of about 470 patients. Then under "Repositories", I applied filters:  Select "RNA-seq" under Experimental Strategy -> Select "Transcriptome Profiling" under Data Category and "Gene Expression Quantification" under Data Type. Then I got 473 files from 469 patients and all of them are `*star_gene_counts.tsv` type files. In each .tsv file, raw and normalised counts are mixed, i.e. row is a gene and the columns represent different measurement units as follows :

| Column Name        | Description                                                          | Status     |
| ------------------ | -------------------------------------------------------------------- | ---------- |
| unstranded         | Raw read counts (recommended for most tools like DESeq2/EdgeR).      | Raw        |
| stranded_first     | Raw counts for the 1st read strand.                                  | Raw        |
| stranded_second    | Raw counts for the 2nd read strand.                                  | Raw        |
| tpm_unstranded     | Transcripts Per Million.                                             | Normalized |
| fpkm_unstranded    | Fragments Per Kilobase Million.                                      | Normalized |
| fpkm_uq_unstranded | FPKM Upper Quartile (stabilizes values for inter-sample comparison). | Normalized |
---

**Unstranded** ignores read orientation so we'll use that. Further normalised metrics adjust for gene length and library size, but are not suitable for DESeq2 or EdgeR which prefer taking 

- If doing Differential Expression: Extract the "unstranded" column from every file.

- If doing Correlation/Clustering: Extract the "tpm_unstranded" column.

Further we have 473 files from 469 patients which means some patients might have both a Primary Tumor and a Metastatic sample.

Bulk RNA-seq is standard for differential expression (comparing primary vs. metastatic) and survival analysis, which is the intended use of the TCGA-SKCM cohort.

Because it is bulk data, your results for a single gene (e.g., CD8A) reflect the combined signal from melanoma cells, infiltrating immune cells, and surrounding connective tissue.

## Understanding the dataset

The TCGA-SKCM lacks the typical "tumor vs. normal" paradigm found in say many GEO studies.

If we look at the 14th and 15th characters of the TCGA barcode (e.g., TCGA-D3-A253-01A).
- 01 to 09: Tumor samples (01 is Primary Solid Tumor, 06 is Metastatic).
- 10 to 19: Normal samples (11 is Solid Tissue Normal).

You can check that the majority of SKCM samples in TCGA are metastatic or primary tumors, leading to an absence of matching normal tissues within the dataset.

## Downloading files

After applying the filters, 

**Step 1: Add to Cart and Download Metadata**

- Click the "Add All Files to Cart" button in the Repository view.

- Click the "Sample Sheet" button. This is a metadata file that links the cryptic GDC File Names to the actual TCGA Barcodes (e.g., TCGA-D3-A253). 

- Click the "Manifest" button. This is a small text file you will need to actually download the data through GDC DTT.

**Step 2: Download the Data (GDC Data Transfer Tool)**

While you can download the cart as a .tar.gz file directly, it often fails for 400+ files. The professional way is to use the GDC Data Transfer Tool (Client).

Download the [GDC Data Transfer Tool](https://gdc.cancer.gov/access-data/gdc-data-transfer-tool) for your OS.

Open your terminal or command prompt.

Run the following command: 
```sh
gdc-client download -m gdc_manifest.txt # Replace gdc_manifest.txt with the name of the manifest you downloaded.
```

This will create 473 separate folders on your hard drive, each containing one .tsv file.
