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

------ Result visualization -------

1) Volcano plot

Shows the relationship between statistical significance (y axis) and size of change (x axis)
- Red dots: up-regulated genes that are activated from the mutation
- Blue dots: down-regulated genes that are repressed

  <img width="865" height="554" alt="image" src="https://github.com/user-attachments/assets/2389f0e8-f5db-4c5a-b889-86e0207bc72a" />

  Interpretation of volcano plot
  - We observe dots reaching values up to 14 on the y axis. This means that some genes have p value of the order of 10^-14, which sugests that the differene in their expression between mutant and wild-type is extremely unlikely to be due to chance. All the colored dots above the horizontal threshold are differentially expressed genes (DEGs)
  - Observing the x axis, we see that some genes have a log2FC>4 , meaning that their expression is above 16 times bigger compared to wild-type patients. Also, there are some genes with log2FC close to -8, which mean they almost have full repression in mutants.
  - The graph shows a symmetry between up-regulated ans down-regulated genes, which indicates that the mutation in PIK3CA acts as a master regulator, radically reorganizing the cell's transcription. Genes located in the top right corner (high significance and high increase are most likely biomarkers or therapeutic targets for patients with this mutation.

2) Barplot

A barplot was made in order to understand the biological meaning of the differentially expressed genes. An Over-Representation Analysis (ORA) was conducted with the use of the library gseapy and the databases KEG_2021_Human and GO_Biolgoical_Process_2021

Top Biological Processes and pathaways

<img width="856" height="277" alt="image" src="https://github.com/user-attachments/assets/fa56e589-f010-41e0-8e34-b69b5d7aeb47" />


The results highlight three main biological trends that are affected by the PIK3CA mutation
- Response to cadmium/metal ions: These pathways are related to the response to oxidative stress. The mutation appears to activate mechanisms that allow cancer cells to survive in toxic environments.
- Cell-cell adhesion: Significant changes were identified in genes that control how cells attach to each other (GO:0016338, GO:0098742). Modification of these mechanisms is crucial for the metastatic ability of the tumor.
- Developmental Plasticity (Developmental pathways): Activation of pathways related to the development of the nervous system (Neurogenesis/Brain development) suggests that cells acquire plasticity characteristics, reprogramming their function to promote disease progression.

3) Heatmap

<img width="865" height="737" alt="image" src="https://github.com/user-attachments/assets/e2891a55-6c97-42ed-bb5a-3d3953307320" />

The heatmap procides a visualization of the expression of the top 30 most significant genes (based on padj) for all the patients of the study.

Despite the statistically significant differential expression recorded in the Volcano Plot, the Heatmap reveals a  biological heterogeneity between samples. The absence of a single pattern suggests that the PIK3CA mutation interacts with other genetic or epigenetic factors, creating different molecular signatures in subgroups of patients. This highlights the complexity of cervical cancer and the need for further categorization of patients.

