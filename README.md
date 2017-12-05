# metagenomic_pipeline
Description of the metagenomic pipeline we have for analysis of 16S and 18S amplicon NGS.

At the Microbial Genomics Laboratory we have developed a simplified pipeline to analyse NGS sequences comming from the Illumina platform. The aim of this pipeline is to have a very simple, highly standarised, workflow to quickly process many samples. To achieve this, we have worked out the parameters that are enough to get a good result, of course, if you want to change some of this parameters, you'll have to mess with the code and since it is in bash it is quite straithforward.

Not all script are already uploades here in Github, but soon will be.

The pipeline has four simple bash scripts that have to be run in the following order:

1. [pair-end_cleaner](https://github.com/GenomicaMicrob/pair-end_cleaner) to unzip, clean, assemble, and convert illumina pair-end fastq files in all subdirectories for 16S amplicon data (V3, V4 and V3-V4 regions).
2. chimera_detector to detect and eliminate chimeric sequences based on their similarity with a selected database.
3. [mg_classifier](https://github.com/GenomicaMicrob/mg_classifier), a very fast script that taxonomically classifies sequences.
4. metagenomic_reporter a script that collects reports and log files from the previous three scripts and produces a comprehensive report.

## Dependencies

Each script describe the dependencies they need, but basically, all of them are already incorporated in any [Ubuntu](https://www.ubuntu.com) distro. You will also need:

1. [CUTADAPT](https://github.com/marcelm/cutadapt)
2. [PEAR](https://sco.h-its.org/exelixis/web/software/pear/doc.html)
3. [VSEARCH](https://github.com/torognes/vsearch)
4. And several databases that you can download from [figshare](https://figshare.com/account/home#/projects/20254).

## Databases

You need to have the preformatted databases in a subdirectory named `/mg_pipeline/databases/`

Available databases are:
### 16S
- SILVA ver. 128
- RDP ver. 11.5
- RDP V3-V4 ver. 11.5
### 18S
- Protista ver. 4.5

We recommend getting the [EzBioCloud](http://www.ezbiocloud.net/resources/pipelines) curated database, but since it is not publicly available (although it is free for academia), we cannot distributed it. If you get it, then you´ll have to formatted accordingly. You can use our script [db_reformatter.sh](https://github.com/GenomicaMicrob/db_reformatter)

Since the RDP database is to big and consumes a lot of time and memory, we have only the V3-V4 regions cut out from the original db, this is a lot faster, of course it only works if your sequences are from the V3 and/or V4 16S rRNA regions. Also the RDP db has many sequences (362,293) duplicated, it is now dereplicated (only one sequence of the identical ones was kept).  

A much quicker analysis is done if the databases are converted to [UDB](https://www.drive5.com/usearch/manual/udb_files.html) format (a UDB file is a database file that contains the sequences and a [k-mer index](https://en.wikipedia.org/wiki/K-mer) for those sequences). These type of databases a considerably bigger than the fasta file used to generate it (56 Mb vs. 471 Mb for the SILVA-128 db), therefore, it is best to download the fasta file and then convert it.

To convert the fasta file with vsearch 2.5.0 just type:
`vsearch --makeudb_usearch file.fasta --output file.udb`

Keep both the fasta and the udb files, since mg_classifer can use both, but chimera_detector only the fasta file. This inconvenience has to be with the algorithm that vsearch uses to identify chimeric sequences.
