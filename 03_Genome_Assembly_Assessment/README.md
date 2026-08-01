# Genome Assembly Assessment

## Overview

Evaluate the quality, completeness, and reliability of the assembled fungal genome before genome annotation and downstream comparative analyses.

---

## Rationale

Following genome assembly, the quality of the assembled genome must be evaluated to determine whether it is suitable for downstream analyses. In this study, four different genome assemblies generated during the assembly workflow were assessed and compared to identify the most reliable assembly.

Multiple complementary assessment tools were used to evaluate different aspects of assembly quality, including contiguity, completeness, read support, and sequencing depth.

The assessment results were compared across all four genome assemblies to identify the most complete, contiguous, and accurate assembly for downstream analyses, including genome annotation, gene prediction, and laccase gene characterization etc.

---

## Bioinformatics Workflow

```
    Assembled genome
            │
            ▼
        QUAST
 (Assembly Statistics)
            │
            ▼
        BUSCO
 (Genome Completeness)
            │
            ▼
        BBMap
(Read Mapping Assessment)
            │
            ▼
      Mosdepth
 (Coverage Analysis)
            │
            ▼
       MultiQC
(Integrated Quality Report)
```

---

## Assessment Tools

### QUAST

#### Purpose
QUAST was used to evaluate assembly contiguity and fragmentation using metrics such as N50, L50, total assembly size, GC content, and number of contigs.

#### Methodology
QUAST v5.3.0 was used to assess four genome assemblies generated during the assembly workflow. The resulting metrics were compared to identify the assembly with the best structural quality.

#### Representative command
```bash
quast.py \
~/spades.fasta \
~/sspace.fasta \
~/sspace1000.fasta \
~/pilon.fasta \
-o ~/quast_comparison
```
#### Representative Screenshot

The screenshot below shows the execution of the QUAST command used to compare four genome assemblies and generate assembly quality statistics.

![QUAST summary](images/quast_summary.png)

#### Results
| Metric | SPAdes | SSPACE | Filtered (>1000 bp) | Pilon |
|:-------|-------:|--------:|--------------------:|------:|
| Genome size (Mb) | 60.41 | 60.41 | 55.59 | 55.59 |
| Total contigs | 15,627 | 15,627 | 8,289 | 8,289 |
| Largest contig (bp) | 133,223 | 133,223 | 133,223 | 133,223 |
| GC (%) | 54.88 | 54.88 | 54.85 | 54.85 |
| N50 (bp) | 10,705 | 10,705 | **11,930** | 11,927 |
| L50 | 1,540 | 1,540 | **1,326** | 1,326 |

#### Interpretation
Comparison of the four genome assemblies showed that the initial SPAdes and SSPACE assemblies produced similar assembly statistics. Filtering scaffolds shorter than 1000 bp substantially reduced assembly fragmentation and improved continuity, as indicated by an increased N50 (10,705 to 11,930 bp) and a decreased L50 (1,540 to 1,326), while maintaining a stable GC content. These results indicate that scaffold filtering improved assembly quality without altering the overall genome composition.

#### Conclusion
The QUAST analysis demonstrated that filtering scaffolds shorter than 1000 bp substantially improved assembly continuity while maintaining genome composition. The filtered assembly was therefore selected for downstream genome annotation and subsequent analyses.

---
### MultiQC

The MultiQC report summarized the QUAST contig size distribution across the genome assemblies. The results showed that filtering scaffolds shorter than 1000 bp substantially reduced the number of short contigs while retaining longer contigs, indicating improved assembly continuity. The scaffolded assembly exhibited a similar contig size distribution to the SPAdes assembly prior to filtering, demonstrating that the primary improvement resulted from removing fragmented sequences rather than scaffolding alone.

#### Representative Screenshot

The screenshot below shows MultiQC visualization of QUAST contig size distribution across three genome assemblies. (A) SPAdes assembly, (B) SSPACE assembly after filtering scaffolds shorter than 1000 bp, and (C) SSPACE scaffolded assembly before filtering. The figure illustrates the reduction in short contigs following filtering while preserving longer contigs.

![MultiQC](images/MultiQC.png)

---

### BUSCO

#### Purpose
BUSCO (Benchmarking Universal Single-Copy Orthologs) was used to assess genome assembly completeness by searching for highly conserved single-copy orthologs from the Basidiomycota lineage dataset, providing a standardized measure of the completeness of the assembled gene space.

#### Methodology
BUSCO v6.0.0 was used to evaluate the completeness of each assembled genome (SPAdes, SSPACE, SSPACE (>1000 bp),	Pilon-polished) using the Basidiomycota lineage dataset (`basidiomycota_odb10`). BUSCO searched the assembly for highly conserved single-copy orthologous genes and classified them as Complete (single-copy or duplicated), Fragmented, or Missing. The resulting completeness metrics were used to assess the quality of the assembled gene space and determine its suitability for downstream genome annotation.

