+++ 
date = "2022-10-05"
title = "GRCH38 and Beyond"
slug = "grch38_and_beyond"
tags = []
categories = []
+++

## Founding

Created in 2009, got popular in 2014.

Computational methods are actively being developed.

## Different From Bulk RNASeq

More expensive.

More computationally complex.

Typically used for heterogeneous tissue. Bulk RNA Seq is better for homogeneous tissue.

More information. There is an added dimension of knowing what expression came from what cell.

In a typical bulk RNA Seq experiment ~20% of genes will be zero. In a deeply sequenced single cell experiment ~50% will be zeros. And in a shallow sequenced single cell experiment up to 99% of genes can be zero.

High-throughput droplet based methods identify ~5,000 genes, down to ~1000 for very shallow studies. More sensitive methods like SmartSeq identify twice as many genes.

## Sequencing

### Plate-based Sequencing

Low-throughput high depth sequencing.

Most popular platform was SmartSeq.

Prominent from 2009 to 2014.

Expensive and very labor intensive (pippeting).

Plate based. Cells are separated into wells of a plate. Library prep happens separately in each well, then sequencing is done.

Sequences only a few cells deeply without UMIs. The entire transcript is profiled and achieves higher sensitivity then droplet-based scRNASeq.

### Droplet-based Sequencing

High throughput (many cells) and low depth.

Has been popular since 2015.

Magnetic bead with PCR handle, cell barcode (to identify cells), and UMI (to identify mRNA molecule), and stretch of Ts to bind to the poly A tail of the messenger RNAs.

Magnetic beads are flowed in with cell stream. The bead and cell streams combine and then hit an oil interface. This forms water droplets in the oil where most of the droplets are empty or contain a bead. About 16% contain a bead and a cell. And about 2% contain a bead and multiple cells.

If the concentration of cells and beads are increased, then you get more droplets with one cell and one bead. But you also get many more droplets with two cells and one bead. There isn't a hgihly accurate way of ditinguishing doublets from singlets, so this isn't a good strategy. This is why cells are beads are flowed in at low concentrations.

Eric Chow - Single Cell Sequencing - Eric Chow (UCSF) YouTube Video

_UMIs help distinguish PCR duplicates from higher gene expression. If many transcripts are mapping to the same gene and they each have a different UMI, it means the expression of that gene was relatively high. If however the UMIs are the same, it means they are PCR duplicates and should be removed._

In drop-seq aparatus, beads and flowed in with cells. Droplets are created with ideally one bead bound to one cell. Sometimes two cells or none in the droplet.

Then the cells are lysed within each droplet. The poly A tails of the mRNA molecules bind to the T tails on the bead.

The droplets are broken and reverse transcription and PCR amplification happens.

#### 10X Genomics DropSeq

Most popular droplet-based platform.

Made two improvements to traditional droplet sequencing technology.

The first is they engineered their microfluidic device and beads such that at least 90% of the droplets end up with a bead. Traditionally only a small fraction of droplets would have a bead. This allows them to lead the beads at high concentration.

Secondly, they do reverse transcription inside the droplets. Traditionally the reverse transcription happened after the emulsion was broken. Thereafter they break the emulsion and amplify the cDNA in tube and construct the sequencing library in the same tube.

This process happens on a chip. Each lane of the chip has input for oil, beads, and cells, and one output for droplets. Each chip has eight lanes. You can process 25K cells in each lane. Therefore 200K cells per chip. Process on chip takes fifteen minutes.

cDNA amplification and library prep take half a day.

#### Droplet Problems

The goal is one bead and one cell in each droplet. But the following problems happen. The cutoffs for each problem type need to be determined per dataset.

###### No Cell

Identified by no genes or a small number of genes expressed (from ambient RNA).

###### Two or More Cells

Identified by large number of genes expressed (many more than other cells). Also ~5% of cell barcodes are tagging multiple cells. And up to ~20% of the time multiple cell barcodes are tagging the same cell.

