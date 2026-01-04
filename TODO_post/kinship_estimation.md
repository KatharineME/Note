+++
date = "2020-07-15"
title = "Kinship Estimation"
slug = "kinship-estimation"
tags = []
categories = []
+++

Two adults think they may be related based off old photos and records. How do we know for sure? That's the topic of this post.

If it had not been for a potential family member entering our lives, I probably never would have written this. It provided the necessary motivation to learn how kinship estimation works. In this post I give an introduction to kinship from a genetic perspective. Then I describe how I did the genetic kinship analysis to discover I have another family member.

![siblings](/images/siblings.jpg)

## Genetic Kinship

There is no set amount of DNA that same-related people share. For example, two biological siblings could have been passed many of the same bits of DNA from their parents, or they could been passed many different bits. This element of chance results in the "Range % Shared DNA" column below.

| Relationship                         | Average % Shared DNA | Range % Shared DNA |
| ------------------------------------ | -------------------- | ------------------ |
| Mother / Son                         | 52.5%                | NA                 |
| Father / Son                         | 47.5%                | NA                 |
| Mother / Daughter                    | 50%                  | NA                 |
| Father / Daughter                    | 50%                  | NA                 |
| Full Sibling                         | 50%                  | 38%-61%            |
| Grandparent / Grandchild             | 25%                  | 17%-34%            |
| Aunt or Uncle / Niece or Nephew      | 25%                  | 17%-34%            |
| Half Sibling                         | 25%                  | 17%-34%            |
| Great-grandparent / Great-grandchild | 12.5%                | 4%-23%             |

For this reason, when comparing two people's genomes, it isn't immediately obvious how they are related. But adding in age and life history can give us the answer we're looking for.

_Side note: Sons and mothers share more DNA simply because the X chromosome is much larger than the Y chromosome._

_Side note: Some DNA we can only get from one of our parents. For boys obviously, the Y chromosome comes from their father. And for everyone, mitochondrial DNA comes from the mother. Whatever mitochondrial DNA the fertilized egg held, is the mitochondrial DNA the child will have._

## Method

