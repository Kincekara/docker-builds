# IRMA container

Main tool: [IRMA](https://wonder.cdc.gov/amd/flu/irma/)

Basic information on how to use this tool:

- executable: `IRMA`
- help: `IRMA`
- version: `IRMA` (version info is output on first line)
- description: IRMA was designed for the robust assembly, variant calling, and phasing of highly variable RNA viruses. Currently IRMA is deployed with modules for influenza, ebolavirus and coronavirus.

Additional information:

The IRMA team at the CDC maintain a docker image as well at [https://hub.docker.com/r/cdcgov/irma](https://hub.docker.com/r/cdcgov/irma). The StaPH-B docker image is not maintained by IRMA developers at the CDC, so if users encounter issues with this docker image specifically they should file an issue on this github repo and do not contact CDC for support.

Full documentation: [https://wonder.cdc.gov/amd/flu/irma/](https://wonder.cdc.gov/amd/flu/irma/)

## Example Usage

```bash
Usage:
#(PAIRED-END):   IRMA <MODULE|MODULE-CONFIG> <R1.fastq.gz|R1.fastq> <R2.fastq.gz|R2.fastq> [path/to/]<sample_name> [options]
#(SINGLE-END):   IRMA <MODULE|MODULE-CONFIG> <fastq|fastq.gz> [path/to/]<sample_name> [options]

IRMA FLU Sample1_R1.fastq.gz Sample1_R2.fastq.gz Sample1

IRMA EBOLA Patient1_R1.fastq Patient1_R2.fastq MyPatient

IRMA FLU-utr Sample1_R1.fastq.gz Sample1_R2.fastq.gz Sample1WithUTRs

# Single read files:
# USAGE:	IRMA <MODULE-config> <fastq/fastq.gz> <sample_name>

IRMA FLU SingleEndIllumina.fastq.gz MyIlluminaSample

IRMA FLU-pacbio ccs_reads.fastq MyPacBioSample

IRMA FLU-pgm pgm_reads.fastq MyIonTorrentSample
```
