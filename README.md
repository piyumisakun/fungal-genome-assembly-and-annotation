# Fungal Genome Assembly and Annotation

## Overview
This repository presents a comprehensive bioinformatics workflow for fungal genome assembly, annotation, and laccase gene identification using Illumina paired-end sequencing data. The documented pipeline encompasses quality assessment, genome assembly, assembly quality evaluation, barcoding gene extraction and validation, repeat masking, structural and functional genome annotation, and conserved domain-based identification of laccase genes. The repository serves as a practical reference for implementing an end-to-end fungal genome analysis workflow using widely adopted bioinformatics tools and reproducible command-line methods.

---

## Objectives

- Perform quality assessment of Illumina sequencing reads.
- Assemble the fungal genome from Illumina paired-end sequencing data using de novo assembly methods and improve assembly continuity through scaffolding.
- Assess genome assembly quality using multiple evaluation tools.
- Extract and validate fungal DNA barcoding genes.
- Identify and mask repetitive genomic regions.
- Predict protein-coding genes.
- Functionally annotate predicted proteins.
- Identify and characterize laccase genes using conserved domain analysis.
---

## Bioinformatics Workflow

```text
Raw Illumina Reads
        │
        ▼
01 Quality Control
        │
        ▼
02 Genome Assembly
        │
        ▼
03 Genome Assembly Assessment
        │
        ▼
04 Barcoding Gene Extraction and BLAST Validation
        │
        ▼
05 Repeat Masking and Soft Masking
        │
        ▼
06 Structural Genome Annotation
        │
        ▼
07 Functional Genome Annotation
        │
        ▼
08 Laccase Gene Identification
```
---
## Repository Structure

```text
fungal-genome-assembly-and-annotation/
│
├── 01_Quality_Control/
├── 02_Genome_Assembly/
├── 03_Genome_Assembly_Assessment/
├── 04_Barcoding_Gene_Extraction_and_BLAST_Validation/
├── 05_Repeat_Masking_and_Soft_Masking/
├── 06_Structural_Genome_Annotation/
├── 07_Functional_Genome_Annotation/
├── 08_Laccase_Gene_Identification/
└── README.md
```
---
## Workflow Summary
| Step | Description                                                                |
| ---- | -------------------------------------------------------------------------- |
| 01   | Quality assessment of Illumina sequencing reads using FastQC               |
| 02   | De novo genome assembly, scaffolding, filtering of short contigs, and genome polishing |            
| 03   | Genome assembly quality assessment using QUAST, BUSCO, BBMap, Mosdepth, and MultiQC for comparative analysis of multiple assembly versions|
| 04   | Extraction and validation of fungal DNA barcoding genes                    |
| 05   | Identification and masking of repetitive genomic regions                   |
| 06   | Structural genome annotation using BRAKER2                                 |
| 07   | Functional annotation using InterProScan                                   |
| 08   | Identification and characterization of laccase genes                       |
---
## Software and Versions
| Software      | Version                            |
| ------------- | ---------------------------------- |
| FastQC        | 0.12.1                             |
| MultiQC       | 1.31                               |
| SPAdes        | 3.15.5                             |
| SSPACE        | 2.1                                |
| QUAST         | 5.3.0                              |
| BUSCO         | 6.0.0 / 5.5.0 / 5.4.4                     |
| BBMap         | 39.06                              |
| Mosdepth      | 0.3.10                             |
| RepeatModeler | 2.0.7                              |
| RepeatMasker  | 4.2.2                              |
| BRAKER2       | 2.1.6                              |
| AGAT          | 0.8.0                              |
| InterProScan  | 5.76-107.0                         |
| HMMER         | 3.4                                |
| Seqtk         | 2.10.1                             |
| Pilon         | 1.24                               |
| Blastn        | 2.16.0+                            |
---
## Skills Demonstrated

- Linux command-line bioinformatics
- Genome assembly and polishing
- Genome quality assessment
- Repeat identification and masking
- Structural genome annotation
- Functional protein annotation
- Protein domain analysis using HMMER and Pfam
- DNA barcoding gene extraction and validation
- Sequence manipulation using Seqtk
- Bioinformatics workflow documentation using GitHub
---
## References

- Andrews, S. (2010). *FastQC: A Quality Control Tool for High Throughput Sequence Data*. https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

- Bankevich, A., et al. (2012). *SPAdes: A New Genome Assembly Algorithm and Its Applications to Single-Cell Sequencing*. Genome Research, 22(5), 455–477.

- Gurevich, A., et al. (2013). *QUAST: Quality Assessment Tool for Genome Assemblies*. Bioinformatics, 29(8), 1072–1075.

- Manni, M., et al. (2021). *BUSCO Update: Novel and Streamlined Workflows Along With Broader and Deeper Phylogenetic Coverage for Scoring of Eukaryotic, Prokaryotic, and Viral Genomes*. Molecular Biology and Evolution, 38(10), 4647–4654.

- Flynn, J. M., et al. (2020). *RepeatModeler2 for Automated Genomic Discovery of Transposable Element Families*. Proceedings of the National Academy of Sciences, 117(17), 9451–9457.

- Smit, A. F. A., Hubley, R., & Green, P. *RepeatMasker*. https://www.repeatmasker.org/

- Brůna, T., Hoff, K. J., Lomsadze, A., Stanke, M., & Borodovsky, M. (2021). *BRAKER2: Automatic Eukaryotic Genome Annotation with GeneMark-EP+ and AUGUSTUS Supported by a Protein Database*. NAR Genomics and Bioinformatics, 3(1), lqaa108.

- Jones, P., et al. (2014). *InterProScan 5: Genome-Scale Protein Function Classification*. Bioinformatics, 30(9), 1236–1240.

- Eddy, S. R. (2011). *Accelerated Profile HMM Searches*. PLOS Computational Biology, 7(10), e1002195.
