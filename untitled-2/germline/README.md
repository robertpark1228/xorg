---
description: 분석 실습
---

# \[DNA\]Germline Mutation 분석

## 분석 개요

![](../../.gitbook/assets/image%20%2851%29.png)

## 필수  다운로드

{% tabs %}
{% tab title="SampleQC" %}
1\) [https://www.bioinformatics.babraham.ac.uk/projects/trim\_galore/](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

2\) [http://www.usadellab.org/cms/?page=trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)

3\) [https://multiqc.info/](https://multiqc.info/)
{% endtab %}

{% tab title="Mapping" %}
1\) bwa :  [https://github.com/lh3/bwa](https://github.com/lh3/bwa)
{% endtab %}

{% tab title="Mapping + QC" %}
1\) GATK4 : [https://github.com/broadinstitute/gatk/releases](https://github.com/broadinstitute/gatk/releases)
{% endtab %}

{% tab title="래퍼런스데이터다운로드" %}
1. GRCh37 또는 GRCh38
2. GATK4 BQSR 을 위한 필수 다운로드
{% endtab %}

{% tab title="EXTRA 파일" %}
1\) vcftools : Exome / Target seq 을 bed 를 이용하여 자를 때 필요   
apt install vcftools  
2\) 사용 bedfile download
{% endtab %}
{% endtabs %}

## 명령어 정리

### Preprocessing - 전처리

[https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim\_Galore\_User\_Guide.md](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md)

#### Trim\_galore 

```
$ trim_galore --trim1 --illumina --paired [file1] [file2] -O [OUTPUT_FOLDER] -j [동시에 일 코어 숫자]
```

{% hint style="info" %}
Paired End  및 일루미나 데이터 기준
{% endhint %}

### Mapping - 맵핑작업 

#### BWA Mapping / Sorting 동시작업

```bash
$ bwa mem -M -t 16 -R "@RG\tID:TEST_1\tSM:TEST_1\tPL:ILLUMINA" Ref_file read_1.fq read_2.fq | samtools view -@ 16 -Sb | samtools sort 16 -O bam -o [Filename].sorted.bam
```

{% hint style="info" %}
-M : GATK Markduplicate 분석을 위함   
-t : 코어수 \(동시에 일할 코어수\)  
-R : ReadGroup \(샘플 이름\)  
Ref\_file : 래퍼런스 파일 \(GRCh37, GRCh38\)  
read\_1 : read 1번  
read\_2 : read 2번  
-Sb: BAM 파일 Output  
Sort : Read 를 Chromosome 별로 Sorting 하고 중복 Masking  
{% endhint %}

### 

### GATK - Mapping QC

```bash
$ java -jar gatk.jar MarkDuplicates REMOVE_DUPLICATES=true ${bam_input} O=${folder}/${custom_name}_sorted_MarkDuplicate.bam M=${folder}/${custom_name}_markduplicate.txt COMPRESSION_LEVEL=1 CREATE_INDEX=true
```

{% hint style="info" %}
Markduplicate 기능    
REMOVEDUPLICATE=true : Duplicate 없앤 파일 제작  
CREATE\_INDEX=true / bam file index 제작
{% endhint %}

### 

### GATK-BQSR 1단계 - 통계치 작성 

```bash
$ ${gatk_4} BaseRecalibrator -L ${interval_bed} -R ${reference} -I ${folder}/${custom_name}_sorted_MarkDuplicate.bam --known-sites ${gatk4_dbsnp} --known-sites ${gatk4_mills} --known-sites ${gatk4_1000g_indel} --known-sites ${gatk4_1000g_snp} -O ${folder}/${custom_name}_sorted_MarkDuplicate.table
```

{% hint style="info" %}
BaseRecalibrator 기능  
-L 옵션 : 필수는 아닙니다. 뽑고 싶은 크로모섬 위치가 필요한 경우  
-R 옵션 : 래퍼런스 파일 위치   
-I 옵션 : 인풋파일  
--Known-sites : Base Quality 향상을 위한 모델 파일\(GATK 제공\)  
-O 옵션 : 최종 파일 \(텍스트파일\) / 계산 정보를 담고 있습니다.
{% endhint %}

### 

### GATK-BQSR 2단계 - 작성 통계치 적용된 파일 제작  

```bash
$ ${gatk_4} ApplyBQSR -L ${interval_bed} -R ${reference} -I ${folder}/${custom_name}_sorted_MarkDuplicate.bam -bqsr ${folder}/${custom_name}_sorted_MarkDuplicate.table -O ${folder}/${custom_name}_sorted_MarkDuplicate_bqsr.bam
```

