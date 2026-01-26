# Bioinformatic Analysis of PIK3CA Mutations in Cervical Cancer (TCGA-CESC)

## 📋 Overview
This study investigates the effect of mutations in the **PIK3CA** gene on the transcriptional profile (transcriptome) of patients with cervical cancer. Using data from **TCGA (The Cancer Genome Atlas)**, a Differential Gene Expression (DGE) analysis was conducted to identify the genes and biological pathways influenced by this specific mutation.

## 📊 Data Source
Data was obtained from **cBioPortal** (Cervical Squamous Cell Carcinoma - TCGA, Firehose Legacy) and includes:
* `data_mrna_seq_v2_rsem.txt`: Gene expression levels (RNA-seq).
* `data_mutations.txt`: List of all mutations (used to isolate PIK3CA variants).
* `data_clinical_sample.txt`: Patient IDs and clinical information.

# Bioinformatic Analysis of PIK3CA Mutations in Cervical Cancer (TCGA-CESC)

## 📋 Overview
This study investigates the effect of mutations in the **PIK3CA** gene on the transcriptional profile (transcriptome) of patients with cervical cancer. Using data from **TCGA (The Cancer Genome Atlas)**, a Differential Gene Expression (DGE) analysis was conducted to identify the genes and biological pathways influenced by this specific mutation.

## 📊 Data Source
Data was obtained from **cBioPortal** (Cervical Squamous Cell Carcinoma - TCGA, Firehose Legacy) and includes:
* `data_mrna_seq_v2_rsem.txt`: Gene expression levels (RNA-seq).
* `data_mutations.txt`: List of all mutations (used to isolate PIK3CA variants).
* `data_clinical_sample.txt`: Patient IDs and clinical information.

## ⚙️ Methodology

### 1. Data Preprocessing & Cleaning
* **Mutation Detection:** Isolated patients with PIK3CA mutations using unique `Tumor_Sample_Barcode`.
* **Matrix Cleaning:** Handled missing values (`dropna`), set gene symbols as indexes, and removed non-essential numerical IDs.
* **Group Separation:** Created two cohorts: **Mutant** (test group) and **Wild-Type** (control group).
* **Low Counts Filtering:** Retained genes with total counts > 10 to reduce statistical noise and improve power.

### 2. Statistical Analysis
Differential expression was performed using the **PyDESeq2** library:
* **Normalization:** Corrected for differences in sequencing depth.
* **Dispersion Estimation:** Calculated variance per gene to account for biological replicates.
* **Model Fitting:** Fit a Negative Binomial distribution to the data.

### 3. Significance Criteria
Genes were considered statistically significant (DEGs) if they met:
* **$p_{adj} < 0.05$**: Less than 5% probability of a result being a false positive.
* **$|log2FoldChange| > 1$**: At least a 2-fold increase or decrease in expression.

## 📈 Result Visualization & Interpretation

### 1. Volcano Plot
The Volcano Plot visualizes the relationship between statistical significance ($y$-axis) and the magnitude of change ($x$-axis).
* **Red dots:** Up-regulated genes (activated by the mutation).
* **Blue dots:** Down-regulated genes (repressed by the mutation).


  <img width="865" height="554" alt="image" src="https://github.com/user-attachments/assets/2389f0e8-f5db-4c5a-b889-86e0207bc72a" />
  
**Interpretation:**
We observe significance values reaching $10^{-14}$ on the $y$-axis, suggesting extremely robust differences. Some genes show a $log2FC > 4$ (16-fold increase) or $log2FC < -8$ (severe repression). The symmetry of the plot indicates that **PIK3CA acts as a master regulator**, radically reorganizing cellular transcription.

### 2. Pathway Enrichment (Barplot)
An **Over-Representation Analysis (ORA)** was conducted using `gseapy` with the **KEGG_2021** and **GO_Biological_Process_2021** databases.

<img width="856" height="277" alt="image" src="https://github.com/user-attachments/assets/fa56e589-f010-41e0-8e34-b69b5d7aeb47" />

**Key Biological Trends:**
* **Response to metal ions/cadmium:** Related to oxidative stress; suggesting the mutation helps cancer cells survive toxic environments.
* **Cell-cell adhesion:** Crucial for metastatic potential; identified via GO:0016338 and GO:0098742.
* **Developmental Plasticity:** Activation of neurogenesis pathways suggests cells acquire plasticity, a hallmark of cancer progression.

### 3. Heatmap

The heatmap visualizes the top 30 most significant genes across all patients.

<img width="865" height="737" alt="image" src="https://github.com/user-attachments/assets/e2891a55-6c97-42ed-bb5a-3d3953307320" />

**Interpretation:**
While the Volcano Plot shows clear statistical differences, the Heatmap reveals **biological heterogeneity**. The absence of a single, uniform pattern across all mutants suggests that the PIK3CA mutation interacts with other genetic or epigenetic factors. This highlights the complexity of cervical cancer and the necessity for personalized patient categorization.



