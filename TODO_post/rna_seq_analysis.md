+++
date = "2021-05-05"
title = "RNA-Seq Analysis"
slug = "rna-seq-analysis"
tags = []
categories = []
+++

## DNA vs. RNA

{{< rawhtml >}}

<div style="text-align:center">
<img style="height: 250px;" src="/images/dna_vs_rna.jpg">
<p style="font-size:18%; color: #8f8f8f; margin: 0;">Photo credit to biologydictionary.net</p>
</div>
{{< /rawhtml >}}

To review, DNA and RNA are different molecules and hold different information. DNA is more or less fixed and consistent across all your cells, with the exception of de novo mutations (in skin cells from UV light for example), whereas RNA produced by the cell is changing all the time.

From acetylation to methylation, alternative splicing, and other regulatory mechanisms, our cells use many different strategies to specialize the way they transcribe and utilize the genome, resulting in different functions (why skin cells are different from heart cells).

{{< rawhtml >}}

<div style="text-align:center">
<img style="height: 220px;" src="/images/skin_heart_cell.png">
<p style="font-size:18%; color: #8f8f8f; margin: 0;">Photo credit to Biorender</p>
</div>
{{< /rawhtml >}}

RNA is like a snapshot of the cell. Results of RNA-Seq will be vastly different between cell types and over time in the same cell. For example, a cell might be fighting a pathogen or in the process of division, these will produce different results. Even you may give a different impression if someone meets you at work versus out at a bar. It's the same concept. Cell type dependent and time dependent.

In sum: DNA tells you what the cells are working with, RNA tells you what they're up to.

## RNA-Seq

Whether we are trying to figure out how cells respond to a drug or delineate the cell states of a tumor, RNA-Seq is currently the best way to assess the gene expression of cells. However, it's a bit of a misnomer because we don't actually sequence the RNA. We first convert it into cDNA, which is more stable, then sequence it. Let's break it down step by step.

#### Sampling

The main challenge with isolating RNA from a sample is it's instability. Compared to DNA, RNA is less stable because it is single stranded and because it has a reactive hydroxyl group (OH) on the second carbon of its sugar ring instead of a hydrogren.

However, assuming RNA was successfully isolated, what kind of RNA will be recovered? This is the typical breakdown:

- 85% is ribosomal RNA (rRNA)
- 10-12% is transfer RNA (tRNA)
- 2-3% is messenger RNA (mRNA)
- <3% is lncRNA, miRNA, ncRNA, and circRNA

Some researches are interested in studying the ribosomal RNA, in which case this is good. For researchers interested in different RNAs, enrichment techniques can be applied to increase those signals. See below.

#### Library Preparation

**1. Check for Degredation**

Can be done with RNA gel. If mRNA is degraded, special treatments need to be done to repair transcripts first.

**2. Enrich for mRNA**

- Poly-A Enrichment
  - Beads that bind to the poly A tails on mRNA trancripts and pull them down, isolating them from other RNAs.
  - Can't be used in prokaryotic cells becuase they typically don't have poly A tails on their mRNA transcripts.
- rRNA depletion
  - Selectively remove rRNA sequences.

**3. Convert to cDNA**

Why? Increases stability and its easier for sequencing.

**4. Fragment**

After conversion, fragments will be many different sizes, because transcripts vary in length. To fix this, we do fragmentation (enzymatic or via sonication). As usual, small biases in per base sequence content in the first ~10 bases can be introduced here.

Agilent bioanalyzer can tell you how efficient fragmentation was and the distribution of fragement size in your sample.

**5. Ligate Adapters**

**6. QC Library**

#### Sequencing

Recommended sequencing depth, read length, number of replicates, and single versus paired-end sequencing are all variables that change depending on the project goal. For example, in this image you can the consequences of varying sequencing depths.

{{< rawhtml >}}

<div style="text-align:center">
<img style="height: 320px;" src="/images/rna_seq_depth_consequences.png">
<p style="font-size:18%; color: #8f8f8f; margin: 0;">Photo credit to Iowa Institute of Human Genetics</p>
</div>
{{< /rawhtml >}}

In terms of read length, if the end goal is differential expression analysis or variant calling, 75 bp is a commonly used. This will lower the number of reads that flank splice-junctions which are more difficult to align.

Single-end is usually enough for differential expression analysis. De novo sequencing or splice variant anlaysis would require paried-end longer reads at greater depth.

## Alignment

This is where it gets interesting.

There is alignment and there is _pseudo-alignment_. Alignment is maping RNA-Seq reads to the human reference genome. After doing this you can call variants or quantify expression. Pseudo-alignment maps RNA-Seq reads to a reference transcriptome. It can be used to quantify expression but not for variant calling. However, it's much faster and less computationally expensive than regular alignment. So how you align the reads depends on your goal.

**Pseudo (Transcriptome) Aligners**

- Salmon
- Kallisto

