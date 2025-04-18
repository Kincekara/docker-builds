# TBProfiler Container

Main tool: [TBProfiler](https://github.com/jodyphelan/TBProfiler)

The pipeline aligns reads to the H37Rv reference using bowtie2, BWA or minimap2 and then calls variants using bcftools. These variants are then compared to a drug-resistance database. It also predicts the number of reads supporting drug resistance variants as an insight into hetero-resistance.

## Database

This tool relies on a database to run. The version (AKA git commit hash) of the database that is included in the docker image is `00a0db4e`. This is from the GitHub repository https://github.com/jodyphelan/tbdb. This can be confirmed in the json file: `/opt/conda/share/tbprofiler/tbdb.variables.json`:

```bash
$ grep 'commit' /opt/conda/share/tbprofiler/tbdb.variables.json
{"db-schema-version": "2.0.0", "snpEff_db": "Mycobacterium_tuberculosis_h37rv", "drugs": ["rifampicin", "isoniazid", "ethambutol", "pyrazinamide", "moxifloxacin", "levofloxacin", "bedaquiline", "delamanid", "pretomanid", "linezolid", "streptomycin", "amikacin", "kanamycin", "capreomycin", "clofazimine", "ethionamide", "para-aminosalicylic_acid", "cycloserine"], "tb-profiler-version": ">=6.6.0,<7.0.0", "version": {"name": "tbdb", "repo": "https://github.com/jodyphelan/tbdb.git", "branch": "HEAD", "commit": "00a0db4e", "status": "clean", "author": "Jody Phelan", "date": "Sun Feb 16 11:09:04 2025 +0100", "db-schema-version": "2.0.0", "tb-profiler-version": ">=6.6.0,<7.0.0"}, "amplicon": false, "files": {"ref": "tbdb.fasta", "gff": "tbdb.gff", "bed": "tbdb.bed", "json_db": "tbdb.dr.json", "variables": "tbdb.variables.json", "spoligotype_spacers": "tbdb.spoligotype_spacers.txt", "spoligotype_annotations": "tbdb.spoligotype_list.csv", "bedmask": "tbdb.mask.bed", "rules": "tbdb.rules.yml", "barcode": "tbdb.barcode.bed"}}
```

Additionally you can run the command `tb-profiler list_db` to list the same information

```bash
$ tb-profiler list_db
tbdb    00a0db4e        Jody Phelan     Sun Feb 16 11:09:04 2025 +0100  /opt/conda/share/tbprofiler/tbdb]
```

## Additional included tools/dependencies

- bcftools 1.21
- bedtools 2.31.1
- bwa 0.7.18-r1243-dirty
- delly 1.2.6 (the more recent version 1.3.1 did not allow for the conda environment to resolve; pathogen-profiler has specifically pinned v1.2.6. More info here: https://github.com/jodyphelan/TBProfiler/issues/393#issuecomment-2452076859 and here: https://github.com/bioconda/bioconda-recipes/blob/master/recipes/pathogen-profiler/meta.yaml)
- freebayes 1.3.6
- gatk4 4.6.1.0
- kmc 3.2.4
- minimap2 2.28-r1209
- parallel 20241222
- pathogen-profiler 4.8.0
- perl 5.32.1
- python 3.12.9
- trimmomatic 0.39
- samclip 0.4.0
- samtools 1.21
- snpeff 5.2
- tqdm 4.67.1

## Example Usage

Run whole pipeline on Illumina paired-end reads:

```bash
tb-profiler profile -1 ERR1664619_1.fastq.gz -2 ERR1664619_2.fastq.gz -t 4 -p ERR1664619 --txt
```

Make alternative database:

```bash
tb-profiler create_db --prefix <new_library_name>
tb-profiler load_library --prefix <new_library_name>
```

## Updates

Release 5.0.1 implemented sqlite3 database locking with https://py-filelock.readthedocs.io/en/latest/index.html. This should fix issues using it over network filing systems (NFS). For more information, official documentation can be found [here.](https://jodyphelan.gitbook.io/tb-profiler/)
