# Snakemake workflow: CFB RNAseq Prokaryote

[![Snakemake](https://img.shields.io/badge/snakemake-≥5.5.4-brightgreen.svg)](https://snakemake.bitbucket.io)
[![Build Status](https://travis-ci.org/snakemake-workflows/cfb_rnaseq_prokaryote.svg?branch=master)](https://travis-ci.org/snakemake-workflows/cfb_rnaseq_prokaryote)

Not meant as a general purpose pipeline! 

This is a simple RNAseq pipeline constructed to deal with a single sample. Pipeline steps are as follows:
- Generation of HISAT2 indexes for mapping (this is not centralized as hisat indexes might be generated by different versions of hisat2).  
- Trimming of reads with trimmomatic.
- Mapping of reads to a given reference with HISAT2.
- Generation of read counts with subreads/featureCounts.


## Requirements for analysis:

- A fasta sequence for the reference.
- A gff annotation for the reference.
 
## Notes on parameters:

Currently, several parameters are fixed (eg: for trimmer with trimmomatic, mask generator with samtools, ...) through config file. Modify these as the sequencing process changes.
   
## Authors

* Emre Ozdemir (@meono)

## Usage

    snakemake --snakefile /path_to_this_pipeline/Snakefile \
              --configfile /path_to_this_pipeline/config.yaml \
              --directory /path_to_analysis_folder \
              --config sample_id=sample \
                       raw_read_path=/path_to_reads \
                       reference_id=reference_id \
                       reference_table=/path_to_reference_table \
              --use-conda
              --conda-prefix /path_to_conda_environments 
              --cores 4

Most of the required parameters are set from command line though `--config` or specific flags:
 
 - `directory`: where analysis output will be written.
 - `sample_id`: sample to be analyzed. Should match sample names part of read files.
 - `raw_read_path`: path to reads folder. Either relative to `workdir` or an absolute path.
 - `reference_id`: a reference id to specify an entry in `reference_table`.
 - `reference_table`: a yaml file with details on paths to reference fasta and gff to be used.
 - `conda-prefix`: setting this to a specific path in a container will eliminate the need to create environments every time the pipeline is run.   
  

## Testing

Tests cases are in the subfolder `.test`. They are automatically executed via continuous integration with Travis CI.
