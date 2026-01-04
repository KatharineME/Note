+++
date = "2020-09-20"
title = "SAM BAM CRAM"
slug = "sam-bam-cram"
tags = []
categories = []
+++

SAM is the current standard format for storing aligned sequences. It was defined by the community in the early days of the 1000 genomes project.

[SAM format tutorial](https://www.youtube.com/watch?v=XU8atPxM0VQ)

[Samtools commands tutorial](http://quinlanlab.org/tutorials/samtools/samtools.html)

BAM is the compressed version of SAM.

However the newest format is CRAM. A CRAM file is about 30% the size of a BAM file. CRAM is now being adopted by important bioinformatics tools like Samtools.

# Size, why are SAM /BAM files so big?

Roughly speaking, a SAM file is about 1 byte per base, a BAM file is about 4 bits per base, and a CRAM file is 1-2 bits per base.

The human genome has about 3.3 billion base pairs, meaning that whole genome sequencing at only 10x would result in a SAM file with 33 bilion bytes of data (aka 33Gb of data). Therefore a 30X whole genome sequencing would produce a SAM file of 90Gbs at a minimum. HOWEVER, that is taking into account the genome sequence only. SAM files also contain quality scores for each base call, cigar strings, and etc.
