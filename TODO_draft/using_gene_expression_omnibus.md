+++ 
date = "2023-01-08"
title = "Using Gene Expression Omnibus"
slug = "using_gene_expression_omnibus"
tags = []
categories = []
+++

GEO is a public repository for gene expression data, (if you're looking for a public repository for raw sequencing data, that's [SRA](https://www.ncbi.nlm.nih.gov/sra)). Individual scientists submit data from their microarray, RNA-Seq, Next-Gen Seq, or other high throughput experiment to make it accessible by the community and in preparation for publications.

It is a great source for gene expression data in a wide range of organisms and experimental designs.

### Data Types

GEO accepts

- Gene expression profiling by microarray or next-generation sequencing
- Non-coding RNA profiling by microarray or next-generation sequencing
- Chromatin immunoprecipitation (ChIP) profiling by microarray or next-generation sequencing
- Genome methylation profiling by microarray or next-generation sequencing
- High-throughput RT-PCR
- Genome variation profiling by array (arrayCGH)
- SNP arrays
- Serial Analysis of Gene Expression (SAGE)
- Protein arrays

### GEO Series vs DataSet

When you start your GEO data search you'll notice there are GEO Series and GEO DataSets. A GEO Series "GSExxx" is an "original submitter-supplied record that summarizes a study". These data are manually curated by GEO staff into DataSets "GDSxxxx", which are statistically and biologically comparable sample sets processed on the same platform. There are also GEO Profiles on individual genes within a DataSet (mostly informative and can be used for hypothesis building).

There are many more Series than DataSets. This is partially because DataSets are compilations of multiple Series, and partially because GEO is majorly backlogged. I prefer to search Series because I know I'll be searching through everything that has been submitted, instead of a subset.

###
