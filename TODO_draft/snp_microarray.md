+++ 
date = "2022-03-20"
title = "SNP Microarray"
slug = "snp_microarray"
tags = []
categories = []
+++

Here I'm going to talk about what a SNP microarray is and how it works with a simple example.

A SNP microarray is a method of checking the allele for hundreds of thousands of SNPs in a single sample or group of samples with. Most SNP microarrays cost around 100 to 200 dollars and are around 99% accurate.

The SNP microarray is a 10 centimeter slide with hundreds of thousands of tiny wells. Each well has a bead in it. Stuck on the surface of the beads are many copies of the same oligonucleotide probe dna sequence. This sequence stops right before a base of interest, which is the location of a particular SNP. In this picture I'm just drawing one oligonucleotide probe for simplicity. The sample DNA is broken into small pieces and washed over the microarray. When the sample DNA complements the oligonucleotide probe, it binds to it. Then when the microarray is washed with the four labeled bases, A, T G, and C, the base that complements the sample DNA is added. A laser then excites the labeled nucleotide, emitting a signal of a particular color. That color identifies the base which was added, which is the sample's allele at this SNP.

Lets look at an example. For the SNP in this example the possible alleles are A and G. Therefore, based on what alleles the sample has there are three possible outcomes. The first possible outcome is that the sample dna is homozygous for A at this SNP. In that case, after the sample DNA is washed over the microarray, and the sample's complement sticks to the oligonucleotide probe DNA sequence leading up to this SNP. Then the labels nucleotides are washed over the microarray and adenine is added. The laser excites the label on the adenine and a yellow signal is produced. Remember, even though I'm only showing one oligonucleotide probe here , there are actually hundreds of this exact probe on this bead. So you can imagine 100s of instances of this reaction occurring simultaneously, producing a strong yellow signal.

The second possibility is that the sample is homozygous for G at this SNP. In that case, once the sample's complement DNA leading up to SNP location is bound, a G is added. After being excited by the laser, a blue signal is produced, which again is amplified by this same reaction happening hundreds of times across the surface of this bead.

The third possibility is that the sample is heterozygous and has a genotype of AG. In this case, some of the hundreds of oligonucleotide probes would had an adenine added, and some would have a Guanine added. This would happen of course because the sample DNA has roughly equal amounts of DNA fragments complementing each, in this case roughly equal amounts of T and C at this location in the sample genome. Therefore when the laser excites the nucleotide labels, half of the signal will be yellow and half will be blue, resulting in a green signal, indicating the heterozygous genotype AG.

And this is happening simultaneously on each of the hundreds of thousands of beads on the SNP microarray. In the end, the allele at each locus is known with some level of confidence. That level of confidence varies depending on the strength and clarity of the light signal.

When it comes down to it, performing a microarray assay is a bit like dna fishing. It works great for summarizing a individual or sample in the space of known variants.

If you still have questions comment below and I will actually respond. If this was helpful, please like and subscribe. Have a healthy happy day everyone.