#### Representative command
```bash
busco \
-i filtered_1000.fasta \
-l basidiomycota_odb10 \
-o busco_filtered_1000 \
-m genome \
--cpu 12
```
#### Representative Screenshot
The screenshot below shows the BUSCO v6.0.0 command used to assess genome assembly completeness using the Basidiomycota lineage dataset.

![BUSCO command](images/busco_command.png)

#### Results

##### BUSCO Completeness Comparison
| BUSCO Metric | SPAdes<br>(BUSCO 5.4.4) | SSPACE<br>(BUSCO 6.0.0) | Filtered (>1000 bp)<br>(BUSCO 6.0.0) | Pilon<br>(BUSCO 6.0.0) |
|:------------|:-----------------------:|:-----------------------:|:------------------------------------:|:----------------------:|
| Complete BUSCOs (C) | 603 (79.5%) | 1452 (82.3%) | 1437 (81.5%) | 603 (79.6%) |
| Complete and single-copy BUSCOs (S) | 376 (49.6%) | 878 (49.8%) | 874 (49.5%) | 349 (46.0%) |
| Complete and duplicated BUSCOs (D) | 227 (29.9%) | 574 (32.5%) | 563 (31.9%) | 254 (33.5%) |
| Fragmented BUSCOs (F) | 94 (12.4%) | 218 (12.4%) | 190 (10.8%) | 96 (12.7%) |
| Missing BUSCOs (M) | 61 (8.1%) | 94 (5.3%) | 137 (7.8%) | 59 (7.8%) |
| Total BUSCO groups searched (n) | 758 | 1764 | 1764 | 758 |
| Genes with internal stop codons (%) | – | 10.9% | 11.0% | 12.4% |
| Internal stop codons (count) | – | 158 | 158 | 75 |

#### Interpretation
The filtered genome assembly achieved 81.5% complete BUSCOs, including 49.5% single-copy and 31.9% duplicated orthologs. Only 10.8% of BUSCO genes were fragmented, while 7.8% were missing, indicating good representation of the conserved fungal gene space. Approximately 11.0% of complete BUSCOs contained internal stop codons, suggesting that a small proportion of predicted genes may require further refinement.

#### Conclusion
BUSCO analysis demonstrated that the filtered assembly contained the majority of conserved Basidiomycota genes with relatively low fragmentation, supporting its suitability for downstream genome annotation and functional analyses.

---

### BBMap

#### Purpose
BBMap was used to evaluate the reliability of the genome assembly by mapping cleaned paired-end Illumina reads back to the assembled genome. Read mapping statistics were used to assess assembly accuracy, mapping efficiency, pairing consistency, and sequencing error rates.

#### Methodology
BBMap was used to align cleaned paired-end reads to the SPAdes genome assembly. Mapping statistics, including mapping rate, properly paired reads, insert size distribution, substitution, insertion, and deletion rates, were evaluated to determine the quality and reliability of the assembled genome.

#### Representative command
```bash
bbmap.sh
ref=/home/ubuntu/contigs.fasta \
in1=/home/ubuntu/DD18_trim_1.fastq \
in2=/home/ubuntu/DD18_trim_2.fastq \
out=/home/ubuntu/mapped.sam \
covstats=/home/ubuntu/covstats.txt \
threads=16
```
#### Results

The key BBMap alignment statistics are summarized below.
| Metric | Value |
|--------|------:|
| Total reads | 25,346,186 |
| Mapped reads | 25,300,412 |
| Mapping rate | **99.82%** |
| Properly paired reads | **84.04%** |
| Average coverage | **50.11×** |
| Reference bases covered | **94.84%** |

#### Representative Screenshot
The screenshot below shows the BBMap alignment summary generated after mapping paired-end Illumina reads to the assembled genome. 

![BBMap results](images/bbmap_results.png)

#### Interpretation
The BBMap alignment results demonstrated excellent agreement between the paired-end Illumina sequencing reads and the assembled genome. As summarized in the table and supported by the detailed alignment report, **99.82%** of reads successfully mapped to the assembly, **84.04%** were properly paired, and the average sequencing depth was **50.11×**. Furthermore, **94.84%** of the reference genome was covered by mapped reads, indicating that the assembly is well supported by the sequencing data and is suitable for downstream analyses, including genome annotation, repeat analysis, and gene characterization.

#### Conclusion
BBMap confirmed that the assembled genome is highly consistent with the original sequencing reads. The high mapping rate, substantial genome coverage, and adequate sequencing depth indicate that the assembly is accurate and reliable for downstream bioinformatics analyses.

---

### Mosdepth

#### Purpose
Mosdepth was used to calculate sequencing depth and genome coverage by analyzing the alignment of Illumina paired-end reads to the assembled genome. Coverage statistics were used to evaluate whether the genome assembly had sufficient and uniform sequencing support for downstream analyses.

