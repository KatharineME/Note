## Aim 1

Be the most reputable source of information for genomics consumers. Provide exactly the information they need to make educated consumer choices and how they can interpret results.

### Videos toward this aim

- Why does your genome sequence matter?

  - What value does it provide you?
  - How does is effect your family?
  - Why should it be kept private?
    - The more we learn about each base pair, each persons genome sequence will become exponentially more valuable and have more real life consequences

- Types of DNA tests - which one is right for you?

  - Compare wgs, wes, and genotyping
  - How they work roughly and pros and cons of each

- Whats the best way to sample dna?

  - Saliva, blood, cotton swab, or other?
  - Compare on cost, stability, accuracy, and implications for privacy

- The types of genomic data you need to have and what you can do with them

  - Your genome sequence is an asset. Just like you wouldnt buy a house and settle to only occupy one room, you should have your dna sequenced and settle to receive only part of your data
  - The advantages of having your raw reads: ability to re-analyze, transferable, protection of investment
  - View your BAM file in IGV
  - Use tabix to take a look at a section of your vcf

- How sequencing data is processed and interpreted

  - For a lay person

- History of direct to consumer genomics

  - Price of sequencing
  - Common consumer offerings over time
  - FDA reactions, Genetic nondiscrimination act, robert green reveal study on implications of learning about Alzheimer's susceptibility

- Your genotype is usually not a diagnosis
  - Commonly, the environment has a large impact on the manifestation of phenotypes.

## Aim 2

Be the most reputable and educational source for germline dna analysis. This is includes best practices for germline dna sequence analysis and variant interpretation. Later this will include how to make omics apps. The will support the founding of the omics apps system by setting a strong technical example.

### Videos toward this aim

Sequencing technologies series

- SNP microarrays - how they actually work

- Illumina next generation sequencing by synthesis explained

- Third generation sequencing: Is is the future?

Sequence analysis series (how we achieved the most accurate pipeline)

- The reference genome, is it really the average human?

  - How was the reference genome made
  - What are its limitations
  - Are these limitations real problems
  - What is being done about it

- How to pre-process reads. Is it necessary?

  - Describe types of read preprocessing: adapter trimming, quality trimming, hard trimming, removing pcr duplicates
  - Recommend settings for germline read length use case
  - Compare available trimmers on accuracy, speed, support, and momentum

- Which aligner and why?

  - Compare different germline aligners (short and long read) on their accuracy, speed, support, and momentum
  - Discuss important flags and options

- Types of variants and how we treat them

  - Describe snp, mnp, indel, translocation, inversion, etc
  - How do variant callers deal and call these different types of variants?

- How to call variants - the step that matters most

  - Why this step has the most weight in determining accuracy of the pipeline
  - Compare variant callers on their accuracy, speed, support, and momentum
  - Recommend variant caller and flags for germline use case

- Annotating variants

  - Compare annotation tools like snpeff, clinvar, etc
  - How to filter variants in terminal

- I checked my DNA for the top 5 most dangerous variants
  - Using genome explorer to check in real time
