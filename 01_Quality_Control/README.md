# Quality Control

## Overview

This section describes the quality assessment of raw Illumina paired-end sequencing reads of _Perenniporia cf. tephropora_ DD18 prior to genome assembly.

---

## Objective

- Assess the quality of raw Illumina sequencing reads.
- Identify potential sequencing issues.
- Evaluate base quality, GC content, sequence duplication, and adapter contamination.
- Ensure sequencing data are suitable for downstream genome assembly.

---

### Input Data

- Forward Illumina paired-end reads (FASTQ)
- Reverse Illumina paired-end reads (FASTQ)

---

### Methodology

Raw Illumina paired-end sequencing reads were analyzed using FastQC to evaluate sequencing quality before genome assembly. The analysis included per-base sequence quality, GC content, sequence duplication levels, adapter contamination, and overrepresented sequences.

---

#### Representative Command

```bash
fastqc DD18_trim_1.fastq.gz DD18_trim_2.fastq.gz
```
#### Representative Screenshot

The screenshot below shows the successful execution of FastQC on the forward (DD18_trim_1.fastq.gz) and reverse (DD18_trim_2.fastq.gz) Illumina paired-end sequencing reads.

![FastQC execution](images/Fastqc.png)

---

## Output

### Quality Assessment Summary
| Quality Metric | Forward Reads (R1) | Reverse Reads (R2) | Interpretation |
|---------------|:------------------:|:------------------:|----------------|
| Basic Statistics | ✅ Pass | ✅ Pass | Sequencing completed successfully |
| Per-base Sequence Quality | ✅ Pass | ✅ Pass | High-quality reads suitable for downstream analysis |
| Per-tile Sequence Quality | ✅ Pass | ⚠️ Warning | Minor tile variation observed in R2; acceptable for downstream analysis |
| Sequence Length Distribution | ✅ Pass | ✅ Pass | Uniform read length across sequencing reads |
| GC Content | ✅ Pass | ✅ Pass | GC distribution consistent with fungal genomic DNA |
| Adapter Content | ✅ Pass | ✅ Pass | No significant adapter contamination detected |
| **Overall Assessment** | ✅ Excellent | ✅ Good | Sequencing reads are suitable for genome assembly |

### Interpretation

The FastQC analysis demonstrated that both paired-end sequencing datasets were of high quality and suitable for downstream genome assembly. The forward reads (R1) passed all assessed quality metrics, while the reverse reads (R2) showed a warning for the *Per tile Sequence Quality* module. This warning is commonly associated with localized sequencing variation across flow-cell tiles and does not necessarily indicate poor overall read quality. The absence of significant adapter contamination, consistent sequence length distribution, and expected GC content further support the quality of the sequencing data for de novo genome assembly.



---


---

## References

Andrews, S. (2010). FastQC: A Quality Control Tool for High Throughput Sequence Data.
