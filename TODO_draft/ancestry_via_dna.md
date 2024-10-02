+++ 
date = "2020-10-15"
title = "Trace Ancestry Through DNA"
slug = "trace-ancestry-through-dna"
tags = []
categories = []
+++

Ever since the human genome was sequenced in 2000, we have been trying to use it to figure out where we came from.

However, this field is at the intersection of society-wide misunderstandings about race / ethnicity, all the problems that come with a new technology (genome sequencing), and a public shortage of sequencing data for diverse human genomes. The obstacles between us and the "truth" about our genetic heritage are currenlty great. But these obstacles will be slowly toppled by better technology, better bioinformatics, and more human genome sequences.

## Up to Now

So where are we now?

## Comprehensive Analysis of Genetic Ancestry and Its Molecular Correlates in Cancer Paper (2020)

They determined genetic ancestry using 5 different "methods".

Among the 9,257 patients for whom at least three methods provided base calls, 98.1 percent exhibited agreement and 99.7 percent of admixed patients exhibited condordance with prior ancestry assignments from Yuan et al 2018.

The data they used for ancestry classification was either SNP array and or whole-exome sequencing.

## Integrated Analysis of Genetic Ancestry and Genomic Alterations across Cancers (2018)

They aimed to estimate the genetic ancestry of each TCGA patient, including the 12% of TCGA samples that have no race / ethniticty classification, by applying multiple already made computational tools

EIGENSTRAT (Price et al., 2006) was applied to infer the global genetic structure of TCGA patients.

is a method to study human diversity based on Principal Component Analysis (PCA), which reduces the information contained in SNP frequencies to components that capture most genetic variability. The EIGENSOFT package (EIGENSTRAT algorithms included) version 6.1.4 was downloaded from GitHub (https://github.com/DReichLab/EIG).

The smartpca program was applied to run PCA on the combined genotype data of TCGA and the reference populations.

we adopted the default value K=10 for running the smartpca program.

Overall, the number of patients classified into the four genetic ancestry groups (EA, AA, EAA and NA) remained stable after randomly changing the number of snps fed into the program, suggesting that a small number of SNPs is sufficient to assess global ancestry

STRUCTURE (Pritchard et al., 2000) was used to quantitatively determine the ancestral composition for each patient

is an algorithm using multi-locus genotype data to infer population structure utilizing a model-based clustering method. The STRUCTURE algorithm (version 2.3.4) was downloaded from http://web.stanford.edu/group/pritchardlab/structure.html. The USEPOPINFO model was applied in our analysis.

To estimate the genetic ancestry at a particular chromosomal locus, LAMP (Sankararaman et al., 2008) was applied to the genotype data of TCGA.

There was a 95.6% overlap between TCGA patient written race and identified global ancestry.

## Principal components analysis corrects for stratification in genome-wide association studies

## Genetic ancestry analysis on >93,000 individuals undergoing expanded carrier screening reveals limitations of ethnicity-based medical guidelines

https://www.nature.com/articles/s41436-020-0869-3

Self-reported ethnicity was an imperfect indicator of genetic ancestry, with 9% of individuals having >50% genetic ancestry from a lineage inconsistent with self-reported ethnicity.

39.6% of individuals cannot identify the ancestry of all four grandparents,

Current professional guidelines by the American College of Medical Genetics and Genomics (ACMG) and the American College of Obstetricians and Gynecologists (ACOG) recommend pan-ethnic carrier screening for cystic fibrosis and spinal muscular atrophy.2,3,4 In addition, these two professional societies have long recommended carrier screening for a partially overlapping and expanded set of conditions based on a patient’s self-reported ethnicity (SRE)

We analyzed the genetic ancestry of 93,419 individuals undergoing routine ECS for over 90 serious Mendelian disorders via NGS.

Genetic ancestry was inferred using a proprietary analysis method (patent application WO2018144135A1). In brief, the method is based on sparse non-negative matrix factorization (with seven components) and was used to estimate ancestry shared with hypothetical ancestral populations using an imputed reference panel of 51 populations and an additional imputed reference panel of the Ashkenazi Jewish population.

Overall, relative to pan-ethnic ECS, ethnicity-based guidelines would have identified only 23% of carriers in the study cohort, with large differences per ethnicity (Fig. 4, SI Table 4). Just over two-thirds (68%) of carriers among African Americans would have been identified based on screening guidelines from ACMG and ACOG; that number decreases to only 13% of carriers identified among Hispanics when guidelines are followed.

## Inferring Genetic Ancestry: Opportunities, Challenges, and Implications

https://www.sciencedirect.com/science/article/pii/S0002929710001552

Its an older paper from 2010, but its a great overview of the state of genetic ancestry analysis.

Each part of the genome has its own ancestry, we are made up of dna from lots of people.

Continental / race ancestry

Biogeographic ancestry

- a persons origin is associated with the geographic locations of presumed ancestors

we cannot recreate out ancestors genomes, we dont know what they are. but assuming that the people who live in a given geographical area are a good represention of people who lived there a long time ago, is a rather large assumption.

also, just because you lived somewhere, or came from somewhere, doesnt mean you are genetically similar to the people who have historically lived there.

If we are being honest, linking "ancestry" to a physical location on earth is not as useful as simply understanding your genome in the context of all other genomes. What "group" or component you belong to, doenst necessarily corerlate to a physical place where people have lived.

There is probably a relationship between location and your genome at this point, however future generations may slowly loose this association.

AIM - an ancestry informative marker.

Currently, we only have partial knowledge of how human genetic diversity is distributed across the globe, but initial studies38 are revealing the degree of resolution possible in testing the relationship between genetic ancestry and geographic origins. A number of these studies have used a collection of ∼1100 DNA samples obtained from 51 populations living in different parts of the world; these samples constitute the Human Genetic Diversity Panel (HGDP).39 Analysis of 987 microsatellites typed in the HGDP collection, for example, inferred six population clusters that correspond to continental regions (i.e., Africa, America, Central/South Asia, East Asia, and Oceania)

## Up Next

How will we improve?

## Sources

https://www.sciencedirect.com/science/article/pii/S1535610820302117?casa_token=zp7T4IwBxIQAAAAA:botdxT6ZyGsA2HdUgT-3W9s_LWgO2lkigHgrvEj1pqmL3VeDaE4IRdERi5v2AZC0WykmfpqM-EQ

Self-reported ethnicity was an imperfect indicator of genetic ancestry, with 9% of individuals having >50% genetic ancestry from a lineage inconsistent with self-reported ethnicity. Limitations of self-reported ethnicity led to missed carriers in at-risk populations: for 10 ECS conditions, patients with intermediate genetic ancestry backgrounds—who did not self-report the associated ethnicity—had significantly elevated carrier risk.
