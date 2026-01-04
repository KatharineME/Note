+++
date = "2020-10-15"
title = "Sequence Analysis"
slug = "sequence-analysis"
tags = []
categories = []
+++

The human genome is 3.3 billion base pairs long.

75% of RNA-Seq sequences will be ribosomal.

Replicates

- Technical replicate: sequence same sample 2x
  - NGS technical replicates are usually 97% matching or better
- Biological replicate: different individual sequenced under same conditions

## Reference Genomes

- 2 major verisons
  - GRCh
    - NIH Genome Reference Consortium
    - Latest release is GRCh38
  - UCSC
    - Latest release is hg38
    - Previous release was hg19
    - After hg19 they decided to jump release number to match GRCh

## Decoy Genomes

When you sequence a human genome, you dont just get human DNA. If you sequence many human genomes, you begin to identify genomes of other organisms that live inside us. In order to conveniently identify and filter out these common non-human sequences, decoy genomes of these symbioitic viruses have been created that you can align to. Examples are:

- Epstein-Bar Virus
  - Causes mono
  - 50% of people have it
  - Stays in white blood cells for a long time
- Cytomegalovirus
  - 33% of people have it
  - Usually has no effect on health

Decoys can also be sets of human sequences that we currently don't know how to align. Some very long repetitive sequences are very hard to align especially when sequenced with short reads.

## General

- Variant calling algorithms have a larger effect on what variants are “found” than alignment algorithms.
- Novel variants, rare variants, and indels have more variance between pipelines than SNPs.
- Genomes of European descent are better represented in the public genome knowledge base and are therefore variant called more accurately. Genomes of African descent (where there is greater genetic diversity) , because they are more different from the human reference and are therefore more likely to have “rare” variants, are prone to more inaccurate variant calling.

## Cool Papers

- [Bioinformatics and Computational Tools for Next-Generation Sequencing Analysis in Clinical Genetics](https://www.mdpi.com/2077-0383/9/1/132)
- [Comparative analysis of whole-
  genome sequencing pipelines to
  minimize false negative findings](https://www.nature.com/articles/s41598-019-39108-2)