{% hint style="info" %}
ApplyBQSR 기능  
-L 옵션 : 필수는 아닙니다. 뽑고 싶은 크로모섬 위치가 필요한 경우  
-R 옵션 : 래퍼런스 파일 위치  
-I 옵션 : 인풋파일  
-bqsr 옵션 : 1단계에서 작성된 통계치 파일   
-O 옵션 : 아웃풋 파일 명
{% endhint %}



### GATK-HaplotypeCaller

```bash
$ ${gatk_4} HaplotypeCaller -L ${interval_bed} -R ${reference} -I ${folder}/${custom_name}_sorted_MarkDuplicate_bqsr.bam -O ${folder}/${custom_name}.g.vcf --native-pair-hmm-threads 32 --ERC GVCF -G StandardAnnotation -G AS_StandardAnnotation -G StandardHCAnnotation
```

{% hint style="info" %}
HaplotypeCaller기능  
-L 옵션 : 필수는 아닙니다. 뽑고 싶은 크로모섬 위치가 필요한 경우  
-R 옵션 : 래퍼런스 파일 위치  
-I 옵션 : 인풋파일  
-O 옵션 : 아웃풋 파일 명  
--native-pair-hmm-threads : 알고리즘 계산시 들어갈 코어 숫자 \(숫자\)  
--ERC GVCF : g.vcf 아웃풋  
-G StandardAnnotation : 콜링 알고리즘 옵션  
-G AS\_StandardAnnotation : 콜링 알고리즘 옵션  
-G StandardHCAnnotation : 콜링 알고리즘 옵션  
  
\([https://gatk.broadinstitute.org/hc/en-us/articles/360035890551-Allele-specific-annotation-and-filtering-of-germline-short-variants](https://gatk.broadinstitute.org/hc/en-us/articles/360035890551-Allele-specific-annotation-and-filtering-of-germline-short-variants)\)  
Using the locally-realigned reads, HaplotypeCaller will generate GVCFs with all of its usual standard annotations, plus raw data to calculate allele-specific versions of the standard annotations. That means each alternate allele in each VariantContext will get its own data used by downstream tools to generate allele-specific QualByDepth, RMSMappingQuality, FisherStrand and allele-specific versions of the other standard annotations. For example, this will help us sort out good alleles that only occur in a few samples and have a good balance of forward and reverse reads but occur at the same position as another allele that has bad strand bias because it’s probably a mapping error.
{% endhint %}



### GATK-GenotypeGVCFs

```bash
$ ${gatk_4} GenotypeGVCFs -R ${reference} -V ${folder}/${custom_name}.g.vcf -O ${folder}/${custom_name}.vcf
```

{% hint style="info" %}
GenotypeGVCFs 기능  
-R 옵션 : 래퍼런스 파일 위치  
-V 옵션 : 인풋파일 \(haplotype Caller 결과\)  
-O 옵션 : 아웃풋 파일
{% endhint %}



### Exome / Target Seq 의 경우 후 처리 방법 

1. vcftools 를 이용한 방법 

```bash
$ vcftools --vcf ${file} --bed ${PWD}/references/6format_grch37_agil_v6.bed --out ${file}_agilent_v6.vcf --recode --keep-INFO-all
```

{% hint style="info" %}
--vcf : 원하는 vcf 파일  
--bed : 자를때 필요한 위치 정보 파일  
--out : Output 파일 이름   
--recode : 내용 변화를 주지 않  
--keep-INFO-all: vcf 내용에 변화를 주지 않
{% endhint %}

   2. bcftools 를 이용한 방법 \(tabix 를 이용하여 vcf 파일 indexing 필수\) 

```bash
$ bcftools norm --threads 40 -f ${ref} ${file} -R ${exome5bed} -O z -o ${file}.agil5.gz
```

{% hint style="info" %}
norm : indel 위치 normalization \(노멀라이즈 아래 Figure 참고\)  
--threads : 동시에 동작할 코어 수  
-f : 레퍼런스 파일  
-R : bed 로 된 region 파일  
-O : gzip 으로 나가는 경우  
-o : output 파일    
${file} : input 파일   
  
\([https://genome.sph.umich.edu/wiki/Variant\_Normalization](https://genome.sph.umich.edu/wiki/Variant_Normalization)\)  
{% endhint %}

![](../../.gitbook/assets/image%20%2889%29.png)

### 기타

## 래퍼런스 파일 다운로드 받는 위치 

[https://console.cloud.google.com/storage/browser/genomics-public-data/resources/broad/hg38/v0/  
](https://console.cloud.google.com/storage/browser/genomics-public-data/resources/broad/hg38/v0/
)

