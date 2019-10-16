# SV_pipeline
a framework for DNA seq mapping and call structural variant for WGS data.

Recently WGS processing standards were proposed by Regier A. et al. 2017 Nature Comm. Here this pipeline includes these guiding principles for future integrateion of different WGS bam file from different source of public database, likes TOPMed.
SV call was made by [smoove](https://github.com/brentp/smoove)
Most tools are already satisfied at ACCRE(under CQS group at VUMC), except thess two need to be installed
* [sambamba](https://github.com/biod/sambamba)
* [snakemake](https://snakemake.readthedocs.io/en/stable/getting_started/installation.html)

## Quick start
    ```
    Program: sv_pipeline
    Version: 0.1
    Author:  Youhuang Bai (youhuang.bai.1@vumc.org)
    usage:   sv_pipeline <command> [options]
    command: mapping     align FASTQ files with BWA-MEM, add MC MQ Tags with samblaster and sort by name
             refine      apply gatk recalibration to it 
             call        SV call with lumpy in smoove
             merge       merge SVs with smoove
             genotype    genotype SVs with svtyper in smoove
             combine     combine all samples into one file
             annotate    annotate with genome annotation
    ```
## 1. Install
```
git clone https://github.com/biobai/sv_pipeline.git
cd sv_pipeline
chmod a+x bin/sv_pipeline
```
## 2. RUN

mapping test fastq file to hg19(default)/hg38 genome

    ```
    sv_pipeline mapping -o test ./test.R1.fq.gz ./test.R2.fq.gz
    ```
    
    
refine the bam file

    ```
    sv_pipeline refine -o test -O ./ -V hg38 -i test.bam
    ```
    
smoove call

    ```
    sv_pipeline call -i ./test.mkDup.sorted.recal.bam -o test -V hg38
    ```
    
    
smoove merge

    ```
    sv_pipeline merge -o test -O ./ -i *.genotyped.vcf.gz -V hg38
    ```

smoove genotype

    ```
    sv_pipeline genotype -v test.sites.vcf.gz -i ../2_bamMerge_hg38/result/SL345196/SL345196.mkDup.sorted.recal.bam -o SL345196 -O ./ -V hg38
    ```

## 3. run it on snakemake version