Here we're going to apply the KING kinship estimator. KING stands for Kinship based INference for Genome-wide association studies. [This is the paper that descsribes KING's algorithm](https://academic.oup.com/bioinformatics/article/26/22/2867/228512?login=true).

The KING method calculates a "kinship coefficient" which they define as "the probability that two alleles sampled at random from two individuals are identical by descent," where monozygotic twins get about 0.35, 1st degree relatives are 0.177 - 0.35, 2nd degree relatives are 0.08 to 0.17, 3rd degree relatives are between 0.04 and 0.08, and a negative number indicates an unrelated relationship.

First we use bcftools to merge the VCFs of all the people we want to compare. In this example, we'll only compare two people. Then use Plink to create the necessary input files. Finally we'll call the KING program via Plink to calculate the pair-wise kinship coefficient.

#### 1. bcftools merge

First we need to merge all the variants from all the people we want to compare into one VCF file. Here we are going to check the kinship of two people, however this method will work with more than two people as well. To merge the VCF's of the people we want to comapre, we'll use trusty `bcftools`.

If you don't have bcftools and your on a Mac, `brew install bcftools` is a good way to get it. If you like using conda, `conda install -c bioconda bcftools` is another way to get it.

`bcftools merge` by default will merge VCF1 with VCF2 to make a merged VCF in the manner below. Before merging, make sure the sample names in the header of each VCF (column names 10 and beyond) are what you want them to be. If they are not, one option is manually changing them in a text editor. At a minimum, sample names must be unique before merging.

Person1 VCF:

```sh
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	Person1
chr1 10120 . T C 1 LowGQX
LowDepth
NoPassedVariantGTs SNVHPOL=4
MQ=6 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:22:0:2:2:1,1:0,1:1,0:0:LowGQX
LowDepth:30,0,22
chr1 51898 . C A 6 LowGQX
NoPassedVariantGTs SNVHPOL=2
MQ=35 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:38:5:6:0:4,2:1,2:3,0:2.1:PASS:40,0,101
```

Person2 VCF:

```sh
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	Person2
chr1 10250 . A C 1 LowGQX
NoPassedVariantGTs SNVHPOL=4
MQ=11 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:23:0:4:0:3,1:1,0:2,1:0:LowGQX:26,0,80
chr1 51898 . C A 6 LowGQX
NoPassedVariantGTs SNVHPOL=2
MQ=35 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:17:0:7:0:6,1:3,1:3,0:0:LowGQX:19,0,146
```

Merge command (must create indices for each VCF first).

```sh
bcftools index person 1.vcf.gz

bcftools index person 2.vcf.gz

bcftools merge person1.vcf.gz person2.vcf.gz > merged.vcf
```

Merged VCF:

```sh
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	Person1	Person2
chr1 10120 . T C 1 LowGQX
LowDepth
NoPassedVariantGTs SNVHPOL=4
MQ=6 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:22:0:2:2:1,1:0,1:1,0:0:LowGQX
LowDepth:30,0,22 ./.:.:.:.:.:.:.:.:.:.:.
chr1 10250 . A C 1 LowGQX
NoPassedVariantGTs SNVHPOL=4
MQ=11 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL ./.:.:.:.:.:.:.:.:.:.:. 0/1:23:0:4:0:3,1:1,0:2,1:0:LowGQX:26,0,80
chr1 51898 . C A 6 LowGQX
NoPassedVariantGTs SNVHPOL=2
MQ=35 GT:GQ:GQX:DP:DPF:AD:ADF:ADR:SB:FT:PL 0/1:38:5:6:0:4,2:1,2:3,0:2.1:PASS:40,0,101 0/1:17:0:7:0:6,1:3,1:3,0:0:LowGQX:19,0,146
```

Take a look at the three rows of the merged VCF. The first row is a variant that only Person1 has, the second row is a variant that only Person2 has, and the third row is a variant that both people have. This is great, because variant information for either person wasn't lost in the merge.

#### 2. Plink File Preparation

We are going to now use Plink for file preparation and to call the KING kinship estimator. We use Plink1.9 to prep the files. Then Plink2.0 to call the kinship estimator.

_Side note: As of February 2021, Plink1.9 and Plink2.0 are not completely independent. Some Plink2.0 commands require input made by Plink1.9. And that is the case for the command we'll run, so you need both._

[Get Plink1.9 here](https://www.cog-genomics.org/plink/1.9/)

[Get Plink2.0 here](https://www.cog-genomics.org/plink/2.0/)

Now, we feed Plink1.9 our merged VCF and ask it to make us a .bed and other accompanying files with the file prefix "people_kinship_check".

```sh
plink --vcf merged.vcf.gz --make-bed --out people_kinship_check
```

This command will create four files:

```css
people_kinship_check.bed
people_kinship_check.bim
people_kinship_check.fam
people_kinship_check.log
```

In short, .log is just the plink STDOUT saved to a file, .bed is a binary genotype file, .fam is a family file, and .bim is a map file. Its worth checking the .bim file and comparing it to the merged VCF to make sure they agree. You should also look at and edit the .fam file as needed. You may need to adjsut the sex code or other family IDs. [Here is the .fam format explained](https://www.cog-genomics.org/plink/1.9/formats#fam).

[Read more about these file formats here](https://www.cog-genomics.org/plink/1.9/formats)

#### 3. Plink Kinship Check

We have the input files we need. Now we call the KING kinship estimator via Plink with the `make-king` command:

```sh
plink2 --bfile people_kinship_check --make-king --out people_kinship_check_result
```

_Note: the `bfile` flag tells plink2 what type of input data we are passing (.bed, .bim, .fam). Then we only need to give the prefix for these files ("people_kinship_check")._

This command will make three files:

```css
people_kinship_check_result.king
people_kinship_check_result.king.id
people_kinship_check_result.log
```

The .king file holds the kinship estimation coefficient for the two people we are comparing: what we have been striving for. In my case, the kinship coefficeint was higher than 2.0, indicating a first degree relationship. It is amazing to see the answer as a single number. I hope if you're reading this and you followed the steps here, you get a result just as satisfying.
