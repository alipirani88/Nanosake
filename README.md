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

