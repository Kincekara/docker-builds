# Racon + Minimap2 Container

This container combines [Racon](https://github.com/lbcb-sci/racon) and [Minimap2](https://github.com/lh3/minimap2) for efficient polishing and alignment in long-read assembly workflows.

## Tools Included
- **Racon**: Polishes long-read assemblies.
- **Minimap2**: Generates alignments for use with Racon and other polishing tools.

### Code Repositories
- Racon: [GitHub Repository](https://github.com/lbcb-sci/racon)
- Minimap2: [GitHub Repository](https://github.com/lh3/minimap2)

## Basic Information

### Racon
- **Executable**: `racon`
- **Help**: `racon -h`
- **Version**: `racon -v`
- **Description**: 
  > Racon is intended as a standalone consensus module to correct raw contigs generated by rapid assembly methods that do not include a consensus step.

### Minimap2
- **Executable**: `minimap2`
- **Help**: `minimap2 -h`
- **Version**: `minimap2 --version`
- **Description**: 
  > Minimap2 is a versatile sequence alignment program that aligns DNA or mRNA sequences against a large reference database.

## Example Usage

```bash
# Racon general
racon <sequences> <overlaps> <target sequences>

# more specific
racon --match 8 --mismatch -6 --gap -8 --window-length 500 --threads {threads} {input.reads} {input.alignment} {input.assembly}

#Combined Racon and Minimap2 Workflow

#Align Reads to Assembly Using Minimap2:
minimap2 -x map-ont <assembly.fasta> <reads.fastq> > overlaps.paf

#polish assembly using Racon
racon --threads {threads} <reads.fastq> overlaps.paf <assembly.fasta> > polished_assembly.fasta
```