###### Dead or Broken Cell

Identified by high percentage (more than 5%) of mitochondrial transcripts, unmappable or multi-mapped transcripts,

#### Weaknesses

Only allow for the 5' or 3' end to be sequenced.

The entire transcript isn't sequenced. Still don't understand this.

Lots of dropouts. Even when a gene is expressed, the detected expression level is zero.

Noisy. High level of variation between cells due to percentage of mRNAs captured, reverse transcription efficiency, amplification bias, sequencing depth, cell size, and cell-cycle stage.

Multimodal Expression Distributions. Cell heterogeneity and lots of zero cause this.

### Microwell-based Sequencing

SeqWell, CelSee Genesis, Becton Dickinson Rhapsody.

Between thousands and hundreds of thousands of microwells on a plate. The beads are engineered so yo get one bead in each well. Beads coat the microwell array. Then cells are applied. Small subset of wells will get a cell. Those wells will go through reverse transcription, amplification and library prep.

## Analysis

#### FastQC

#### Alignment

Tools such as Cell Ranger (commercial software from 10x Genomics) [Zheng et al., 2017], zUMIs [Parekh et al., 2018], alevin [Srivastava et al., 2019], RainDrop [Niebler et al., 2020], kallisto|bustools [Melsted et al., 2021], STARsolo [Kaminow et al., 2021] and alevin-fry [He et al., 2022] provide dedicated treatment for aligning scRNA-seq reads along with parsing of technical read content (CBs and UMIs), as well as methods for demultiplexing and UMI resolution.

STARsolo has performs very similarly to 10X genomics on 10x data, but runs ten times faster than CellRanger.

https://kb.10xgenomics.com/hc/en-us/articles/115000794686-How-is-the-MEX-format-used-for-the-gene-barcode-matrices-

Spliced mapping, contiguous mapping, and varieties of lightweight mapping.

Cell Barcode Assignment. This should be done by the aligner.

Get gene by cell matrix for each sample.

#### Clean Gene by Cell Counts Matrix

"Cell QC is commonly performed based on three QC covariates: the number of counts per barcode (count depth), the number of genes per barcode, and the fraction of counts from mitochondrial genes per barcode (Ilicic et al, 2016; Griffiths et al, 2018). The distributions of these QC covariates are examined for outlier peaks that are filtered out by thresholding"

from: https://www.embopress.org/doi/full/10.15252/msb.20188746

Remove unvarying genes. Remove genes with all zeros.

Remove low quality cells. (emptyDrops via STARsolo flag)

Normalize sequencing depth. SCnorm, sctransform, and bayNorm.

From STARsolo manuscript:

"
We further filtered the cells that contain < 200 genes or > 20% of mitochondrial
reads based on CellRanger counts, resulting in the final set of 4,473 cells which
was used for all tools. 2. For each tool, genes detected in less than 3 cells were excluded. 3. Counts were normalized to 10000 reads per cell, a pseudocount of 1 was added
and the natural log transformation was performed. These normalized expression
values were used for differential gene expression calculations (see below). 4. For UMAP embedding, neighborhood graph calculation and clustering, the ex- pression values were scaled to unit variance and zero mean across the cells and
truncated at a maximum value of 10. 5. 3, 000 highly variable genes were selected using the ’seurat v3’ algorithm.
"

#### ?

Imputation and smoothing. SAVER. Fills some zeroes with imputed values based on overall structure of the data. Many false positives. Imposes patterns. Risky.

Cell cycle assignment. cyclone and Seurat. cyclone uses pairs of genes to asssign G1,S, G2/M, but doesn't recognize non-cycling cells well. Seurat scores cells based on expression of known cycle markers. Once cells have been assigned a stage, they both use linear model to regress out differences.

Feature selection. Select the genes with highest variance across cells, they have the most signal. Its been shown that noise follows a negative binomial distribution. Seurat selects features by empirically fitting the relationship between variance and mean expression. The reverse approach can also be taken, by removing features with more zeroes than expected.

