---
layout: post
title: "Wrangling Sequencing Data. Part 2: File Formats"
date: 2025-02-02
categories: Blog
authors:
  - Olivia Smith 
tags: spaam, blog
---

_by [Olivia Smith](https://www.oliviasmith.me/home)_

This is Part 2 of the Wrangling Sequencing Data series and it will focus of Download Tools. To find out about different file formats, make sure you have read Part 1.

One of the things that I often found challenging when I first started was figuring out how to find the best databases and download tools. Here, I’ll discuss a couple options.

**tl;dr**
You can download practically any file you’d want using a command line HTTP/FTP package (usually wget for Linux, curl for MacOS). Some databases also have browser interfaces or command line APIs; ENA has enaBrowserTools and NCBI has SRAToolkit. Learn and use bash loops for repetitive tasks!

### Command Line
#### wget (Linux)

wget is a tool that can be used to download any HTTP(S) and FTP(S) from the internet, which is perfect for our uses (you can see more [wget details](https://www.gnu.org/software/wget/) on the GNU Project page). It is installed by default on most Linux machines. It seems to also be available for Windows PowerShell with a slightly different syntax, but I have never used it myself. wget is not installed on MacOS (Unix) systems by default, but you can add it using another package manager (e.g. brew install wget to install with [Homebrew](https://brew.sh/)) or use curl instead (see below).

To download, simply use *wget {URL}*, as so:
```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR365/008/ERR3654168/ERR3654168.fastq.gz
```

If your command works, you should see a few statements like this letting you know that it is accessing the data: 
```
Resolving ftp.sra.ebi.ac.uk (ftp.sra.ebi.ac.uk)... 193.62.193.138
Connecting to ftp.sra.ebi.ac.uk (ftp.sra.ebi.ac.uk)|193.62.193.138|:21... connected.
Logging in as anonymous ... Logged in!
```

And then a progress bar will appear. Once the download is done, it will let you know what the saved file is called - in this case the file is called "ERR3654168.fastq.gz". By default this will be the name of the file from the source.
```
2023-04-26 11:48:32 (16.8 MB/s) - ‘ERR3654168.fastq.gz’ saved [60429342]
```

#### curl (MacOS, Unix)

On MacOS, you can use [curl](https://curl.se/) instead! And the good news is it works the same way, but you’ll want to explicitly tell it where to save the file. 

Use curl -o {OUT_FILE} {URL} and you’re good to go.
```
curl –o ERR3654168.fastq.gz ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR365/008/ERR3654168/ERR3654168.fastq.gz
```

With curl you should just get your download details right away (it will show 100 under % Total when it’s done):
```
 % Total	% Received % Xferd  Average Speed   Time	Time 	Time  Current
                                   Dload   Upload  Total     Spent	Left  Speed
 30 57.6M    30 17.6M	0 	0  2829k  	0  0:00:20  0:00:06  0:00:14 3645k
```

### European Nucleotide Archive (ENA) Toolkit
The [European Nucleotide Archive](https://www.ebi.ac.uk/ena/browser/home) (ENA) is where you will find most of the metagenomic data from publications in our field. If you want to use the browser to download files, I’ve created a walkthrough here: [Downloading Metagenomic Data from ENA](https://drive.google.com/file/d/1rRBSqLbZLe-Y7A0dEfxm43cIwHjYqS02/view?usp=share_link)

The enaBrowserTools toolkit is great for downloading lots of sequences at one time (without writing a bash loop!). Download and install enaBrowserTools using the instructions [here](https://github.com/enasequence/enaBrowserTools/blob/master/README.md).

### National Center for Biotechnology Information (NCBI) Toolkit
NCBI has built a command line tool for accessing their sequencing database called SRA Toolkit. Download and install SRA Toolkit using the instructions [here](https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit). The SRA Toolkit quickstart tutorial for downloading fastq files is [here](https://github.com/ncbi/sra-tools/wiki/08.-prefetch-and-fasterq-dump).

### Bash loops
One last thing – it will be a huuuuuuuge help if you learn how to write simple loops in bash to perform downloads. This will make it so that you can take many links, accession numbers, etc. and just let the terminal do its thing while you drink coffee. 

Let’s walk through a simple bash loop (the base and $ are just to show that these are two different commands):
```
(base) $ var=(2 3 6 8 11)
(base) $ for v in ${var[@]}; do echo $v; done
```

Let’s break this down:
1. Use for to mark the beginning of the loop
2. Create a variable var (can be named anything) and give it a value which is an array of integers. Notation for arrays in bash is variable=(value_1 value_2 … value_n). Note that in bash we use spaces to separate values in an array.
3. Loop over every element (number) in that variable using the syntax ${YOUR_VARIABLE[@]} . The [@] says that we want to go through each element instead of specifying just one of them (i.e. ${var[1]} would return the value 2). 
4. Follow this with ; do and our command; in my example I used echo, which is just a print statement. Here again we see the notation for variables which is $ followed by the variable name. In cases where the variable is surrounded by other characters, you can use ${YOUR_VARIABLE} to ensure that your command is executed properly.
Use ; done to say where the loop should end.

Here’s an example of a loop to download a few different sets of BAMs using wget (these are not real, don’t try to actually download them):

```
(base) $ ftps=(ftp://ftp.example01.fastq.gz ftp://ftp.example02.fastq.gz ftp://ftp.example03.fastq.gz)
(base) $ for f in ${ftps[@]}; do wget ${f}; done
```

Two things: Remember to always use the three terms for, do, and done, each separated by ;. Also note that you don’t need to use quotation marks around characters the way you do in many other programming languages.

One final note: I constantly use loops to split up jobs across chromosomes (or sequence chunks, whatever works for you) since parallelizing your work is important for speed. Here I use BCFTools to make pileups from an alignment chromosome by chromosome. Check out [their documentation](https://samtools.github.io/bcftools/bcftools.html) if you want to learn more about the command, options, and flags.

Looks like this:
```
for c in {1..22}; do bcftools mpileup -b -o mypileup.${c}.bam myalignment.bam ${c}; done
```

That's the end of Part 2. Be sure to check out Part 3 on Analysis Tools! 
