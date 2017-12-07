# metagenomic_pipeline
Description of the metagenomic pipeline we have for analysis of 16S and 18S amplicon NGS.

At the Microbial Genomics Laboratory we have developed a simplified pipeline to analyse NGS sequences comming from the Illumina platform. The aim of this pipeline is to have a very simple, highly standarised, workflow to quickly process many samples. To achieve this, we have worked out the parameters that are enough to get a good result, of course, if you want to change some of this parameters, you'll have to mess with the code and since it is in bash it is quite straithforward.

Not all script are already uploades here in Github, but soon will be.

The pipeline has four simple bash scripts that have to be run in the following order:

1. [pair-end_cleaner](https://github.com/GenomicaMicrob/pair-end_cleaner) to unzip, clean, assemble, and convert illumina pair-end fastq files in all subdirectories for 16S amplicon data (V3, V4 and V3-V4 regions).
2. [chimera_detector](https://github.com/GenomicaMicrob/chimera_detector) to detect and eliminate chimeric sequences based on their similarity with a selected database.
3. [mg_classifier](https://github.com/GenomicaMicrob/mg_classifier), a very fast script that taxonomically classifies sequences.
4. metagenomic_reporter a script that collects reports and log files from the previous three scripts and produces a comprehensive report.

## Dependencies

Each script describe the dependencies they need, but basically, all of them are already incorporated in any [Ubuntu](https://www.ubuntu.com) distro. You will also need the following scripts, so please install them first:

1. [CUTADAPT](https://github.com/marcelm/cutadapt)
2. [PEAR](https://sco.h-its.org/exelixis/web/software/pear/doc.html)
3. [VSEARCH](https://github.com/torognes/vsearch)
4. And several [databases](https://github.com/GenomicaMicrob/metagenomic_pipeline/blob/master/databases.md)

## Databases

You need to have the preformatted databases in a subdirectory named `/opt/mg_pipeline/databases/`

More info about the [databases here](https://github.com/GenomicaMicrob/metagenomic_pipeline/blob/master/databases.md).
