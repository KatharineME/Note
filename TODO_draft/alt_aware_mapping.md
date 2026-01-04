+++
date = "2022-11-16"
title = "Alt Aware Mapping"
slug = "alt_aware_mapping"
tags = []
categories = []
+++

https://support-docs.illumina.com/SW/DRAGEN_v38/Content/SW/DRAGEN/GPipelineAltMap_fDG.htm

https://gatk.broadinstitute.org/hc/en-us/articles/4410953761563-Introducing-DRAGMAP-the-new-genome-mapper-in-DRAGEN-GATK

Learned from the webinar on December 2nd 2022:

Basis for alt aware mapping:

GRCH38 comes with more alternate contigs than grch37 did. And we expect future reference genome versions to continue to increase the number of alternate contigs. Therefore, we need aligners that can take advantage of these smarter reference genomes.

How exactly does it work? What does it mean for there to be a variant in an alt aware section of the genome?

If you use an alt aware mapping, the vcf, in combination with the reference genome version that was used, can no longer tell you the exact and entire genome sequence of the sample. This is because at the locations with alternate contigs, if there is no snp, we dont know which reference alternate contig the sample has. In many cases, the phenotype may be the same. But this fact remains.
