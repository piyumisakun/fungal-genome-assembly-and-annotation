# Structural Genome Annotation

## Overview
Structural genome annotation identifies protein-coding genes within the assembled genome by integrating genomic sequences with external protein evidence. In this workflow, the soft-masked fungal genome was annotated using BRAKER2 with homologous protein evidence from related fungal species. The quality of the predicted proteins was subsequently assessed using BUSCO before downstream functional annotation and laccase gene identification.

## Rationale
Accurate structural annotation is essential for identifying genes and predicting their biological functions. BRAKER2 combines ab initio gene prediction with external protein evidence, resulting in more reliable gene models than ab initio prediction alone. Evaluating the predicted proteins using BUSCO provides an independent assessment of annotation completeness before functional annotation and candidate gene analysis.

## Workflow
```
Soft-masked Genome
        │
        ▼
Prepare Protein Evidence
        │
        ▼
Configure BRAKER2
        │
        ▼
Run BRAKER2
        │
        ▼
Predicted Gene Models
        │
        ▼
BUSCO Assessment
        │
        ▼
High-quality Annotated Proteins
```

## Step 1 – Configure BRAKER2 Environment
### Methodology
Before annotation, the GeneMark-ES package was configured within the BRAKER2 environment by extracting the software, defining the required environment variables, and verifying the GeneMark license file. This ensured that all dependencies required by BRAKER2 were available before running structural annotation.

#### Representative command
```
export GENEMARK_PATH=~/gmes_linux_64_4
export PATH=$GENEMARK_PATH:$PATH
export GM_KEY_PATH=$HOME/.gm_key
```
#### Representative Screenshot

Figure 1. Configuration of the GeneMark-ES environment required for BRAKER2 structural genome annotation.

![BRAKER2](images/gm_key.png)

#### Interpretation

The GeneMark software was successfully configured by defining the executable path and license file, enabling BRAKER2 to access GeneMark during gene prediction.

## Step 2 – Prepare Protein Evidence
### Methodology
Protein sequences from several related fungal species were collected, combined into a single FASTA file, and cleaned to remove unnecessary header information. The resulting protein database served as external evidence for BRAKER2 gene prediction.

#### Representative commands
```
cat *.fasta > all_proteins_combined.fasta
```
#### Representative Screenshot

Figure 2. Preparation of homologous protein evidence by combining protein datasets from related fungal species and standardizing FASTA headers before structural genome annotation.

![BRAKER2](images/clean_protein.png)
