# CCS container

Main tool: [CCS](https://ccs.how/)
  
Code repository: https://github.com/PacificBiosciences/ccs

Additional tools:
- ccs-alt

Basic information on how to use this tool:
- executable: ccs
- help: -h, --help
- version: --version
- description: 
>ccs combines multiple subreads of the same SMRTbell molecule using a statistical model to produce one highly accurate consensus sequence, also called a HiFi read, along with base quality values. This tool powers the Circular Consensus Sequencing workflow in SMRT Link for Sequel and Sequel II platform. On Sequel IIe and Revio platforms, HiFi generation is perfomed on the instrument.

 
Full documentation: https://ccs.how/

## Example Usage

```bash
ccs movie.subreads.bam movie.ccs.bam
```
