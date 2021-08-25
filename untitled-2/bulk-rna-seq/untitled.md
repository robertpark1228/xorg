---
description: 사용법 위주
---

# STAR2 - HT-seq

### Reference Genome 및 Transcriptome 만들기 

```text
$ STAR --runThreadN 6 --runMode genomeGenerate --genomeDir ./genome --genomeFastaFiles ./Homo_sapiens.GRCh38.dna.primary_assembly.fa --sjdbGTFfile ./Homo_sapiens.GRCh38.104.gtf --sjdbOverhang 150

```

{% hint style="info" %}
--runThreadsN : CPU 갯수  
--runMode : 어떤 작업을 할지 genomeGenerate\(래퍼런스 제작\)   
--genomeDir : 래퍼런스 저장될 폴더 \(STAR 용\)  
--genomeFastaFiles : 래퍼런스 데이터 FASTA  
--sjdbGTF : 래퍼런스 데이터 GTF
{% endhint %}

{% hint style="info" %}
Reference Data Download:

GTF : [ftp://ftp.ensembl.org/pub/release-104/gtf/homo\_sapiens/](ftp://ftp.ensembl.org/pub/release-104/gtf/homo_sapiens/)

FASTA : [ftp://ftp.ensembl.org/pub/release-104/fasta/homo\_sapiens/dna/Homo\_sapiens.GRCh38.dna.primary\_assembly.fa.gz](ftp://ftp.ensembl.org/pub/release-104/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz)
{% endhint %}

```text
$ STAR --genomeDir /n/groups/hbctraining/intro_rnaseq_hpc/reference_data_ensembl38/ensembl38_STAR_index/ \
--runThreadN 6 \
--readFilesIn Mov10_oe_1.subset.fq \
--outFileNamePrefix ../results/STAR/Mov10_oe_1_ \
--outSAMtype BAM SortedByCoordinate \
--outSAMunmapped Within \
--outSAMattributes Standard 
```





## 기타 : STAR Aligner 알고리즘 / 참고

#### STAR Alignment Strategy <a id="star-alignment-strategy"></a>

STAR is shown to have high accuracy and outperforms other aligners by more than a factor of 50 in mapping speed, but it is memory intensive. The algorithm achieves this highly efficient mapping by performing a two-step process:

1. Seed searching
2. Clustering, stitching, and scoring

**Seed searching**

For every read that STAR aligns, STAR will search for the longest sequence that exactly matches one or more locations on the reference genome. These longest matching sequences are called the Maximal Mappable Prefixes \(MMPs\):

![STAR\_step1](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/img/alignment_STAR_step1.png)

The different parts of the read that are mapped separately are called ‘seeds’. So the first MMP that is mapped to the genome is called _seed1_.

STAR will then search again for only the unmapped portion of the read to find the next longest sequence that exactly matches the reference genome, or the next MMP, which will be _seed2_.

![STAR\_step2](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/img/alignment_STAR_step2.png)

This sequential searching of only the unmapped portions of reads underlies the efficiency of the STAR algorithm. STAR uses an uncompressed suffix array \(SA\) to efficiently search for the MMPs, this allows for quick searching against even the largest reference genomes. Other slower aligners use algorithms that often search for the entire read sequence before splitting reads and performing iterative rounds of mapping.

**If STAR does not find an exact matching sequence** for each part of the read due to mismatches or indels, the previous MMPs will be extended.

![STAR\_step3](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/img/alignment_STAR_step3.png)

**If extension does not give a good alignment**, then the poor quality or adapter sequence \(or other contaminating sequence\) will be soft clipped.

![STAR\_step4](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/img/alignment_STAR_step4.png)

**Clustering, stitching, and scoring**

The separate seeds are stitched together to create a complete read by first clustering the seeds together based on proximity to a set of ‘anchor’ seeds, or seeds that are not multi-mapping.

Then the seeds are stitched together based on the best alignment for the read \(scoring based on mismatches, indels, gaps, etc.\).

![STAR\_step5](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/img/alignment_STAR_step5.png)

