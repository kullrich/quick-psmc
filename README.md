# Quick-PSMC

## Introduction

A quick pipeline to infer population size history starting from
FASTQ files or directly from already mapped BAM files like human
history data (denisova and neandertal).

BAM sources:

- https://cdna.eva.mpg.de/neandertal
- https://cdna.eva.mpg.de/denisova

Requirements:

- FASTQ files
- Reference

## Install

define base directory
```
export BASEDIR=$HOME/opt
mkdir -p "$BASEDIR"
cd "$BASEDIR"
```

psmc (https://github.com/lh3/psmc)
```
git clone https://github.com/lh3/psmc
cd "$BASEDIR/psmc"
make
cd "$BASEDIR/psmc/utils"
make
```

minimap2 (https://github.com/lh3/minimap2)
```
cd "$BASEDIR"
git clone https://github.com/lh3/minimap2
cd "$BASEDIR/minimap2"
# see install section https://github.com/lh3/minimap2#install
# 64 bit ARM
make arm_neon=1 aarch64=1
# x86-64
#make
```

seqtk (https://github.com/lh3/seqtk)
```
cd "$BASEDIR"
git clone https://github.com/lh3/seqtk
cd "$BASEDIR/seqtk"
make
```

bam2iupac (https://github.com/kullrich/bam2iupac)
```
cd "$BASEDIR"
git clone --recursive https://github.com/kullrich/bam2iupac
cd "$BASEDIR/bam2iupac"
make
```

## Example

To mask problematic regions, like coverage depth,
use the options provided by `bam2iupac`:
```
  --minMQ      Minimum mapping quality (default: 0)
  --minBQ      Minimum base quality (default: 0)
  --minC       Minimum coverage (default: 0)
  --maxC       Maximum coverage (default: 9999)
```

### FASTQ >>> BAM >>> IUPAC >>> PSMC

create working directory
```
export WORKDIR=$HOME/example_fastq
mkdir -p "$WORKDIR"
cd "$WORKDIR"
```

### BAM >>> IUPAC >>> PSMC

Example is given just for one chromosome, please before running PSMC
concatenate the individual chromosome files into one `psmcfa` file, like:
```
cat chr1.psmcfa chr2.psmcfa chr3.psmcfa > genome.psmcfa
```

create working directory
```
export WORKDIR=$HOME/example_bam
mkdir -p "$WORKDIR"
cd "$WORKDIR"
```

Extract chromosomes from already mapped BAM file
```
"$BASEDIR/bam2iupac/bam2iupac" --b \
https://cdna.eva.mpg.de/denisova/alignments/T_hg19_1000g.bam \
--n Denisova \
--r 1:0-0 > Denisova_chr1.fasta

"$BASEDIR/bam2iupac/bam2iupac" --b \
https://cdna.eva.mpg.de/neandertal/altai/AltaiNeandertal/bam/AltaiNea.hg19_1000g.1.dq.bam \
--n AltaiNea \
--r 1:0-0 > AltaiNea_chr1.fasta
```

Convert IUPAC FASTA to FASTQ
```

```
