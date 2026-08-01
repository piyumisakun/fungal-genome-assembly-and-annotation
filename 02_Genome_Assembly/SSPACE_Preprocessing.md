# SSPACE Preprocessing

## Overview

This section describes the preprocessing steps performed before genome scaffolding with **SSPACE Basic v2.1**. These steps ensured that the paired-end sequencing reads were correctly formatted, computationally manageable, and properly configured for scaffold construction. The preprocessing workflow included FASTQ validation, splitting large sequencing files into smaller paired chunks, and automatic generation of the SSPACE library configuration file.

---

## Rationale

Paired-end sequencing reads require appropriate preparation before genome scaffolding with SSPACE. Preprocessing was therefore performed to verify the FASTQ files, manage large sequencing datasets, and prepare the library configuration required for paired-end read mapping and subsequent scaffolding. This ensured that the input data were correctly formatted and organized for the SSPACE scaffolding workflow.

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

### 1. FASTQ Validation

#### Purpose

To verify the presence and basic formatting of paired-end FASTQ files before preprocessing and genome scaffolding.
- Confirm the presence of both paired-end sequencing files.
- Check that the FASTQ files are non-empty.
- Inspect representative records for the expected FASTQ structure.
- Confirm that the files are appropriately formatted for subsequent preprocessing.

#### Input

- `DD18_trim_1.fastq`
- `DD18_trim_2.fastq`

#### Methodology

The FASTQ Validation process was performed using the following workflow:

- Check the presence and file size of paired-end FASTQ files.
- Inspect representative FASTQ records.
- Confirm the expected four-line FASTQ structure.
- Confirm that the paired-end FASTQ files are appropriately formatted for subsequent preprocessing.
  
---

### 2. Split Large FASTQ Files

#### Purpose
To divide large paired-end FASTQ files into smaller paired chunks for efficient processing while maintaining the correct pairing of sequencing reads.

- Reduce computational demands when processing large sequencing files.
- Facilitate efficient paired-end read mapping during scaffolding.
- Preserve the correct relationship between forward and reverse reads.

#### Representative Command

```bash
split -l 40000 \
DD18_trim_1.fastq \
DD18_trim_1_chunk_ \
--numeric-suffixes=1 \
--suffix-length=2
```

The same procedure was repeated for the reverse paired-end FASTQ file.

#### Representative Image

The following screenshot shows the paired-end FASTQ files being split into smaller chunks prior to SSPACE scaffolding.

![FASTQ splitting](images/Split.png)

#### Methodology

The FASTQ files were split using the following workflow:

- Divide the forward and reverse FASTQ files into smaller chunks.
- Maintain synchronization between corresponding paired-end reads.
- Generate paired FASTQ chunks of manageable size.
- Use the resulting paired chunks for downstream read mapping and SSPACE scaffolding.

#### Output

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

### 3. Generate the SSPACE Library File

#### Purpose

To automatically generate the `library.txt` configuration file required by SSPACE for paired-end read mapping and genome scaffolding.

- Ensure that each forward read chunk is correctly paired with its corresponding reverse read chunk.
- Define the library parameters required by SSPACE.
- Reduce manual editing and minimize errors when preparing the library configuration file.

#### Methodology

A Bash script was used to automatically generate the SSPACE `library.txt` file using the following workflow:

- Match corresponding forward and reverse FASTQ chunks.
- Assign unique library identifiers to each read pair.
- Specify the insert size and insert-size variation.
- Define the forward-reverse (FR) read orientation.
- Generate a complete `library.txt` configuration file for SSPACE.
  
#### Representative Workflow

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
#### Representative Command

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
#### Representative Screenshot

The following screenshot shows the execution of the Bash commands used to automatically generate the `library.txt` configuration file required by SSPACE.

![Library generation](images/Library.png)

#### Output

```
library.txt
```

This file was subsequently used by **SSPACE** during genome scaffolding.

---

## Summary

The preprocessing workflow ensured that the paired-end sequencing data were correctly formatted, efficiently organized, and fully configured for genome scaffolding. These steps improved computational efficiency, minimized processing errors, and enabled reliable scaffold construction using SSPACE.
