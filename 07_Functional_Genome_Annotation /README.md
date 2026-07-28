# Functional Genome Annotation

## Overview
This repository documents the functional genome annotation workflow performed after structural genome annotation of Perenniporia cf. tephropora DD18. The BRAKER2 annotation was standardized using AGAT to generate consistent gene identifiers and extract coding and protein sequences. The predicted proteins were subsequently annotated using InterProScan to identify conserved protein domains, protein families, Gene Ontology (GO) terms, and biological pathways, enabling comprehensive functional characterization of the predicted proteome.

## Rationale
Functional annotation links predicted genes to known biological functions by identifying conserved protein domains, families, and Gene Ontology terms. Standardizing the structural annotation before functional analysis ensures consistent feature identifiers and improves compatibility with downstream bioinformatics analyses.

## Workflow
```
BRAKER2 Annotation
        │
        ▼
AGAT Standardization
        │
        ▼
Extract CDS & Protein Sequences
        │
        ▼
Clean Protein FASTA
        │
        ▼
InterProScan
        │
        ▼
Protein Domains
GO Terms
Protein Families
Pathways
```

## Step 1. Standardize BRAKER2 Annotation

### Purpose
Standardize the BRAKER2 GFF3 annotation by assigning consistent gene identifiers and validating the annotation structure for downstream analyses.

### Representative Commands
```
agat_sp_manage_IDs.pl \
  --gff braker.gff3 \
  --prefix DD18_ \
  -o braker_clean_fixed.gff3
```