#### Methodology
The BBMap alignment file (mapped.sam) was converted to a sorted and indexed BAM file using Samtools. The sorted BAM file was then analyzed with Mosdepth to calculate genome-wide sequencing depth and coverage statistics. The resulting coverage profiles were used to assess average sequencing depth, genome coverage, and the distribution of read coverage across the assembled genome.

#### Representative command
```bash
mosdepth -t 4 mapped mapped.sorted.bam
```
#### Representative Screenshot
The screenshot below shows the workflow used to prepare the sorted BAM file and execute Mosdepth for genome-wide sequencing depth analysis.

![mosdepth command](images/mosdepth.png)

#### Results

#### Coverage per Contig (Overall)

![Coverage per Contig](images/coverage_plot.png)

**Figure 1.** Genome-wide mean sequencing coverage across all assembled contigs generated using Mosdepth. The plot shows the overall distribution of coverage, including a small number of high-coverage contigs.

---

#### Coverage per Contig (Zoomed View)

![Coverage per Contig (Zoomed)](images/coverage_plot_zoom.png)

**Figure 2.** Zoomed view of the coverage distribution (capped at 100×) highlighting the coverage pattern across the majority of assembled contigs. This view improves visualization by minimizing the influence of extreme high-coverage contigs.

#### Interpretation
The overall coverage plot indicates that most contigs were covered at moderate sequencing depths, while a small number of contigs exhibited substantially higher coverage. The zoomed view shows that the majority of contigs were covered within the expected range, consistent with the average sequencing depth of approximately 50×. These results indicate that sequencing coverage was generally sufficient and well distributed across the assembled genome, supporting the reliability of the assembly for downstream analyses.

#### GC Content vs Coverage

The figure below illustrates the relationship between GC content and mean sequencing coverage for assembled contigs. Each point represents an individual contig.

![GC vs Coverage](images/gc_vs_coverage.png)

#### Interpretation
No strong relationship was observed between GC content and sequencing depth. Most contigs clustered around 52–60% GC, with moderate sequencing coverage, indicating minimal GC-related sequencing bias. A few contigs showed unusually high coverage regardless of GC content, suggesting that these regions may represent repetitive elements or highly abundant sequences rather than GC-dependent coverage variation.


#### Interpretation

Mosdepth analysis demonstrated that the assembled genome achieved an average sequencing depth of approximately **50×**, with **94.84%** of reference bases covered by mapped reads. Coverage profiles indicated that most contigs had moderate and relatively uniform sequencing depth, while a small number of contigs exhibited exceptionally high coverage, likely corresponding to repetitive genomic regions. Furthermore, GC content analysis showed no strong relationship between GC percentage and sequencing coverage, suggesting minimal GC bias and providing additional support for the quality and reliability of the genome assembly.

### Conclusion

Mosdepth analysis demonstrated that the assembled genome was supported by high sequencing depth and broad read coverage. The average genome coverage (~50×) and extensive coverage across the reference genome indicate that the assembly is well supported by the sequencing data. Together with the BBMap mapping results, these findings provide strong evidence that the assembly is reliable and suitable for downstream analyses, including genome annotation, repeat identification, and gene characterization.

---

## Key Outcomes

- Assembly statistics were evaluated using QUAST.
- Genome completeness was assessed using BUSCO.
- High read mapping efficiency was confirmed using BBMap.
- Genome sequencing depth was estimated using Mosdepth.
- Quality assessment reports were summarized using MultiQC.
- The combined assessment demonstrated that the assembled genome was suitable for downstream genome annotation and comparative genomic analyses.

---

## Repository Structure

- **QUAST.md** – Assembly statistics assessment
- **BUSCO.md** – Genome completeness assessment
- **BBMap.md** – Read mapping assessment
- **Mosdepth.md** – Genome coverage analysis
- **MultiQC.md** – Integrated quality assessment report

---

## References

Gurevich, A., Saveliev, V., Vyahhi, N., & Tesler, G. (2013). QUAST: Quality Assessment Tool for Genome Assemblies. *Bioinformatics*, 29(8), 1072–1075.

Manni, M., Berkeley, M. R., Seppey, M., Simão, F. A., & Zdobnov, E. M. (2021). BUSCO Update: Novel and Streamlined Workflows Along with Broader and Deeper Phylogenetic Coverage for Scoring of Eukaryotic, Prokaryotic, and Viral Genomes. *Molecular Biology and Evolution*, 38(10), 4647–4654.

Bushnell, B. BBMap: A Fast and Accurate Short Read Aligner. Lawrence Berkeley National Laboratory.

Pedersen, B. S., & Quinlan, A. R. (2018). Mosdepth: Quick Coverage Calculation for Genomes and Exomes. *Bioinformatics*, 34(5), 867–868.

Ewels, P., Magnusson, M., Lundin, S., & Käller, M. (2016). MultiQC: Summarize Analysis Results for Multiple Tools and Samples in a Single Report. *Bioinformatics*, 32(19), 3047–3048.
