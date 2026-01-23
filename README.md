# Bioinformatic-Analysis-of-PIK3CA-Mutations-in-Cervical-Cancer-TCGA-CESC-

------ Overview -----
This study investifates the effect of mutations in the gene PIK3CA in the trancriptional profile (transcriptome) of patients with cervical cancer. Using data from TCGA (The Cancer Genome Atlas), an analysis of differential expression (DGE) was conducted, in order to find the genes and biological pathaways that are influenced by this particular mutation.

------ Data  -----

The data was obtained from cBioPortal (Cervical Squamous Cell Carcinoma - TCGA, Firehose Legacy) and includes:

- data_mrna_seq_v2_rsem.txt: Levels of gene expression (RNA-seq).
- data_mutations.txt: list of all the mutations (from where PIK3CA was isolates)
- data_clinical_sample.txt: patient IDs and clinical information.

----- Methodology -----

1) Data preprocessing & cleaning

- Mutants Detection: Isolation of patients with a mutation in PIK3CA using unique Tumor_Sample_Barcode.
- Matrix cleaning: Removing unnamed genes (dropna), setting genes as indexes (set_index) and removing unnecessary numerical IDs (drop).
- Groups Separation: Create Mutant (mutation) and Wild-Type (control group) groups.
- Low Counts Filtering: Retain genes with a total sum of counts >10 to reduce statistical noise.

2) Statistical Analysis

Using the PyDESeq2 library in order to detect differential expression
- Normalization: Correcting for differences in sequencing depth
- Dispersion Estimation: Calculating variance per gene
- Model Fitting: Fitting a negative binomial distribution

3) Filtering of statistical significant genes

The genes were found statistically significant if they met the following criteria:
- padj < 0.05: less than 5% possibility of a result due to chance
- |log2FoldChange| > 1: at least double (or half) expression in mutants.
