---
description: Star 2Pass Mapping
---

# STAR2-GATK Variants Calling



STAR2 INDEXING -&gt; 원타



STAR2 2PASS MAPPING

```text
$ STAR --genomeDir /disk1/oneomics_analysis/rna_rnaseq_pipeline/STAR_Align_referece/ --runThreadN 6 --runMode alignReads --readFilesIn ./sample_hfsips1-p23_1_val_1.fq ./sample_hfsips1-p23_2_val_2.fq --outFileNamePrefix ./STAR_htseqcount_p22 --outSAMtype BAM SortedByCoordinate --outSAMunmapped Within --outSAMattributes Standard --outSAMattrRGline ID:hfsips_p23 SM:hfsips_p23 PL:ILLUMINA --quantMode GeneCounts
```

{% embed url="https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-" %}





