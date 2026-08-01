# SSPACE Preprocessing

## Overview

This section describes the preprocessing steps performed before genome scaffolding with **SSPACE Basic v2.1**. These steps ensured that the paired-end sequencing reads were correctly formatted, computationally manageable, and properly configured for scaffold construction. The preprocessing workflow included FASTQ validation, splitting large sequencing files into smaller paired chunks, and automatic generation of the SSPACE library configuration file.

---

## Rationale

Paired-end sequencing reads require appropriate preparation before genome scaffolding with SSPACE. Preprocessing was therefore performed to verify the FASTQ files, manage large sequencing datasets, and prepare the library configuration required for paired-end read mapping and subsequent scaffolding. This ensured that the input data were correctly formatted and organized for the SSPACE scaffolding workflow.

---

## Methodology

Paired-end FASTQ files were first validated to confirm their format and integrity. Due to the large size of the sequencing datasets, the forward and reverse reads were divided into smaller paired chunks while maintaining synchronization between read pairs. The resulting files were then used to generate the library.txt configuration file required by SSPACE. Following preprocessing, the prepared paired-end read files and library information were used for Bowtie-based read mapping and subsequent genome scaffolding with SSPACE.

---

## Workflow

```text
Paired-end FASTQ Files
          │
          ▼
FASTQ Validation
          │
          ▼
Split Large FASTQ Files
          │
          ▼
Generate library.txt
          │
          ▼
Ready for Bowtie Mapping
```

---

# 1. FASTQ Validation

## Purpose

The paired-end sequencing files were verified before scaffolding to ensure that they were correctly formatted and suitable for downstream processing.

## Why was this step necessary?

- Verify FASTQ file integrity.
- Ensure paired-end read files were complete.
- Prevent errors during scaffolding.
- Confirm compatibility with downstream bioinformatics tools.

## Input

- `DD18_trim_1.fastq`
- `DD18_trim_2.fastq`

- Paired-end FASTQ files
        │
        ▼
Check file presence and size
        │
        ▼
Inspect representative FASTQ records
        │
        ▼
Confirm basic four-line FASTQ structure
        │
        ▼
Proceed to SSPACE preprocessing

## Output

Validated paired-end FASTQ files ready for preprocessing.

---

# 2. Split Large FASTQ Files

## Purpose

The paired-end FASTQ files were divided into smaller paired chunks to improve computational efficiency during genome scaffolding while maintaining paired-end read relationships.

## Why was this step necessary?

Large sequencing files require substantial memory during read mapping and scaffolding. Splitting the files into smaller paired chunks reduced memory requirements and enabled efficient processing while preserving the correct pairing of sequencing reads.

## Representative Command

```bash
split -l 40000 \
DD18_trim_1.fastq \
DD18_trim_1_chunk_ \
--numeric-suffixes=1 \
--suffix-length=2
```

The same procedure was repeated for the reverse paired-end FASTQ file.

## Representative Image

The following screenshot shows the paired-end FASTQ files being split into smaller chunks prior to SSPACE scaffolding.

![FASTQ splitting](images/Split.png)

## Output

- Forward read chunks
- Reverse read chunks

Example:

```text
DD18_trim_1_chunk_01
DD18_trim_1_chunk_02
...

DD18_trim_2_chunk_01
DD18_trim_2_chunk_02
...
```

---

# 3. Generate the SSPACE Library File

## Purpose

A Bash script was developed to automatically generate the `library.txt` configuration file required by SSPACE.

## Why was this step necessary?

The library file specifies how paired-end sequencing reads should be interpreted during scaffolding. Automating its generation reduced manual editing, minimized human error, and ensured that every forward read chunk was correctly paired with its corresponding reverse read chunk.

The script automatically:

- matched forward and reverse read chunks
- assigned unique library identifiers
- specified insert size
- defined insert size variation
- assigned forward-reverse (FR) read orientation
- generated a complete `library.txt` file for SSPACE

## Representative Workflow

```text
Forward Chunks
        │
        ├──────────────┐
        │              │
Reverse Chunks         │
        │              │
        ▼              │
Match Read Pairs       │
        │              │
        ▼              │
Generate library.txt ◄─┘
```
### Representative Command

```bash

counter=1

> /home/ubuntu/sspace_run/library.txt

for file1 in DD18_trim_1.chunk_*; do
    chunk_suffix=${file1#DD18_trim_1.chunk_}
    file2="DD18_trim_2.chunk_$chunk_suffix"

    if [ -f "$file2" ]; then
        echo "lib$counter /home/ubuntu/sspace_run/reads_new_chunks/$file1 \
/home/ubuntu/sspace_run/reads_new_chunks/$file2 350 0.25 FR" \
>> /home/ubuntu/sspace_run/library.txt

        ((counter++))
    fi
done
```
## Representative Screenshot

The following screenshot shows the execution of the Bash commands used to automatically generate the `library.txt` configuration file required by SSPACE.

![Library generation](images/Library.png)

## Output

```
library.txt
```

This file was subsequently used by **SSPACE** during genome scaffolding.

---

## Summary

The preprocessing workflow ensured that the paired-end sequencing data were correctly formatted, efficiently organized, and fully configured for genome scaffolding. These steps improved computational efficiency, minimized processing errors, and enabled reliable scaffold construction using SSPACE.