But here we'll focus on alignment to the reference genome.

Aligning cDNA to the genome is more difficult than aligning DNA to the genome becasue cDNA is missing introns and other regions that were spliced out when the RNA was made. Enter "splice-aware" aligners.

Splice aware aligner's are exactly what they sound like. They take into account that many of the reads will be non-contiguous and that most of sequence will be exonic. Here are some splice-aware aligners:

**Splice-Aware (Genome) Aligners**

- STAR
  - https://github.com/alexdobin/STAR
- HISAT2
- Minimap2
  - https://academic.oup.com/bioinformatics/article/34/18/3094/4994778
  - https://github.com/lh3/minimap2
- TopHat2

Stats

- There are ~7-8 exons per gene on average.
- The average exon size in eukaryotes is 150bp.
- About 80% of exons are less than 200bp in length.

## Normalization

**1. The number of reads per sample**

Without this normalization, you would think the gene expression of sample B is double the gene expression of sample C. Wherein reality, their gene expression maybe almost identical.

{{< rawhtml >}}

<div style="text-align:center">
<img style="height: 220px;" src="/images/sample_depth.png">
<p style="font-size:18%; color: #8f8f8f; margin: 0;">Photo credit to Biorender</p>
</div>
{{< /rawhtml >}}

**2. The number of reads per gene**

Genes vary in length. If you counted the number of reads mapped to genes A and B below, you would think gene B has twice the expression of gene A. Normalizing by gene length sovles this problem.

{{< rawhtml >}}

<div style="text-align:center">
<img style="height: 250px;" src="/images/genes_different_length.png">
</div>
{{< /rawhtml >}}

\_\_3. The outlier genes that skew expression of all other genes

#### Common normalization methods

The methods below normalize by the number of reads per sample and by the number of reads per gene. They differ in the order of operations and few other details.

- RPKM (Reads Per Kilobase Million)
  - For single end RNA-Seq
  - Steps for a single sample:
    - Step 1: Divide total number of reads by 1,000,000
      - We call this number the per million scaling factor
    - Step 2: Divide the read count for each gene by the per million scaling factor
      - We call this reads per million
    - Step 3: Divide the reads per million of each gene by the length of the gene in kilobases (divide by 1 if gene length is 1kb).
- FPKM (Fragments Per Kilobase Million)
  - For paired end RNA-Seq
  - It is the same as RPKM except that is avoids counting forward and reverse reads twice for a given fragment on the flow cell.
- TPM (Transcript Per Million)
  - Uses the same steps as RPKM but in a different order.
  - Steps for a single sample:
    - Step 1: Divide the read count for each gene by the length of the gene in kilobases (divide by 1 if gene length is 1kb).
      - We call this reads per kilobase
    - Step 2: Calculate total reads per kilobase
    - Step 3: Divde total reads per kilobase by 1,000,000

#### Normalization Programs

DESeq2

- Adjusting for differences in library size (all mehtods above deal with this)
- Adjusting for differences in library composition
  - When one sample has a gene that is transcribed a lot and the other sample doesnt transcribe that gene at all, the other samples makes the expression of all other genes artificially high to make up for this.
- Steps for a single sample:
  - Calculate scaling factor for each sample
    - Takes the log base e of all the count values
    - Average each gene across all samples
    - Filter out genes with infinity averages (these genes that for one or more samples, had zero expression). This causes scaling factors to be based on house keeping genes (genes transcribed ar similar levels regardless of tissue type).
    - Subtract the average log value from the log(counts) for each gene of each sample
    - Calculate the median value for each sample.
    - Raise e to the median value for each sample to get the scaling factor for each sample.

edgeR

#### Whats the best method to use?

Probably TPM. Here's why.

Lets say we performed RNA-Seq on three samples (each sample gets one pie in the pictures below).

**RPKM**

If we normalize our data with RPKM, for each sample, the total RPKM is different. As you can see below, One sample has a total RPKM of 10, while the other two are 8 and 9. Because of this, we can't directly compare the RPKM of Gene B across the three samples. For example if we compare the RPKM of Gene B in samples 1 and 2, we would think it's the same, even though it accounts for a larger percentage of total expression in sample 2.

{{< rawhtml >}}
<br>

<div style="text-align:center">
<img style="height: 280px;" src="/images/rpkm.png">
</div>
{{< /rawhtml >}}

**TPM**

If we normalize our data with TPM, for each sample, the total TPM is the same. That means that when Gene B's TPM is greater in sample 2 than sample 1, its expression is actually greater in sample 2 than sample 1.

{{< rawhtml >}}
<br>

<div style="text-align:center;">
<img style="height: 280px;" src="/images/tpm.png">
</div>
{{< /rawhtml >}}

Joshua Starmer has [a fantastic video](https://www.youtube.com/watch?v=TTUrtCY2k-w&t=495s) on these methods if you're looking for more.
