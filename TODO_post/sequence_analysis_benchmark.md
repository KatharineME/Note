+++ 
date = "2020-10-02"
title = "Sequence Analysis Benchmark"
slug = "sequence-analysis-benchmark" 
tags = []
categories = []
+++

## Precision FDA Truth Challenge

- Genome in Bottle’s NA12878 also known as HG001.
- Used to develop the actual tools (overfitted to analyzing this sample).
- So using this sample to test for accuracy and quality is too easy.
- The NA24285 sample known as HG002 (the son of an Ashkenazi trio) was about to be released from GiaB (genome in a bottle) as another truth dataset. So this challenge took advantage of that HG002 to assess SA pipelines.
- HG002 and HG001 were sequenced under similar conditions and instruments at the same site.
  HG001 is female, HG002 is male.
- People doing the challenge would run their SA pipeline on HG001 and HG002 FASTQ files provided, and submit the resulting VCF files as entry into the challenge.
- When the challenge was over GiaB published the truth VCF for HG002.
- Some pipelines are gender aware for the x chromosome and won’t call an snp on chromosome x heterozygous for an XY male, but other tools will (which is incorrect).
- They used Real Time Genomics’ vcfeval and Illuminas hap.py to compare vcfs.
- Vcfeval generates an intermediate vcf and hap.py counts indels and snp.
- The ga4gh benchmarking workflow that the Precision FDA Truth Challenge developed is [here](https://github.com/ga4gh/benchmarking-tools/tree/master/doc/ref-impl)
- In the `tools` folder of G4agh's benchmarking repository, there are pointers to specific versions of vcfeval and hap.py that were used in the Precision FDA Truth Challenge.
- [Precision FDA Truth Challenge site](https://precision.fda.gov/challenges/truth)

## Genome in a Bottle (Giab)

- NA12878

  - Pilot genome
  - Female, caucasian ancestry
  - From CEPH Utah Reference Collection
  - 6 vials of DNA (ua0-ua5)
  - 14 libraries prepared in parallel using Illumina TruSeq (LT) PCR Free Kit
  - 3 libraries were made from first and last tubes
  - 2 libraries were made from others
  - Fragmented with sonication, size selected with beads, sequenced on HiSeq 2500 2x148 PE reads

- Chinese Han family trio
  - Son was sequenced at 100X, others were sequenced at 300X
  - Son is what giab recommends using at this time, because its being actively updated
- Ashkenazi Jew family trio
- [Giab benchmark data paper](https://www.nature.com/articles/sdata201625)

## Running the Benchmark

-vcfeval - vcfeval is made by Real Time Genomics - It comes as one utility in a utility package called [RTG Tools](https://github.com/RealTimeGenomics/rtg-tools) - Overall it performs sophisticated comparisons of VCF files - It performs variant comparisons at the haplotype level. It determines whether or not the genotypes asserted by the VCFs result in the same genomic sequence when applied to the reference. In other words, vcfeval evaluates the VCFs its comparing in order to compare the actual sequences. - It is supposedly the fastest tool to perform this kind of anlaysis as of 20201211. - It outputs VCF files containing the results of the comparison as well as summary metrics and ROC curve files.
