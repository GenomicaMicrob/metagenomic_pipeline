## Databases
Several databases are needed for some of the scripts of this pipeline, and they need to be formatted in a certain way.
You need to have the preformatted databases in a subdirectory named `/mg_pipeline/databases/`

Available databases in [figshare](https://figshare.com/account/home#/projects/27328) are:
### 16S or 18S
- [SILVA ver. 128](https://www.arb-silva.de)
- Ribosomal Database Proyect [RDP ver. 11.5](https://rdp.cme.msu.edu).
### 16S
- RDP V3-V4 ver. 11.5 Trimmed for the 16S V3 and V4 regions.
### 18S
- Protist Ribosomal Reference database (PR2) [Protista ver. 4.7.2](https://github.com/vaulot/pr2_database).

We recommend getting the [EzBioCloud](http://www.ezbiocloud.net/resources/pipelines) curated database, but since it is not publicly available (although it is free for academia), we cannot distributed it. If you get it, then you´ll have to formatted accordingly. You can use our script [db_reformatter.sh](https://github.com/GenomicaMicrob/db_reformatter)

Since the RDP database is to big and consumes a lot of time and memory, we have only the V3-V4 regions cut out from the original db, this is a lot faster, of course it only works if your sequences are from the V3 and/or V4 16S rRNA regions. Also the RDP db has many sequences (362,293) duplicated, it is now dereplicated (only one sequence of the identical ones was kept).  

A much quicker analysis is done if the databases are converted to [UDB](https://www.drive5.com/usearch/manual/udb_files.html) format (a UDB file is a database file that contains the sequences and a [k-mer index](https://en.wikipedia.org/wiki/K-mer) for those sequences). These type of databases a considerably bigger than the fasta file used to generate it (56 Mb vs. 471 Mb for the SILVA-128 db), therefore, it is best to download the fasta file and then convert it. UDB databases can be used only with [mg_classifier](https://github.com/GenomicaMicrob/mg_classifier); [chimera_detector](https://github.com/GenomicaMicrob/chimera_detector) only accepts fasta files.

To convert the fasta file with [vsearch](https://github.com/torognes/vsearch) 2.5.0 just type:
`vsearch --makeudb_usearch file.fasta --output file.udb`

Keep both the fasta and the udb files, since mg_classifer can use both, but chimera_detector only the fasta file. This inconvenience has to be with the algorithm that vsearch uses to identify chimeric sequences.

## Download

1. Download the databases from [figshare](https://figshare.com/articles/mg_pipeline_databases/5675242): `wget https://ndownloader.figshare.com/files/9924862` Since the databases are quite big (584 Mb) it might take a while to download. The four databases are compressed into one file.
2. Rename the file, Figshare assigns just a number to the downloaded file, so it is best to give it a meaningful name.: `mv 9924862 mg_pipeline_dbs.tar.gz` 
3. Uncompress them: `tar xzf mg_pipeline_dbs.tar.gz`
4. If the needed directory was not already created, create it to house the mg_pipeline script and databases: `sudo mkdir /opt/mg_pipeline/ /opt/mg_pipeline/databases` for this you'll have to be a super user with `sudo`. You can use a different directory, but the scripts points to this one and you'll have also to mess with the code of the scripts to point to the other directory so, better stick to this.
5. Move the dbs to their appropiate directory: `sudo mv *.fasta /opt/mg_pipeline/databases`
