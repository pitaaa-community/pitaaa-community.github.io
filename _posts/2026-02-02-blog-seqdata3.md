---
layout: post
title: "Wrangling Sequencing Data. Part 3: File Formats"
date: 2026-02-02
categories: Blog
authors:
  - Olivia Smith 
tags: spaam, blog
---

_by [Olivia Smith](https://www.oliviasmith.me/home)_

**Don't forget to check out Part 1 and 2!**

[Part 1: File formats](https://www.spaam-community.org/blog/2026/02/02/blog-seqdata1/)

[Part 2: Download tools](https://www.spaam-community.org/blog/2026/02/02/blog-seqdata2/)

**tl;dr**
There’s tons of options for working with sequencing reads and consensus sequences, so pick whatever floats your boat and/or what the nearest-to-you scholar uses. You should learn SAMTools and BCFTools for alignment files. My best advice is **(1) read the documentation and (2) read all the options/flags for a function before you use it.**

### Sequencing reads: fastp, fastqc, GATK
The first step in many analyses is processing of the raw sequencing reads in some way. A huge number of tools are out there for this, but I like [fastp](https://github.com/OpenGene/fastp) ([Chen, et al., 2018](https://academic.oup.com/bioinformatics/article/34/17/i884/5093234)) and many folks use [GATK](https://gatk.broadinstitute.org/hc/en-us) from the Broad Institute or [SeqKit](https://bioinf.shenwei.me/seqkit/) ([Shen, et al., 2016](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0163962)). [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is another nice tool for QC which creates really readable HTML reports!

A few examples of things you might do in ancient metagenomics with reads:
- Filter on base call quality
- Trim adapters used for sequencing and barcoding but that we don’t want for our analysis
- Merge paired-end reads
- Align them!

### Alignment and index files: SAMTools, BCFTools
You need to know how to use [SAMTools](http://www.htslib.org/) and [BCFTools](https://samtools.github.io/bcftools/bcftools.html). They have great documentation and are really powerful, so are worth the investment to understand. Also this is the way that you will generate the index file to go with your alignment, which is usually required for any tool that uses alignments.

Some examples of what you can do with SAMTools and BCFTools:
- [Call](https://samtools.github.io/bcftools/bcftools.html#call) variants
- Build a [consensus](https://samtools.github.io/bcftools/bcftools.html#consensus) sequence
- Make [pileups](https://samtools.github.io/bcftools/bcftools.html#mpileup) to count the base calls at each position or compute genotype likelihoods
- Take a random subset or fraction of reads to test on (an option when using [view](https://www.htslib.org/doc/samtools-view.html))

### Consensus sequences: BLAST, Geneious, Benchling, ApE
You’ve perhaps seen these before, as these are the most accessible tools. They often have straightforward user interfaces via applications or browsers! These are great for things like homology analysis ([BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi)), or plasmid visualization and genetic engineering ([Geneious](https://www.geneious.com/), [Benchling](https://www.benchling.com/), [ApE](https://jorgensen.biology.utah.edu/wayned/ape/)). These tools are super powerful and flexible, so anything you can imagine doing with a consensus sequence is probably possible in one of these. 

### Managing environments
As you can tell, there’s a broad range of tools and software used in the field. It is good practice to “manage your computational environments” – you can imagine this as organizing boxes of stuff. Environments are isolated computational setups that keep your software dependencies from conflicting and causing problems; each environment has only what is necessary for that task. [Conda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) is a widely used package/environment software which makes installing lots of the tools I’ve mentioned here easy (I use miniconda). You can do similar environment management [with Python](https://docs.python.org/3/library/venv.html). Also available are container platforms like [Docker](https://www.reddit.com/r/docker/comments/keq9el/please_someone_explain_docker_to_me_like_i_am_an/) or [Singularity](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html). These are more common when working in industry settings or with high performance computing environments.

____________________________________

Olivia Smith is a PhD candidate at The University of Texas at Austin working under the supervision of Dr. Arbel Harpak. You can find her on Twitter at [@SmithOliviaS](https://twitter.com/smitholivias).
