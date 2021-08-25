---
description: Star 2Pass Mapping
---

# STAR2-GATK Variants Calling



STAR2 INDEXING -&gt; 원타



STAR2 2PASS MAPPING

```text
$ STAR --genomeDir ./genome/ --readFilesIn ./hfsips1_p23_1.fq.gz ./hfsips1_p23_2.fq.gz --runThreadN 64 --readFilesCommand zcat --outFilterType BySJout --alignSJoverhangMin 8 --alignSJDBoverhangMin 1 --outFilterMismatchNmax 999 --outSAMtype BAM SortedByCoordinate --outSAMattrRGline ID:hfsips1_p23 LB:library PL:illumina PU:machine SM:hfsips1_p23 --twopassMode Basic
```

{% embed url="https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-" %}





