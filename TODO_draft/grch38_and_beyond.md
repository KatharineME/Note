+++ 
date = "2022-10-05"
title = "GRCH38 and Beyond"
slug = "grch38_and_beyond"
tags = []
categories = []
+++

From [this paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6699627/#SD1) on benchmarking practices:

"Moving forward, groups will also need to modify benchmarking strategies to address changes in the way the human genome itself is represented. Today the most common way of representing the human genome involves a set of linear chromosomes (e.g., the most common usage of GRCh37). There are key advantages to non-linear representations of the genome, including ability to recognize copy-number and other polymorphisms directly in the reference, and as a result more graphical structures are in development.25 The GRCh38 build of the human genome makes a key step towards this with its use of ALT loci, which provide multiple distinct versions of specific regions of the genome.26 These ALT loci are not well-accounted for by most aligners or the benchmarking tools we describe, and their impact on benchmarking studies is largely unexplored and likely would require a variety of samples with differing ALT alleles. It is likely that the core representation of the genome will continue to evolve over time, and benchmarking tools will continue to evolve."

In this video we are going to talk about reference genomes. We'll cover the most popular reference genomes,how they were made, their limitations, and what changes we can expect in the future.

Why do we need a reference?
First, to review, the existence of a reference genome allows us to summarize a person by simply listing the 4 to 5 million places where they are different from the reference, instead of listing all 3.2 billion of their bases. Which is a massive convenience. However, since the reference is what everybody is comparing against, creating and maintaining the reference genome is a big responsibility.

History of the reference genome
still maintained by four organizations – the National Center for Biotechnology Information (NCBI), the Genome Institute at Washington University, the Wellcome Trust Sanger Institute, and the European Bioinformatics Institute – that were leading players in the original Human Genome Project. These organizations make up the GRC/ or genome reference consortium, which is responsible for keeping the reference genome at the forefront of accuracy and completeness, and is still plugging gaps and making refinements over a decade after the Human Genome Project was declared complete.

Most popular reference genomes, their names, whats equivalent to what, and how they are made.
WHo is in charge of creating and maintaining reference genomes?

NCBI36 from March 2006
grch37 released in march 2009
grch38 released in december 2013

grch37 and hg19 are the same

ghrch38 versus grch37

- Repair incorrect reads (Build 38 alters around 8,000 single nucleotides across the genome, which “in many cases will improve the annotation or analysis of clinically relevant genes,” )
- inclusion of model centromere sequences
- addition of alternate loci (Build 38 contains 261 alternate loci across 178 regions – one of which, the KIR locus involved in the differentiation of natural killer cells, has been given 35 distinct sequences.)
- GRCh38 also adds new sequences to help fill those pieces of the genome that have never been fully captured

This is the first reference to have centromere sequences. however there are still gaps present in the genome.

One novel resource that GRCh38 takes advantage of is a hydatidiform mole, a type of abnormal pregnancy that occurs when a sperm fertilizes an enucleated egg. That sperm then reduplicates its DNA, resulting in two identical copies of each chromosome. Since the resulting cell has no allelic variation, it can be used to generate unambiguous reads in regions of very high diversity.

The biggest leap in coverage in GRCh38 comes from the centromeres, which are modeled for the first time in the reference genome’s history. Once thought to be important only during cell division, centromeres have generated new interest as the evidence mounts that their sequences may have real impacts on function.

applied a new analytical approach to create estimated centromere sequences. Centromeres are defined largely by very slight variations on a massively repeated 171-base sequence

GRC did incorporate some changes to the MUC5AC gene based on a PacBio sequence posted in GenBank. As more sequences generated on long-read technology are placed in public repositories, the GRC will have more opportunity to benefit from advances in capturing these difficult regions.

CONVERSION TOOL from GRCH37 to GRCH38. MCBI remapping tool: https://www.ncbi.nlm.nih.gov/genome/tools/remap

Show a diagram of the life of reference genomes, as they progress from various sources.

Limitations of the latest and greatest reference. What regions we dont have sequence for, what was the sample pool for this genome.

## Future

GRC says they:

"have decided to indefinitely postpone our next coordinate-changing update (GRCh39) while we evaluate new models and sequence content from ongoing efforts to better represent the genetic diversity of the human pangenome, including those of the Telemore-to-Telomere Consortium and the Human Pangenome Reference Consortium"

## Notes To Be Integrated

Gencdoe is basically a collaborative project across nearly 10 univerisituies with a. Goal of annotating al proteins coding loci as well as psuedgenes. Ensemble has been using / relaying not heir annotations since 2009. Each gencode release corresponds to an ensembl release. At least for human and mouse, ensembl release is exactly the egencode release.

When aligning ran-see reads. It helps to have the genes annotations when the genome index is built because the aligner can use information about gene start and end sites and well as splice junctions to predict and map more accurately.

The “analysis set” which was coined by UCSC refers to grch38 or hg38 with the alternative contigs filtered out. This is one reason why Heng Li’s favorite genome is: GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.gz

And finally, YES your gtf needs to match the version and patch of the reference genome you are using. Ensembl which is one of the main producers of gene annotations (gifs) releases updates more frequently than arch. However, ensembl keeps up to date with all the arch patches. Therefore you just need to make are your reference genome patch matches the gif file you are using.

A particularly enlightening Boosters answer on this topic:
Both Ensembl version 74/75 and version 73 are based on GRCh37, and should also work fine with hg19 - the coordinates should correspond.
Each version of ensembl, which is a set of gene annotations, is based on a particular version of the genome sequence. But ensembl is updated far more regularly than the genome sequence is. There are new versions of Ensembl every six months if I remember correctly. Thus there is no "corresponding GTF" for a particular genome sequence version.
As for your inconsistent results - thats just what happens when annotation versions get updated. Transcript models are retired or added - thus transcripts that were expressed in 73 simply don't exist in 75. Not every change that is made is correct - some transcripts for which you might have very good evidence in your RNAseq, might be removed from one version to the next, but we have to assume the newest version is the best. In the course of a three year project you are going to go through 6 versions of ensembl, and things are going to change quite a bit - thats just the nature of the beast.
The current newest version of Ensembl annotations is version 85, which is based on the GRCh38 version of the sequence. Many people will tell you that you should be moving over to using GRCh38/hg38. However, as yet there aren't any RNA-seq tools that I am aware of that will cope with some of the new features of hg38, in particular the alternative contigs. Thus, if you want to use hg38, you will need to filter out the \_alt contigs before you map (or use the "analysis set" from the UCSC website, which already has these filtered out).
You would then be able to use Ensembl 85.”
