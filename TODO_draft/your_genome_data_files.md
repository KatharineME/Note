+++ 
date = "2022-03-28"
title = "Whats in your genome data files and what can you do with them"
slug = "what-you-can-do-with-your-genome-data-files" 
tags = []
categories = []
+++

Hi folks, today I am going to explain 3 genome data file types (FASTQ, BAM, and VCF). I am going to explain what these files are, what they contain, and roughly how they are structured. If you've had a DNA test before, depending on the type of DNA test you've had, you probably received one if not all of these file types. Just to clarify, this post is for everyone, you do not need a PhD
to understand whats to come, so no sweat.

Quick tip before we jump in. If you have had a DNA test and you only received written report, contact the company and ask them for your actual genome data files, including your raw unprocessed genomic data. If you don't get these files, you are not capitalizing on the investment you made when you ordered the test. Data is power today, so go get yours.

## FASTQ

FASTQ is the first genome data file type we are going to talk about. A FASTQ file is a file that's generated directly after sequencing, Imagine that the sequencer read off 150 letters of your DNA at a time and spit out each 150 letter block into a file. That is what the FASTQ file is. And we actually call each of these 150 letter blocks a "read". And just to clarify they dont have to be 150 base pairs long, they could be 100 or 200 base pairs long for example. If you open up a FASTQ file you'll see one read after another. But you'll also see other lines of data too. What are these other lines for? For each sequence read, there are three lines of data that accompany it. The first line includes the name of the read, what sequencing machine was used to create it, and exactly where on the sequencer flow cell the read was located. The second line is the read (which again is about 150 bases of your DNA), the third line is always a plus, mostly for aesthetics, and the fourth line has the quality scores for the read. So for each of the letters or bases in the read, the sequencing machine estimates how confident it was that that base is correct. Sometimes for example the signal of a base call is weak so the sequencer gives that base a low quality score. These quality scores look strange at first, but each symbol corresponds to a numerical quality score.

Now you might be wondering here, what happens if one of the base calls in a read is very low quality, will you ever know what base you have there? The answer is yes because your genome was probably read more than once, in fact it may have been read up to 30 times. That means that if one base on chromosome one is low quality, you probably have 30 more reads that cover that same spot and have higher quality base calls there. And so you'll be able to tell that you have Adenine there for example. This concept is called genome coverage. With 30X coverage, meaning that your genome was read about 30 times, we should be able to tell each base of your DNA with at least 99.9% accuracy.

Now, that was a lot of detailed info but what it comes down to is that the FASTQ file holds your whole genome sequence in unordered pieces called reads. And it holds information about how accurate each base of each read sequence is. Now if we pick a random read from your FASTQ file, we will have no idea where in your genome that read is coming from, we don't even know what chromosome its on. So what we have is a very big genome puzzle on our hands. This is where "genome alignment" comes in.

## BAM

Genome alignment is the process of taking each read sequence from your FASTQ files and finding where it came from in your genome. This is done using the human reference genome. The human reference is the whole genome sequence of the average human. During alignment, each read gets compared to the human reference genome and gets assigned the location where it matches best. This happens for each read in your FASTQ files. At the end of the alignment you'll have your whole genome sequence in order, and this is whats stored in a BAM file.

Now a BAM file is actually a binary file so if you open it you will see a bunch of strange characters and weirdness. The non-compressed version of a BAM file is called a SAM file, which is actually human readable. Here is what a SAM file looks like inside. I wont get into the details here, but here is the sequence that was aligned, here is the chromosome and the position on that chromosome where it was aligned. And the SAM file goes on like this. And if you were to stitch together each of these sequences, you would get your complete genome sequence in order.

## VCF

However, as you may have guessed, your genome is not exactly the same as the reference human genome. Its estimated that there are about four to five million places in your genome that are different from the human reference genome. Typically, these are precisely the locations we are interested in, because they are what make you unique. So after genome alignment your genome sequence is again compared to the human reference genome, and each place where it differs is listed in a file called VCF.

So the VCF contains the 4 to 5 million places where you are different from the average human (in so far as the reference genome accurately represents the average human).

Each row in a VCF is one variant. And no matter where you get your VCF from, it should have these eight columns. The first column is the chromosome, followed by its position on the chromosome and its name if it has one. Some variants have names that can be added here. Their names are called rsIDs. But in many cases you will just see a dot here. The dot doesn't necessarily mean that this variant doesn't have a name. It may just means that this VCF hasn't been annotated with variant names yet.

Moving on the REF column has the reference base (which is the base that the reference genome has at this location), and the ALT column has the variant base or bases that you have!

Now the last detail I'll cover here is how to tell if you have two copies of this variant or just one. If you remember, you have two copies of each chromosome, one from your dad and one from your mom. So for each variant that you have, its possible that you have it on both of your copies of that chromosome, or just on one. If you have two copies of the variant, whatever effect it has may be stronger. So here's how to tell. This GT field here stands for genotype. Here the zero represents the reference base, and the 1 represents the variant or alternate base. If you have 0/1 here, that means you have only one copy of the variant. If you have 1/1, that means you have two copies.

## Conclusion

And that it is folks, you now have a basic understanding of what FASTQ, BAM, and VCF files are and what they contain. In the next post I am going to cover cool things that you can do with these files even if you have zero technical experience.
