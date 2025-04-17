+++ 
date = "2020-12-03"
title = "Strelka"
slug = "strelka" 
tags = []
categories = []
+++

Germline and somatic variant caller made by Illumina. All methods are optimized by default for whole genome DNA-Seq. RNA-Seq is still in development and not fully supported. For best somatic indel performance, Strelka is deisgned to be run with Manta. Somatic calls require a matched normal sample to make variant calls. The matached normal sample helps Strelka identify the germline variants versus somatic variants.

## Manta

A structural variant and indel caller. Manta provides additional indel candidates to Strelka up to a given maximum indel size (49 by default).

## Input

Strelka accepts BAM or CRAM. Reads lengths above 400bp have not been tested. The default settings in all workflows assume a whole genome DNA-Seq analysis. Input other than paired-end reads are ignored by default.

- All input alignment and reference sequence files must contain the same chromosome names in the same order.
- Alignments cannot contain the "=" character in the SEQ field.
- RG (read group) tags are ignored -- each alignment file must represent one sample.
- Alignments with basecall quality values greater than 70 will trigger a runtime error (these are not supported on the assumption that the high basecall quality indicates an offset error)

## Output

Put in `output/strelka/results/variants/`.

VCF 4.1 format.

Germline analysis is reported to the following variant files:

- variants.vcf.gz

  - This describes all potential variant loci across all samples. Note this file includes non-variant loci if they have a non-trivial level of variant evidence or contain one or more alleles for which genotyping has been forced. Please see the multi-sample variants VCF section below for additional details on interpreting this file.

- genome.S${N}.vcf.gz
  This is the genome VCF output for sample ${N}, which includes both variant records and compressed non-variant blocks. The sample index, ${N} is 1-indexed and corresponds to the input order of alignment files on the configuration command-line.

Somatic analysis provides somatic variants in the following two files:

- somatic.snvs.vcf.gz
  - All somatic SNVs inferred in the tumor sample.
- somatic.indels.vcf.gz
  - All somatic indels inferred in the tumor sample.

## Run

Strelka is run in a two step procedure: 1) configuration and 2) execution.

In the configure step, you pass in the alignment file, reference data, output directory, and more like this:

```sh
{STRELKA_INSTALL_PATH}/bin/configureStrelkaGermlineWorkflow.py \
  --bam NA12878.bam \
  --referenceFasta hg19.fa \
  --runDir ${STRELKA_ANALYSIS_PATH}
```

which creates a `runWorkflow.py` script with those settings in `output/strelka/` using Pyflow. `runWorkflow.py` is then run in the execution step where you can pass in parameters like number of jobs:

```sh
{STRELKA_ANALYSIS_PATH}/runWorkflow.py -m local -j 8
```
