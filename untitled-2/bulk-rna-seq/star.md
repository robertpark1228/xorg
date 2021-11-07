---
description: Star 2Pass Mapping
---

# \[RNA-Variants Calling] STAR2-GATK Variants Calling

## RNA Variants Calling

### RNA Variants Calling 의 목적

RNA Variants Calling 은 해석에 주안점을 두고 접근 해야 합니다.

![](<../../.gitbook/assets/image (142).png>)





![](<../../.gitbook/assets/image (141).png>)







![](<../../.gitbook/assets/image (137).png>)

![](<../../.gitbook/assets/image (138).png>)

![](<../../.gitbook/assets/image (139).png>)

![](<../../.gitbook/assets/image (143).png>)





## RNAseq short variant discovery (SNPs + Indels), ([https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-](https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-))

![](<../../.gitbook/assets/image (136).png>)

### STAR2 2PASS MAPPING

```
STAR --genomeDir ./genome/ --readFilesIn ./hfsips1_p23_1.fq.gz ./hfsips1_p23_2.fq.gz --runThreadN 64 --readFilesCommand zcat --outFilterType BySJout --alignSJoverhangMin 8 --alignSJDBoverhangMin 1 --outFilterMismatchNmax 999 --outSAMtype BAM SortedByCoordinate --outSAMattrRGline ID:hfsips1_p23 LB:library PL:illumina PU:machine SM:hfsips1_p23 --twopassMode Basic
```



{% embed url="https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-" %}

### MarkDuplicates ([https://gatk.broadinstitute.org/hc/en-us/articles/4404604742683-MarkDuplicates-Picard-](https://gatk.broadinstitute.org/hc/en-us/articles/4404604742683-MarkDuplicates-Picard-))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar MarkDuplicates -I ./Aligned.sortedByCoord.t.bam -O ./Aligned.sortedByCoord.out.mark.bam -M ./Aligned.sortedByCoord.out.bam.txt --REMOVE_DUPLICATES TRUE
```

{% hint style="info" %}
WGRS 동일
{% endhint %}

{% content-ref url="../../untitled/whole-genome-sequencing.md" %}
[whole-genome-sequencing.md](../../untitled/whole-genome-sequencing.md)
{% endcontent-ref %}



### SplitNCigarREADS([https://gatk.broadinstitute.org/hc/en-us/articles/4404604888731-SplitNCigarReads](https://gatk.broadinstitute.org/hc/en-us/articles/4404604888731-SplitNCigarReads))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar SplitNCigarReads -R ./Homo_sapiens.GRCh38.dna_rm.primary_assembly.fa -I ./Aligned.sortedByCoord.out.mark.bam -O ./Aligned.sortedByCoord.out.mark.splitNCigar.bam
```

{% hint style="info" %}

{% endhint %}

### BaseQualityScoreRecalibrator([https://gatk.broadinstitute.org/hc/en-us/articles/4404604765467-BaseRecalibrator](https://gatk.broadinstitute.org/hc/en-us/articles/4404604765467-BaseRecalibrator))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar BaseRecalibrator -I ./Aligned.sortedByCoord.out.mark.splitNCigar.bam -R ./Homo_sapiens.GRCh38.dna_rm.primary_assembly.fa --known-sites ./1000G_omni2.5.hg38.vcf.gz -O Aligned.sortedByCoord.out.mark.splitNCigar.bam.table
```

{% hint style="info" %}
WGRS 동일
{% endhint %}

### ApplyBQSR ([https://gatk.broadinstitute.org/hc/en-us/articles/4404604653979-ApplyBQSR](https://gatk.broadinstitute.org/hc/en-us/articles/4404604653979-ApplyBQSR))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar ApplyBQSR -R ./Homo_sapiens.GRCh38.dna_rm.primary_assembly.fa -I ./Aligned.sortedByCoord.out.mark.splitNCigar.bam --bqsr-recal-file recal_data.table -O ./Aligned.sortedByCoord.out.mark.splitNCigar.bqsr.bam
```

{% hint style="info" %}
WGRS 동일
{% endhint %}

### HaplotypeCaller([https://gatk.broadinstitute.org/hc/en-us/articles/360037225632-HaplotypeCaller](https://gatk.broadinstitute.org/hc/en-us/articles/360037225632-HaplotypeCaller))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar HaplotypeCaller -R ./Homo_sapiens.GRCh38.dna_rm.primary_assembly.fa -I ./Aligned.sortedByCoord.out.mark.splitNCigar.bqsr.bam -O ./Aligned.sortedByCoord.out.mark.splitNCigar.bqsr.bam.g.vcf
```

{% hint style="info" %}
WGRS 동일
{% endhint %}

### GenotypeGVCFs([https://gatk.broadinstitute.org/hc/en-us/articles/360037057852-GenotypeGVCFs](https://gatk.broadinstitute.org/hc/en-us/articles/360037057852-GenotypeGVCFs))

```
$ java -jar /DAS3/oneomics_analysis/alpha_pipeline/modules/gatk.jar GenotypeGVCFs -V ./Aligned.sortedByCoord.out.mark.splitNCigar.bqsr.bam.g.vcf -O Aligned.sortedByCoord.out.mark.splitNCigar.bqsr.bam.vcf -R ./Homo_sapiens.GRCh38.dna_rm.primary_assembly.fa
```

{% hint style="info" %}
* We have not yet fully tested the interaction between the GVCF-based calling or the multisample calling and the RNAseq-specific functionalities. Use those in combination at your own risk.
{% endhint %}



1\) [https://www.researchgate.net/figure/Comparison-between-WES-data-and-RNA-Seq-data-This-image-shows-the-motivation-and-the\_fig1\_275525746](https://www.researchgate.net/figure/Comparison-between-WES-data-and-RNA-Seq-data-This-image-shows-the-motivation-and-the\_fig1\_275525746)\
2\)[https://www.nature.com/articles/s41467-020-20573-7](https://www.nature.com/articles/s41467-020-20573-7)\
3\) [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3791257/#:\~:text=The%20ability%20to%20independently%20call,by%20using%20RNA%2Dseq%20data.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3791257/#:\~:text=The%20ability%20to%20independently%20call,by%20using%20RNA%2Dseq%20data.)\
4\) [https://www.nature.com/articles/s41598-021-89938-2](https://www.nature.com/articles/s41598-021-89938-2)\
5\)  [https://www.biorxiv.org/content/10.1101/2020.06.03.131532v2.full](https://www.biorxiv.org/content/10.1101/2020.06.03.131532v2.full)\
6\) [https://www.nature.com/articles/s41596-021-00591-5](https://www.nature.com/articles/s41596-021-00591-5)(Identification of cancer-related mutations in human pluripotent stem cells using RNA-seq analysis)
