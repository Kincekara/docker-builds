FROM ubuntu:noble AS app

ARG AMRFINDER_VER="4.0.23"
ARG AMRFINDER_DB_VER="2025-06-03.1"
ARG BLAST_VER="2.16.0"

LABEL base.image="ubuntu:noble"
LABEL dockerfile.version="1"
LABEL software="NCBI AMRFinderPlus"
LABEL software.version="${AMRFINDER_VER}"
LABEL description="NCBI resistance gene detection tool"
LABEL website="https://github.com/ncbi/amr"
LABEL license="https://github.com/ncbi/amr/blob/master/LICENSE"
LABEL maintainer="Kelsey Florek"
LABEL maintainer.email="kelsey.florek@slh.wisc.edu"
LABEL maintainer2="Curtis Kapsak"
LABEL maintainer2.email="kapsakcj@gmail.com"
LABEL maintainer3="Anders Goncalves da Silva"
LABEL maintainer3.email="andersgs@gmail.com"
LABEL maintainer4="Erin Young"
LABEL maintainer4.email="eriny@utah.gov"
LABEL maintainer5="Holly McQueary"
LABEL maintainer5.email="holly.c.mcqueary@mass.gov"

# ncbi-blast+ installed via apt is v2.12.0 - DISABLING so that we can manually install a more recent version
# see here for reason why I'm manualy installing 2.16.0 instead of using apt-get: https://github.com/ncbi/amr/releases/tag/amrfinder_v3.11.8

# hmmer installed via apt is v3.4
# removed because likely unnecessary since we are not compiling from source: make g++
# libgomp1 required for makeblastdb
RUN apt-get update && apt-get install -y --no-install-recommends \
  ca-certificates \
  wget \ 
  curl \
  libgomp1 \
  hmmer \
  procps \
  gzip && \
  apt-get autoclean && \
  rm -rf /var/lib/apt/lists/*

# download and install amrfinderplus pre-compiled binaries; make /data
RUN mkdir amrfinder && cd /amrfinder && \
  echo "downloading amrfinderplus v${AMRFINDER_VER} pre-compiled binaries from GitHub..." && \
  wget -q https://github.com/ncbi/amr/releases/download/amrfinder_v${AMRFINDER_VER}/amrfinder_binaries_v${AMRFINDER_VER}.tar.gz && \
  tar zxf amrfinder_binaries_v${AMRFINDER_VER}.tar.gz && \
  rm -v amrfinder_binaries_v${AMRFINDER_VER}.tar.gz && \
  mkdir -v /data

# install ncbi-blast linux binaries
RUN echo "downloading ncbi-blast-${BLAST_VER}+ linux binaries from NCBI FTP..." && \
  wget -q https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/${BLAST_VER}/ncbi-blast-${BLAST_VER}+-x64-linux.tar.gz && \
  tar -xzf ncbi-blast-${BLAST_VER}+-x64-linux.tar.gz && \
  rm -v ncbi-blast-${BLAST_VER}+-x64-linux.tar.gz

# set PATH and locale settings for singularity compatibiliity, set amrfinder and manually-installed blast as higher priority in PATH
ENV PATH="/amrfinder:/ncbi-blast-${BLAST_VER}+/bin:$PATH" \
  LC_ALL=C

# download databases and index them
# done in this manner to pin the database version instead of pulling the latest version with `amrfinder -u` 
# softlink is required for `amrfinder -l` and typical `amrfinder` use cases to work properly
RUN mkdir -p /amrfinder/data/${AMRFINDER_DB_VER} && \
  echo "Downloading amrfinder databases and indexing them now..." && \
  wget -q -P /amrfinder/data/${AMRFINDER_DB_VER} ftp://ftp.ncbi.nlm.nih.gov/pathogen/Antimicrobial_resistance/AMRFinderPlus/database/4.0/${AMRFINDER_DB_VER}/* && \
  amrfinder_index /amrfinder/data/${AMRFINDER_DB_VER} && \
  ln -s /amrfinder/data/${AMRFINDER_DB_VER} /amrfinder/data/latest

# set final working directory
WORKDIR /data

# default command is to print help options
CMD [ "amrfinder", "--help" ]

## Test stage
FROM app AS test

# run recommended tests from amrfinder; check that stx2a_operon is detected in outputs, meaning stxtyper ran correctly
RUN cd /amrfinder && \
  bash test_amrfinder.sh . && \
  echo && echo "Checking for stx2a_operon string in test outputs..." && \
  grep 'stx2a_operon' test_both.got

# run amrfinder on Salmonella, without and with --organism option
RUN echo "Downloading Salmonella genome (GCA_010941835.1_PDT000052640.3) for testing now..." && \
  wget -q https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/010/941/835/GCA_010941835.1_PDT000052640.3/GCA_010941835.1_PDT000052640.3_genomic.fna.gz  && \
  amrfinder --threads 4 --plus --nucleotide GCA_010941835.1_PDT000052640.3_genomic.fna.gz --output test1.txt && \
  amrfinder --threads 4 --plus --nucleotide GCA_010941835.1_PDT000052640.3_genomic.fna.gz --organism Salmonella --output test2.txt && \
  head test1.txt test2.txt

# run amrfinder on Klebesiella oxytoca using --organism/-O flag
RUN echo "Downloading Klebsiella oxytoca genome (GCA_003812925.1_ASM381292v1) for testing now..." && \
  wget -q https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/003/812/925/GCA_003812925.1_ASM381292v1/GCA_003812925.1_ASM381292v1_genomic.fna.gz && \
  amrfinder --threads 4 --plus --name GCA_003812925.1 -n GCA_003812925.1_ASM381292v1_genomic.fna.gz  -O Klebsiella_oxytoca -o GCA_003812925.1-amrfinder.tsv && \
  head GCA_003812925.1-amrfinder.tsv

# list database version and available --organism options; check stxtyper & blast versions
RUN amrfinder -l && \
  stxtyper --version && \
  blastn -version