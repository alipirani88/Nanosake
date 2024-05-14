# Nanosake
A Snakemake workflow to basecall, quality control and assemble Nanopore data.

## Installation

> Clone the github directory onto your system.

```
git clone https://github.com/alipirani88/Nanosake.git
```

> Load bioinformatics and snakemake module from Great Lakes modules.

```
module load Bioinformatics
```

```
module load snakemake singularity
```

### Customize the config.yaml according to your samples
Customise snakemake configuration settings in config/config.yaml file as per your needs and create a sample list file in config/samples.tsv

## Quick start

### Run Nanosake on a set of samples.

```
snakemake -s Nanosake.smk -p --use-conda -j 999 --cluster "sbatch -A {cluster.account} -p {cluster.partition} -N {cluster.nodes}  -t {cluster.walltime} -c {cluster.procs} --mem-per-cpu {cluster.pmem}" --conda-frontend conda --cluster-config config/cluster.json --configfile config/config.yaml --latency-wait 1000
```

![Alt text](./dag.svg)

### Summary

The Nanosake workflow can be split into two parts - based on the type of data it uses first to assemble the reads.  

- It generates various pre and post Filtlong QC plots (performs quality/length trimming with Filtlong) for ONT long reads using Nanoplot.
- It then takes a long reads first approach and assembles the clean ONT long reads with Flye assembler.
- Flye assembly is then polished with long reads using Medaka, followed by polishing with clean trimmed illumina reads using Polypolish.
- For the short illumina read first approach, Long reads are assembled in hybrid mode using Unicycler, followed by polishing with short reads using Polypolish.
- The Flye-only, Flye+Medaka+Polypolish, Unicycler+Polypolish assembles the passes through Prokka for annoatation, BUSCO for assembly completeness statistics and QUAST for assembly statistics.

All the final assembly and annotation are finally placed in prokka directory.


