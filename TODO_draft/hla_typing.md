## Tool

SoapTyping - NO
Sanger based, requires ABIF input format.
Desktop app, nice interface.

HLA Haplotype Validator (HLAHapV) - NOT TRIED
NGS based
uses IMGT (IMmunoGeneTics)/HLA Database v3.20.0 as the default database
11 stars on GitHub
Input is GL strings
Does not look like good software

HLAreporter - TRIED  
Its paper claims to outperform HLAminer and PHLAT
http://paed.hku.hk/genome/software.html
From a University of Hong Kong lab which produces lots of NGS software
Designed more for exome NGS, but capable of WGS as well
Called samtools incorrectly and doesn’t have threads

HLA-LA - TRYING
Found from GitHub, 118 stars
Not easy to install
There is a docker image for it, need to try it out
Paper has 91 citations and 11K views
Claim 99% accuracy for whole genome
Written in C++ and Perl

Using skchronicles docker image:

Cd data2/

HLA-LA.pl \
    --BAM /data/dev/genome-seek/test_subsampled_docker/BAM/HG004.sorted.bam \
    --graph /data/OpenOmics/references/genome-seek/HLA-LA/graphs/PRG_MHC_GRCh38_withIMGT \
    --sampleID sample \
    --maxThreads 8 \
    --workingDir /data/dev/genome-seek/test_subsampled_docker/HLA/HG004

HATK - NOT TRIED
26 stars on GitHub
Updated two years ago
Installation is conda suggested
Written in python
Usage example makes it look like specialized input data type required

PHLAT - NOT TRIED
There is a docker image with only two stars: https://github.com/chrisamiller/docker-phlat
Is not on GitHub!
Link to public version is broken
Paper has 59 citations and 11K views

xHLA - WORKED
94 stars on GitHub
79 citations and 57K views
Written mostly in python and perl
Has a docker image advertised in the readme
Documentation is simple and clear
Claims to take 3 minutes for 30X WGS data
Github has not been updated for 7 years
Paper: https://www.pnas.org/doi/full/10.1073/pnas.1707945114
Paper claims to outperform many of the other tools on this list

This alignment worked for typing: 
bwa mem -t 10 -o G2596.sam -R @RG\tID:G2596\tSM:G2596 /Users/kate/craft/jl/FASTQ.jl/data/GRCh38/GRCh38.primary_assembly.genome/GRCh38.primary_assembly.genome.fa /Users/kate/Customer/2596/2596 Raw Data/01.RawData/G2596/G2596.R1.fastq.gz /Users/kate/Customer/2596/2596 Raw Data/01.RawData/G2596/G2596.R2.fastq.gz

xHLA command:
docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/hla \ 
--sample_id G2596 --input_bam_path tests/G2596.bam \
--output_path test

HLA*PRG - NOT TRIED
Written by Alexander Dilthey. HLA-LA is written by his lab..
Github has not been updated for 6 years
But HLA-A was updated 6 months ago.
HLAminer
