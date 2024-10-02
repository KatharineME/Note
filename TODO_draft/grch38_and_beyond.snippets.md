## Centromere Sequencing

How did we manage to model centromere sequences in GRCH38?

Using a rare type of pregnancy called a hydatidiform mole. This happens when a sperm fertilizes and egg without a nucleus.

That sperm then reduplicates its DNA, resulting in two identical copies of each chromosome. Since the resulting cell has no allelic variation, it can be used to generate unambiguous reads in regions with high diversity or repetition.

## Transposable Elements and Centromeres

Transposable elements are just selfish genome parasites, right? Maybe not.

In flies, highly conserved transposable elements form the
functional core of their centromeres. But we dont know yet whether they are active TEs or ancient insertions selected for over time 🤔.

## GRCH38 Conversion Tool

Need to convert data aligned to GRCH37 to GRCH38?
You may not need to re-align the raw reads.
NCBI has a remapping tool: https://www.ncbi.nlm.nih.gov/genome/tools/remap

## Future of Reference

What changes can we expect to the reference genome in 2023?

Probably just minor patches.

GRC has indefinitely postponed the GRCH39 release as it evaluates new genome models and work from the Telomere-to-Telomere Consortium and the Human Pangenome Reference Consortium.

## 2023 Sequence Analysis

GRCH37 had 9 alternative loci
GRCH38 has 261 alternative loci
GRCH39 will probably have more...

It's 2023. Time to invest in an alt-aware aligner!

## Non-linear Genome

The human reference genome used to be a list of bases. One base was assigned for every genomic location.

But as we sequenced more genomes around the world, we recognized that for some spots in the human genome, there are multiple common alleles. Instead of calling these common alternate alleles variants, its more useful to include in the reference genome. This is how ALT contigs were born.

Alternative (ALT) contigs make the human reference genome non-linear.

## Alt-aware aligners

Alternative loci complicate the reference genome by making it non-linear. So why are they useful?

They improve alignment. Reads that align well with an ALT contig would otherwise be misaligned elsewhere in the genome, or discarded.

They improve variant calling. At the 261 in GRCH38 locations with ALT contigs, if the base call doesn't match the primary assembly but matches an ALT contig, it doesn't wrongfully end up in the VCF.