Scale expression.

Reduce dimensions using PCA.

Determine significant principal components.

Use the principal components to cluster cells. Graph it.

Visualize clusters with non-linear dimensional reduction (UMAP, tSNE) using the principal components.

Detect and visualize marker genes for the clusters.

## Analysis

Used Cell Ranger Reference Genome and Gene Annotations From Here
https://support.10xgenomics.com/single-cell-gene-expression/software/release-notes/build#grch38_3.0.0

Generate Genome Index using the same Reference Genome and Gene Annotations as CellRanger
star --runMode genomeGenerate --runThreadN 4 --genomeDir ./star_index/ --genomeFastaFiles ./Homo_sapiens.GRCh38.dna.primary_assembly.fa --sjdbGTFfile ./Homo_sapiens.GRCh38.93.gtf

Ran the alignment
star
--runThreadN 4
--genomeDir ~/Downloads/ferreira_treg/star_index
--readFilesIn ~/Downloads/ferreira_treg/7166-MR-91/7166-MR-91_S1_L005_R2_001.fastq.gz ~/Downloads/ferreira_treg/7166-MR-91/7166-MR-91_S1_L005_R1_001.fastq.gz
--readFilesCommand "gzip --decompress --stdout"
--outSAMtype BAM SortedByCoordinate
--outFileNamePrefix ~/Downloads/ferreira_treg/output/testrun
--soloType Droplet
--soloCBwhitelist ~/Downloads/ferreira_treg/3M-february-2018.txt
--soloUMIlen 12
--clipAdapterType CellRanger4
--outFilterScoreMin 30
--soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts
--soloUMIfiltering MultiGeneUMI_CR
--soloUMIdedup 1MM_CR

Another run for good measure:

star
--runThreadN 8
--genomeDir ~/Downloads/ferreira_treg/input/star_index
--readFilesIn ~/Downloads/ferreira_treg/input/7166-MR-99/7166-MR-99_S1_L005_R2_001.fastq.gz ~/Downloads/ferreira_treg/input/7166-MR-99/7166-MR-99_S1_L005_R1_001.fastq.gz
--readFilesCommand "gzip --decompress --stdout"
--outSAMtype BAM SortedByCoordinate
--outFileNamePrefix ~/Downloads/ferreira_treg/7166-MR-99/output/
--soloType Droplet
--soloCBwhitelist ~/Downloads/ferreira_treg/input/3M-february-2018.txt
--soloCBstart 1
--soloCBlen 16
--soloUMIstart 17
--soloUMIlen 12
--clipAdapterType CellRanger4
--outFilterScoreMin 30
--soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts
--soloUMIfiltering MultiGeneUMI_CR
--soloUMIdedup 1MM_CR
--soloBarcodeReadLength 151

Alignment attributes to add when things are working

--outSAMattributes NH HI nM AS CR UR CB UB GX GN sS sQ sM
--soloMultiMappers Uniform

Understanding Output:

https://kb.10xgenomics.com/hc/en-us/articles/115000794686-How-is-the-MEX-format-used-for-the-gene-barcode-matrices-
https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/output/matrices
https://kb.10xgenomics.com/hc/en-us/articles/360023793031-How-can-I-convert-the-feature-barcode-matrix-from-Cell-Ranger-3-to-a-CSV-file-

## Sources

https://www.biorxiv.org/content/10.1101/2021.05.05.442755v1.full.pdf

https://www.nature.com/articles/s41596-020-00409-w

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4481139/

https://pubmed.ncbi.nlm.nih.gov/28263961/

https://pubmed.ncbi.nlm.nih.gov/28212749/

https://www.sc-best-practices.org

https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon/lessons/04_quasi_alignment_salmon.html#:~:text=The%20quasi%2Dmapping%20approach%20estimates,base%2Dby%2Dbase%20alignment.
